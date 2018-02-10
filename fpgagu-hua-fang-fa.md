固化的流程：先将一个 bit 流文件转换为 mcs 文件，将 mcs 文件下载到板上一个 SPI flash 上。

1 生成 mcs 文件

首先，需要确保 FPGA 设计的 bit 流文件已经生成。但 bit 流文件是用于直接下载到 FPGA 芯片里的文 件，而不能下载到 flash 芯片里，因而需要转换为 mcs 文件。

在 ISE 工具里，可以再图形界面下点击选择就可生成 mcs 文件，但在 Vivado 工具里，生成 mcs 文件需 要在命令控制器（tcl console）里输入命令。如下图：

![](/assets/固化1)





上图中蓝色的为输入的命令，pwd 用于查看目录。随后使用 cd 命令进入 bit 流文件所在的目录。 

假设生成的 bit 流文件为 project\_11.bit，则输入命令串 write\_cfgmem -format mcs -interface spix1 -size

16 -loadbit "up 0 project\_11.bit" -file project\_11.mcs 即可生成 mcs 文件，如下图。

![](/assets/固化2)





其中命串里的 project\_11.bit 为待转换的 FPGA 设计的 bit 文件，project\_11.mcs 为生成的 mcs 文件名， 可以自定义。

上述命令，是先输入 cd 命令将目录切换到了 bit 流文件的目录，再使用 write\_cfgmem 命令将 bit 流文 件转换为 mcs 文件。这两步可以使用命令串 write\_cfgmem -format mcs -interface spix1 -size 16 -loadbit "up 0 F:/vivado\_test/project\_11/project\_11.runs/impl\_1/project\_11.bit" -file F:/vivado\_test/project\_11/project\_11.runs/impl\_1/project\_11.mcs 一步到位。这一命令串中明确指定了 bit 流 文件的目录，和生成的 mcs 文件的保存目录，bit 流文件的目录和文件名必须正确，但 mcs 文件的目录和文 件名可以自定义。



2 下载 mcs 文件

生成好 mcs 文件后，就需要将其下载到实验板上 SPI flash 上。

像下载 bit 流文件一样，打开 Vivado 工具里的“Open Hardwar Target”，连接设备。

![](/assets/固化3)





选中 xc7a200t 后，右键选择“Add Configuration Memory Device”，出现如下界面：

![](/assets/固化4)





在 search 栏 输 入 s25fl128 ， 出 现 两 个 可 选 的 芯 片 型 号 ： s25fl128sxxxxxx0-spi-x1\_x2\_x4 和 s25fl128sxxxxxx1-spi-x1\_x2\_x4。具体选择哪个型号的，需要与板上固定的 flash 芯片型号相同（看板上 flash 型号标识的末尾是 0 还是 1）。也可以两者先任选一个，如果后续编程 flash 失败，再回来选另一个型号的。

选好 flash 型号后，点击 OK，弹出如下窗口，询问是否现在编程 flash：



点击 OK，出现编程 flash 的界面：

![](/assets/固化5)





在 Configuration file 那栏选到 1 小节生成的 mcs 文件，如下图：



![](/assets/固化6)



点击 OK 即可。后续等待下载 mcs 到 flash 芯片完成即可，如下图：

![](/assets/固化7)





Flash 芯片会先进行擦除，在进行编写，完成后会提示 completed Successfully，如下图：



![](/assets/固化8)

此时烧写就完成了，需要将实验板断电重新上电，等待一段时间，就可发现固化进实验板的 FPGA 设 备自动加载完成了。



