## 1. Linux系统下搭建

\(1\)安装tftp-server

```
sudo apt-get install tftpd-hpa

sudo apt-get install tftp-hpa（如果不需要客户端可以不安装）

tftp-hpa是客户端

tftpd-hpa是服务器端
```

\(2\)配置TFTP服务器

```
sudo vim /etc/default/tftpd-hpa

将原来的内容改为:

TFTP\_USERNAME="tftp"

TFTP\_ADDRESS="0.0.0.0:69"

TFTP\_DIRECTORY="tftp根目录"      \#服务器目录,需要设置权限为777,chomd 777

TFTP\_OPTIONS="-l -c -s"
```

\(3\)重新启动TFTP服务

```
sudo service tftpd-hpa restart
```

## 2. windows系统下搭建

Windows 下可以使用 tftpd32 软件进行搭建（位于目录 lab\_environment\_v1.00\tftp 下）。

\(1\) 双击打开应用程序 tftpd32：

![](/assets/tftp1.png)

其中 Current Directory 为 tftp 服务器的根目录，可以点击 Browse 进行更改。点击 Show Dir 可以查看 该根目录下的文件。

Server interfaces 为选择网卡作为 tftp 服务器的网络入口，可以下拉进行选择，示例中选择了有线网卡 接入的 IP：10.90.50.43。

\(2\) 至此，Windows 上的 tftp 服务器已正常开启了，但局域网里的其他设备还无法访问，需要关闭电脑上 的防火墙。

在控制面板中找到 Windows 防火墙，选择“打开或关闭 Windows 防火墙”:

![](/assets/tftp2.png)

选择关闭 Windows 防火墙即可。

![](/assets/tftp3.png)

这样同一局域网上的设备就可通过 tftp://10.90.50.43 访问电脑上搭建的 tftp 服务器了，可以从根目录下载文件，或上传文件到根目录下。

