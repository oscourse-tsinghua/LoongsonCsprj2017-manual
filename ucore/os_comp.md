# ucore 编译方法

## 编译 SoC\_up 上的 ucore

### 获取源码

源码位于[https://github.com/z4yx/ucore-thumips](https://github.com/z4yx/ucore-thumips)的for-ls232-soc\_up分支下。

### 根据环境修改编译选项

由于编译环境不同，需要在Makefile中更正选项。修改的选项有：

1. GCCPREFIX

将 GCCPREFIX 修改为适合交叉编译环境的值。如果使用的是Debian/Ubuntu系统提供的工具链，可以将其改为`mipsel-linux-gnu-`。

2. CFLAGS

向CFLAGS添加选项：
 1. `-fno-builtin-fprintf` 源代码中的部分函数与C标准库函数名称重合。GCC可能将这些函数优化为其他未实现的C库函数。若编译失败应检查是否有其他函数出现类似的现象。
 2. `-fno-pic -mno-abicalls -mno-shared` MIPS的ABI要求gp寄存器的值必须有效，导致内核以及用户态进程在初始化时因gp无效而出错。

3. 汇编器选项：

Makefile中没有为汇编器设置参数方便调整，需要找到这些部分并添加`-fno-pic -mno-abicalls -mno-shared`。

[Patch文件](https://gist.github.com/mentha/155733c3fd5ca81053b9165a44a9f011)

### 编译

在源代码目录执行`make`即可编译。`flash.img`即指向编译结果。
