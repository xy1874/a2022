# 验收与提交

## 实验报告要求

&emsp;&emsp;实验报告需包含以下内容：

&emsp;&emsp;（1）借助Pin的API文档，阅读`ManualExamples目录`下`inscount0`、`inscout1`和`inscout2`工具的源码，分析插桩工具的代码框架和执行过程；

&emsp;&emsp;（2）思考并回答以下问题，将答案以及分析过程写入实验报告。

&emsp;&emsp;&emsp;&emsp;a. 可以发现，指令依赖距离分布图存在很长的尾巴。试分析尾巴可能是由哪些寄存器引
&emsp;&emsp;&emsp;&emsp;起的？为什么？

&emsp;&emsp;&emsp;&emsp;b. 设有2个不同架构的处理器平台，现分别于其上运行由相同版本编译器所编译的测试程
&emsp;&emsp;&emsp;&emsp;序，并分别在这两个平台上使用insDependDist工具对该测试程序进行插桩分析，得到如
&emsp;&emsp;&emsp;&emsp;图6-1所示的指令依赖距离分布图。请问架构A、B之中谁具有更多的寄存器？为什么？

<center><img src="../assets/6-1.png" width = 500></center>
<center>图6-1 2种架构下，同一程序的指令依赖距离分布图</center>

&emsp;&emsp;&emsp;&emsp;c. 现有基于相同ISA设计的架构A和架构B。若架构A采用
&emsp;&emsp;&emsp;&emsp;停顿法解决流水线数据冲突，架构B则采用数据转发法，当二者执行相同的测试程序时，
&emsp;&emsp;&emsp;&emsp;它们的指令依赖距离分布图是否相同？为什么？

## 验收与提交要求

- 必做题：

&emsp;&emsp;&emsp;&emsp;将源码（`makefile.rules`、`*.cpp`）及实验报告打包成`.zip`，以“<font color=orange>**学号_姓名_ARCH实验1.zip**</font>”命名并提交。

- 附加题：

&emsp;&emsp;&emsp;&emsp;将附加题源文件重命名（如`insDependDist1.cpp`），并与必做题提交材料一同打包提交；实验报告的要求见题目。

!!! warning "作业提交 :calendar:"
    &emsp;&emsp;作业提交方法见《[HITsz Grader作业提交指南](../../ojguide/)》。

    &emsp;&emsp;作业提交<font color = red>**Deadline**</font>：<font color = purple>**下周同一上课时间前**</font>。
