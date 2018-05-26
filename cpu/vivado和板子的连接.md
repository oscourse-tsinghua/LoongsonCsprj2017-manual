打开hardware manager以后，想要连接板子却始终找不到，按照一下教程安装一下驱动

官方给的解决方案如下：

https://www.xilinx.com/support/answers/54381.html

这个方法其实我经过实测是可行的，但是具体的步骤要变，因为对应的目录变了。

我的操作系统是ubuntu 16.04 LTS ，IDE版本是Vivado 2017.1，官方给的方案应该是针对好几个版本以前的，才会出现目录对不上的情况

我的情况是



就像这样auto connect 找不到我的板子，我使用lsusb命令是可以看到我的usb信息的，说明ubuntu知道有这个usb存在，只是因为没有驱动导致跟vivado连不上。

那么我们根据官方的方法来看



Full sudo -s access

Run sudo -s

Go to&lt;Xilinx install&gt;/bin/\[lin\|lin64\] or common/bin/\[lin\|lin64\] in an installed area.

Copy the install\_script directory to /opt.

Run "./install\_drivers" in /opt/install\_script/install\_drivers.

Add windrvr6 read/write access by running "chmod 666 /dev/windrvr6".



第一步是进入root账户，对，不进入root是操作不了的。

第二步是进入vivado的安装文件下的bin/\[lin \| lin64\]

啊，对不起，我的bin目录下面没有这个文件夹。

怎么办呢

你看下一条，我们的任务是为了寻找一个叫做 “ install\_script ” 的文件夹，所以我们搜一下这个install\_script

然后就



找到了它，原来放在的目录不是bin，而是SDK的data。。。

继续第三步，复制到/opt

因为现在是root所以不用sudo，命令如下： 

```
cd ~/opt/pkg/vivado/SDK/2016.4/data/xicom/cable_drivers/lin64  
```

```
cp -i -r install_script /opt   
```



我的vivado安装在 ~/opt/pkg/vivado  你的自己改。因为版本不一样，后面也可能不一样，你也要记得按照你的版本的真实路径来。

然后复制完了，第四步

```
cd /opt/install_script/install_drivers  
```

```
./install_drivers  
```

重申，必须是root账户，就是说每一条sh指令前面必须是\#而不是@ 

到这里可以结束了，反正我可以找到我的板子了，第五步嘛。。。我找不到那个文件夹。。所以也就没法对它chmod了

说真的，我搜了全盘，没有找到名字是 “ wind ” 开头的。

不过能用了，管他呢。



