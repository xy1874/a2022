# 实验原理

## 1. Pin工具简介

&emsp;&emsp;为了设计出高性能的处理器架构，架构师必须对典型程序的特性具有充分的理解和认识。一般可以通过模拟或程序插桩的方法来评估处理器和程序的特性。模拟的方法需要在CPU软件模型上运行测试程序。若想要充分了解测试程序的特性，必须设计足够精细的CPU模型；而模型越精细，在其上的测试程序运行效率就越低。相比之下，程序插桩获得的程序信息可能比不上模拟的方法，但程序插桩不仅更容易实现，还可以让测试程序直接运行在物理CPU上，因此效率更高。事实上，几乎所有的程序员和架构师都常常使用最简单的程序插桩（如`printf`语句）。然而手动插桩终究效率较低，为此，Intel开发了一套非商用免费（但不开源）的程序插桩分析工具包Pin。

&emsp;&emsp;目前，Pin被广泛应用在了体系结构研究、软件开发和安全分析等学术界和工业界的相关领域。Pin最大的优势是能对任何可运行在X86架构CPU上的程序进行插桩分析，并且不需将被分析程序进行重新编译或链接；进一步地，Pin还支持多线程应用程序和运行时进程。利用Pin工具包，插桩工具的开发人员可以在目标可执行文件的任意位置插入任意由C/C++编写的代码。插桩工具编写、编译完成后，可利用Pin工具启动目标可执行文件。此时，插桩工具中的相应代码会被Pin工具包动态地插入正在运行的目标可执行文件当中。

&emsp;&emsp;Intel为Pin提供了丰富的API。这些API为插桩工具的开发人员屏蔽了指令集的特异性，并能够将寄存器内容等上下文信息以参数的形式传递给插桩工具。此外，Pin将自动保存并恢复被插桩工具修改过的寄存器，以便目标程序能够正常且正确地运行。

