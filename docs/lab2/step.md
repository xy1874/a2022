# 实验步骤

## 1. 编写预测器

&emsp;&emsp;实验包中的`brchPredict.cpp`已搭建好Pin插桩的代码框架。其中，插桩代码首先从目标程序中挑选出会被真实执行的控制流指令（包括条件分支指令、Call指令和Ret指令等），然后分别在这些指令的分支成功和失败处插入分析代码，如图4-1所示。

<center><img src="../assets/4-1.png" width = 500></center>
<center>图4-1 插桩代码</center>

&emsp;&emsp;分析代码获取分支指令地址，调用分支预测器模型进行分支预测，并记录模型的预测结果数据，如图4-2所示。

<center><img src="../assets/4-2.png" width = 300></center>
<center>图4-2 分析代码</center>

&emsp;&emsp;此外，`brchPredict.cpp`定义了饱和计数器类`SaturatingCnt`、移位寄存器类`ShiftReg`和分支预测器模型的基类`BranchPredictor`等。其中，`BHTPredictor`、`GlobalHistoryPredictor`等其余预测器模型均是`BranchPredictor`的派生类。

&emsp;&emsp;补全分支预测器模型的代码后，在`main`函数中对预测器模型进行实例化。保存代码后，将`brchPredict.cpp`拷贝到虚拟机的`~/pin-X.XX/workspace`目录中，然后在终端执行`make`命令，即可编译生成分支预测模型的插桩工具。

!!! info "Tips :book:"
    &emsp;&emsp;（1）请以GB2312编码打开`brchPredict.cpp`，否则里面的中文注释将乱码。

    &emsp;&emsp;（2）虚拟机和PC机之间的共享文件操作请参考[VirtualBox虚拟机共享文件设置](../../vm)。

## 2. 测试与分析

&emsp;&emsp;本实验使用SPEC 2006标准测试程序集中的 **`bzip2`**（压缩算法）、**`sjeng`**（国际象棋算法）、**`wrf`**（天气预报算法）和 **`sphinx3`**（语音识别算法）对分支预测器模型进行测试。其中，**`bzip2`** 和 **`sjeng`** 是定点测试程序，**`wrf`** 和 **`sphinx3`** 是浮点测试程序。

### 2.1 测试方法

&emsp;&emsp;任意一项SPEC Benchmark运行时，默认都进行build操作。为了提高测试效率，建议首先单独运行相应的Benchmark。以`bzip2`为例，运行命令如下：

``` sh
$> runspec --size=test --noreportable bzip2
```

&emsp;&emsp;如此，后续再运行`bzip2`时，即可添加`--nobuild`参数来关闭build操作，提高测试效率。

&emsp;&emsp;测试时，打开终端，用cd命令进入到`~/pin-X.XX/workspace`目录，然后执行类似下列命令，分别对4个Benchmark进行测试。

``` sh
$> ../pin -t obj-intel64/brchPredict.so -- runspec --size=test --noreportable --nobuild bzip2
```

&emsp;&emsp;每次测试完成后，插桩工具都将预测结果输出到`brchPredict.txt`文件，同时将预测结果打印在终端，如图4-3所示。

<center><img src="../assets/4-3.png" width = 550></center>
<center>图4-3 查看测试结果</center>

### 2.2 测试要求

&emsp;&emsp;分别测试所实现的4个预测器的预测准确率，在实验报告中填写如表4-1所示的测试表格。

<center>表4-1 分支预测模型测试表格</center>
<center><img src="../assets/t4-1.png" width = 400></center>

&emsp;&emsp;预测准确率需以百分数形式填写，且保留2位小数。此外，需注明各预测器的关键参数。

&emsp;&emsp;测试表格填写完整后，在实验报告中对实验结果进行对比分析。
