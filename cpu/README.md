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



## 开发环境部署

## 代码编译过程

## CPU实现分析

## 基本功能测试使用分析

## 仿真环境下的功能测试

## FPGA开发板上的功能测试



