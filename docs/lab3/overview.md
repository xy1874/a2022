# 实验目的

&emsp;&emsp;1. 通过设计Cache模型，探索不同参数对Cache命中率的影响；

&emsp;&emsp;2. 了解虚拟内存下，三种地址索引方式的Cache工作原理；

&emsp;&emsp;3. 掌握使用Pin插桩工具进行体系结构建模和分析的基本方法。

&emsp;&emsp;

# 实验内容

&emsp;&emsp;本实验将基于Pin插桩分析的工作机理，使用C/C++实现Cache软件模型，具体内容包括：

&emsp;&emsp;（1）利用Pin插桩工具，编写全相联、组相联映射的Cache模型；

&emsp;&emsp;（2）探讨命中率与Cache基本参数（如块大小、块数/容量、相联度、替换算法等）的关系，自行选择若干组参数进行测试，绘制折线图并分析之；

&emsp;&emsp;（3）实现VIVT、PIPT、VIPT三种地址索引方式的组相联Cache，并通过测试，对比分析三种索引方式的区别和优缺点。

!!! danger "要求 :information_desk_person:"
    &emsp;&emsp;Cache的容量不能超过32KB。
