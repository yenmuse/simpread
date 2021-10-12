> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/145936888)

专栏正篇：
-----

**是作者对协议 specification 的阅读与理解。**

**以纯数字编号（参照粗粮手机的旗舰命名）。**

[ljgibbs：深入 AXI4 总线（O）专栏目录与资料集合](https://zhuanlan.zhihu.com/p/145936888) （本篇）

[ljgibbs：深入 AXI4 总线（一）握手机制](https://zhuanlan.zhihu.com/p/44766356)

[ljgibbs：深入 AXI4 总线（二）架构](https://zhuanlan.zhihu.com/p/45122977)

[ljgibbs：深入 AXI4 总线（三）传输事务结构](https://zhuanlan.zhihu.com/p/46538028)

[ljgibbs：深入 AXI4 总线（四）传输事务属性（draft）](https://zhuanlan.zhihu.com/p/148813963) （该篇目前为草稿状态）

[ljgibbs：深入 AXI4 总线（五 A）单机多传输事务场景](https://zhuanlan.zhihu.com/p/149071260) （A、B 分别表示上下篇）

[ljgibbs：深入 AXI4 总线（五 B）多主机传输事务场景](https://zhuanlan.zhihu.com/p/149112312)

实战篇：
----

**结合专栏正篇的协议阅读，搭建实验平台，进行实战的系列文章**

**以 E + 数字编号**

[ljgibbs：深入 AXI4 总线（E0）实战：平台搭建](https://zhuanlan.zhihu.com/p/147673445)

[ljgibbs：深入 AXI4 总线（E1）实战：Hello AXI handshake](https://zhuanlan.zhihu.com/p/147807965)

以下两篇尚未迁移至新的实战平台，会在近期更新或转入旧版存档

[ljgibbs：深入 AXI4 总线（E2）实战：RAM 读取](https://zhuanlan.zhihu.com/p/46672113)

[ljgibbs：深入 AXI4 总线（E3）实战：制作一个 AXI 接口 IP](https://zhuanlan.zhihu.com/p/77528158)

附录：
---

**以 A + 数字编号 表示术语表等内容，作为对正篇的补充**

[ljgibbs：深入 AXI4 总线（A）附录 · 术语表](https://zhuanlan.zhihu.com/p/146900371)  

**以 B + 数字编号 表示其他内容**

[ljgibbs：深入 AXI4 总线（B）附录 · 一次从〇开始的 AXI 总线入门研讨](https://zhuanlan.zhihu.com/p/87030667)

专栏旧版存档：
-------

**已被或者等待被更新、替换、移除的正篇移至此处，用于保留讨论的内容与专栏演进的历史。这些文章相较于正篇，可能包含更多错误，但仍有一定价值，读者可以按照兴趣阅读。**

**以 C + 数字编号**

[ljgibbs：深入 AXI4 总线（C0）旧版存档：突发传输机制](https://zhuanlan.zhihu.com/p/147674451)

[ljgibbs：深入 AXI4 总线（C1）旧版存档：AXI4 的兄弟协议](https://zhuanlan.zhihu.com/p/49834243)

专栏的历史
-----

当我第一次接触 AXI 总线时，我以为只是一种应用于 FPGA IP 之间的总线。

后来才发现 AXI 以及其所属的 AMBA 总线的使用是如此广泛。我们身边大多数的电子产品中都有 AMBA 协议的存在。

当我对 AXI 有了一些了解后，我就撰写了这个专栏最初的一些文章，虽然无论是当时的我还是现在的我都只是略懂，

并在实验室里组织了一次小型的研讨活动，

曾经努力推动实验室项目使用 AXI 接口，取代 FPGA IP 的 native 总线，比如 Xilinx MIG 的 app 总线。（然后我失败了，MIG 的 AXI 接口在部分情况下效率要低于 app 总线，后续想深究一下这个事儿）

我在 AXI 文章的评论区收到了很多感谢，非常令我感到鼓舞。也进行了很多探讨，让我考虑了一些之前未曾想到的问题。也有很多很多读者对我 AXI 的 ppt 感兴趣。

![](https://pic3.zhimg.com/v2-57dab1af2519d0e5300f992a3b0ad5a6_r.jpg)

而且，后来当我的同学去实习或者入职时，过去带给他们的入门知识确实地帮助到他们，感觉 e 可赛艇！

![](https://pic3.zhimg.com/v2-2e27a86254c9baa5afbe2f620e6c0b62_r.jpg)

所以当这是插播的一个广告吧！接下来整理一些 AXI 的资料和公开课。

首先是 AXI 的手册，在 ARM AMBA 的网站注册账号后可以下载，我觉得网速还行

首先是 AXI PPT 的 PDF 版本，可以前往下方链接，在极术社区文章页面中下载，同时还有一份官方手册。注册账号并完成新手任务之后，就可以下载了~

带有讲稿的 PPT，可以通过私信向我索取，我一般都会给啦~ 后续会把讲稿的内容直接更新到文章里。

接下来是推荐一些网络资料的时间：

1.  移知的 AXI 免费公开课，一个小时干活满满的视频，我很多 AXI 知识，尤其是与访存相关的，都来自于这个视频。额，事实上我觉得那位老师讲的比我的 PPT 好

2. 极术社区上收录的 AMBA 协议介绍公开课回放与 PPT，来自 ARM China 的资深工程师。公开课中还讨论了与缓存一致性相关的协议 ACE 等，有兴趣的读者可以了解下。

3. 来自 xilinx 官方论坛的 AXI 资料，并向大家推荐 Xilinx AXI VIP 作为学习 AXI 总线的工具

Xilinx 中文社区微信号做了中文翻译，这是其中一篇

笔者感觉 AXI VIP 及其 Example 工程看上去非常令人 e 可赛艇，估计后续会采用 AXI VIP 来作为实战讲解工具。

4.CSDN 上的优秀文章，博主在这两篇文章中讨论了 4k 地址边界以及其他一些 AXI 中比较复杂的问题。

5. 一份来自 Xilinx 的 introduction PPT

不过我还是推荐本专栏中的 introduction PPT，^_^

6.AXI4 相比 AXI3 协议的变化

7. 介绍了 AMBA 体系与协议（英文的，所以还没细看，看上去不错。如果读者觉得不错的话，欢迎翻译~）

[AXI Interconnects Tutorial: Multiple AXI Masters and Slaves in Digital Logic - Technical Articles](https://link.zhihu.com/?target=https%3A//www.allaboutcircuits.com/technical-articles/what-are-axi-interconnects-tutorial-master-slave-digital-logic/)

8. 一篇详细讨论 AXI Interconnect 构成的文章

9. 来自 ARM 官方的 AXI 科普视频，由极术社区翻译，笔者个人觉得，简短但都讲在点子上，推荐可以花几分钟看看这个以及该系列的其他视频：

**AXI 实现相关**

1. 一个 AXI4 /AXI4-lite SV 实现（待品鉴，欢迎有兴趣和时间的读者品鉴）

AXI4 and AXI4-Lite synthesizable modules and verification infrastructure

2.AXI VIP

TVIP-AXI is an UVM package of AMBA AXI4 VIP（待品鉴，欢迎有兴趣和时间的读者品鉴）

3. Xilinx AXI interconnect IP 的文档

资料将持续整合和收集，深入 AXI 系列文章也将继续修正和更新，谢谢读者们的资瓷。