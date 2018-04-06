## 1. U-boot源码下载

https://github.com/z4yx/u-boot-naivemips.git



## 2. U-Boot介绍

U-Boot 是一个启动引导程序， 常见于嵌入式系统中， 用于引导 Linux 等操作系统。 在基于 NaiveMIPS 的 SoC 上，运行 U-Boot 引导程序，支持从 Flash、网络等来源加载 Linux、uCore 系 统镜像并引导。

在NaiveMIPS移植实验中，我们以U-Boot 作为二级引导程序，放置在外部存储设备中。CPU 复位后首先执行一 级引导程序（NaiveBootloader，转换为coe文件初始到NaiveMIPS RAM IP核中），一级引导程序将 U-Boot 复制到内存后，跳转到 U-Boot 入口地 址，U-Boot 开始运行。



## 3. 源码说明

该源码以U-Boot 2017.7 版本作为基础，添加 NaiveMIPS 平台和板级支持代码实现兼容。改动部分如下：

### 添加 SoC 平台

在源码树的 arch/mips 下面是整个 MIPS 平台的支持代码，我们需要向其中添加一款新的芯片， 即  NaiveMIPS。这主要涉及到修改或添加几个文件。

arch/mips/Kconfig 文件    添加 MACH\_NAIVEMIPS 选项，描述平台的基本特征 

arch/mips/Makefile 文件    针对 NaiveMIPS 平台的特定编译选项 

arch/mips/mach-naivemips/Kconfig  文件    NaiveMIPS 支持的电路板列表

NaiveMIPS 处理器与 MIPS32 Release1 规范最接近，因此在 MACH\_NAIVEMIPS 选项 中 select SUPPORTS\_CPU\_MIPS32\_R1，从而复用 U-Boot 中对于该规范已有的实现代码。

同时由于指令实现上的差异，在编译选项中，针对本平台添加代码  -mno-branch-likely 和-mdivide-breaks， 这是为了避免产生未实现的 Branch Likely 和 TEQ 指令。



### 添加板级描述

在新的 SoC 平台基础上进一步增加使用该平台的电路板描述和外设驱动描述。以大赛实验箱平台 为例，该步骤主要涉及到修改或添加几个文件。

arch/mips/mach-naivemips/Kconfig  文件   平台支持的电路板列表中添加 TARGET\_NAIVEMIPS\_NSCSCC

board/nscscc/Kconfig  文件     实验板的基本信息描述 

board/nscscc/FPGA-A7/ 文件    新建实验板的代码目录 

board/nscscc/FPGA-A7/Makefile  文件    实验板的代码编译控制文件 

board/nscscc/FPGA-A7/naivemips\_nscscc.c 文件   实验板初始化代码 

arch/mips/dts/naivemips\_nscscc.dts 文件   实验板设备树文件 

include/configs/naivemips\_nscscc.h  文件     实验板配置类宏定义 

configs/naivemips\_nscscc\_defconfig  文件   实验板默认配置



当用户在构建时，通过 menuconfig 工具选择实验板时，board/nscscc/FPGA-A7/目录下面 相关的代码将被编译，成为板级支持代码，负责电路板相关的初始化工作（如探测内存大小）。由 于实验板比较简单，初始化函数基本为留空。

dts 设备树文件和 include/configs/naivemips\_nscscc.h 头文件中的宏定义，共同提供了电 路板具体的硬件信息。这些信息包括外设的型号、地址映射、中断连接及存储空间等。头文件中还 包含默认的 U-Boot 环境变量，利用这些环境变量可以实现自动启动等功能，避免每次上电都要求 用户手工输入启动命令。



由于 U-Boot 具有高度的可裁剪性，内部大多数功能模块都可以在构建时通过 menuconfig 工 具进行选择。最适合龙芯实验板的选项保存到configs/naivemips\_nscscc\_defconfig   中， 故编译时无需再对源码或者配置文件做任何修改，便于新用户快速完成构建工作。



## 4. 编译环境配置

需要配置mips下的交叉编译环境，使用了 Ubuntu  16.04  操作系统提供的软件包“gcc-mipsel-linux-gnu”和“binutils-mipsel-linux-gnu”。

输入以下命令进行安装：

```
sudo apt-get install gcc-mipsel-linux-gnu  binutils-mipsel-linux-gnu
```



此外还要安装 dtc（设备树编译器）

```
sudo apt install device-tree-compiler
```



## 5. 编译U-Boot

进入顶层目录，用如下命令选择默认的构建配置选项并构建 U-Boot。

```
make CROSS_COMPILE=mipsel-linux-gnu- naivemips_nscscc_defconfig 
```



```
make CROSS_COMPILE=mipsel-linux-gnu-
```

在构建成功完成后，顶层目录中会生成 u-boot 文件，即为 ELF 格式的 U-Boot 程序，该文件 可以由 NaiveBootloader 运行。



