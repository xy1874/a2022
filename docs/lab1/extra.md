# 附加题

## 题目：插桩效率优化（<font color=green>**+2**</font>分）

&emsp;&emsp;必做题中的insDependDist插桩工具是基于指令级的插桩粒度开发的，这意味着可执行文件的每条指令被执行时，都需要先执行分析代码。显然，如此细粒度的插桩分析，其效率较低。

&emsp;&emsp;请同学们阅读Pin用户手册所述的性能优化方法，并参考inscount1和inscount2的源码，修改insDependDist.cpp的代码，使其能够基于轨迹级的较粗粒度进行插桩分析，以提高insDependDist工具的效率。要求：

&emsp;&emsp;（1）比较修改前后的插桩效率有何不同；

&emsp;&emsp;（2）将设计思路、关键实现代码、测试方法、测试结论及分析写入实验报告当中。
