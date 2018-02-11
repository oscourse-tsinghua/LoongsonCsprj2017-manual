此方法用于烧写程序至可插拔 flash中

原理：该方法是使用龙芯开源的 gs132 核搭建了一个小的 soc，该 soc 外设有串口、flash 芯片和指令数据 ram,该 soc 在 FPGA 上生成的 bit 流文件可实现通过串口在线编程 flash 芯片, 编程过程中，不需要拔下 flash 芯片，且速率达到 6KB/sec。 

 

 实现：

 1. 将目录ab\_environment\_v1.00\flash\_programmer\programmer\_by\_uart 下的programmer\_by\_uart.bit 文件通过vivado下载至FPGA

 

 2.通过串口线将FPGA连接至主机，通过串口软件ECOM或SecureCRT将PMON的二进制文件 （FPGA\_test\_v1.00\FPGA\_soc\_test目录下的gzrom.bin文件）烧录至flash中，其中波特率选为230400，连接正常后，根据提示，键盘输入x表示开始使用xmodem模式传输串口软件使用xmodem模式传输binary文件。等待传输完成,编程过程中，不需要拔下 flash 芯片。 

 

 （ ECOM和SecureCRT 软件位于lab\_environment\_v1.00\uart\_soft，可直接运行与windows环境下，Linux环境下使用 minicom工具）

