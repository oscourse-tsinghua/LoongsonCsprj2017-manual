# Target triplet （目标三元组）

在准备交叉编译工具链时，需要确定目标机器的架构、环境，才能选择出合适的工具链配置。工具链的配置以Target triplet表示。其基本格式为：

```
machine-vendor-os
```

`machine`代表目标的架构，即目标所使用的CPU类型和配置。需要注意的是，同样的CPU，在不同字节序下工作时所对应的`machine`并不相同。具体可以参照[Installing GCC](https://gcc.gnu.org/install/specific.html)。常见的`machine`列表如下：

|CPU|machine|
|:-:|:-:|
|80386|i386|
|64位PC CPU|x86\_64或amd64|
|MIPS32 Big endian|mips|
|MIPS32 Little endian|mipsel|

`vendor`可以用来对工具链的架构进行细节的配置。当不需要深入配置时，可以将其设为`unknown`或直接省略。在ucore开发中，需要注意的是，如果使用的CPU没有浮点运算指令，`vendor`需要包括`softfloat`，避免工具链生成不支持的指令。

`os`指定目标的操作系统环境，一般包括系统名称以及基础C库名称。该项可以设定工具链的一些默认行为，例如目标文件格式和连接时自动使用的C库等。ucore运行不依赖于外部的软件，因此该项影响不大，可以直接设为常见的`linux-gnu`，也可以设为`ucore-elf`。

需要注意的是，对应同样的目标可能有多种不同的Target triplet。在选择具体的工具链时，只需要一种满足需要的即可。例如，在准备对应于MIPS32 Big endian算的ucore工具链时，可以选择如下的Target triplet:

* mips-softfloat-linux-gnueabi
* mipsel-linux-gnu (Debian/Ubuntu)

在Debian/Ubuntu下，可以直接执行`apt-get install binutils-mipsel-linux-gnu gcc-mipsel-linux-gnu`安装需要的工具。

# Binutils

[GNU Binutils](https://www.gnu.org/software/binutils/)包括汇编器、连接器以及许多用来操作目标文件的工具。常用的工具有：

|命令|说明|
|:-:|:-:|
|ld|GNU连接器|
|as|GNU汇编器|
|objdump|可以查看目标文件内容、进行反汇编等|
|objcopy|可以复制、修改目标文件|

使用交叉编译工具链时，需要在命令前附加目标的Target triplet，例如，要使用`mips-linux-gnu`的`as`，应该使用的命令为`mips-linux-gnu-as`。

## ld

GNU连接器，可以根据[连接器脚本](https://sourceware.org/binutils/docs/ld/Scripts.html#Scripts)进行连接。[文档](https://sourceware.org/binutils/docs/ld/)

* `ld`连接多个目标文件。作为参数的目标文件的先后顺序对连接结果有影响。
* 需要生成静态文件时，输入只能包括静态库（`.a`）或目标文件（`.o`)。

格式：`ld [选项] 目标文件`

常用选项：

* `-o <文件>` 设置输出文件
* `-L <目录>` 设置库搜索路径
* `-l <库名>` 在库搜索路径中寻找对应的库并加入链接
* `-T <脚本>` 使用自定义连接器脚本

## as

GNU汇编器，根据汇编程序产生目标文件。[文档](https://sourceware.org/binutils/docs/as/)

* `as`需要不含预处理的汇编程序（一般以`.s`作为文件名结尾）。如果需要进行预处理（文件名以`.S`结尾），可以使用`gcc`汇编或自行调用`cpp`预处理。
* 不同架构的汇编语言有不同的语法。

格式：`as [选项] [文件]`
当不指定输入文件时，as从标准输入读入源程序。

常用选项：

* -o <文件> 设置输出文件

## objdump

显示目标文件信息。[文档](https://sourceware.org/binutils/docs/binutils/objdump.html)

格式：`objdump [选项] [文件]`

当不制定输入文件时，默认使用`a.out`。

常用选项：

* `-h` 查看Section headers。列出目标文件内所有段的信息。
* `-t` 查看符号表。
* `-x` 查看所有区块信息。相当于`-a -f -h -p -r -t`（Archive信息、文件头、Section headers、文件格式特有的信息、重定位信息、符号表）。
* `-d` 反汇编所有可执行段。该操作会产生相当多的输出，一般应将输出重定向到文件或编辑器。
* `-j` 限定仅处理某个段。
* `-s` 输出段的内容。一般与`-j`连用。

## objcopy

修改目标文件。[文档](https://sourceware.org/binutils/docs/binutils/objcopy.html)

格式：`objcopy [选项] <输入> [输出]`

若不指定输出文件，将会直接删除输入文件并使用输入的文件名。

常用选项：

* `-I <bfdname>` 指定输入格式
* `-O <bfdname>` 指定输出格式
* `-j <段名>` 限定仅处理某个段

常见用法：

提取`.text`段：

```
$ objcopy -Obinary -j .text bootsect.elf bootsect.mbr
```

该命令从`bootsect.elf`中提取`.text`段（代码段），存入`bootsect.mbr`，对段的内容不进行改动。

# GCC

[GCC(GNU Compiler Collection)](https://gcc.gnu.org/)包括一系列编程语言的编译器以及语言的底层库。在ucore的开发中，仅使用其C编译器(gcc)。

与Binutils相同，使用交叉编译工具链时，需要在命令前附加目标的Target triplet。要使用`mips-linux-gnu`的`gcc`，应该使用的命令为`mips-linux-gnu-gcc`。

## gcc

GCC的C编译器。一般用来编译C程序，也可以进行连接或汇编带预处理的汇编语言。GCC所支持的选项根据不同版本有所不同。

格式：`gcc [选项] 输入`

常用选项：

* `-o <输出>` 指定输出文件名
* `-c` 仅进行编译而不连接
* `-I` 设定预处理include搜索路径
* `-Wall` 开启大部分警告
* `-std=<标准>` 设定C语言标准
* `-ffreestanding` 设定运行环境为`freestanding`，即无标准库

常见用法：

编译程序至目标文件：

```
$ gcc -std=c99 -I ../include -Wall -c -o mm.o mm.c
```

该命令编译`mm.c`，在`../include`以及系统头文件中搜索#include所引用的头文件，并且对大多数可能存在问题的地方提出警告，最终将结果输出至`mm.o`。
