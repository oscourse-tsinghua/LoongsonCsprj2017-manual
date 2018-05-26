###### 本节主要介绍通过龙芯提供的带TLB功能的cpu soc\_up和启动程序pmon，移植ucore操作系统龙芯实验箱的过程及方法

###### 过程如下：

\(1\) 烧写 PMON 文件（gzrom.bin）到可插拔 SPI flash 上。（具体方法见2.2.1flash的烧录）

\(2\) 下载 由sop\_up综合生成的 bit 流文件至龙芯FPGA中。

\(3\) 运行 PMON（具体方法见）。

\(4\) 搭建 tftp 服务器 Load 内核\(vmlinux\)。

\(5\) 启动内核。

本实验所需工具均位于wiki cpu部分中的第一届大赛资源包[http://os.cs.tsinghua.edu.cn/oscourse/project/LoongsonCsprj2017/bit/cpu](https://legacy.gitbook.com/book/oscourse-tsinghua/loongsoncsprj2017-manual/edit#)

