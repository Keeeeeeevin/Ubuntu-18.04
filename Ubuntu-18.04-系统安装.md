# 安装配置Win10+Ubuntu18.04.05双系统（单硬盘）
------

## 1. Win10系统安装

1. Win10启动盘：用“大白菜”等软件制作WIN10系统启动U盘《Software01.Ubuntu 系统》。

2. 安装Win10系统：采用MBR分区方式，安装Win10操作系统。

3. 压缩卷：进入Win10系统，磁盘管理 --> 压缩卷（由“扩展分区”压缩）。

## 2. Ubuntu系统安装

1. Ubuntu系统下载：官网下载Ubuntu18.04.05，桌面版，64位系统《Software01.Ubuntu 系统》。

2. 利用UltraISO制作Ubuntu启动盘：

* 选择文件-->打开-->打开Ubuntu18.04.05.ISO。
  * 备注1：Ubuntu18.04.01 Kernel为4.15.0，Ubuntu18.04.03Kernel为5.0.0。

* 选择启动-->写入硬盘映像（最好RAW格式写入），选择插入的USB2.0 U盘，点击写入。
  * 备注1：若选择U盘启动后，出现Failed to load ldlinux.c32问题，可以尝试使用“rufus”软件写入镜像《Software01.Ubuntu 系统》。
  * 备注2：若在安装界面卡死，开机按ESC（或Shift、e键）退出图形安装界面，选择“Install Ubuntu”，找到“quiet splash - - -”改成为“quiet splash acpi=off”，然后继续安装。

3. 禁用Secure Boot。

4. U盘启动，安装Ubuntu系统，过程中不联网，不安装第三方软件，语言选择English，安装类型选择“与Windows共存”即可。

5. 完成安装：
  ** 备注1：重启系统，首先进入Win10，界面无进入Ubuntu选项，则用“EasyBCD软件”重建引导。
  ** 备注2：重启系统，能够进入Ubuntu和Win10系统，但是Ubuntu系统界面卡死等，是Nvidia显卡问题，需独立显卡驱动。