&emsp;&emsp;Pin支持Linux、Windows和macOS的操作系统，并支持在32位和64位的X86架构CPU上运行。Intel官网提供了3种OS的不同版本的[Pin工具包下载链接](https://software.intel.com/content/www/us/en/develop/articles/pin-a-binary-instrumentation-tool-downloads.html)。本实验默认使用Linux环境下的Pin v3.24工具包，同学们也可以下载任意OS下的任意版本的Pin工具包，如图3-1所示。

<center><img src="../assets/3-1.png" width = 650></center>
<center>图3-1 下载Pin插桩工具包</center>

&emsp;&emsp;Pin插桩工具包是“开箱即食”的，即下载完成后，只需将所下载的压缩包解压，即可使用。

&emsp;&emsp;关于Pin的插桩API，同学们既可以从[Intel官网](https://software.intel.com/sites/landingpage/pintool/docs/98437/Pin/html/group__API__REF.html)访问，也可以从解压后工具包的`doc/html/group__API__REF.html`页面进行访问。

## 2. 插桩原理简介

&emsp;&emsp;程序插桩的基本原理和方法是将额外代码插入到目标程序当中，以收集目标程序的信息。根据插桩后目标程序是否需要重新编译或链接，可将插桩方法分为源码插桩（Source Code Instrument）和二进制插桩（Binary Instrument）两类。前者对目标程序的源码进行插桩，因此需要对目标程序进行重新编译或链接；后者则直接对目标程序的二进制可执行文件进行插桩，插桩后的二进制文件能够直接执行。进一步地，可将二进制插桩分为静态插桩和动态插桩两种。静态插桩是指在目标程序的二进制可执行文件中插入插桩，并生成一个新的二进制可执行文件；动态插桩则是指在目标程序运行时插入插桩，不生成额外的文件。本实验使用的Pin是动态的二进制插桩工具包。

### 2.1 插桩工具（Pintool）

&emsp;&emsp;插桩工具主要由插桩代码和分析代码两部分组成。插桩代码用来决定在哪些地方插入哪些代码，而分析代码则是指被插入的代码。

&emsp;&emsp;下载Pin工具包后，我们可以打开工具包的`source/tools/ManualExamples`目录下的插桩工具示例代码`inscount0.cpp`，如图3-2所示。

<center><img src="../assets/3-2.png" width = 600></center>
<center>图3-2 插桩代码与分析代码</center>

&emsp;&emsp;在图3-2中，`Instruction()`和`docount()`函数分别是插桩工具inscount0的插桩代码和分析代码。`Instruction()`决定了在可执行文件的每一条指令前插入`docount()`函数；而`docount()`每次执行都将计数器`icount`的值加1。显然，插桩工具inscount0的功能是统计可执行文件的指令数量。

&emsp;&emsp;Pin拦截可执行文件中第一条指令 I~0~ 的执行，然后由插桩代码在 I~0~ 及其后顺序执行的代码中注入分析代码，从而生成新的代码。随后，CPU开始执行新代码。如果执行过程中出现分支跳转，则Pin将拦截分支目标的第一条指令，并由插桩代码向分支注入分析代码，从而生成该分支的新代码。由此可见，利用Pin对目标程序进行插桩分析时，只有Pin生成的代码才会被执行，而目标程序的原始代码则仅仅是一个参考。另外，Pin只对可执行文件中会被实际执行的指令进行插桩，即如果一条指令从未被执行，那么它不会被插桩（条件分支例外）。

### 2.2 可执行文件格式

&emsp;&emsp;Pin的输入可以是一个常规的可执行文件，如Windows下的PE（Portable Executable）文件（包括`.obj`、`.lib`、`.dll`和`.exe`文件）和Linux下的ELF（Executable Linkable Format）文件（包括`.o`、`.a`、`.so`、`.out`以及无后缀名的可执行文件）。

&emsp;&emsp;一般地，一个可执行文件对应一个二进制镜像（Binary Image），在镜像中包含若干个字段和节段（Section），一个节段包含若干个函数（Routine），一个函数又包含若干条指令（Instruction），如图3-3所示。

<center><img src="../assets/3-3.png" width = 400></center>
<center>图3-3 [ELF可执行文件格式](https://blog.csdn.net/feglass/article/details/51469511?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161611488316780262552143%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=161611488316780262552143&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~hot_rank-7-51469511.pc_v2_rank_blog_default&utm_term=ELF+%E6%A0%BC%E5%BC%8F)</center>

&emsp;&emsp;注：更多可执行文件格式的相关内容，可自行搜索相关资料，此处不作展开。

### 2.3 插桩的粒度

&emsp;&emsp;针对可执行文件的格式和组成部分，Pin提供了4种不同粒度的插桩模式：

- 指令级插桩（Instruction Instrumentation）

&emsp;&emsp;即插桩工具以单条指令为基本单位对目标程序进行插桩分析。指令级插桩是最细粒度的插桩模式。该模式可用于遍历目标程序的所有指令，比如用来统计目标程序总共包含的指令数。指令级插桩通过函数[`INS_AddInstrumentFunction()`](https://software.intel.com/sites/landingpage/pintool/docs/98612/Pin/doc/html/group__INS__INSTRUMENTATION.html#ga1333734dbf7d552365a24cd945d5691d)实现。

- 轨迹级插桩（Trace Instrumentation）

&emsp;&emsp;即插桩工具每次对目标程序中的一个Trace进行遍历和插桩。Trace指的是以分支跳转的目标语句开始、以无条件跳转语句（包括函数调用和返回语句）结束的指令序列。一个Trace只有一个入口，但通常有多个出口。如果Trace中间存在条件分支语句，则存在一个以该分支的目标语句为首的另一个Trace。为了实现对Trace的遍历和插桩，Pin将一个Trace划分为若干个具有单入口、单出口的基本块（Basic Block，BBL）。如此，在进行轨迹级插桩时，就可以以BBL为基本单位进行插桩，而无须为每条指令进行插桩，从而提高了插桩分析的效率。轨迹级插桩通过[`TRACE_AddInstrumentFunction()`](https://software.intel.com/sites/landingpage/pintool/docs/98612/Pin/doc/html/group__TRACE.html#gab2f19ff0a9198c83715eea79ada22503)函数实现。

- 镜像级插桩（Image Instrumentation）

&emsp;&emsp;即在首次加载目标程序的二进制可执行镜像时，插桩工具对整个镜像进行遍历和插桩。镜像级插桩是最粗粒度的插桩模式。在该模式下，插桩工具可以遍历镜像中的节段、节段中的函数以及函数中的指令，且可将分析代码注入到镜像的任何位置。镜像级插桩通过[`IMG_AddInstrumentFunction()`](https://software.intel.com/sites/landingpage/pintool/docs/98612/Pin/doc/html/group__IMG.html#ga4a067152140ead3e23279ab2bd6cd723)函数实现。需要注意的是，镜像级插桩需要依靠符号（Symbol）信息来确定函数的边界，因此在调用[`PIN_Init()`](https://software.intel.com/sites/landingpage/pintool/docs/98612/Pin/doc/html/group__PIN__CONTROL.html#gade9cf860942a12f2f81eff43be7861f4)函数对Pin进行初始化之前，必须先调用[`PIN_InitSymbols()`](https://software.intel.com/sites/landingpage/pintool/docs/98612/Pin/doc/html/group__PIN__CONTROL.html#ga5a74f4530db5d94bede1391c59e39939)函数以初始化符号表。

- 函数级插桩（Routine Instrumentation）

&emsp;&emsp;即在首次加载包含某函数的二进制可执行镜像时，插桩工具对整个函数进行遍历和插桩。在该模式下，插桩工具可以遍历函数中的所有指令，且可将分析代码注入到函数的任何位置。在编写镜像级插桩时，可以使用函数级插桩来遍历镜像中的节段和函数，从而为插桩工具的编写者提供便利。函数级插桩通过[`RTN_AddInstrumentFunction()`](https://software.intel.com/sites/landingpage/pintool/docs/98612/Pin/doc/html/group__RTN.html#ga63bd82d1e10ee8c83d67529845f5ca46)函数实现。

&emsp;&emsp;与指令级插桩和轨迹级插桩不同，镜像级插桩和函数级插桩都是在镜像加载时（而不是镜像执行时）完成的，因此这两种插桩模式又称为提前插桩；但也正因如此，我们不能确定插桩工具所注入的分析代码是否一定会被执行。

### 2.4 插桩的开销

&emsp;&emsp;插桩分析需要将额外的代码插入到可执行文件中，以收集可执行文件的运行时信息，这使得插桩工具必然存在开销（Overhead）。

&emsp;&emsp;插桩工具的开销包括插桩代码的开销和分析代码的开销；而分析代码的开销又取决于分析代码的执行次数和执行开销，如图3-4所示。

<center><img src="../assets/3-4.png" width = 350></center>
<center>图3-4 插桩工具的开销</center>

&emsp;&emsp;插桩代码只在可执行文件的指令被首次执行时执行，因此插桩代码的开销是相对固定的；而分析代码则在指令每次执行时都会被执行。因此，要想减小插桩分析的开销，必须减小分析代码的执行次数和执行开销。

&emsp;&emsp;为了提高插桩分析的效率，在编写插桩工具时，应遵循以下2条原则：

- a. 尽可能使用较大粒度的插桩模式，以减少分析代码的执行次数；

- b. 尽可能使用简单的分析代码，以减小分析代码的执行开销。

&emsp;&emsp;基于这两条原则，Intel在Pin的用户手册中给出了包括分析代码任务转移、控制流消除、编译器优化等优化方法，具体可参考Pin用户手册首页中的“[Performance Considerations When Writing a Pintool](https://software.intel.com/sites/landingpage/pintool/docs/98612/Pin/doc/html/index.html#PERFORMANCE)”。

## 3. 指令依赖距离

&emsp;&emsp;指令依赖距离是指当一个寄存器被某条指令写入，且被后续指令读取时，这两条指令之间的指令数。例如，对于如图3-5所示的汇编程序，SI寄存器的依赖距离是2和3。

<center><img src="../assets/3-5.png" width = 120></center>
<center>图3-5 指令依赖距离示例</center>

&emsp;&emsp;指令依赖距离的分布情况隐含了重要的体系结构信息，比如架构师在设计流水线时，可参考测试程序的指令依赖距离信息来决定应该使用什么方法来解决RAW的数据冲突问题。
