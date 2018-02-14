本文介绍RAM的初始化文件（后缀名为.coe）

# 编译

编译和链接环境位于func\_test/soft/src/目录下，其中：

bin.lds 为链接脚本，bin.lds.S 为其源码；

convert 将编译生成的二进制可执行文件转换成存储器 RAM 的初始化配置文件，convert.c 为其源码；

Makefile 和 rules.make 规定了编译的配置信息和规则。

将编写好的程序（后缀名为.S）存入编译目录下，通过make命令编译为coe文件。



交叉编译最终生成可执行文件为 资源发布包目录下 func\_test/soft/ 下 main.bin 和 main.data

RAM 的初始化配置文件位于 资源发布包目录下 func\_test/soft/delay\_ram\_coe 和 func\_test/soft/nodelay\_ram\_coe 下。



# 注意

Makefile 对于两个测试部分（存储器访问 无随机延迟和有随机延迟）有分别对应的版本，默认版本是无随机延迟，因此

对于第 1 部分测试存储器无 随机延迟的make 命令为`make ` 或  `make ver=test_nodelay_ram`;

make clean 命令为 `make clean ` 或   `make clean ver=test_nodelay_ram`；

第2 部分测试存储器有随机延迟的make 命令为 `make ver=test_delay_ram`;

make clean 命令为 `make clean ver=test_delay_ram`，

```
需要加上版本的定义，否则就按照默认版本（无随机延迟）生成或 清除对应的文件。
```

