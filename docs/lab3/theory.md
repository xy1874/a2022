# 实验原理

## 1. Cache简介

&emsp;&emsp;为了缓解CPU和主存之间的存储墙（Memory Wall）问题，架构设计师在CPU和主存之间增加了高速缓存器（Cache）。根据程序访问的局部性原理，如果在Cache中缓存CPU刚刚访问过的数据及其邻近的数据，那么当CPU再次访问（且大概率会再次访问）这些数据时，就可以直接从Cache中获取，减少CPU的等待时间，从而提高数据访问效率和CPU利用率，进而提高计算机系统的整体性能。

&emsp;&emsp;为了平衡性能和成本，现代计算机系统通常采用“寄存器 - Cache - 主存 - 辅存”的多级存储结构。通常Cache又进一步分为L1 Cache（一级缓存）和L2 Cache（二级缓存）等，如图3-1所示。

<center><img src="../assets/3-1.png" width = 380></center>
<center>图3-1 现代计算机系统的多级存储系统</center>



## 2. 基本工作流程

&emsp;&emsp;Cache存放的是主存数据的副本，而Cache容量远小于主存，因此需要为Cache块增加块标记（Tag）以标记Cache块当前存放的是哪一个内存数据块。此外，还需为Cache块增加有效位（valid）以表示Cache块当前是否存放了有效数据。一般地，将各个Cache块的块标记和有效位合称为Cache的 **目录表**，也称Cache的 **元数据**（Metadata）。

&emsp;&emsp;Cache完成CPU访问的工作流程有三步：第一步，将主存地址映射成Cache地址，即按照相应的地址映射规则从主存地址中分解出块标记，并用块标记查询Cache目录表，判断是否命中；若命中则进行地址变换；若不命中，则需要先结合替换策略从主存中调入数据块再进行地址变换；第二步，使用变换得到的Cache地址访问Cache，完成CPU的访存需求，这与访问普通RAM完全相同；第三步，根据写策略在适当的时候将所写数据写回主存，以保持Cache和主存的数据一致性。



## 3. 地址映射规则

&emsp;&emsp;Cache地址和主存地址之间的有全相联映射、直接映射和组相联映射三种映射规则。

&emsp;&emsp;全相联映射方式下，只要Cache中还有空闲的Cache块，主存中的任意一个数据块就都可以缓存在Cache中。因此，全相联Cache能够获得较高的空间利用率和命中率，但由于访问数据块时需要遍历所有Cache元数据，故其访问延迟较大。

&emsp;&emsp;直接映射方式下，主存被划分成若干个区，每个区所包含的数据块数与Cache块数相同，且每个区中的第$i$块只能映射到Cache中的第$i$块。明确的映射方式使得访问Cache时只需读取一条元数据即可判断是否命中，故直接映射Cache具有较快的访问速度，但命中率也随之受到一定影响。

&emsp;&emsp;组相联映射是全相联映射和直接映射的折中方案。在组相联映射方式下，主存被划分成若干个区，Cache被划分成若干个组，并且主存中每个区包含数据块个数等于Cache的组数；Cache组间采用直接映射，组内采用全相联映射。假设某组相联Cache容量为32KB，主存容量为2MB，块大小为128B，则Cache与主存的映射关系以及主存地址格式如图3-2所示。

<center><img src="../assets/3-2.png"></center>
<center>图3-2 组相联映射示意图及主存地址格式</center>



## 4. 地址索引方式

&emsp;&emsp;在支持虚拟存储的系统当中，CPU访问存储器的地址都是虚拟地址。虚拟地址首先需要经过MMU或页表转换成对应的物理地址，才能从存储器中访问到CPU想要的数据。

&emsp;&emsp;对组相联映射方式而言，CPU访问Cache时，首先从主存地址中分解出区内块号作为Cache组号的索引，以定位数据可能在Cache中的哪个组；然后从主存地址中分解出区号作为块标记，跟Cache组中的现有块标记进行匹配。根据CPU访问Cache时，Cache接收到的区内块号（即Cache组索引）、区号（即块标记）所对应的地址是虚拟地址还是物理地址，可将Cache的地址索引方式分为虚拟索引-虚拟标识（Virtually Indexed and Virtually Tagged，VIVT）、物理索引-物理标识（Physically Indexed and Physically Tagged，PIPT）和物理索引-虚拟标识（Virtually Indexed and Physically Tagged，VIPT）三种。

### 4.1 虚拟索引-虚拟标识

&emsp;&emsp;VIVT的Cache又称虚拟Cache，采用虚拟地址作为索引和块标记，可以直接接受虚拟地址访问，其基本结构如图3-3所示。当CPU需要访问存储器时，将虚拟地址同时发给TLB和Cache，以同时进行Cache访问和虚实地址转换 —— Cache接受虚拟地址并判断当前访问是否命中，如果命中则直接从相应的Cache块中取出数据并返回给CPU；如果发生缺失，则使用TLB转换得到的物理地址访问主存，以取出数据块并装入Cache。

