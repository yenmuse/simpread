> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/HiWangWenBing/article/details/118880746)

作者主页 ([文火冰糖的硅基工坊](https://blog.csdn.net/HiWangWenBing))：[https://blog.csdn.net/HiWangWenBing](https://blog.csdn.net/HiWangWenBing)

本文网址：[https://blog.csdn.net/HiWangWenBing/article/details/118880746](https://blog.csdn.net/HiWangWenBing/article/details/118880746)

**目录**

[第 1 章 RF 通用架构概述](#t0)

[第 2 章 RF 架构改进诉求与动力](#t1)

[第 3 章 RF 架构演进](#t2)

[3.1 关键技术指标的演进过程](#t3)

[3.2 RFIC 两大 RF 架构](#t4)

[3.3 数字前端架构的演进](#t5)

[附录：常见专业词汇](#t6)

第 1 章 RF 通用架构概述
---------------

![](https://img-blog.csdnimg.cn/20210718223440610.png)

![](https://img-blog.csdnimg.cn/20210718223426768.png)

 ![](https://img-blog.csdnimg.cn/20210808111657751.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hpV2FuZ1dlbkJpbmc=,size_16,color_FFFFFF,t_70)

第 2 章 RF 架构改进诉求与动力
------------------

![](https://img-blog.csdnimg.cn/2021071822395057.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hpV2FuZ1dlbkJpbmc=,size_16,color_FFFFFF,t_70)

第 3 章 RF 架构演进
-------------

### 3.1 关键技术指标的演进过程

![](https://img-blog.csdnimg.cn/20210808155822136.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hpV2FuZ1dlbkJpbmc=,size_16,color_FFFFFF,t_70)

### 3.2 RFIC 两大 RF 架构

**（1）零中频的架构（传统架构）**

![](https://img-blog.csdnimg.cn/20210808160740169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hpV2FuZ1dlbkJpbmc=,size_16,color_FFFFFF,t_70)

*   上下行的切换在模拟域进行选择，模拟域的上下行的转换的不完美性通过数字域弥补
*   主要用于单频带和虚拟双频带（4G+5G 共 RF 射频）
*   最大带宽 400-600 兆，极限在 1G 带宽。
*   功耗取决于较低的数据采样率（模拟到数据转换的数据采样率取决于带宽）

**（2）RF 采样的架构（5G 新架构）**

![](https://img-blog.csdnimg.cn/20210808161421974.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hpV2FuZ1dlbkJpbmc=,size_16,color_FFFFFF,t_70)

*    上下行的切换**数字域**进行选择，克服了模拟域切换的缺点
*   主要应用与双频带或多频带的场合
*   支持的带宽在 1G 以上
*   适当的功耗的增加（更好的数字域的采样频率）

### 3.3 数字前端架构的演进

**（1）数字前端与 RFIC 分离的架构**

![](https://img-blog.csdnimg.cn/20210808162411732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hpV2FuZ1dlbkJpbmc=,size_16,color_FFFFFF,t_70)

*    在此架构中，DFE 与 RFIC 是分离的，DFE 处理处置前端，RFIC 处理射频
*   DEF 与 RFIC 之间采用 JESD2048 标准接口
*   JESD2048 有额外的功率损耗

**（2）数字前端与 RFIC 集成一体的架构**

![](https://img-blog.csdnimg.cn/20210808162711235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hpV2FuZ1dlbkJpbmc=,size_16,color_FFFFFF,t_70)

*   在此架构中，DFE 与 RFIC 集成在一个数字 + 模拟的混合芯片中。
*   更低的功耗（没有 DFE 与 RFIC 接口、体积更小）
*   更小的体积
*   更高的抗干扰能力

附录：常见专业词汇
---------

*   **RRH (Remote Radio Head)：[射频拉远](https://baike.baidu.com/item/%E5%B0%84%E9%A2%91%E6%8B%89%E8%BF%9C/3702095)头**

是用于移动宽带网络基站中的新技术设备，将以前的基站模块的一部分分离出来，通过将无线[基带](https://baike.baidu.com/item/%E5%9F%BA%E5%B8%A6/897959)控制与 RRH 分离。

*   **RRU (Remote Radio Unit)：远程射频单元**

与 RRU 基本同义，除了 RRU 是不带带线本身的。

*   **AAS (Active Antenna System): 有源天线系统**

为了支持更强的 MIMO 和分集接收能力，RRU 需要支持的天线端口越来越多，从两端口，发展到 4 端口甚至 8 端口，再到几个百个端口，对天线的要求越来越高，**连接**也日趋复杂。

这时自然而然地可以想到，既然 RRU 需要和天线近距离安装，还必须用射频线连在一起，那何不把这对搭档合二为一，搞成一个模块呢？

所谓有源，就因为这种带天线的设备需要供电才能把信号发出去，

传统的天线是不需要供电的，也叫做无源天线。

*   **RAP  （**Radio Access Point）：无线接入点、无线访问点

是一种无线网络转换成有线网络的设备。形像点说，无线 AP 是无线网和有线网之间沟通的[桥梁](https://baike.baidu.com/item/%E6%A1%A5%E6%A2%81)。

是一种集成了无线模拟射频 + 无线数字基带 + 传输三大功能为一体的设备，就是一个一体化、集成化的基站。

*   **RF BB**  （Radio Frequency BaseBand）射频基带

在 RF 侧的基带处理，包括数字预失真、削峰，下层到 RRU 中的 PHY_LOW 等功能, RF BB 重在在 RRU 中的基带处理。

与 DFE 数字前端的大部分功能重叠，DFE 不包括 PHY_LOW 的功能，重在与射频前端的数字衔接处理。

*   **DFE  （Digital FrontEnd）：**数字前端

是指最靠近**射频前端**的数字处理，后端是指基带数字处理 BBU.

*   **CFR  （Crest Factor Reduction）：**削峰技术

波峰因子消减技术，简称削峰技术。

*   **DPD  （Digital Pre-Distortion）：**数字预失真处理

通过数字处理技术，使得在功率放大前，对数字信号进行预失真，使得功率放大器 PA 的输出可能的工作在接近饱和区。

*   **DTX  （Discontinuous Transmission）：**不连续发送 / 传送

就是允许发射机在不通话期间，暂时停止发送的技术，主要用于终端侧，降低终端的功耗，同时也需要基站一侧的支持。

*   **TRX** (Transceiver)：收发信机

在基站系统中，TRX 的功能通常是有 RFIC 完成的。每个 RFIC 支持 1T1R, 2T2R, 4T4R, 8T8R, 32T32R........

在 FDD 系统中，通常称为 Tx 和 Rx，发送机和接收机，Tx 和 Rx 可以同时工作。

在 TDD 系统中，用 TRX 替代 Tx 和 Rx，代表 TDD 的接收和发送是分时复用的。

*   **ADVM  （Autonomous Drain Voltage Modulation）：**自主漏极电压调制

这是一种射频的混频或调制技术。

*   **ETA**  （Envelope Tracking Advanced）：高级包络跟踪技术

主要用于接收端的射频解调和混频，RF 射频调制和解调，本质上是模拟幅度调制解调。

![](https://img-blog.csdnimg.cn/20210808112734124.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hpV2FuZ1dlbkJpbmc=,size_16,color_FFFFFF,t_70)

调制的过程就是把基带模拟信号的包络加载到高频载波信号上。 

解调的过程就是从高频载波上恢复信号基带模拟信号的包络的过程。

*   **DVGA  （Digital Variable Gain Amplifie**r）：数字可变增益放大器。

纯模拟电路的放大器，其增益是固定的，而数字可变增益放大器可以通过数字电路控制放大器的增益，按需控制增益，使得放大器的性能更优。

*   **SoC** （System on Chip）片上系统

称为系统级芯片，也有称[片上系统](https://baike.baidu.com/item/%E7%89%87%E4%B8%8A%E7%B3%BB%E7%BB%9F)，意指它是一个产品，是一个有专用目标的集成电路，其中包含完整系统并有嵌入[软件](https://baike.baidu.com/item/%E8%BD%AF%E4%BB%B6)的全部内容。

*   **FEM  （**FrontEnd Module）：前端模块。

硬件电路中的前端模块，完成射频信号的发送放大以及接收放大，甚至包含功率检测、控制和开关的这样一个作用，不包括滤波，是针对 RFIC 而言的，是在 RFIC 的前端。

FEM 是 PA 和 LNA 的统称。

*   **PA  （**Power Amplifier）：功率放大器

是指能输出大功率信号的[放大电路](https://baike.baidu.com/item/%E6%94%BE%E5%A4%A7%E7%94%B5%E8%B7%AF/5118230)， 在无线基站系统中，把无线信号从基站发射出去时的功率放大器。

*   **LNA  （**Low Noise Amplifier）：低噪声放大器

噪声系数很低的放大器。这里放大器，自身的引入的噪声很低，PA 的目标是放大，包括对自身噪声信号的放大，LNA 的目标是低噪声，防止 LNA 自身的噪声对微弱的输入信号造成大的影响。

一般用作各类无线电接收机的高频或中频前置放大器，以及高灵敏度电子探测设备的放大电路。在放大微弱信号的场合，放大器自身的噪声对信号的干扰可能很严重，因此希望减小这种噪声，以提高输出的信噪比。

*   **PHEMT  （**Pseudomorphic High Electron Mobility Transistor）：高电子迁移率晶体管

是一种通过电场效应控制**电流**的电子元件。在基站系统中，主要应用与低噪声放大器的电路中，降低 **LNA** 的噪声系数。

*   **b/o （back-off）：**回退功率

PA 在操作时必须 Back-off，确保输出功率操作在线性区域范围内以维持线性度。否则若输出功率超出线性区，波形可能会被截波，致失真，进而使线性度变差

*   **EIRP（**equivalent isotropically radiated power**）：**等效全向辐射功率

为无线电发射机供给天线的功率与在给定方向上天线绝对增益的乘积。各方向具有相同单位增益的理想全向天线，通常作为无线通信系统的参考天线。EIRP 定义为：EIRP=Pt*Gt，它表示同全向天线相比，可由发射机获得的在最大天线增益方向上的 发射功率。Pt 表示发射机的发射功率，Gt 表示发射天线的天线增益。在无线通信工程中，通常用来衡量干扰的强度，以及发射机发射强信号的能力.

*   **MMIC  （**Monolithic Microwave Integrated Circuit）：单片式微波集成电路

是在半绝缘半导体[衬底](https://baike.baidu.com/item/%E8%A1%AC%E5%BA%95/5853489)上用一系列的[半导体](https://baike.baidu.com/item/%E5%8D%8A%E5%AF%BC%E4%BD%93/385669)工艺方法制造出无源和有源元器件，并连接起来构成应用于微波（甚至[毫米波](https://baike.baidu.com/item/%E6%AF%AB%E7%B1%B3%E6%B3%A2/2458350)）频段的功能电路。

它包括多种功能：[低噪声放大器](https://baike.baidu.com/item/%E4%BD%8E%E5%99%AA%E5%A3%B0%E6%94%BE%E5%A4%A7%E5%99%A8/3400107)（LNA）、[功率放大器](https://baike.baidu.com/item/%E5%8A%9F%E7%8E%87%E6%94%BE%E5%A4%A7%E5%99%A8/1267443)、[混频器](https://baike.baidu.com/item/%E6%B7%B7%E9%A2%91%E5%99%A8/4403010)、上变频器、[检波器](https://baike.baidu.com/item/%E6%A3%80%E6%B3%A2%E5%99%A8/7567676)、[调制器](https://baike.baidu.com/item/%E8%B0%83%E5%88%B6%E5%99%A8/3314711)、[压控振荡器](https://baike.baidu.com/item/%E5%8E%8B%E6%8E%A7%E6%8C%AF%E8%8D%A1%E5%99%A8/11044622)（[VCO](https://baike.baidu.com/item/VCO/7423201)）、移相器、开关、MMIC 收发前端，甚至整个发射 / 接收（T/R）组件（收发系统）。

用白话说，就是模拟电路的 SOC（system on chip）芯片技术，把各种模拟电路集成在单一的芯片内部的技术。

由于 MMIC 的[衬底](https://baike.baidu.com/item/%E8%A1%AC%E5%BA%95/5853489)材料（如 [GaAs](https://baike.baidu.com/item/GaAs/795426)、InP）的[电子迁移率](https://baike.baidu.com/item/%E7%94%B5%E5%AD%90%E8%BF%81%E7%A7%BB%E7%8E%87/246443)较高、[禁带宽度](https://baike.baidu.com/item/%E7%A6%81%E5%B8%A6%E5%AE%BD%E5%BA%A6/9150459)宽、工作温度范围大、微波传输性能好，所以 MMIC 具有电路损耗小、噪声低、频带宽、[动态范围](https://baike.baidu.com/item/%E5%8A%A8%E6%80%81%E8%8C%83%E5%9B%B4/6327032)大、功率大、附加效率高、抗电磁辐射能力强等特点。

*   **MiMo  （**Multiple In Multiple Out）：多输入多输出

为极大地提高信道容量，在发送端和接收端都使用多根天线，在收发之间构成多个信道的[天线系统](https://baike.baidu.com/item/%E5%A4%A9%E7%BA%BF%E7%B3%BB%E7%BB%9F/5931997).

*   **MAA  （**massive MiMo Adaptive Antenna）：大规模 MIMO 自适应天线

自适应天线（adaptive antenna）能够自动将天线最大辐射方向对准对所需电台而将[天线方向图](https://baike.baidu.com/item/%E5%A4%A9%E7%BA%BF%E6%96%B9%E5%90%91%E5%9B%BE/6116246)的天线，这种天线通过控制发送信号的相位，来控制天线信号的发送方向，是大规模天线波束赋型中一个关键技术。

通常 MAA 的天线数至少有 16 个天线。

*   **AA** （Activate Antenna）：有源天线

有源天线内部集成了接收天线模块、低噪声放大模块、电源供给模块

*   **AE** (Antenna element)：天线单元

![](https://img-blog.csdnimg.cn/20210808192830814.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hpV2FuZ1dlbkJpbmc=,size_16,color_FFFFFF,t_70)

 上图是 4 * 3 * 8 * 2 = 192 天线阵列，其中每个天线称为天线单元。

*   **AISG** (Antenna Interface Standards Group): 智能天线接口标准组

控制智能电调天线接口标准

*   **IBW** (_Instantaneous Bandwidth):_ 瞬时带宽

通信系统在瞬间（或者说，在某一时刻，在不动态改变任何参数的情况下）可发射和接受信号的最大带宽。这个参数主要由射频前端的收发**滤波器**或者**收发双工器**决定。

*   **OBW** (_Operation Bandwidth):_ 工作带宽

是指一个通信设备或系统允许的最大**可工作**的带宽。OBW 通常小于 OBW, OBW=IBW 时，才能发挥硬件设备的最大效能。

OBW 受中频芯片、算法、功放、散热等多方面的软硬件能力的限制。

（1）PA 是线性器件，如果频带太宽，其线性特性很难保证，一旦超过线性范围，PA 性能下降，峰均比严重；

（2）RRU 中的数字采样一般有 ADC 完成，高精度的 ADC 十分昂贵，在低成本的前提下，OBW 很难作的很宽。

OBW 和 IBW 是 5G NR 的 AAU 的一个重要指标。据说 N 在 5G 招标中的失利就与这个 OBW 有关。中国电联提出了苛刻的 200MHz 的 OBW。

![](https://img-blog.csdnimg.cn/20210808171803210.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hpV2FuZ1dlbkJpbmc=,size_16,color_FFFFFF,t_70)

*   **OBW** (_Occupied Bandwidth):_ 占用带宽

这是一个不常用的概念，指所有小区实际占用的带宽。

*   **PSU  （**Power Supply Unit）：电源供应单元 / 模块。

电源设备（PSU）是一种为内部计算机组件提供直流（DC）电压的电源转换器。其中一些设备是专门为 110 或 230 伏交流电（AC）而设计的，而另一些则可以在两者之间切换，甚至可以接受该范围内的任何电压。它们提供的直流电压水平也可能不同，通常在 3 到 12 伏直流电之间。

台式机和笔记本电脑都使用电源装置，虽然它们在设计上有些不同。一个桌面电源设备通常有几束电线，用来直接连接到各种内部组件上，而笔记本电脑的 "电源砖" 是通常使用同轴电源连接器的外部设备。

*   **PCB   （**Printed Circuit Board）：印制电路板 / 印刷线路板

![](https://img-blog.csdnimg.cn/img_convert/b6571ddb6f3a826f26c30fe1440c7492.png)

 是[电子元器件](https://baike.baidu.com/item/%E7%94%B5%E5%AD%90%E5%85%83%E5%99%A8%E4%BB%B6/9042493)的支撑体，是电子元器件电气相互连接的[载体](https://baike.baidu.com/item/%E8%BD%BD%E4%BD%93/13130262)。由于它是采用电子印刷术制作的，故被称为 “印刷” [电路板](https://baike.baidu.com/item/%E7%94%B5%E8%B7%AF%E6%9D%BF/10106124).

*   **PWB  （**Printed Wiring Board）：印刷线路板

与 PCB 同义。

PWB 是英国人早期的叫法，因为当时线路板上只有线路图，而没有印制元件等，所以属于较为原始的板子；由于传统好多英国人和部分香港人还称线路板为 PWB；

*   **FFC （**Flexible Printed Circuit board）：柔性印刷电路板

![](https://img-blog.csdnimg.cn/img_convert/7a77f2434633be68f7a4275319d02eab.png)

 FFC(Flexible Flat Cable)柔性扁平电缆是一种用 PET 绝缘材料和极薄的镀锡扁平铜线，通过高科技自动化设备生产线压合而成的新型数据线缆，具有柔软、随意弯曲折叠、厚度薄、体积小、连接简单、拆卸方便、易解决电磁屏蔽 (EMI) 等优点。

通俗讲就是用软性材料（可以折叠、弯曲的材料）做成的 PCB）连接器用于 LCD 显示屏到驱动电路 (PCB) 的连接。

 作者主页 ([文火冰糖的硅基工坊](https://blog.csdn.net/HiWangWenBing))：[https://blog.csdn.net/HiWangWenBing](https://blog.csdn.net/HiWangWenBing)

本文网址：[https://blog.csdn.net/HiWangWenBing/article/details/118880746](https://blog.csdn.net/HiWangWenBing/article/details/118880746)