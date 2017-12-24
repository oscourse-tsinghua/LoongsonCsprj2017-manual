# 基于FPGA的MIPS32SCPU实现

[参考链接](https://github.com/xyongcn/LoongsonCsprj2017#mips32s-cpu及外设的参考实现)

## vivado开发环境

vivado2017.1版本（鉴于安装包比较大，不在此处提供连接，同学直接相互拷贝，辛成鑫U盘中有备份）

### **一. windows下的安装**

1.本安装包支持win7、10、Ubantu等系统（64位）。

2.解压后，在点击xsetup即可进入安装向导界面。

点击next

![](/assets/import.png)

点击所有Agree

![](/assets/import1.png)

进入版本选择

![](/assets/import2.png)

若选择WebPack版本，则为完全免费版本，有一些功能的限制，一共是15个G，建议选择System Edition或者是DesignEdition

![](/assets/import4.png)

接下来就一路默认就行

若选择其他版本，则会有更全的功能，大概25个G而且需要License

因为磁盘空间不足足我个人安装的是WebPack版本，若安装其他版本可以提供Licences

![](/assets/import5.png)

文件在群里有

3.安装后遇到的问题

vivado需要vc2015 redistributable packages，安装完成后第一次打开会要求安装，点击确认即可。

但是如果电脑上安装有vs2017，则会出现不兼容。

解决方法：

![](/assets/import6.png)

![](/assets/import7.png)

![](/assets/import8.png)

问题：

打开装完后的桌面vivado图标，会弹出Microsoft visual C++2015的一个界面，提示repair或者uinstall，无论点哪个都运行不了，而且之后会有运行超时的提示。

解决：

这个情况是C++库的问题，下一个DiretX Repair修复一下，C++redistribution，然后重启即可。

### 二. Ubantu系统下的安装

为了开发环境的统一，建议在Ubantu下面安装

1. Ubantu虚拟机的安装

首先下载Vmware WorkStation软件作为虚拟机运行的环境（需要双系统请自行安装），下载Ubantu16.04的镜像文件ubuntu-16.04.iso。

在Vmware中点击新建虚拟机

![](/assets/xu1)

点击下一步

![](/assets/xu2.png)

添加下载好的Ubantu镜像文件路径

![](/assets/xu3.png)

填写用户名和密码

此处建议将全名和用户名填写同一个名字，需要记住密码以登陆

![](/assets/xu4)

点击下一步，默认存储路径和虚拟机名称

![](/assets/xu5)

给虚拟机配置硬盘大小，至少分配50个G，80个G可以保证空间充足

![](/assets/xu7)

点击下一步至完成即可

等待虚拟即开机。

可以在虚拟机关机状态下进入虚拟机-&gt;设置，调节该虚拟机内存，硬盘和共享文件夹等配置

![](/assets/xu8)

1. vivado的安装

vivado压缩包大小为20个G上下，若硬盘大小足够，可以直接在Ubantu中通过浏览器进入wiki

![](/assets/xu9)

将下面17个压缩包下载到虚拟机中（默认在download文件夹中），还有一件事，走的不是内网，不要问我是怎么知道的。。。

![](/assets/xu10)

下载完成后，在download文件夹中点击右键，用terminal打开。

用命令"cat split-\* &gt; Xilinx\_Vivado\_SDK2017.31005\_1.tar.gz"

合并17个压缩包。

之后使用“tar zxvf 文件名.tar.gz”解压。

如何硬盘不够大或者没有流量，可以在其他渠道拷贝安装包（本人用在Mac解压后的安装包直接复制到虚拟机中也行，不知道windows的解压软件可不可以）。

注意：Ubantu不能随意复制东西到系统盘，建议复制文件到HOME根目录下。

进入到安装包目录中，右键使用Terminal打开。

![](/assets/xu11)

输入指令“chmod -R a+x \*” 获取安装权限，

输入指令“./xsetup”进入安装

![](/assets/xu12)

接下来的安装界面与windows界面安装相同，安装路径建议设置为home。

3.打开软件

安装完成后（默认装在opt文件夹之中）

在Terminal回到根目录，使用指令”sudo sh /opt/Xilinx/Vivado/2017.1/bin/vivado”打开软件。（小问题：/opt/Xilinx/Vivado/2017.1/bin/setupEnv.sh: \[\[: not found）

## 代码编译过程

### 一 . SIMULATION

1. 新建工程

打开Vivado软件，直接在欢迎界面点击Create New Project，或在开始菜单中选择File - New Project即可新建工程。![](/assets/new project.png)点击Next![](/assets/new project1.png)

![](/assets/new project2.png)

![](/assets/new project4)

![](/assets/new project5)

## CPU实现分析

## 基本功能测试使用分析

## 仿真环境下的功能测试

## FPGA开发板上的功能测试



