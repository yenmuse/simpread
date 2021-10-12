> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/404224698)

译者序
---

最近不是 RISC-V 很火嘛，我就随一波大流，学学 CPU 的设计。虽然已经会了 SpinalHDL 这门高级点的开发语言，但看起 VexRiscv 这个开源软核的源码还是有种混沌邪恶的感觉。CPU 说白了就取指、译码、执行这些东西分别实现，然后拼成一个流水线。我按照这种思路去看 VexRiscv 源码的时候，看到了流水线，但完全没发现数据是怎么流动的，就像是玄学一样。然后我看到了这篇博客才明白：原来 VexRiscv 使用了新的思路来设计 CPU，这里的新是指代码写法上的新，而不是微架构上的新，这种新思路能带来开发效率的极大提升，仅此一份的新。读完这篇博客后，我直呼内行！遂翻译过来为 SpinalHDL 的推广事业添砖加瓦。原文：

阅读建议：听说过 SpinalHDL 就行；有 RTL 开发经验；不用写过 CPU 但至少对 CPU 的各个名词要了解。

第一次翻译东西，凑合着看吧。原文读起来更流畅建议看，不知道为啥我翻译过来就感觉有点啰嗦。文章需要点耐心结合代码慢慢看，很值得一读。

**目录**
------

*   简介
*   设计 CPU 的传统方法
*   SpinalHDL 简单回顾
*   VexRiscv 流水线插件架构
*   桶移位器与乘法器插件
*   一些好家伙
*   缺点
*   结论

简介
--

