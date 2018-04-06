# QEMU

[主页](https://www.qemu.org/)

QEMU可以用来模拟MIPS系统。通过修改其源代码可以模拟真实的硬件平台，并且配合GDB可以对操作系统进行除错。

## 编译

为使用QEMU模拟特殊的硬件平台，需要自行进行配置及编译。

### 获取源代码

从QEMU主页可以找到下载。

### 修改源代码添加硬件平台

根据[SoC\_up介绍](https://github.com/xyongcn/LoongsonCsprj2017/blob/master/doc/soc_lite%E5%92%8Csoc_up%E4%BB%8B%E7%BB%8D_v0.01.pdf)的结构以及地址空间分配，添加新的硬件平台。

[Patch文件](https://gist.github.com/mentha/bbc6044e6cc39e38bc583b06b6bf9393)

### 进行编译

准备一个编译文件夹，在其中执行源码目录下的configure进行配置，并根据情况关闭不需要的功能。模拟SoC\_up所需要的target为`mipsel-softmmu`。

配置完成后，执行`make`即可进行编译。编译完成的模拟器位于`mipsel-softmmu/qemu-system-mipsel`。

## 运行使用

具体的使用说明可以参考[QEMU的文档]()。需要模拟SoC\_up时，可使用如下的参数启动：

```
 $ ./mipsel-softmmu/qemu-system-mipsel -machine soc_up -serial stdio -kernel ../ucore-thumips/flash.img
```

## 配合GDB除错

QEMU的`-s`选项可以开启GDB的除错接口。如果需要在启动时暂停CPU，可以加上`-S`选项。

GDB可以使用`target remote <host>:<port>`命令连接GDB。
