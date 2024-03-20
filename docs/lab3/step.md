# 实验步骤

## 1. 编写Cache模型

&emsp;&emsp;源文件`cacheModel.cpp`中定义了6个Cache类，其中`CacheModel`是基类，其他是子类。请根据代码中的注释提示，实现全相联映射、组相联映射的Cache模型。

&emsp;&emsp;编写完成后，在虚拟机中使用SPEC2006基准测试程序中的`gcc`、`astar`、`zeusmp`和`tonto`对所实现的5种Cache模型进行测试。

!!! info "提示 :mega:"
    &emsp;&emsp;`cacheModel.cpp`中定义的`get_phy_addr()`函数用于虚实地址转换，可将其视为MMU或TLB。
    
## 2. 探究基本参数对命中率的影响

&emsp;&emsp;针对组相联映射的Cache模型，设置不同的Cache参数（至少包括块数/容量、块大小、相联度），并分别运行上述4个benchmark。将不同参数下的命中率变化绘制成折线图，并在实验报告中进行分析。

!!! info "补充说明 :mega:"
    &emsp;&emsp;绘制三个折线图即可 —— 分别是“**命中率-块数**”、“**命中率-块大小**”、“**命中率-相联度**”。折线图样式可参考图3-7，画图工具不限。
    
    <center><img src="../assets/3-7.png" width = 300></center>
    <center>图3-7 折线图样式参考</center>
    
    &emsp;&emsp;PS：曲线上的每个点的数据 = 跑4个benchmark得到的命中率取平均值

    &emsp;&emsp;不限制每个Cache参数的取值数量，只要画出来的折线图能反映命中率与该参数的关系即可。

## 3. 索引方式对比分析

&emsp;&emsp;~~改变Cache容量、块大小、相联度的参数，使用三种索引方式的组相联Cache模型运行上述4个benchmark，分别测量三种索引方式下的Cache命中率。观察不同索引方式的Cache，其命中率有多大区别？尝试分析原因。将测试参数、测试结果及相关分析写入实验报告。~~

&emsp;&emsp;~~索引方式的对比不要求画折线图，只需选取若干组数据运行benchmark，且测试结果能反应出不同索引方式的对比即可。~~

&emsp;&emsp;实现三种索引方式的组相联Cache模型，运行SPEC2006基准测试程序中的`gcc`、`astar`、`zeusmp`和`tonto`测试用例，记录实验结果，观察不同索引方式Cache的命中率有多大区别，并尝试分析原因。
