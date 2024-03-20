# 实验步骤

## 1. 导入虚拟机

&emsp;&emsp;本实验提供已安装好实验环境的Debian 11.5.0虚拟机。实验开始前，需要在VirtualBox中导入虚拟机镜像。

!!! warning "特别说明 :fire:"
    &emsp;&emsp;T2-102实验室的PC机已完成虚拟机的导入，但需要在虚拟机的设置中把“处理器数量”改成不大于4的数值。

&emsp;&emsp;首先，安装并打开VirtualBox。

&emsp;&emsp;然后，按照如图4-1所示，依次点击相应的按钮以导入虚拟机。

<center><img src="../assets/4-1.png" width = 600></center>
<center>图4-1 导入虚拟机</center>

&emsp;&emsp;点击“打开”按钮后，检查图4-2所示的设置是否与物理机兼容。**如果出现不兼容，需根据物理机的配置进行相应的修改**。

<center><img src="../assets/4-2.png" width = 400></center>
<center>图4-2 检查虚拟机配置是否需要修改</center>

&emsp;&emsp;检查无误后，点击窗口右下方的“导入”按钮，耐心等待虚拟机导入完成。

!!! info "补充说明 :mega:"
    &emsp;&emsp;导入虚拟机时，如果不确定哪些参数需要改，不妨先完成导入操作。导入完成后，打开虚拟机的设置，检查VirtualBox是否提示“发现无效设置”，如图4-3所示。

    <center><img src="../assets/4-3.png"></center>
    <center>图4-3 打开虚拟机设置，查看是否存在无效设置</center>

    &emsp;&emsp;如果存在无效设置，把鼠标悬停在图4-3底下的警告图标上，此时VirtualBox将弹出提示信息。按照提示信息，修改相应设置，直到不存在无效设置即可。

## 2. 运行Pin

&emsp;&emsp;启动虚拟机后，**以用户名`debian`、密码`123`登录系统**。然后，双击桌面上的“主文件夹”以打开`/home/debian`目录，其中以“pin-3.24”开头的压缩包即为Pin工具包。

&emsp;&emsp;在主文件夹上右键打开终端，并输入tar命令解压Pin的压缩包，如图4-4所示。

<center><img src="../assets/4-4.png" width = 500></center>
<center>图4-4 解压下载的Pin压缩包</center>

&emsp;&emsp;解压得到的文件夹名字冗长，不妨将其重命名为`pin-3.24`。

&emsp;&emsp;进入Pin工具包的根目录，可以看到类似图4-5的目录结构。

<center><img src="../assets/4-5.png" width = 560></center>
<center>图4-5 Pin工具包根目录</center>

&emsp;&emsp;其中，`doc`文件夹存放的是网页形式的Pin文档，内含API说明文档。

&emsp;&emsp;接着，使用`cd`命令进入到Pin工具根目录下的`source/tools/ManualExamples`目录，如图4-6所示。该目录存放的是基于Pin工具包开发的插桩工具范例。

<center><img src="../assets/4-6.png" width = 500></center>
<center>图4-6 查看插桩工具demo</center>

&emsp;&emsp;然后，执行`make`命令，编译ManualExamples目录下的插桩工具。

&emsp;&emsp;编译完成后，即可通过Pin工具包调用所生成的插桩工具对可执行文件进行插桩分析，其语法如图4-7所示。

<center><img src="../assets/4-7.png" width = 500></center>
<center>图4-7 Pin命令格式</center>

&emsp;&emsp;其中，`toolname`表示插桩工具，指编译后存放在`obj文件夹`下的`.so文件`；`output_file`表示插桩工具输出到文件；`target_executable`则表示被插桩分析的可执行文件。例如，基于上述命令，可以使用inscount0的指令计数插桩工具，对`ls`命令进行插桩，并将插桩工具的输出保存到`result.txt`，如图4-8所示。

<center><img src="../assets/4-8.png" width = 550></center>
<center>图4-8 运行inscount插桩工具</center>

&emsp;&emsp;执行上述命令后，Pin将在当前的ManualExamples目录下调用`ls`工具，从而在终端中打印出当前目录的文件列表。此时，可查看inscount0插桩工具的输出信息，如图4-9所示。

<center><img src="../assets/4-9.png" width = 600></center>
<center>图4-9 inscount0对`ls`插桩分析的输出</center>

## 3. 编写insDependDist插桩工具

&emsp;&emsp;在虚拟机的`/home/debian`目录下，以“workspace”开头的压缩包即为本实验的实验包。

&emsp;&emsp;使用`unzip`命令解压，或在右键菜单解压，然后将解压后的`workspace目录`移动到Pin工具包的根目录下，如图4-10所示。

