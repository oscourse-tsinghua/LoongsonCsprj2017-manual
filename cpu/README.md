# 基于FPGA的MIPS32SCPU实现

[参考链接](https://github.com/xyongcn/LoongsonCsprj2017#mips32s-cpu及外设的参考实现)

## vivado开发环境

vivado2017.1版本（鉴于安装包比较大，不在此处提供连接，同学直接相互拷贝，辛成鑫U盘中有备份）

### **一. windows下的安装**

1.本安装包支持win7、10、Ubantu等系统（64位）。

2.解压后，在点击xsetup即可进入安装向导界面。

点击next

![](/assets/import.png)

点击所有Agree

![](/assets/import1.png)

进入版本选择

![](/assets/import2.png)

若选择WebPack版本，则为完全免费版本，有一些功能的限制，一共是15个G，建议选择System Edition或者是DesignEdition

![](/assets/import4.png)

接下来就一路默认就行

若选择其他版本，则会有更全的功能，大概25个G而且需要License

因为磁盘空间不足足我个人安装的是WebPack版本，若安装其他版本可以提供Licences

![](/assets/import5.png)

文件在群里有

3.安装后遇到的问题

vivado需要vc2015 redistributable packages，安装完成后第一次打开会要求安装，点击确认即可。

但是如果电脑上安装有vs2017，则会出现不兼容。

解决方法：

![](/assets/import6.png)

![](/assets/import7.png)

![](/assets/import8.png)

问题：

打开装完后的桌面vivado图标，会弹出Microsoft visual C++2015的一个界面，提示repair或者uinstall，无论点哪个都运行不了，而且之后会有运行超时的提示。

解决：

这个情况是C++库的问题，下一个DiretX Repair修复一下，C++redistribution，然后重启即可。

### 二. Ubantu系统下的安装

为了开发环境的统一，建议在Ubantu下面安装

1. Ubantu虚拟机的安装

首先下载Vmware WorkStation软件作为虚拟机运行的环境（需要双系统请自行安装），下载Ubantu16.04的镜像文件ubuntu-16.04.iso。

在Vmware中点击新建虚拟机

![](/assets/xu1)

点击下一步

![](/assets/xu2.png)

添加下载好的Ubantu镜像文件路径

![](/assets/xu3.png)

填写用户名和密码

此处建议将全名和用户名填写同一个名字，需要记住密码以登陆

![](/assets/xu4)

点击下一步，默认存储路径和虚拟机名称

![](/assets/xu5)

给虚拟机配置硬盘大小，至少分配50个G，80个G可以保证空间充足

![](/assets/xu7)

点击下一步至完成即可

等待虚拟即开机。

可以在虚拟机关机状态下进入虚拟机-&gt;设置，调节该虚拟机内存，硬盘和共享文件夹等配置

![](/assets/xu8)

1. vivado的安装

vivado压缩包大小为20个G上下，若硬盘大小足够，可以直接在Ubantu中通过浏览器进入wiki

![](/assets/xu9)

将下面17个压缩包下载到虚拟机中（默认在download文件夹中），还有一件事，走的不是内网，不要问我是怎么知道的。。。

![](/assets/xu10)

下载完成后，在download文件夹中点击右键，用terminal打开。

用命令"cat split-\* &gt; Xilinx\_Vivado\_SDK2017.31005\_1.tar.gz"

合并17个压缩包。

之后使用“tar zxvf 文件名.tar.gz”解压。

如何硬盘不够大或者没有流量，可以在其他渠道拷贝安装包（本人用在Mac解压后的安装包直接复制到虚拟机中也行，不知道windows的解压软件可不可以）。

注意：Ubantu不能随意复制东西到系统盘，建议复制文件到HOME根目录下。

进入到安装包目录中，右键使用Terminal打开。

![](/assets/xu11)

输入指令“chmod -R a+x \*” 获取安装权限，

输入指令“./xsetup”进入安装

![](/assets/xu12)

接下来的安装界面与windows界面安装相同，安装路径建议设置为home。

3.打开软件

安装完成后（默认装在opt文件夹之中）

在Terminal回到根目录，使用指令”sudo sh /opt/Xilinx/Vivado/2017.1/bin/vivado”打开软件。（小问题：/opt/Xilinx/Vivado/2017.1/bin/setupEnv.sh: \[\[: not found）

## 代码编译过程

### 一. 新建工程

打开Vivado软件，直接在欢迎界面点击Create New Project，或在开始菜单中选择File - New Project即可新建工程。

点击Next

  
![](/assets/new project.png)



输入工程名称和路径

![](/assets/new project1.png)



选择RTL Project，勾选Do not specify......（这样可以跳过添加源文件的步骤，源文件可以后面再添加）

![](/assets/new project6)

根据自己的开发板选择器件型号，可以直接通过型号进行搜索，例如龙芯开发板上的芯片型号为Artix-7 AC701。如果不了解或者暂时不写进开发板，可以随便选一个型号，后面需要的时候再修改



![](/assets/new project4)



点击Finish，项目新建完成



![](/assets/new project5)

### 

### 二. 添加Verilog设计文件（Design Source）

在Project Manager窗口中，选择Source子窗口，在空白处或任意文件夹上右击，选择Add Sources，或者点击上面的绿色加号图标



![](/assets/add_source1)

选择Add or Create Design Sources，点击Next。![](/assets/add_source2)

点击Create File按钮，弹出的小窗口中输入文件名，点击OK。![](/assets/add_source3)

可以一次性新建或添加多个文件，最后点击Finish。![](/assets/add_source4)

稍后会弹出定义模块的窗口，也就是刚刚添加的test文件。可以在这里设置test模块的输入输出端口；或者直接点击OK，稍后再自行编写。![](/assets/add_source5)

点击OK后，如果弹出下面窗口直接点击Yes。

test文件和对应的模块即创建完成，如图![](/assets/add_source6)

### 三. 添加Verilog仿真文件（Simulation Source）

操作和上一步添加Verilog设计文件基本一致，唯一的区别是选择Add or Create Simulation Sources。我们新建一个名为simu的仿真文件。![](/assets/add_simu1)

设计文件新建完成后，在Design Sources和Simulation Sources中都有，而仿真文件只会出现在Simulation Sources文件夹中。设计文件可以用于仿真，也可以用于最终烧写进开发板，而仿真文件仅用于仿真。

![](/assets/add_simu2)

### 四.  编写代码

打开test模块，编写代码实现一个简单的非门电路如下。

```
module test(
 input in,
 output out
);
assign out = ~in;
endmodule
```

### 五.  行为仿真（Behavioral Simulation）与Testbench

为了验证代码是否正确，可以对代码进行行为仿真。我们给上面的test模块输入端in接入一个时钟信号，则输出端out就会产生一个电平相反的时钟信号。

行为仿真时，输入信号可以使用Testbench编写。

如果直接修改test模块，在其中添加Testbench代码，再进行仿真，是一种不太正确的做法。因为test模块是设计文件，后面可能会直接烧写进板子。进行仿真时添加了Testbench代码，之后再烧写进板子又得删掉Testbench代码，这样容易出现错误，而且操作起来也比较麻烦。尤其是接口数量多，内部比较复杂的模块。

所以我们将Testbench代码全部写到仿真文件simu中，并在simu文件中调用test模块，从而进行仿真。

### 六.  编写仿真代码

在simu模块中编写代码如下。

```
module simu(
)
;
// testbench 时钟信号
reg clk = 0;
always #10 clk <= ~clk;
// 输出信号
wire out;
// 调用test模块
test mytest(clk, out);
endmodule
```

代码说明：

reg clk = 0声明了一个reg信号，并赋初值为0。

always \#10 clk &lt;= ~clk为testbench代码，让clk每隔10ns翻转一次，产生周期为20ns的时钟信号。

wire out声明了一个wire信号，用于连接到test模块的输出。

test mytest\(clk, out\)调用了前面写好的test模块，其中mytest是模块名称，这里的clk和out分别连接了mytest模块内部的in和out信号。这种写法类似于面向对象的编程语言中，对象的实例化，test为类名，而mytest为对象名称。同样，Verilog中调用模块时，可以实例化多个test对象。

更多Testbench的写法请上网搜索相关资料。

### 七.  行为仿真

右击simu模块，选择Set as Top，将simu模块设置为仿真时的顶层模块。顶层模块类似于C编程时的入口函数，即main函数。main\`函数可以调用其他子函数；类似的，顶层模块可以调用其他模块。![](/assets/simu1)



在Flow Navigator窗口中点击Run Simulation - Run Behavioral Simulation；或者在菜单中选择Flow - Run Simulation - Run Behavioral Simulation，即可启动行为仿真。![](/assets/simu2)

![](/assets/simu3)

稍后Behavioral Simulation窗口打开，即可看到输出的仿真波形。![](/assets/simu4)

### 八.  操作技巧

双击图中右侧的Untitled 2标签，可以最大化仿真波形窗口。在波形窗口按住Ctrl键并滚动鼠标滚轮，可以横向缩放波形；按住Shift并滚动鼠标滚轮，可以横向平移波形。

如图，可以看出clk为周期20ns的时钟信号，而out和clk的电平始终相反，即test模块中的非门工作正确。![](/assets/simu5)

在Behavioral Simulation窗口中的Scopes子窗口，根据模块调用关系选中mytest，在右侧的Objects窗口即可看到test模块中所有的信号（包括内部信号，即没有写到模块声明语句module\(a,b,c\)括号中的信号）。

右击信号，选择Add To Wave Window，可将波形添加到右侧的仿真波形窗口，保存仿真文件，再次仿真时就可以看到该信号的波形。![](/assets/simu6)

对于多位信号例如wire \[7:0\] p，默认使用二进制形式显示，可以根据需要修改。例如右击选择Radix - Unsigned Decimal即可设置为无符号十进制显示，如图。![](/assets/simu7)

## CPU实现分析

## 基本功能测试使用分析

## 仿真环境下的功能测试

## FPGA开发板上的功能测试



