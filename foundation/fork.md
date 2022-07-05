### 1、背景
我们常在Linux平台bash环境下执行一条cmd，如看下当前文件有哪些"ls -l"。这条cmd会fork一个新的进程,然后完成ls可执行程序的加载和执行。
https://mp.weixin.qq.com/s/OhvLguQtJucoIzecB7Tejw
https://blog.csdn.net/tiankong_/article/details/76190708
### 2、用户态触发
在用户态调用fork，生成一个新进程（子进程）,如图1。成功后，子进程的pid = 0，子进程就可以为所欲为了，当然对于bash而言，主要还是执行cmd，如ls -l。
```c
#include<stdio.h>
#include<unistdio.h>
int main() {
	/*内核起点*/
	pid_t pid = fork();
	/*各自分工*/
	if (pid > 0){
		printf("i am parent program/n");
	}
	else if (pid == 0){
		printf("i am new child program/n");
	}
	else
		printf("error/n")

	return 0;
}
```
下图就比较清晰的说明fork前父进程和子感觉像是运行一样的进程，判断pid之后，父子开始分道扬镳，至于为何pid不同，需要从内核层面找到原因。

从用户态的汇编代码可以知道：父、子进程在fork后会判断寄存器eax (x86架构)的返回值，即pid。

![[f56c5502783ddc1787624f466bd3f77d.png]]
### 3、内核fork
从用户空间调用的fork通过中断调用到内核sys_fork，继续调用到`kernel_clone`，该函数注意看返回的是pid_t，即进程pid。
```c
#ifdef __ARCH_WANT_SYS_FORK
{
#ifdef CONFIG_MMU
	struct kernel_clone_args = {
		.exit_signal = SIGCHLD,
	};

	return kernel_clone(&args);
#else
	return -EINVAL;
#endif
}
#endif
```
下面是`kernel_clone`的关键操作，`copy_process`，`wake_up_new_task`
```c
pid_t kernel_clone(struct kernel_clone_args *args)
{
	u64 clone_flags = args->flags;
	struct completion vfork;
	struct pid *pid;
	struct task_struct *p;
	int trace = 0;
	pid_t nr;

	...
	p = copy_process(NULL, trace, NUMA_NO_NODE, args);
	add_latent_entropy();
	
	...
	wake_up_new_task(p);
	
	...
	put_pid(pid);
	return nr;
}
```
### 4、进程信息拷贝copy_process
这一站就是fork的核心操作了，一步步探索其中的奥秘。从注释看到，`copy_process`函数仅做进程所拥有的各种资源复制，复制完成后，新进程并没有启动
```c
 * This creates a new process as a copy of the old one,
 * but does not actually start it yet.
 *
 * It copies the registers, and all the appropriate
 * parts of the process environment (as per the clone
 * flags). The actual kick-off is left to the caller.
 */
static __latent_entropy struct task_struct *copy_process(
					struct pid *pid,
					int trace,
					int node,
					struct kernel_clone_args *args)
{
	int pidfd = -1, retval;
	struct task_struct *p;
	struct multiprocess_signals delayed;
	struct file *pidfile = NULL;
	u64 clone_flags = args->flags;
	
```
#### 首先开始一系列的flag检查
```c
/*
 * Don't allow sharing the root directory with processes in a different
 * namespace
 */
if ((clone_flags & (CLONE_NEWNS|CLONE_FS)) == (CLONE_NEWNS|CLONE_FS))
	return ERR_PTR(-EINVAL);

if ((clone_flags & (CLONE_NEWUSER|CLONE_FS)) == (CLONE_NEWUSER|CLONE_FS))
	return ERR_PTR(-EINVAL);
...

/*
 * If the new process will be in a different time namespace
 * do not allow it to share VM or a thread group with the forking task.
 */
if (clone_flags & (CLONE_THREAD | CLONE_VM)) {
	if (nsp->time_ns != nsp->time_ns_for_children)
		return ERR_PTR(-EINVAL);
}

if (clone_flags & CLONE_PIDFD) {
	/*
	 * - CLONE_DETACHED is blocked so that we can potentially
	 *   reuse it later for CLONE_PIDFD.
	 * - CLONE_THREAD is blocked until someone really needs it.
	 */
	if (clone_flags & (CLONE_DETACHED | CLONE_THREAD))
		return ERR_PTR(-EINVAL);
}
```
#### 复制task_struct关键信息
`copy_process`开始复制第一个重要的资源：<mark style="background: #ABF7F7A6;">task_struct</mark> 。
```c
p = dup_task_struct(current, node);
if (!p)
	goto fork_out;
```
每个进程在内核中都有一个进程控制块(PCB)来维护进程相关的信息，Linux内核的进程控制块是task_struct结构体。
这个结构体主要是用于内核管理任务，包括进程状态、进程标识符、进程亲属关系、ptrace系统调用、进程高度、进程地址空间、进程返回码判断标志、时间统计、信号处理、内存、文件描述符等。
`dup_task_struct`，主要工作有：  

