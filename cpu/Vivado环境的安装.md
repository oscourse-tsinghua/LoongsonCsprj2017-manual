安装版本为 Vivado2017.1版

官方下载地址：

[https://china.xilinx.com/support/download.html](https://china.xilinx.com/support/download.html)

为了方便参赛队，建立了一个网盘下载：

[http://pan.baidu.com/s/1miE408S](http://pan.baidu.com/s/1miE408S)   密码：nvgx

# 一. windows下的安装

1. 本安装包支持win7、10、Ubuntu等系统（64位）。
2. 解压后，在点击xsetup即可进入安装向导界面。

点击next

![](/assets/import.png)

点击所有Agree

![](/assets/import1.png)

进入版本选择

![](/assets/import2.png)

直接选择WebPack版本，为完全免费版本，虽然有功能的限制，但是完全满足竞赛要求，一共是15个G

![](/assets/import4.png)

接下来一路默认就行

若选择其他版本，则会有更全的功能，大概25个G而且需要License

3.安装后遇到的问题

3.1 vivado需要vc2015 redistributable packages，安装完成后第一次打开会要求安装，点击确认即可。

但是如果电脑上安装有vs2017，则会出现不兼容。

解决方法：

![](/assets/import6.png)

3.2 打开装完后的桌面vivado图标，会弹出Microsoft visual C++2015的一个界面，提示repair或者uinstall，无论点哪个都运行不了，而且之后会有运行超时的提示。

解决方法：

这个情况是C++库的问题，下一个DiretX Repair修复一下，C++redistribution，然后重启即可。

# 二. Ubuntu系统下的安装（方法一）

为了开发环境的统一，建议在Ubuntu下面安装

1.Ubuntu虚拟机的安装

首先下载Vmware WorkStation软件作为虚拟机运行的环境（需要双系统请自行安装），下载Ubuntu16.04的镜像文件ubuntu-16.04.iso。

在Vmware中点击新建虚拟机

![](/assets/xu1.png)

点击下一步

![](/assets/xu2.png)

添加下载好的Ubuntu镜像文件路径

![](/assets/xu3.png)

填写用户名和密码

![](/assets/xu4.png)

点击下一步，默认存储路径和虚拟机名称

![](/assets/xu5.png)

给虚拟机配置硬盘大小，至少分配50个G，80个G可以保证空间充足

![](/assets/xu6.png)

点击下一步至完成即可

等待虚拟即开机。

可以在虚拟机关机状态下进入虚拟机-&gt;设置，调节该虚拟机内存，硬盘和共享文件夹等配置



2.Vivado的安装

vivado压缩包大小为20个G上下，可以在Windows系统下解后再复制到虚拟机中，或在Ubuntu中使用命令 `tar -zxvf 压缩文件名.tar.gz解压`。

注意：Ubuntu不能随意复制东西到系统盘，建议复制文件到HOME根目录下。

进入到安装包目录中，右键使用Terminal打开。

输入命令`chmod -R a+x *`获取安装权限，

输入命令`./xsetup`进入安装

接下来的安装界面与windows界面安装相同，安装路径建议设置为home



3.打开软件

安装完成后（默认装在opt文件夹之中）

在Terminal回到根目录，使用指令"sudo sh /opt/Xilinx/Vivado/2017.1/bin/vivado”打开软件，或者进入到bin目录下，用“./vivado”命令打开

# 二. Ubuntu系统下的安装（方法二）

基于docker的开发环境部署

1.安装docker

	apt install docker.io

如果出现错误

    E:Unable to locate package docker.io

则先执行再安装

    sudo apt-get update

2.下载Vivado 2018.1的Docker下镜像（预留至少35GB）

    ./install-vivado-image.sh （该文件的下载[链接](https://github.com/z4yx/NaiveMIPS-HDL/install-vivado-image.sh)）

如果出现错误

    bash: ./install-vivado-image.sh: Permission denied

则先执行再下载

    chmod u+x install-vivado-image.sh

3.编译

    git clone https://github.com/z4yx/NaiveMIPS-HDL.git
	cd NaiveMIPS-HDL
	# building process takes about one hour
	docker run -ti --rm -v $PWD:/home/vivado/project vivado:2018.1 /opt/Xilinx/Vivado/2018.1/bin/vivado -mode tcl -source xilinx/NaiveMIPS/build.tcl xilinx/NaiveMIPS/PrjVivao.xpr

4.打开界面

    sudo docker run -ti --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v $PWD:/home/vivado/project vivado:2018.1

