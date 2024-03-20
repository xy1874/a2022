# 实验步骤

## 1. 查看Cache和TLB参数

&emsp;&emsp;首先，利用第三方工具查看PC机的Cache和TLB参数，用于验证所编写的测量程序是否正确。

&emsp;&emsp;运行实验包中的CPU-Z工具，点击“缓存”标签页，即可查看本机的Cache结构，如图4-1所示。

<center><img src="../assets/4-1.png" width = 400></center>
<center>图4-1 查看Cache结构</center>

&emsp;&emsp;在虚拟机中输入命令`sudo apt install -y x86info`，安装x86info工具。安装完毕后，在终端中执行`x86info -c`命令，即可查看PC机的TLB结构信息，如图4-2所示。

<center><img src="../assets/4-2.png" width = 600></center>
<center>图4-2 查看TLB结构信息</center>

&emsp;&emsp;由图4-2可知，示例PC机的内存页大小为4KB，且TLB采用4路组相联结构，共64条entry。

!!! info "补充说明 :book:"
    &emsp;&emsp;每台PC机的TLB结构都不相同，请以实际情况为准。



## 2. 编写测试程序

&emsp;&emsp;将实验包中的`cache_test.c`程序拷贝到虚拟机，完成各测量函数的编写。

&emsp;&emsp;编写测量函数时，依次在不同的配置参数下进行访存，要求记录并打印每组参数下的平均访存时间。

!!! warning "特别注意 :fire:"
    &emsp;&emsp;记录时间时，需保证不同配置参数下的访存次数相同。


## 3. 记录实验结果，绘制访问时间折线图

&emsp;&emsp;执行`gcc -Og cache_test.c -o cache_test && ./cache_test`对程序进行编译、运行。

&emsp;&emsp;查看程序运行结果。对如图4-1和图4-2所示的PC机，参考测量结果如图4-3所示。

<center><img src="../assets/4-3.png" width = 500></center>
<center>图4-3 执行程序并查看运行结果</center>

!!! info "特别说明 :loudspeaker:"
    &emsp;&emsp;（1）程序运行时，应当如图4-3一般，实时输出每组参数下的各个平均访存时间。不同参数下的平均访存时间应当能观察到较为明显的梯度变化。此外，可自行编写程序，以根据所记录的一系列平均访存时间，解析出所测出的Cache参数值。

    &emsp;&emsp;（2）Cache参数的测量容易受到替换策略、其他层次Cache等影响，尤其是L2 Cache，故其实验结果可能不甚明显。例如，在图4-3中，测量L2 Cache相联度的平均访问时间梯度变化远不如测量其他Cache参数时那般明显。如果测量结果不明显，在实验报告中给出自己的分析。

&emsp;&emsp;将测量各参数时的Cache平均访存时间绘制成折线图，结合折线图分析实验结果。

## 4. 思考题

&emsp;&emsp;（1）为了进一步提高访存性能，现代处理器往往利用时间局部性和空间局部性，将所访问数据块相邻的数据块预取到Cache中。试分析数据预取将给实验结果带来什么影响？

&emsp;&emsp;（2）试分析Cache对测量TLB造成何种影响？如何排除Cache的干扰？

&emsp;&emsp;（3）测量TLB前，是否需要遍历访问一次全部空间？为什么？

&emsp;&emsp;（4）要写出高效的程序，必须对底层硬件有足够的了解。请谈谈如何利用Cache或TLB提高程序性能。
