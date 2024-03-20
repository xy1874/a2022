# 附加题

## 题目1：实现分支目标预测（<font color=green>**+2**</font>分）

&emsp;&emsp;必做部分的分支预测器只对分支跳转方向进行预测。请修改相应的代码，使锦标赛预测器支持分支目标地址的预测。要求：

&emsp;&emsp;（1）测试预测器的准确率；

&emsp;&emsp;（2）在实验报告中描述设计思路，将测试结果列出表格，并对测试结果进行分析。

!!! hint "提示 :bulb:"
    &emsp;&emsp;在插桩代码中使用[IARG_BRANCH_TARGET_ADDR](https://software.intel.com/sites/landingpage/pintool/docs/98437/Pin/html/group__INST__ARGS.html#gga089c27ca15e9ff139dd3a3f8a6f8451daa1b650f4229df434f3ac5ef61f14d0ed)参数来获取分支指令的目标地址。

## 题目2：实现TAGE的其他变体（<font color=green>**+2**</font>分）

&emsp;&emsp;自行查阅论文，实现TAGE变体。要求：

&emsp;&emsp;（1）变体的预测性能需明显高于TAGE；

&emsp;&emsp;（2）在实验报告中介绍变体的架构、预测过程、更新策略；

&emsp;&emsp;（3）测试预测器的准确率，以图表的形式写入实验报告并分析实验结果。
