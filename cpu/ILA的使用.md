# ILA的使用方法

ILA是vivado下的一个DEBUG IP，类似于片上逻辑分析仪，可以实时抓取信号的数值。

1.  在block design中通过+键，输入ILA查找IP核，并将其添加至block design中

2.  双击ILA IP，出现如下界面![](/assets/ILA_initial.png)



 Monitor type 

 如果选择的是AXI，则监控的是整个AXI的信号，此时number of probes 无效

 如果选择的是Native，则监控的是单条信号线，number of probes指明监控的信号数



 Sample Data Depth 

 修改抓取信号的采样个数

 

 在probe\_portions 一页

 probe width 选择各分组信号的位宽![](/assets/ILA_monitorinterface.png)

 3. 将ILA的输入直接连入需要检测的信号线上，注意AXI也是直接连接到一条AXI线上，不是单独连到AXI adapter 的一个 slave中

 

 4.重新Generate block design，并综合运行生成bit流文件下板，仿真环境下 ILA并没有意义。

 

 5.下板后，在vivado中便会出现ILA检测的数据窗口及波形图。![](/assets/ILA_board.png) 直接点击运行，默认为自动检测触发，每有数据更新便会采样，但是每次采样都会覆盖之前的值。 ![](/assets/ILA_run.png)

 6. 也可以设置触发条件，当满足触发条件时，ILA采样，此时采样结束后需要重新点击按钮让ILA进入wait状态继续等待触发。![](/assets/ILA_trigger.png)