1 、分配task_struct内存

2 、分配thread_info内存

3、 复制父进程的task_struct信息

4 、设置线程堆栈

关于thread_info，每当进程从用户态进入内核态后都要使用栈，这个栈叫做进程的内核栈。当进程已进入内核栈，CPU就自动设置该进程的内核栈，这个栈位于内核的数据段上。为了节省空间，linux把内核栈和一个紧挨近PCB的小数据结构thread_info放在一起，占用8KB的内存空间。
在`dup_task_struc`t中会调用`alloc_thread_info`分配两个页（8KB）的空闲内存（其内部调用的是一个__get_free_psages函数）。
有了task_struct结构体，后边`copy_process`继续复制其他资源到该结构体中。
#### 调度相关初始化
task_struct结构初始化之后，开始进行调度相关的初始化，这里仅设置任务状态和优先级
```c
/* Perform scheduler related setup. Assign this task to a CPU. */
retval = sched_fork(clone_flags, p);
if (retval)
	goto bad_fork_cleanup_policy;
```
`sched_fork`该函数主要工作：

1 、初始化跟调度相关的值，比如调度实体，运行时间等

2 、根据父进程的运行优先级设置设置进程的优先级

3 、设置调度类rt_sched_class（实时进程）或者fair_sched_class（普通进程）
#### 复制进程主要数据
接着继续复制files、fs、signal、mm、io等数据：
>copy_mm：如果是进程，则将父进程的mm_struct结构复制到子进程中，然后修改当中属于子进程有别于父进程的信息(如页目录)。如果是线程，则将子线程的mm指针和active_mm指针都指向父进程的mm指针所指结构。
```c
if (retval)
	goto bad_fork_cleanup_semundo;
retval = copy_fs(clone_flags, p);
if (retval)
		goto bad_fork_cleanup_files;
retval = copy_sighand(clone_flags, p);
if (retval)
		goto bad_fork_cleanup_fs;
retval = copy_signal(clone_flags, p);
if (retval)
	goto bad_fork_cleanup_sighand;
retval = copy_mm(clone_flags, p);
if (retval)
	goto bad_fork_cleanup_signal;
retval = copy_namespaces(clone_flags, p);
if (retval)
	goto bad_fork_cleanup_mm;
retval = copy_io(clone_flags, p);
if (retval)
	goto bad_fork_cleanup_namespaces;
```
#### 复制线程
```c
retval = copy_thread(clone_flags, args->stack, args->stack_size, p, args->tls);
if (retval)
	goto bad_fork_cleanup_io;
```
copy_thread函数将子进程的ip寄存器值设置为ret_from_fork()的地址，childregs->ax，即新进程的返回值设置为0,执行完ret_from_fork中的一些代码，返回到用户态后，开始执行fork之后的代码。
```c
int copy_thread(unsigned long clone_flags, unsigned long sp, unsigned long arg,
		struct task_struct *p, unsigned long tls)
{
	struct inactive_task_frame *frame;
	struct fork_frame *fork_frame;
	struct pt_regs *childregs;
	int ret = 0;
	...
	frame->ret_addr = (unsigned long) ret_from_fork;
	...
	frame->bx = 0;
	*childregs = *current_pt_regs();
	childregs->ax = 0;
	if (sp)
		childregs->sp = sp;
```
ret_from_fork函数主要完成新进程寄存器的恢复，包括SS、CS段寄存器等，然后返回用户模式。
```c
.pushsection .text, "ax"
SYM_CODE_START(ret_from_fork)
	UNWIND_HINT_EMPTY
	movq	%rax, %rdi
	call	schedule_tail			/* rdi: 'prev' task parameter */

	testq	%rbx, %rbx			/* from kernel_thread? */
	jnz	1f				/* kernel threads are uncommon */

2:
	UNWIND_HINT_REGS
	movq	%rsp, %rdi
	call	syscall_exit_to_user_mode	/* returns with IRQs disabled */
	jmp	swapgs_restore_regs_and_return_to_usermode
```
#### 设置全局PID和收尾工作
https://blog.csdn.net/qq_33160790/article/details/81346663
将子进程的PID设置为在全局namespace中分配的PID，在不同namespace中进程的PID不同，而p->pid保存的是全局的namespace中所分配的PID。
```c
/* ok, now we should be set up.. */
	p->pid = pid_nr(pid);
```
接着进行TGID、PGID、SID的设置，并加入到链表中。
```c
init_task_pid(p, PIDTYPE_TGID, pid);
init_task_pid(p, PIDTYPE_PGID, task_pgrp(current));
init_task_pid(p, PIDTYPE_SID, task_session(current));
...
list_add_tail(&p->sibling, &p->real_parent->children);
list_add_tail_rcu(&p->tasks, &init_task.tasks);
attach_pid(p, PIDTYPE_TGID);
attach_pid(p, PIDTYPE_PGID);
attach_pid(p, PIDTYPE_SID);
```
然后进行proc文件系统等的关联，最后返回task_struct结构体指针。
```c
proc_fork_connector(p);
...
return p;
```
### 5、新进程调度运行
经过漫长的复制操作，最后回到第2站内核的fork过程，在kernel_clone中，copy_process结束后，调用wake_up_new_task函数。
这表明新进程调度相关的初始化已经完成，但是还没有被调度器加入到队列中，是不能被调度执行的。因此，该函数主要是将新进程加入到队列中。
```c
wake_up_new_task(p);

/* forking complete and child started to run, tell ptracer */
if (unlikely(trace))
	ptrace_event_pid(trace, pid);

if (clone_flags & CLONE_VFORK) {
	if (!wait_for_vfork_done(p, &vfork))
		ptrace_event_pid(PTRACE_EVENT_VFORK_DONE, pid);
}

put_pid(pid);
return nr;
```
在wake_up_new_task函数中，将进程加入到运行队列的函数为activate_task,而activate_task函数最后会调用到新进程调度类中的enqueue_task指针所指函数。
该函数会调用CFS调度类的enqueue_task_fair（普通进程），接着会调用enqueue_entity函数，完成调度中运行时间、权重等设置，最后加入CFS红黑树进程运行队列中，等待被调度执行。
```c
void wake_up_new_task(struct task_struct *p)
{
	struct rq_flags rf;
	struct rq *rq;

	raw_spin_lock_irqsave(&p->pi_lock, rf.flags);
	p->state = TASK_RUNNING;
	...
	rq = __task_rq_lock(p, &rf);
	update_rq_clock(rq);
	post_init_entity_util_avg(p);

	activate_task(rq, p, ENQUEUE_NOCLOCK);
```
### 6、主要流程
![[651d2fdc3fab94b28ef2fa508e9daaa5.png]]