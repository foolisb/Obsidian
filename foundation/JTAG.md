# JTAG
### WHAT IS JTAG
JTAG英文全称是Joint Test Action Group，翻译过来就是联合测试工作组。JTAG是一种IEEE标准用来解决板级问题，诞生于20世纪80年代。今天被用来烧录、debug、探查端口。当然最早是用于边界测试。

### 1、边界测试 
举个例子，有两个芯片，者两个芯片之间连接了很多的线，怎么确保这些线之间的连接是正确的呢？用JTAG可以控制所有的IC引脚。这叫做芯片边界测试。
![[Pasted image 20220512101744.png]]
### 2、JTAG引脚
JTAG发展到系现在已经有引脚了，通常四个脚：TDI，TDO，TMS，TCK，当然还有个复位脚TRST。对于芯片上的JTAG的脚实际上是专用的。
- TDI：测试数据输入，数据通过TDI输入JTAG口；
- TDO：测试数据输出，数据通过TDO从JTAG口输出；
- TMS：测试模式选择，用来设置JTAG口处于某种特定的测试模式；
- TCK：测试时钟输入；
- TRST：测试复位；

![[Pasted image 20220512112835.png]]
### 3、JTAG如何工作
PC控制JTAG：用JTAG电缆连接PC的打印端口或者USB或者网口。

TMS：在每个含有JTAG的芯片内部，都会有个JTAG TAP控制器。TAP控制器是一个有16个状态的状态机，而TMS就是它的控制信号。当TMS把各个芯片都连接在一起的时候，所有芯片的TAP状态跳转是一致的。下面是TAP控制器的示意图：
![[Pasted image 20220512113417.png]]

改变TMS的值，状态就会发生跳转。如果保持5个周期的高电平，就会跳回test-logic-rest，通常用来同步TAP控制器；通常使用两个最重要的状态是Shift-DR和Shift-IR，两者连接TDI和TDO使用。

IR：命令寄存器，你可以写值到这个寄存器中通知JTAG干某件事。每个TAP只有一个IR寄存器而且长度是一定的。

DR：TAP可以有多个DR寄存器，与IR寄存器相似，每个IR值会选择不同的DR寄存器。