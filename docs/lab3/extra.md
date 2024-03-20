# 附加题

## 题目1：VIVT Cache改造（<font color=green>**+1.5**</font>分）

&emsp;&emsp;由于不同的进程具有不同的虚拟地址空间，因此发生进程切换时，需要清空VIVT Cache。改造VIVT Cache的结构，使进程切换时不需清空Cache。

&emsp;&emsp;通过测试，画出不同Cache大小下，改造前后的命中率变化折线图，并在实验报告中分析之。



## 题目2：探究替换算法及写策略对Cache性能的影响（<font color=green>**+1.5**</font>分）

&emsp;&emsp;（1）替换算法与Cache性能的探究

&emsp;&emsp;至少实现3种常用的替换算法（如随机法、FIFO、伪LRU、LRU等），并通过测试，对比分析替换算法对不同大小Cache的性能影响。

&emsp;&emsp;（2）写策略对Cache性能的探究

&emsp;&emsp;通过`sleep()`函数模拟写入内存的延迟，实现写直达策略和写回策略，并比较分析两种写策略对Cache性能的影响。
