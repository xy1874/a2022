# 附加题

## 题目1：测量L2 Cache的参数（<font color=green>**+0.5**</font>分）

&emsp;&emsp;编写程序，测量L2 Cache的容量、块大小和相联度，并将关键实现、测试结果及分析写入实验报告。

&emsp;&emsp;要求在撰写报告时，着重从原理、实现两方面描述测量L1 DCache和L2 Cache时的区别。

## 题目2：测量Cache写策略（<font color=green>**+1**</font>分）

&emsp;&emsp;测量Cache所使用的替换策略：

&emsp;&emsp;对于写直达，数据在写到Cache块的同时还要写入更低一层的存储器块中。
对于写回法，数据仅被写入Cache中，只有Cache中数据块被替换出去时，才会写回到主存中。 

&emsp;&emsp;因此写直达会一直访问下一层存储，而写回法，如果不是被替换的话，一直是在Cache中进行访问。可以设计两种情况：

&emsp;&emsp;（1）数组被赋值修改，但是数组大小小于Cache的大小，数组没有被替换，如果是写回法，没有对主存储器访问；如果是写直达法则不停访问主存储器。

&emsp;&emsp;（2）数组的访问一直是缺失的，这样一直访问存储器。 

&emsp;&emsp;对于两种情况的时间进行对比：如果是写回法，差距很大，如果是写直达法则差距不大。

&emsp;&emsp;要求在实验报告中描述设计思路、关键实现、测试结果及分析。

## 题目3：测量Cache替换策略（<font color=green>**+1**</font>分）

&emsp;&emsp;根据前面测得的相联度，自行设计不同的访问序列，产生不同次数的缺失，以验证Cache所使用的替换策略。

&emsp;&emsp;要求在实验报告中描述设计思路、关键实现、测试结果及分析。
