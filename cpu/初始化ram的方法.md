vivado 提供相应IP，不需要用户自行编写verilog文件实现ram功能

## 1. 添加内核IP

点击Flow Navigator中的IP Catalog，打开窗口添加IP核

搜索 ram模块，出现如下

![](/assets/ram1.png)

### 选择distributed memory generator和block memorygenerator标准：

dram和bram差别：

```
1、bram 的输出须要时钟，dram在给出地址后既可输出数据。

2、bram有较大的存储空间。是fpga定制的ram资源；而dram是逻辑单元拼出来的。浪费LUT资源

3、dram使用更灵活方便些
```

补充：

在Xilinx Asynchronous FIFO CORE的使用时，有两种RAM可供选择，Block memory和Distributed memory。

区别在于，前者是使用FPGA中的整块双口RAM资源，而后者则是拼凑起FPGA中的查找表形成。

1、较大的存储应用，建议用bram；零星的小ram，一般就用dram。但这仅仅是个一般原则，详细的使用得看整个设计中资源的冗余度和性能要求

2、dram能够是纯组合逻辑，即给出地址立即出数据。也能够加上register变成有时钟的ram。而bram一定是有时钟的。

3、假设要产生大的FIFO或timing要求较高，就用BlockRAM。否则，就能够用Distributed RAM。

Block RAM是比较大块的RAM。即使用了它的一小部分，那么整个Block RAM就不能再用了。

所以。当要用的RAM是小的。时序要求不高的要用Distributed RAM。节省资源。

FPGA中的资源位置是固定的，比如bram就是一列一列分布的。这就可能造成用户逻辑和bram之间的route延时比较长。举个最简单的样例，在大规模FPGA中，假设用光全部的bram。性能通常会下降，甚至出现route不通的情况，就是这个原因。

## 2. 设置参数

Component Name：生成的IP核模块名

Depth：存储深度，即数据点数目

DataWidth：数据位宽，即每个数据点的位数

Memory Type：ROM，单口RAM，简化的双口RAM（一端读一端写），真双口RAM（两端都可读写）

若选择dram 出现如下

![](/assets/ram2.png)

![](/assets/ram3.png)

若选择bram 出现如下

![](/assets/ram4.png)

![](/assets/ram5.png)

## 3. ROM的初始化

使用coe文件（参考测试程序的编及coe文件的生成）可以给ROM输入初值，COE文件最后会生成MIF文件用于初始化ROM，若只用于仿真可以仅替换mif文件即可。

