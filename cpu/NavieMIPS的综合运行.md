## 1. 综合运行

使用如下命令，从GitHub上将Naive MIPS的主分支brd-NSCSCC下载下来\([https://github.com/z4yx/NaiveMIPS-HDL](https://github.com/z4yx/NaiveMIPS-HDL)\)

```
git clone https://github.com/z4yx/NaiveMIPS-HDL
```

使用vivado打开目录下的NaiveMIPS-HDL-brd-NSCSCC\xilinx\NaiveMIPS\PrjVivao.xpr 工程，

综合运行并生成bit流文件，将bit流文件下载至FPGA中

## 2. 加载测试

使用NaiveMIPS-HDL-brd-NSCSCC\utility\serial\_load.py 的python程序测试该cpu。

NaiveMIPS使用NavieBootLoader作为其一级引导程序，已经初始化至cpu的片上内存，故可以直接用 serial\_load.py程序通过串口进行交互，其中a该程序用python2编写，需要预先安装 pyserial，pyelftools，tqdm的python库。

进入程序目录，输入如下命令查看程序使用

```
./serial_load.py -h
```

输入如下命令，测试串口线和内存

```
./serial_load.py -s /dev/ttyUSB0 -t ram
```



