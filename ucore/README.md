# MIPS32S CPU上的ucore教学操作系统

[参考链接](https://github.com/xyongcn/LoongsonCsprj2017#龙芯fpga实验板上的ucore参考实现)

## 编译工具链介绍

在ucore的开发与调试中，使用GNU工具链，包括GNU make, GNU Binutils, GCC以及GDB。

[GNU make](/ucore/make.md)
[交叉编译工具链](/ucore/crosstools.md)

## SoC

[SoC\_up](https://github.com/xyongcn/LoongsonCsprj2017/blob/master/doc/soc_lite%E5%92%8Csoc_up%E4%BB%8B%E7%BB%8D_v0.01.pdf)

[NaiveMIPS (或nscscc)](https://github.com/z4yx/NaiveMIPS-HDL/blob/brd-NSCSCC/documentation/2017nscscc.pdf)

不同的SoC具有不同的硬件结构，因此编译完成的软件在不同SoC平台上并不通用。

## qemu模拟器

[QEMU](/ucore/qemu.md)

## ucore编译过程

[编译方法](/ucore/os_comp.md)

## ucore实现分析

## ucore测试用例分析

