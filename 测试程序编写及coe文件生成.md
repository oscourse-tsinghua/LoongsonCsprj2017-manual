本文介绍测试程序的编写格式（后缀名为 .s文件）即ram初始化文件（后缀名为.coe）

## 格式如下:

`.org 0x0    //指示程序从地址0x0开始`

`.global _start  //定义全局符号 _start`

`.set    noat     //允许自由使用全局符号$1`

`_start:`

```
ori $1, $0, 0x1100        //测试指令 ori
```

## 编译

编译和链接环境位于func\_test/soft/src/目录下，其中：

bin.lds 为链接脚本，bin.lds.S 为其源码；

convert 将编译生成的二进制可执行文件转换成存储器 RAM 的初始化配置文件，convert.c 为其源码；

Makefile 和 rules.make 规定了编译的配置信息和规则，

交叉编译最终生成可执行文件为 资源发布包目录下 func\_test/soft/ 下 main.bin 和 main.data ，

RAM 的初始化配置文件位于 资源发布包目录下 func\_test/soft/delay\_ram\_coe 和 func\_test/soft/nodelay\_ram\_coe 下。

Makefile 对于两个测试部分（存储器访问 无随机延迟和有随机延迟）有分别对应的版本，默认版本是无随机延迟，因此

对于第 1 部分测试存储器无 随机延迟的make 命令为“make”或“make ver=test\_nodelay\_ram”，make clean 命令为“make clean”或“make clean ver=test\_nodelay\_ram”；

第2 部分测试存储器有随机延迟的make 命令为“make ver=test\_delay\_ram”，make clean 命令为“make clean ver=test\_delay\_ram”，需要加上版本的定义，否则就按照默认版本（无随机延迟）生成或 清除对应的文件。

