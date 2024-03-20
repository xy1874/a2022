# VirtualBox虚拟机共享文件设置

## 1. 安装扩展包

!!! info "小提示 :mega:"
    &emsp;&emsp;实验室的VirtualBox工具已安装好扩展包，因此若使用实验室环境，可跳过本步骤。

&emsp;&emsp;首先，打开[VirtualBox官网的下载页](https://www.virtualbox.org/wiki/Downloads)，点击下载扩展包，如图1所示。

<center><img src="../vm.assets/1.png"></center>
<center>图1 下载扩展包</center>

&emsp;&emsp;然后，打开VirtualBox，依次点击 ***工具*** -> ***全局设定*** -> ***扩展*** -> ***添加新包***，选中下载好的扩展包，如图2所示。

<center><img src="../vm.assets/2.png" width = 600></center>
<center>图2 点击安装扩展包</center>

&emsp;&emsp;此时，将自动弹出VirtualBox许可声明窗口。用鼠标滚轮滑到最底下，点击“同意”按钮，如图3所示。

<center><img src="../vm.assets/3.png"></center>
<center>图3 VirtualBox许可声明</center>

## 2. 设置共享文件夹

&emsp;&emsp;扩展包安装完成后，在Windows主机的任意目录新建共享文件夹，如`D:\my_vm_share_folder\`。建议共享文件夹采用英文名且其中不含空格，以免出现bug。

&emsp;&emsp;选中虚拟机，并依次点击虚拟机的 ***设置*** -> ***共享文件夹*** -> ***添加共享文件夹***，然后在 ***共享文件夹路径*** 右侧点击下拉选择 ***其他***，如图4所示。

<center><img src="../vm.assets/4.png"></center>
<center>图4 添加共享文件夹</center>

&emsp;&emsp;点击其他后，在弹出的Windows文件资源管理器中选中所建的共享文件夹，并点击确认按钮，如图5所示。

<center><img src="../vm.assets/5.png" width = 380></center>
<center>图5 确认共享文件夹</center>

&emsp;&emsp;不妨把 ***共享文件夹名称*** 改短，方便后续操作，比如，将其改成“share”，如图6所示。

<center><img src="../vm.assets/6.png" width = 380></center>
<center>图6 更改共享文件夹别名</center>

&emsp;&emsp;现已设置完毕，只需不断点击“OK”按钮以退出虚拟机设置。

## 3. 安装增强功能

!!! info "小提示 :mega:"
    &emsp;&emsp;本实验提供的VirtualBox虚拟机已安装好增强功能，可跳过本步骤。

&emsp;&emsp;启动虚拟机，然后依次点击菜单 ***设备*** -> ***安装增强功能***，如图7所示。

<center><img src="../vm.assets/7.png" width = 400></center>
<center>图7 安装增强功能</center>

&emsp;&emsp;若弹出提示是否安装或是否运行的提示对话框，点击确认即可。随后，输入密码授权安装。

&emsp;&emsp;用户授权后，将自动弹出终端显示安装过程，直至安装完成，如图8所示。

<center><img src="../vm.assets/8.png" width = 580></center>
<center>图8 增强功能安装过程的提示信息</center>

&emsp;&emsp;按照终端的提示，按下回车键关闭终端，即完成增强功能的安装。接下来可继续下一步骤来挂载共享文件夹。若挂载失败，可先重启虚拟机再尝试，或翻看安装日志中是否有报错信息，以检查增强功能是否安装失败。

## 4. 访问共享文件夹

&emsp;&emsp;打开终端，输入`sudo mount –t vboxsf share /mnt`命令，然后输入root密码并回车，如图9所示。该命令将Windows下的共享文件夹挂载到虚拟机的`/mnt`目录。

<center><img src="../vm.assets/9.png" width = 430></center>
<center>图9 挂载共享文件夹</center>

&emsp;&emsp;挂载成功后，即可使用cp命令将所需的文件拷贝到`/mnt`目录，或使用GUI操作在主机和虚拟机之间拷贝文件。比如，将`~/wsp/test.c`文件拷贝到Windows，如图10所示。

<center><img src="../vm.assets/10.png"></center>
<center>图10 使用共享文件夹</center>
