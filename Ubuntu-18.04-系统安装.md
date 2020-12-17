# 安装配置Win10+Ubuntu18.04.05双系统（单硬盘）
------

## 1. Win10系统安装

#####1. Win10启动盘：用“大白菜”等软件制作WIN10系统启动U盘《Software01.Ubuntu 系统》。
#####2. 安装Win10系统：采用MBR分区方式，安装Win10操作系统。
#####3. 压缩卷：进入Win10系统，磁盘管理 --> 压缩卷（由“扩展分区”压缩）。

## 2. Ubuntu系统安装

1. Ubuntu系统下载：官网下载Ubuntu18.04.05，桌面版，64位系统《Software01.Ubuntu 系统》。
2. Ubuntu启动盘制作：
1  选择文件  打开  打开Ubuntu18.04.ISO（Precision安装Ubuntu18.04.05，前期版本不兼容）。
备注1：Ubuntu18.04.03 Kernel为5.0.0，Ubuntu18.04.01Kernel为4.15.0
2  选择启动  写入硬盘映像（最好RAW格式写入），硬盘驱动器选择插入的USB2.0 U盘，点击写入。
备注1：若选择U盘启动后，出现Failed to load ldlinux.c32这个问题，还可以使用“rufus”软件（已下载，《Software01.Ubuntu 系统》文件夹）写入镜像（可能不成功）。
备注2：若在Ubuntu的安装界面卡死，开机按ESC（或Shift键、或e键）退出图形安装界面，选择“Install Ubuntu”，选项找到“quiet splash - - -”改成“quiet splash acpi=off”，然后安装Ubuntu。
