# 一. 新建工程

打开Vivado软件，直接在欢迎界面点击Create New Project

或在开始菜单中选择File - New Project即可新建工程。

![](/assets/新建工程2.png)

点击Next

![](/assets/test1.png)

输入工程名称和路径

![](/assets/新建工程3.png)

选择RTL Project，勾选Do not specify......（这样可以跳过添加源文件的步骤，源文件可以后面再添加）

![](/assets/新建工程4.png)

根据自己的开发板选择器件型号，可以直接通过型号进行搜索，例如龙芯开发板上的芯片型号为Artix-7 AC701。如果不了解或者暂时不写进开发板，可以随便选一个型号，后面需要的时候再修改

![](/assets/新建工程5.png)

点击Finish，项目新建完成

# 二. 添加Verilog设计文件（Design Source）

在Project Manager窗口中，选择Source子窗口，在空白处或任意文件夹上右击，选择Add Sources，或者点击上面的绿色加号图标

选择Add or Create Design Sources

![](/assets/add_source1_1.png)

点击Next。

![](/assets/add_source1_3.png)

点击Create File按钮

![](/assets/add_source1_4.png)

弹出的小窗口中输入文件名，点击OK。

![](/assets/add_source1_5.png)

可以一次性新建或添加多个文件，最后点击Finish。

稍后会弹出定义模块的窗口，也就是刚刚添加的test文件。可以在这里设置test模块的输入输出端口；或者直接点击OK，稍后再自行编写。

![](/assets/add_source1_6.png)

点击OK后，如果弹出下面窗口直接点击Yes。

test文件和对应的模块即创建完成，如图

![](/assets/add_source1_7.png)

# 三. 添加Verilog仿真文件（Simulation Source）

操作和上一步添加Verilog设计文件基本一致，唯一的区别是选择Add or Create Simulation Sources。

我们新建一个名为simu的仿真文件。

设计文件新建完成后，在Design Sources和Simulation Sources中都有，而仿真文件只会出现在Simulation Sources文件夹中。设计文件可以用于仿真，也可以用于最终烧写进开发板，而仿真文件仅用于仿真。

# 四. 编写代码

打开test模块，编写代码实现一个简单的非门电路如下。

`module test(`

`input in,`

`output out`

`);`

`assign out = ~in;`

`endmodule`

# 五. 行为仿真（Behavioral Simulation）与Testbench

为了验证代码是否正确，可以对代码进行行为仿真。我们给上面的test模块输入端in接入一个时钟信号，则输出端out就会产生一个电平相反的时钟信号。

行为仿真时，输入信号可以使用Testbench编写。

如果直接修改test模块，在其中添加Testbench代码，再进行仿真，是一种不太正确的做法。因为test模块是设计文件，后面可能会直接烧写进板子。进行仿真时添加了Testbench代码，之后再烧写进板子又得删掉Testbench代码，这样容易出现错误，而且操作起来也比较麻烦。尤其是接口数量多，内部比较复杂的模块。

所以我们将Testbench代码全部写到仿真文件simu中，并在simu文件中调用test模块，从而进行仿真。

# 六. 编写仿真代码

在simu模块中编写代码如下。

`module simu(`

`)`

`;`

`// testbench 时钟信号`

`reg clk = 0;`

`always #10 clk <= ~clk;`

`// 输出信号`

`wire out;`

`// 调用test模块`

`test mytest(clk, out);`

```
endmodule
```

代码说明：

reg clk = 0声明了一个reg信号，并赋初值为0。

always \#10 clk &lt;= ~clk为testbench代码，让clk每隔10ns翻转一次，产生周期为20ns的时钟信号。

wire out声明了一个wire信号，用于连接到test模块的输出。

test mytest\(clk, out\)调用了前面写好的test模块，其中mytest是模块名称，这里的clk和out分别连接了mytest模块内部的in和out信号。这种写法类似于面向对象的编程语言中，对象的实例化，test为类名，而mytest为对象名称。同样，Verilog中调用模块时，可以实例化多个test对象。

更多Testbench的写法请上网搜索相关资料。

# 七. 行为仿真

右击simu模块，选择Set as Top

![](/assets/add_simu1_1.png)

将simu模块设置为仿真时的顶层模块。顶层模块类似于C编程时的入口函数，即main函数。main函数可以调用其他子函数；

类似的，顶层模块可以调用其他模块。

在Flow Navigator窗口中点击Run Simulation - Run Behavioral Simulation；或者在菜单中选择Flow - Run Simulation - Run Behavioral Simulation，即可启动行为仿真。

![](/assets/simu1_1.png)

稍后Behavioral Simulation窗口打开，即可看到输出的仿真波形。

![](/assets/simu4.png)

# 八. 操作技巧

双击图中右侧的Untitled 2标签，可以最大化仿真波形窗口。在波形窗口按住Ctrl键并滚动鼠标滚轮，可以横向缩放波形；

按住Shift并滚动鼠标滚轮，可以横向平移波形。

如图，可以看出clk为周期20ns的时钟信号，而out和clk的电平始终相反，即test模块中的非门工作正确。

![](/assets/simu5.png)

在Behavioral Simulation窗口中的Scopes子窗口，根据模块调用关系选中mytest，在右侧的Objects窗口即可看到test模块中所有的信号（包括内部信号，即没有写到模块声明语句module\(a,b,c\)括号中的信号）。

右击信号，选择Add To Wave Window，可将波形添加到右侧的仿真波形窗口，保存仿真文件，再次仿真时就可以看到该信号的波形。

![](/assets/simu6.png)

对于多位信号例如wire \[7:0\] p，默认使用二进制形式显示，可以根据需要修改。例如右击选择Radix - Unsigned Decimal即可设置为无符号十进制显示，如图。

![](/assets/simu7.png)

