**一、引言**

无线充电技术最早出现于19世纪末，当时的物理学家Nikola Tesla演示了磁共振耦合——在两个电路（一个发射器一个接收器）之间建立磁场，通过空气来传输电能。但在之后的大约100年时间里，这项技术并没有得到多少实际应用。直到近年来智能终端设备的广泛应用，尤其是智能手机的普及，才让无线充电技术得以重新发展和推广。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/d4hoYJlxOjMIg11UxZhiagLbLuvuc2hztyfkKbaQwtmkxcjVmcWNvMrxp5J6CIduEgmtUnuic9M1Xibq30DekJncg/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1)


**二、基本原理**

**1. 技术概览**

目前，无线充电的方式主要有四种类型：通过电磁感应的磁耦合方式，通过电磁波近场谐振的磁共振方式，通过电场进行能量传输的电场耦合方式，以及通过电磁波辐射的微波传输方式。四种方式的对比如下表：

![图片](https://mmbiz.qpic.cn/mmbiz_png/d4hoYJlxOjMIg11UxZhiagLbLuvuc2hztrrhIeexNsN4ibEvsBFWqcPsXDZS6AJR47ofYo8d2hr2XHTB116iarAIA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

以上四种类型中，电磁感应的磁耦合方式在目前的便携式设备中应用最广泛，方案也最成熟；而微波传输的自由度更高，更为便捷，是未来便携式设备“隔空充电”，“追踪充电”方案实现的基础，但目前方案并不成熟；磁共振方式则更多应用于电动汽车的无线充电。


**2. 手机无线充电原理**

法拉第电磁感应定律：闭合电路的一部分导体在磁场里做切割磁感线的运动时，导体中就会产生电流。这是无线充电最基础的原理。

当电流通过线圈时，会产生磁场；当闭合电路磁通量发生变化时，会产生感应电动势。

结合手机无线充电来说，充电底座和手机背部各有一个线圈，充电底座通过线圈将电流转化为磁场，并且是不断变化的磁场。而手机背部的线圈也因为底座磁场不断的变化，其中的磁通量也在不断变化，产生感应电动势，有了感应电流，再转化为直流电进行充电。


![图片](https://mmbiz.qpic.cn/mmbiz_png/d4hoYJlxOjMIg11UxZhiagLbLuvuc2hztHYSTjuHlEMibfEW0m6D0YHwicMkUnfSaX2TQ5fqO0fwQQgUiasnTbRP9Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

以常见的变压器举例，变压器的初级和次级线圈没有直接连接，而是通过磁场耦合将电能从初级传输到次级。初级和次级通过铁芯相连提高耦合的效率。

![图片](https://mmbiz.qpic.cn/mmbiz_png/d4hoYJlxOjMIg11UxZhiagLbLuvuc2hztmToSzdqD7JWLrh0WjlHWCGxxRpIzIn5udYsoROc2cy949WdFobqxibg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

而手机背部和底座的线圈之间没有铁芯，如图所示，初级线圈的磁场在空间上是发射的。这样就需要次级线圈贴近初级线圈，同时保持对齐才可以获得较大的传输效率。因此磁耦合方式的无线充电，充电距离和自由度相对比较小。

并且，由于能量转化效率和有线充电相比并不高，多余的能量会以热量的形式散发，导致无线充电一段时间后，手机和底座都会发烫。这也是很多无线充电底座都会配置风扇的原因。同时，由于金属有电磁屏蔽的作用，所以带有无线充电的手机的背盖都不会用金属，一般会采用玻璃、陶瓷，或者塑料。

  

尽管磁耦合的方式有以上这些缺陷和限制，但它仍然是目前手机无线充电最成熟、高效和通用的解决方案。

  

**三、方案介绍**

**1. Qi协议**

Qi（发音与命名来自中文字“气”），是一种由无线充电联盟（Wireless Power Consortium，缩写WPC）所制定的短距离（40mm, 1.6英寸）低功率无线感应式电力传输的互连标准，主要目的是提供移动电话手机与其他便携式电子设备便利与通用的无线充电。这也是当前最主流的手机无线充电协议。

![图片](https://mmbiz.qpic.cn/mmbiz_png/d4hoYJlxOjMIg11UxZhiagLbLuvuc2hztic7XrQnIQ9QiaYx06ZlYAQr7sjPrWtxYMdnY4OC2iaYGXwM0cyjMgsfmw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**2. 硬件框图**

下图为QI协议无线充电电路基本框架。一般我们将发射端称为Tx，接收端称为Rx。如图，发射端的逆变桥将DC适配器输入的直流电转换为高频交流电供给Tx线圈，将能量耦合到Rx线圈上，线圈部分还会加补偿电容，提高功率因数。整流桥和稳压器将Rx线圈上的交流电转换成直流电给电池供电。

![图片](https://mmbiz.qpic.cn/mmbiz_png/d4hoYJlxOjMIg11UxZhiagLbLuvuc2hztBsTqrqS80YvUrHX7hriaTzfdXVianXTgicibt4de1pBKgxNyEpVLGFwb0Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

其中，发射端是由初级线圈（Lp）和串联电容（Cp）组成的谐振电路，接收端是由次级线圈（Ls）和串联电容（Cs）组成的功率接收回路。Tx端通过调整输入电压、谐振频率以及PWM占空比来调整输出功率，而Rx端通过监测接收整流输出电压Vrect，来告知Tx升功率或是减小功率。

![图片](https://mmbiz.qpic.cn/mmbiz_png/d4hoYJlxOjMIg11UxZhiagLbLuvuc2hztDZwVrqfbpc8Io9sicpFLjgdP3vCudpTygBB7VusR2tDCbndyXDHP4Zg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**3. 通信协议**

**（1）通信原理**

Qi的数据通信采用的是backscatter调制方式，是一种负载调制方式。如上图所示，接收端通过开关Cm或者Rm调制数据到功率载波上，发射端通过采样发射线圈的电流或者电压波形，解调负载的变化接收数据。

![图片](https://mmbiz.qpic.cn/mmbiz_png/d4hoYJlxOjMIg11UxZhiagLbLuvuc2hztnRbHg8UWOfuXZxN5RGcO6vY8RjHNc4CXvx8n1dD2UfEicPLDqGavFug/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

详细来说，Rx提供数字信息给Tx，如识别信息和所需传递的能量要求。Rx控制器可以通过集成的LDO(<mark style="background: #BBFABBA6;">低压差线性稳压器</mark> )来做负载控制，更改其阻抗调节功率量。此操作将导致Tx的线圈电流或线圈电压周期性变化，Tx通过检测线圈电流或线圈电压变化，解调出交流负载中的各种数字通信信息，根据Rx的请求调整对其输出功率水平。

**（2） 数据包的构成**

数据通信包含接收端Rx发出的ASK和发射端Tx发出的FSK信号。

ASK:Rx通过改变自身谐振腔参数，（切入/切出通讯电容），改变所传递的能量大小，引起Tx 端电压或输入电流产生规律性的波动，用于传递信息；

FSK:Tx通过往复增大/减小驱动频率，改变Rx端接收到能量的波动。当这种波动按照特定的规律波动，就可以传递信息。

![图片](https://mmbiz.qpic.cn/mmbiz_png/d4hoYJlxOjMIg11UxZhiagLbLuvuc2hztN80MPxIokSTEr9U0HYUYHPhUNxNrklxF12ly3t9dcFhjHLeCR4jTLQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

接下来以ASK为例，讲解一下数据包的构成。

ASK数据包由四部分构成：Preamble序言，Header包头，Message数据帧和Checksum校验帧。序言是一组全部为1的脉冲，作用是使功率发射器同步输入数据并准确检测包头的起始位；包头包含一个byte，由01组合构成，用于告诉Tx包的种类；数据帧包含1-27个byte，由01组合构成，用于存储包的数据；校验帧包含一个byte，是提供给Tx用来识别包信息是否正确。包头、数据、校验等都是由一个或者多个byte 构成，每个byte数据又由不同的位（bit）构成。

![图片](https://mmbiz.qpic.cn/mmbiz_png/d4hoYJlxOjMIg11UxZhiagLbLuvuc2hztway53KxbSRBNbCWQO91hUwCc2KGfQsdUzDoHfNxSsKIAwhLRsiaW1Cg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)
  
其中的byte 构成如下图。电源接收端（移动设备端）采用 11 位异步串行格式传输数据字节。数据编码格式为：起始位0、 8 位数据位、一个奇偶校验位（如果数据字节包含偶数个 1 位，则奇偶校验位为 1。否则为零）和一个停止位1组成。


![图片](https://mmbiz.qpic.cn/mmbiz_png/d4hoYJlxOjMIg11UxZhiagLbLuvuc2hztibCJnwOoCzydParbrDJaia3k75DLS6MibWWl058jeibWfoU4KHMy9Kqd9A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)


每个byte数据又由位（bit）构成。如下图，协议规定时钟频率2Kbit/s，所以每一位的传输时间约0.5ms。数据0为0.5ms的高电平或者0.5ms的低电平，数据1为0.25ms高电平+0.25ms低电平，或者0.25ms低电平+0.25ms高电平。

![图片](https://mmbiz.qpic.cn/mmbiz_png/d4hoYJlxOjMIg11UxZhiagLbLuvuc2hzt48lDCibbzic6bE1XIKVGiaBJh2HrV5OS2U9jTAtqHCoc6wKRZ8zdCdnPA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

一些常见的ASK数据包类型：

![图片](https://mmbiz.qpic.cn/mmbiz_png/d4hoYJlxOjMIg11UxZhiagLbLuvuc2hztVyYZsobAx9g0daULhIic7eOHqxUOfBd3Dm1gFrPmD8H9WVyHAtNvictA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

以上为ASK数据包的构成，而FSK数据包与ASK的主要差别在于发包方式、校验方式，以及数据包结构没有序言。


**（3） 通信流程**

QI无线充电协议目前包含两个功率模式：BPP和EPP。BPP（Baseline Power Profile） 支持最高传输5W的功率，EPP（ExtendedPower Profile）支持传输最高15W的功率。要实现EPP高功率模式的传输，需要底座的支持。而一些更高功率的私有协议（手机厂商自有的），一般是在识别BPP模式后，再识别私有协议，并且需要适配器和底座的支持（比如小米、华为40w）。

BPP和EPP两者在通信上的差异就在于前面讲到的两种通信方式：BPP 仅ASK单向通信；EPP 需要ASK 和 FSK双向通信。

通信的主要流程包含四个阶段：selection，ping，identification& configuration， power transfer。后面三个阶段就是通过发数据包的方式进行通信。

![图片](https://mmbiz.qpic.cn/mmbiz_png/d4hoYJlxOjMIg11UxZhiagLbLuvuc2hztY9ic7SyuD2rozCicLO4m6z5VfSRHTGtKlzRojU7hLicVM0XKZTOVick79Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

1）selection阶段:用于检测是否有Rx存在，检测方法可以是定期发射固定频率的信号（Analog Ping），也可以使用检测电容的变化来实现检测；

2）ping阶段：检测到Rx后，Tx会发送足够能量的信号（Digital Ping）启动Rx的通讯功能。Rx回应信号强度指示包，Tx收到信号强度包后，会维持电源信号，系统进入下一阶段；

3）identification & configuration阶段:Rx将WPC信息参数，比如所需最大输出功率等参数发送给Tx。如果是EPP模式，由于其包含5W，10W，15W三种功率水平，不同的底座支持的最大传输功率不同。因此在这里会多出Negotiation阶段和Calibration阶段，用于协商来确定最终的传输功率，以及过程中的功率大小。 

4）power transfer阶段: 完成配置后，进入电力传输阶段。Rx会检测整流后的电压电流，发送错误包（Error Packet）使Tx增大或减少发射功率。此外，Rx会定期发送收到功率包发给Tx，如果Tx发现收到功率与所发射功率差异过大，为确保传输安全,会停止发射功率并关闭系统。如果Rx不再需要电源（电池已经充满）时，会发送结束功率传输包，Tx接收到后返回selection阶段。


**（4） 异物检测（FOD）**

在无线充电功率传输的过程中，一旦金属物体出现在正在进行电源传输的Tx和Rx线圈之间时，金属物体就会因在其内部生成的涡状电流而发热，如果没有采取任何安全措施，金属物体就会变得越来越热，引发危险。因此，无线充电有一项很重要的检测，叫做异物检测（Foreign Object Detection）。
  
其原理是检测功率的损耗:Rx会定期发送收到的功率包发给Tx，如果Tx发现收到功率与所发射功率差异过大，超过某个阈值，为确保电源传输安全，会停止功率传输。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/d4hoYJlxOjMIg11UxZhiagLbLuvuc2hztNNXRajFoIyzcTV2AVdX5NhJQ3BbVbIybLpg9ebicXrYHz9iaTrNqIibGA/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1)

**四、未来发展方向**

目前手机无线充电应用最广泛的Qi协议。就如前面提到的，这种基于电磁感应的磁耦合方式仍然存在较多缺陷，比如自由度不高，能量转换效率不高等等。这些缺陷在日常应用中就会被放大：只能放在底座上不能拿开、无线充容易发热、无线功率比不上有线。这也是无线充电至今还没有取代有线充的原因。

而无线充电未来的发展方向，大概可以归为两类：一是功率的提高，二是方式的改变。功率的提高用于解决无线充电较慢的问题，但也要面临更严重的发热。如何做到速度与温度得兼，是提高功率必须要考虑的。而无线充电方式的改变，则是解决充电自由度的问题。怎样才能摆脱充电线和底座的束缚？前文也提到过了，目前已经有一些基于电磁波辐射的微波传输方式的预研，用来实现便携式设备“隔空充电”，“追踪充电”的方案。但目前方案的缺陷较多，市面上暂时没有落地的产品。