<center><img src="../assets/4-10.png" width = 600></center>
<center>图4-10 解压workspace到Pin根目录下</center>

&emsp;&emsp;打开workspace目录，可见如图4-11所示的文件夹及文件。

<center><img src="../assets/4-11.png" width = 550></center>
<center>图4-11 workspace目录中的内容</center>

!!! attention "注意 :loudspeaker:"
    &emsp;&emsp;本实验只需修改`insDependDist.cpp`和`makefile.rules`，**其余文件夹和文件请勿删改**。

&emsp;&emsp;其中，`inscount0.cpp`是Pin自带的指令计数插桩工具的范例程序；`insDependDist.cpp`是本实验的指令依赖距离的插桩工具，该工具将获取可执行文件的指令依赖信息，并将这些信息输出到`insDependDist.csv`文件；`paintInsDependDist.py`的绘图程序读取`insDependDist.csv`文件并绘制被分析可执行文件的指令依赖分布图；`obj-intel64`文件夹存放编译workspace时生成的ELF文件；`makefile`、`makefile.rules`和`Config`文件夹存放编译workspace所需的编译规则及配置；`divide_by_zero_unix.c`是编译workspace所需的工具源文件。

&emsp;&emsp;然后，打开`insDependDist.cpp`文件，根据注释中的代码提示，补全5处空缺的功能代码。在`insDependDist.cpp`中，全局数组`insDependDistance[]`用于存放被插桩程序的指令依赖距离信息——`insDependDistance[i]`表示依赖距离为`i+1`的出现次数。例如，在图3-5所示的代码中，`SI`寄存器的依赖距离为2和3，`CX`寄存器的依赖距离为1，则插桩工具分析完这4条指令之后，`insDependDistance[0]`、`insDependDistance[1]`、`insDependDistance[2]`的值都将+1。

&emsp;&emsp;`insDependDist.cpp`的代码补全完毕后，右键选择`Mousepad`打开`makefile.rules`文件，将指令依赖距离插桩工具的名字添加到`TEST_TOOL_ROOTS`变量的后面，如图4-12所示。

<center><img src="../assets/4-12.png" width = 650></center>
<center>图4-12 在`makefile.rules`中添加新的插桩工具</center>

&emsp;&emsp;所有代码编写完成后，打开终端，使用`cd`命令进入到`workspace`目录，并输入`make`命令以编译insDependDist工具。

&emsp;&emsp;编译完成后，将在`obj-intel64`（或`obj-ia32`）目录下生成`insDependDist.o`和`insDependDist.so`文件。

&emsp;&emsp;接着，可以在终端中输入如图4-13所示的命令，对`/bin/ls`进行插桩分析。

<center><img src="../assets/4-13a.png" width = 410></center>
<center>a) 默认情况下，最大统计100及以内的依赖距离</center>

<center><img src="../assets/4-13b.png" width = 450></center>
<center>b) 通过`-s`选项设置插桩工具统计的最大依赖距离（如设置为120）</center>

<center>图4-13 使用insDependDist工具对`/bin/ls`进行插桩分析</center>

&emsp;&emsp;需要注意的是，insDependDist工具默认只统计被插桩可执行文件中不超过100的依赖距离。如果想统计更远的距离，可以使用`-s`选项来设置，如图4-13 b)所示。此功能得益于`insDependDist.cpp`中用于设置最大依赖距离的Knob，如图4-14所示。

<center><img src="../assets/4-14.png" width = 650></center>
<center>图4-14 insDependDist工具中，设置最大依赖距离的Knob</center>

&emsp;&emsp;通过编写Knob，可以在调用插桩工具时进行特殊参数的设置。感兴趣的同学可以自行编写新的Knob。

&emsp;&emsp;insDependDist工具运行后，将在当前目录中生成`/bin/ls`的指令依赖距离信息。此时，在终端中使用`python3`运行`paintInsDependDist.py`脚本，即可得到`/bin/ls`的指令依赖距离分布图，如图4-15所示。

<center><img src="../assets/4-15.png" width = 450></center>
<center>图4-15 `/bin/ls`的指令依赖距离分布图参考</center>

!!! info "补充说明 :mega:"
    &emsp;&emsp;paintInsDependDist.py默认绘制45以内的依赖距离。若想绘制更多，可在调用脚本时添加参数，如`python3 paintInsDependDist.py 100`。

&emsp;&emsp;感兴趣的同学还可以其他对常用的命令（如`pwd`、`clear`、`cat`等等），或者自己编写的程序进行插桩分析。

!!! hint "文件备份 :bulb:"
    &emsp;&emsp;若想将虚拟机中的文件拷贝到PC机，或将PC机的文件拷贝进虚拟机，请参考[VirtualBox虚拟机共享文件设置](../../vm)。