在之前的[博客](https://link.zhihu.com/?target=https%3A//tomverbeure.github.io/rtl/2018/08/12/SpinalHDL.html)，我简单地提到了一下 [VexRiscv](https://link.zhihu.com/?target=https%3A//github.com/SpinalHDL/VexRiscv)，一个完全用 SpinalHDL 实现的 RISC-V CPU，它的作者 Charles Papon 同时也是 SpinalHDL 的创造者。

在之前，我为了比较不同的 RISC-V CPU，创建了 [rv32soc](https://link.zhihu.com/?target=https%3A//github.com/tomverbeure/rv32soc) 项目。同时我用 VexRiscv 作为一个基准来和我自己的 RISC-V CPU（[MR1](https://link.zhihu.com/?target=https%3A//tomverbeure.github.io/risc-v/2018/11/19/A-Bug-Free-RISC-V-Core-without-Simulation.html)）作比较。MR1 在任何可能的指标上都比不过 VexRiscv。

在这一过程中，我花了一些时间试图搞明白 CPU 的内部结构及其设计思路。但令人沮丧的是我竟然没看懂。VexRiscv 除了性能和功能过剩以外，它的实现方式还极大地吊打了不灵活的传统 RTL 语言（Verilog、SystemVerilog、VHDL）。

VexRiscv 的代码证明了我们可以写出和优化过的 Verilog 一样高效、配置极其便利的 RTL。所以 VexRiscv 获得 RISC-V 峰会软核大赛一等奖也是实至名归。

话说：” 意志薄弱 “的人是无法理解 VexRiscv 的代码库的。它利用了所有传统面对对象语言（Scala）的特性创造了这个魔法般的东西。由于它并没有遵循 CPU 设计的标准实践流程，使得我花了一段时间才真正理解了它。

这篇文章的目的是解释为什么 VexRiscv 的设计如此重要和新颖。

设计 CPU 的传统方法
------------

为了更好地理解 VexRiscv 设计的创新之处，这里用和它类型相同的传统 CPU 和它作比较，即：标准流水线、顺序执行、单发射等。能找到许多这样的开源 CPU。

既然你看到了这个文章，说明你大概率了解传统 RISC CPU 的标准部分：一条指令是分为多个流水线阶段（stage）进行处理的。每个 stage 只负责整个操作的一个部分。stage 越多，每个 stage 的工作量就越少，进而能够减少每个 stage 的逻辑深度，最终达到提高时钟频率的效果。

一般的 RISC CPU 有 5 个 stage，不过你也能见到 2 到 11 个以上 stage 的 CPU。

毫无疑问，这样的 CPU 设计需要匹配这种流水线架构：如果 CPU 有`取指`stage，那么 RTL 就一定有一个对应的`取指`Verilog 模块。

pulp platform 的开源 RISC-V CPU [RI5CY](https://link.zhihu.com/?target=https%3A//github.com/pulp-platform/riscv) 是上述传统方法的典型。我们看看它的[源码目录](https://link.zhihu.com/?target=https%3A//github.com/pulp-platform/riscv/tree/master/rtl)，可以找到一堆我们意料之中的功能模块：`riscv_if_stage.sv`，`riscv_id.sv`，`riscv_ex_stage.sv`等等。

![](https://pic4.zhimg.com/v2-0c1eb84d31a8ed52f713759b289857a3_r.jpg)

所有这些传统的功能模块在`riscv_core.sv`中紧密地缠绕在一起。

这好吗？这不好。

虽然这并不算一个特别差的实现方式，大家都是这样做的，只有一个小问题：所有的模块是按照流水线阶段划分的，而不是按功能。

就拿一个乘法器举例。一个`MUL`指令在 [decoder](https://link.zhihu.com/?target=https%3A//github.com/pulp-platform/riscv/blob/0a5bb35a4fd63123838978c54d65c8aa5a446756/rtl/riscv_decoder.sv%23L88) 模块（`id`stage）中进行译码。它的结果是 `[mul_operator_ex_o](https://link.zhihu.com/?target=https%3A//github.com/pulp-platform/riscv/blob/0a5bb35a4fd63123838978c54d65c8aa5a446756/rtl/riscv_id_stage.sv%23L128)`和其它一些诸如操作数和控制信号之类的信号。

在`risc_core.sv`中，这些信号从`id` stage 中出来后，改名成 `[mult_operator_i](https://link.zhihu.com/?target=https%3A//github.com/pulp-platform/riscv/blob/0a5bb35a4fd63123838978c54d65c8aa5a446756/rtl/riscv_ex_stage.sv%23L67)`送进`ex` stage。在`ex` stage 中，信号随后被路由至 `[riscv_mult](https://link.zhihu.com/?target=https%3A//github.com/pulp-platform/riscv/blob/0a5bb35a4fd63123838978c54d65c8aa5a446756/rtl/riscv_ex_stage.sv%23L290-L320)`，在这里才真正发生了乘法的计算，最后得到`result_o` 这一信号作为输出。

在`riscv_ex.sv` 的最后，连接到 `[mult_result](https://link.zhihu.com/?target=https%3A//github.com/pulp-platform/riscv/blob/0a5bb35a4fd63123838978c54d65c8aa5a446756/rtl/riscv_ex_stage.sv%23L315)` 的是一个[多路复用器](https://link.zhihu.com/?target=https%3A//github.com/pulp-platform/riscv/blob/0a5bb35a4fd63123838978c54d65c8aa5a446756/rtl/riscv_ex_stage.sv%23L210-L211)，它用于选择哪个 ALU 的结果被送入 regfile_alu_wdata_fw_o 这个信号。这个信号再被传回 id 模块用于把结果[写回寄存器](https://link.zhihu.com/?target=https%3A//github.com/pulp-platform/riscv/blob/0a5bb35a4fd63123838978c54d65c8aa5a446756/rtl/riscv_id_stage.sv%23L966)和[转发（forward）逻辑](https://link.zhihu.com/?target=https%3A//github.com/pulp-platform/riscv/blob/0a5bb35a4fd63123838978c54d65c8aa5a446756/rtl/riscv_id_stage.sv%23L636)。

但是可以看到，我们为了添加这一个小功能，把这个乘法操作分散到了各个不同的文件中。

> 本例如下图，Decode 和 Execute 所属的文件需改动

![](https://pic3.zhimg.com/v2-a11e9e481734cac7f30b954717b5eb7a_r.jpg)

FPGA 通常有硬件乘法器单元可以支持很高的时钟频率。但是这种频率只有在乘法器的输入和输出都被寄存器包围的情况下才能达到：输入输出各有一个寄存器。

如果我们既想维持原有的每时钟一个指令的吞吐量，又想把乘法执行分为两个时钟周期，我们需要做如下的改动：

> 不明白的看这里：[多功能部件及多拍操作引起的问题](https://link.zhihu.com/?target=https%3A//blog.csdn.net/weixin_44849403/article/details/104034348)

![](https://pic2.zhimg.com/v2-220b31c9cb4eed05ce5760030fdb0a59_r.jpg)

此外，这些乘法单元通常是 18x18 bits 规格的。如果我们想实现 32 位的 CPU 所需要的 32x32 乘法器，我们需要用[多个 18x18 的乘法器进行拼接](https://link.zhihu.com/?target=https%3A//tomverbeure.github.io/rtl/2018/08/11/Multipliers.html)。

达到期望频率通常意味着需要更多的寄存器和 stage，就像下图一样：

![](https://pic3.zhimg.com/v2-ed17eaf66ccb1c58d0bb6bff1947c49a_r.jpg)

在上面的 5 阶流水线中，我们滥用了 MEMORY 和 WRITEBACK 这两个阶段来截断时序路径从而提高 CPU 频率。我们同样地需要修改 FETCH 和 DECODE 阶段的相关检测逻辑。

这种事情我们做的越多，我们需要动的文件就越多，需要添加的端口就越多。

如果你脑子里已经有很清晰、很明确的架构，这些改动不是什么难事。但如果你想让一些东西高度可配置的话，这种设计方法很快就变成了维护上的噩梦：倘若 CPU 的时钟频率要求不高，你尚可以把所有逻辑都放到 EXECUTE 阶段，顺便还省了资源。但要做快速的 CPU，只能使用这种费资源的方法。

除此之外，存在未曾设想的道路吗？

SpinalHDL 简单回顾
--------------

我知道你懒得看我之前写的 [SpinalHDL 博客](https://link.zhihu.com/?target=https%3A//tomverbeure.github.io/rtl/2018/08/12/SpinalHDL.html)，所以我就只重复一下最重要的一些点。

**首先它是一个 Scala 库，有一些能够相互连接的硬件原语。**

拿传统语言 Verilog 来说，它工作流程是这样的：

```
Verilog描述的硬件 -> Verilog解释器 -> 硬件原语

```

它描述硬件的能力完全由 Verilog 的语法特性决定。如果该语言不支持确定的选项，你必须搞一些奇技淫巧来实现一些东西。

就拿模块端口的简单例子来说：你的 CPU 有一个可选的 JTAG 调试接口。你在某些配置的情况下需要它，而另一些情况不需要。

Verilog 的语法并不支持可选端口：它的`if generate` 语法不能作用在端口上。所以你要么在不需要的时候直接忽略 JTAG 端口不连它，要么用下面这种丑陋的形式实现它：

```
module cpu(
    ...
`ifdef JTAG
    input  jtag_tck,
    input  jtag_tms,
    input  jtag_tdi,
    output jtag_tdo,
`endif
    ...


```

在 SpinalHDL 中，流程是这样的：

```
Scala和硬件原语混合编程 -> 执行程序 -> 生成硬件原语

```

这些硬件原语连线十分灵活，你可以发挥想象用各种方法实现。

如果你在某个配置下不需要用到 JTAG 端口，只要不调用创建 JTAG 端口的 Scala 函数即可：

可以看看我的 MR1 RISC-V 核，就是[这样写](https://link.zhihu.com/?target=https%3A//github.com/tomverbeure/mr1/blob/352927f0dc217bf421cbe4fddf9956afd9277ad6/src/main/scala/mr1/Decode.scala%23L18)的：的：

```
val rvfi = if (config.hasFormal) RVFI(config) else null

```

为了正确地检验 CPU 核，需要一个包含一捆 RVFI 信号的额外端口。但是这个端口在综合核仿真的时候用不上。

`if (config.hasFormal) ... else` 是标准的 Scala 代码。`RVFI(config)` 是 SpinalHDL 的 Bundle 类，用于描述一堆信号，类似于 SystemVerilog 的接口。

如果觉得这样组织硬件很别扭不方便，那一定是你想象力还不够。毕竟 Verilog 的限制还更多。VexRiscv 核把这种写法运用到了极致。

VexRiscv 流水线插件架构
----------------

VexRiscv 围绕流水线的各阶段来构建，每个功能对象可以以插件的形式随意添加。

我们从 [generic Pipeline](https://link.zhihu.com/?target=https%3A//github.com/SpinalHDL/VexRiscv/blob/6334f430fe1bed302733c6ea6c44f8b514f3e2c6/src/main/scala/vexriscv/Pipeline.scala%23L12-L15) 开始，它有`stages`和`plugins`属性。

```
trait Pipeline {
  type T <: Pipeline
  val plugins = ArrayBuffer[Plugin[T]]()
  var stages = ArrayBuffer[Stage]()
  ...

```

这段代码和 CPU 没有任何关系。

一个 Pipeline 对象有很多个 stage。叫做`Stageable` 的元素可以被一个 stage 传递到下一个 stage。插件会被整合进整个流水线，它们可以给每个 stage 添加逻辑，可以使用其中一个`Stageable` 作为流水线上某个特定 stage 的输入，可以插入新的`Stageable` 到流水线的下一个 stage。

Pipeline 对象会自动负责管理这些 stageable 。如果一个插件在 EXECUTE 阶段需要 stagealbe OP_A，而 OP_A 是 DECODE 阶段由其它插件插入流水线的，随后 Pipline 对象会确保 OP_A 沿着流水线传递到 DECODE 阶段。

如果插件在 EXECUTE 阶段产生了一个 stageable RESULT，随后需要将它导入 WRITEBACK 阶段，流水线阶段会再一次确保它能被送过去，无论在 WRITEBACK 和 EXECUTE 之间有多少中间 stage。

在 VexRiscv 中，流水线阶段的描述如下：

```
class VexRiscv(val config : VexRiscvConfig) extends Component with Pipeline{
  type  T = VexRiscv
  import config._

  //Define stages
  def newStage(): Stage = { val s = new Stage; stages += s; s }
  val decode    = newStage()
  val execute   = newStage()
  val memory    = ifGen(config.withMemoryStage)    (newStage())
  val writeBack = ifGen(config.withWriteBackStage) (newStage())

...

```

可以看出来，VexRiscv 是 Component（SpinalHDL 的一个原语，等价于一个 Verilog 的 module）的一个带有 Pipeline 字段的子类。

CPU 中一定有译码和执行这两个 stage，而访存和写回 stage 是可选的，主要看你期望的配置。stage 的顺序由`newStage()`的调用顺序决定。

一旦定义好了 CPU 的各个 stage，就是时候通过插件来向流水线添加逻辑了！

关于 CPU 的各个方面的文件都统一地放在了一个文件夹下！

![](https://pic3.zhimg.com/v2-574bea9b23090df719f87dd07478056a_r.jpg)

接下来让我们凑近一点看。。。

桶移位器与乘法器插件
----------

VexRiscv 有多种选项来实现 RISV-V 的左移和右移指令。它们都在 `[ShiftPlugins.scala](https://link.zhihu.com/?target=https%3A//github.com/SpinalHDL/VexRiscv/blob/master/src/main/scala/vexriscv/plugin/ShiftPlugins.scala)`中实现。

`[LightShifterPlugin](https://link.zhihu.com/?target=https%3A//github.com/SpinalHDL/VexRiscv/blob/6334f430fe1bed302733c6ea6c44f8b514f3e2c6/src/main/scala/vexriscv/plugin/ShiftPlugins.scala%23L93-L181)` 实现了一个交互式的移位器，每次能左移或右移 1 bit，主要用于低性能少资源的场景。

`[FullBarrelShifterPlugin](https://link.zhihu.com/?target=https%3A//github.com/SpinalHDL/VexRiscv/blob/6334f430fe1bed302733c6ea6c44f8b514f3e2c6/src/main/scala/vexriscv/plugin/ShiftPlugins.scala%23L9-L82)` 实现了一个典型的桶移位器，分为三个操作：

*   如果是左移指令，把输入反转
*   根据变量的值确定移位的步长进行右移
*   如果是左移指令，把输出反转

```
class FullBarrelShifterPlugin(earlyInjection : Boolean = false) extends Plugin[VexRiscv]{
...

```

在插件的顶层可以看到`earlyInjection` 这个配置选项：它决定了是否把最后一次反转操作和前两个操作放在同一个流水线 stage，否则就把它放在下一个 stage。把它放在下一个 stage 会增加面积（你需要通过流水线传输中间值，这要求额外的触发器），但这样做减少了逻辑深度。由于桶移位器不能很好的映射到 FPGA 的 LUT 上，这样做能很好地减少时序路径。

来看一下它是怎么实现的，让我们从 [initial reverse and shift](https://link.zhihu.com/?target=https%3A//github.com/SpinalHDL/VexRiscv/blob/6334f430fe1bed302733c6ea6c44f8b514f3e2c6/src/main/scala/vexriscv/plugin/ShiftPlugins.scala%23L62-L67) 开始看：

```
execute plug new Area{
   import execute._
   val amplitude  = input(SRC2)(4 downto 0).asUInt
   val reversed   = Mux(input(SHIFT_CTRL) === ShiftCtrlEnum.SLL, Reverse(input(SRC1)), input(SRC1))
   insert(SHIFT_RIGHT) := (Cat(input(SHIFT_CTRL) === ShiftCtrlEnum.SRA & reversed.msb, reversed).asSInt >> amplitude)(31 downto 0).asBits
}

```

第一步，这个代码意思是我们要把这一堆逻辑（Area 里面的一堆）放在 EXECUTE 阶段。

移位的幅度（amplitude）通过`input(SRC)`从流水线取得 ，移位的方向由`input(SHIFT_CTRL)`确定。 与此同时，结果以`insert(SHIFT_RIGHT)` 的方式插回流水线。

在[第二部分](https://link.zhihu.com/?target=https%3A//github.com/SpinalHDL/VexRiscv/blob/6334f430fe1bed302733c6ea6c44f8b514f3e2c6/src/main/scala/vexriscv/plugin/ShiftPlugins.scala%23L69-L79)，对`SHIFT_RIGHT` 的结果进行反转操作如下：

```
val injectionStage = if(earlyInjection) execute else memory
injectionStage plug new Area{
  import injectionStage._
  switch(input(SHIFT_CTRL)){
    is(ShiftCtrlEnum.SLL){
      output(REGFILE_WRITE_DATA) := Reverse(input(SHIFT_RIGHT))
    }
    is(ShiftCtrlEnum.SRL,ShiftCtrlEnum.SRA){
      output(REGFILE_WRITE_DATA) := input(SHIFT_RIGHT)
    }
  }
}

```

第一行是关键：基于配置参数`earlyInjection`，if 语句决定接下来的一团逻辑是放在 EXECUTE 阶段还是 MEMORY 阶段！

梅开二度，`input(SHIFT_CTRL)` 被从流水线中取出，决定接下来要不要反转。`input(SHIFT_RIGHT)` 用于生成即将被插入到流水线的`REGFILE_WRITE_DATA` 。

这边有个很优雅的部分：如果`earlyInjection` 为真，`input(SHIFT_RIGHT)` 将会被翻译成纯组合逻辑的赋值操作，与它之前的逻辑组合在一起。然而如果`earlyInjection` 为假，Pipeline 对象会根据需求正确地插入个带寄存器的 stage。

乘法器在 [MulPlugin.scala](https://link.zhihu.com/?target=https%3A//github.com/SpinalHDL/VexRiscv/blob/master/src/main/scala/vexriscv/plugin/MulPlugin.scala) 中也是类似的实现：

[4 - 段乘法器](https://link.zhihu.com/?target=https%3A//github.com/SpinalHDL/VexRiscv/blob/6334f430fe1bed302733c6ea6c44f8b514f3e2c6/src/main/scala/vexriscv/plugin/MulPlugin.scala%23L74-L77)在 EXECUTE 阶段实现，在 EXECUTE 阶段实现。这些中间结果在 MEMORY 阶段相加。最后的结果在 WRITEBACK 阶段创建。

概念上的代码组织看起来像这样：

![](https://pic3.zhimg.com/v2-3a7967a373ede0151a029f7f76e72372_r.jpg)

所有的乘法器代码都在逻辑上组织在一个文件中。这超方便用另一个不同的实现来替换当前实现，或者移除它。

一些好家伙
-----

你可能注意到，不管是移位插件还是乘法插件都以`output(REGFILE_WRITE_DATA)` 作为结尾。没毛病，它会以一种简单的方式被自动处理。默认情况下，一个流水线 stage 只会简单地传递一个来自前一个 stage 的 stageable。如果插件认为有必要，它的值会被重写。如果很多的插件在单个 stage 里对同一个 stageable 重写也是没问题的，因为在任一个 stage 里仅有一条指令是活跃的，自然而然地，插件在活跃的时候就只会对一个 stageable 重写。

这里的译码器，DecodeSimplePlugin，就当是个魔法好了。它包含了逻辑优化器以生成最优的译码逻辑。

`riscv-formal` 支持对生成的 CPU 进行形式验证，但我还没试过。

你可以学学我，多花点心思去探索 CPU 的各个方面。

缺点
--

我认为主要有两个缺点：

对于传统的 RTL 设计者，学习曲线过于陡峭。不仅因为 Scala 本身内容丰富，而且 SpinalHDL/VexRiscv 又极大运用了这种丰富性_（译者注：其实还好啦，谁还没学过点软件编程语言，个人感觉被 Verilog 恶心过的同时又对函数式语言有了解的话，SpinalHDL 是很快上手的）_。我的学习方法是循序渐进地边学边用：首先把它当成 Verilog 的替换工具来取缔一些冗长啰嗦的代码，然后再逐渐引入一些复杂的概念。

但使用 VexRiscv 最大的问题是它产生的 Verilog 只有一个展平的文件，使得它很难 debug 和理解。_（译者注：手写的 Verilog 你过段时间也很难理解。。。据说 SpinalHDL 的 debug 比 Chisel 好多了）_

结论
--

无论你是否将要使用 VexRiscv 和 SpinalHDL，我认为它的实现思路和完全和传统的 RTL 设计泾渭分明。我认为过一遍它的代码搞明白它是怎么工作的非常有意思和身心愉悦。

VexRiscv 不仅仅只是设计思路上的惊艳，它同时还有非常高的性能和很低的资源使用，还有无数的配置选项。高配的 1.4 Dhrystone/MH 核心和低配的 0.5 Dhrystone/MHz 核心仅有几行的配置文件上的区别。

就算你不想接受 SpinalHDL，你依然可以单纯地用它生成 Verilog 代码添加进你的工程。我在开始学习 SpinalHDL 之前就是这么干的。

尽管如此，自从我切换到 SpianlHDL 来开发我的项目后，我的取舍很明了：除非有更好的理由让我用我自己的 MR1 核，否则我肯定用 VexRiscv 啊。