<center><img src="../assets/3-3.png" width = 500></center>
<center>图3-3 VIVT Cache存储系统示意图</center>

&emsp;&emsp;在VIVT Cache存储系统中，Cache访问和虚实地址转换是并行进行的，在Cache缺失时可以马上使用物理地址访问主存，因此VIVT Cache具有较低的访问延迟。

&emsp;&emsp;由于不同的进程具有不同的虚拟地址空间，因此发生进程切换时，需要清空VIVT Cache，从而增加了进程切换的开销，也降低了整体的Cache命中率。

### 4.2 物理索引-物理标识

&emsp;&emsp;PIPT的Cache又称物理Cache，采用物理地址作为索引和块标记，接受物理地址访问，其基本结构如图3-4所示。CPU在访问存储器时，首先需要将虚拟地址发给TLB进行虚实地址转换，然后使用得到的物理地址访存。当Cache命中时，从相应的Cache块中取出数据并返回给CPU；当Cache访问缺失时，使用物理地址访问主存，取出相应的数据块并装入Cache。

<center><img src="../assets/3-4.png" width = 500></center>
<center>图3-4 PIPT Cache存储系统示意图</center>

&emsp;&emsp;与VIVT Cache不同，PIPT Cache存储系统的Cache访问和虚实地址转换是串行进行的，因此具有较大的访问延迟，但发生进程切换时，不需要清空Cache。

### 4.3 虚拟索引-物理标识

&emsp;&emsp;VIPT的Cache采用虚拟地址作为索引、物理地址作为块标记，接受虚拟地址访问，其基本结构如图3-5所示。CPU访问VIPT Cache时，使用虚拟地址作为索引查Cache的元数据，同时将虚拟地址发给TLB进行虚实地址转换。理想情况下，虚拟索引在Cache中查到了相应的数据时，TLB的虚实地址转换也正好完成，此时即可进行物理标识的比对。

<center><img src="../assets/3-5.png" width = 500></center>
<center>图3-5 VIPT Cache存储系统示意图</center>

&emsp;&emsp;VIVT Cache的优点是访问延迟低，缺点是进程切换需要清空Cache；PIPT Cache的优点是进程切换不需要清空Cache，但访问延迟较高。为此，VIPT Cache采用了虚拟地址索引、物理地址标识，使得Cache访问和TLB虚实地址转换具有一定的并行度，从而同时具备了VIVT Cache并行访问的优点和PIPT Cache不受进程切换影响的优点。

!!! question "思考题 :book:"
    &emsp;&emsp;若采用VIPT索引方式，直接映射Cache的大小需要小于单个页的大小，为什么？



## 5. cacheModel.cpp说明

&emsp;&emsp;实验包的源文件`cacheModel.cpp`中定义了6个Cache类，其中`CacheModel`是基类，其他是子类；`FullAssoCache`、`SetAssoCache`分别是全相联映射和组相联映射Cache，如图3-6所示。

<center><img src="../assets/3-6.png"></center>
<center>图3-6 Cache类图</center>

&emsp;&emsp;其中基类CacheModel的各个成员变量的含义如表3-1所示。

<center>表3-1 CacheModel类成员变量</center>
<center>

| 序号 | 成员变量 | 释义 |
| :-: | :- | :- |
| 1 | m_block_num | Cache块的数目 |
| 2 | m_blksz_log | Cache块大小的以2为底的对数 |
| 3 | m_valids | 有效位标记 |
| 4 | m_tags | Cache块标记 |
| 5 | m_replace_q | 发生Cache块替换时，待替换块所组成的候选队列 |
| 6 | m_rd_reqs | 读请求次数 |
| 7 | m_wr_reqs | 写请求次数 |
| 8 | m_rd_hits | 读请求命中次数 |
| 9 | m_wr_hits | 写请求命中次数 |

</center>

&emsp;&emsp;基类CacheModel的各个成员函数的说明如表3-2所示。

<center>表3-2 CacheModel类成员函数</center>
<center>

| 序号 | 成员函数 | 释义 |
| :-: | :- | :- |
| 1 | readReq | 接收到读请求时，查询Cache是否命中，同时更新m_rd_reqs和m_rd_hits |
| 2 | writeReq | 接收到写请求时，查询Cache是否命中，同时更新m_wr_reqs和m_wr_hits |
| 3 | getRdReq | 返回当前接收到的读请求总次数m_rd_reqs |
| 4 | getWrReq | 返回当前接收到的写请求总次数m_wr_reqs |
| 5 | dumpResults | 打印当前接收到的请求次数、命中次数和命中率 |
| 6 | lookup | （**纯虚函数**）用当前的访存地址查Cache，判断是否命中；若命中，记录对应块号 |
| 7 | access | （**纯虚函数**）模拟Cache访问：命中时更新替换候选队列，否则进行Cache块替换 |
| 8 | updateReplaceQ | （**纯虚函数**）更新Cache块替换的候选队列m_replace_q |

</center>
