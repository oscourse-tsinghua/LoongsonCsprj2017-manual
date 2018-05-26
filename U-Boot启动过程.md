1. 下载U-Boot源程序并且编译（具体方法见3.3U-Boot的移植过程）
2. 通过使用 NaiveMIPS-HDL-brd-NSCSCC\utility\serial\_load.py 的python程序直接将U-Boot加载至内存

    输入如下命令，将编译的U-Boot程序加载至内存

```
        ./serial_load.py -s /dev/ttyUSB0 -l 文件绝对路径
```

3. 按下reset键启动U-Boot



