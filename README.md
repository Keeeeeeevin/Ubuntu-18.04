1 	安装Ubuntu
1.1 	电脑安装Win10+Ubuntu18.04.03双系统（Dell/Precision）
1：官方网站下载Ubuntu18.04.03桌面版，64位，（已下载，《Software01.Ubuntu 系统》文件夹）。
2：先用“大白菜”等软件制作WIN10系统的启动U盘（已下载，《Software01.Ubuntu 系统》文件夹）
3：采用传统的MBR分区方式，安装Win10操作系统。
4：在Win10操作系统下，磁盘管理  压缩卷（由“扩展分区”压缩）
5：用UltraISO制作Ubuntu系统的启动U盘，具体为：
1  选择文件  打开  打开Ubuntu18.04.ISO（Precision安装Ubuntu18.04.05，前期版本不兼容）。
备注1：Ubuntu18.04.03 Kernel为5.0.0，Ubuntu18.04.01Kernel为4.15.0
2  选择启动  写入硬盘映像（最好RAW格式写入），硬盘驱动器选择插入的USB2.0 U盘，点击写入。
备注1：若选择U盘启动后，出现Failed to load ldlinux.c32这个问题，还可以使用“rufus”软件（已下载，《Software01.Ubuntu 系统》文件夹）写入镜像（可能不成功）。
备注2：若在Ubuntu的安装界面卡死，开机按ESC（或Shift键、或e键）退出图形安装界面，选择“Install Ubuntu”，选项找到“quiet splash - - -”改成“quiet splash acpi=off”，然后安装Ubuntu。
6：选择U盘启动，安装Ubuntu系统，过程中：
1  安装过程不联网、不安装第三方软件，语言选择English（安装前禁用secure boot）。
2  “安装类型”选择“其他选项”，不要选择“与Windows共存”，并且按照以下分区：
1）/boot，ext4日志文件系统，200M 建议1G，否则升级内核可能空间不足
2）/swap，8G~16G，同内存相同即可
3）/， ext4日志文件系统，30~50G（勾选格式化）
4）/home，ext4日志文件系统，剩余空间（勾选格式化）
3  Device for Boot Loader保持默认即可（笔记本）。
备注：若主分区数量限制，也可建立逻辑分区安装Linux。
4  安装完毕
备注1：重启系统，首先进入Win10，界面无进入Ubuntu选项，则用“EasyBCD软件”重建引导。
备注2：重启系统，能够进入Ubuntu和Win10系统，但是Ubuntu系统界面卡死等，是Nvidia显卡问题，需独立显卡驱动。
7：系统默认选用独立显卡，但是Ubuntu核心对独显支持不够。需重新安装独显驱动，具体为：
1在Nvidia官网下载显卡驱动（已下载GTX 1050Ti驱动，《Software01.Ubuntu 系统》文件夹，但是利用.run文件安装驱动方式较繁琐，下面直接从软件源安装）。
备注：下载地址https://www.geforce.cn/drivers
2关闭nouveau，具体为：
1）在/etc/modprobe.d目录下创建blacklist-nouveau.conf文件，并修改权限：
cd /etc/modprobe.d
sudo touch blacklist-nouveau.conf
sudo chmod a+x blacklist-nouveau.conf
sudo gedit blacklist-nouveau.conf
2）打开的文件中输入：
blacklist nouveau
options nouveau modeset=0
3）保存、关闭文件，然后：
sudo update-initramfs -u
sudo reboot
4）执行下面命令，若无任何输出，则关闭nouveau成功。
lsmod | grep nouveau
3安装显卡驱动
1）如果原来通过apt-get方式安装过显卡驱动，则先按以下方式卸载：
sudo apt-get remove --purge nvidia*
sudo apt autoremove
sudo reboot
2）安装Nvidia显卡驱动：
sudo add-apt-repository ppa:graphics-drivers/ppa #添加ppa源
sudo apt update
ubuntu-drivers devices#列出可选驱动
sudo apt install nvidia-driver-430（Dell）
sudo apt install nvidia-driver-440（2080Ti）
sudo apt install nvidia-driver-440（RTX3000）
3）测试指令，若列出独立显卡信息，则安装成功
sudo reboot
nvidia-smi
