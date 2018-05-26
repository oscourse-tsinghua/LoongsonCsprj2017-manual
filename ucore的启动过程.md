1. 下载源码并对源码进行移植（具体方法3.5ucore的移植过程）
2. 通过使用 NaiveMIPS-HDL-brd-NSCSCC\utility\serial\_load.py 的python程序直接将ucore映像加载至内存，输入如下命令：

   ```
           ./serial_load.py -s /dev/ttyUSB0 -l 文件绝对路径
   ```

此时是直接使用一级引导程序Naive MIPS启动，而没有涉及U-Boot

