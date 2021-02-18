# 安装配置Win10+Ubuntu18.04.05双系统（单硬盘）
----------

## 1. Win10系统安装
----------

1. Win10启动盘：用“大白菜”等软件制作WIN10系统启动U盘《Software01.Ubuntu 系统》。

2. 安装Win10系统：采用MBR分区方式，安装Win10操作系统。

3. 压缩卷：进入Win10系统，磁盘管理 --> 压缩卷（由“扩展分区”压缩）。

## 2. Ubuntu系统安装
----------

1. Ubuntu系统下载：官网下载Ubuntu18.04.05，桌面版，64位系统《Software01.Ubuntu 系统》。

2. 利用UltraISO制作Ubuntu启动盘：

* 选择文件-->打开-->打开Ubuntu18.04.05.ISO。
  * 备注1：Ubuntu18.04.01 Kernel为4.15.0，Ubuntu18.04.03Kernel为5.0.0。

* 选择启动-->写入硬盘映像（最好RAW格式写入），选择插入的USB2.0 U盘，点击写入。
  * 备注1：若选择U盘启动后，出现Failed to load ldlinux.c32问题，可以尝试使用“rufus”软件写入镜像《Software01.Ubuntu 系统》。
  * 备注2：若在安装界面卡死，开机按ESC（或Shift、e键）退出图形安装界面，选择“Install Ubuntu”，找到“quiet splash - - -”改成为“quiet splash acpi=off”，然后继续安装。

3. 禁用Secure Boot。

4. U盘启动，安装Ubuntu系统，过程中不联网，不安装第三方软件，语言选择English，安装类型选择“与Windows共存”即可。

* 完成安装后重启计算机。
  * 备注1：重启系统，首先进入Win10，界面无进入Ubuntu选项，则用“EasyBCD软件”重建引导。
  * 备注2：重启系统，能够进入Ubuntu和Win10系统，但是Ubuntu系统界面卡死等，是Nvidia显卡问题，需独立显卡驱动。

## 3. Ubuntu系统Nidia显卡驱动
----------

1. 关闭nouveau，具体为：

* 在/etc/modprobe.d目录下创建blacklist-nouveau.conf文件，并修改权限：
```html
cd /etc/modprobe.d
sudo touch blacklist-nouveau.conf
sudo chmod a+x blacklist-nouveau.conf
sudo gedit blacklist-nouveau.conf
```

* 在打开的文件中输入：
```html
blacklist nouveau
options nouveau modeset=0
```

* 保存、关闭文件，然后执行：
```html
sudo update-initramfs -u
sudo reboot
```

* 执行以下命令，若无任何输出，则关闭nouveau成功。
```html
lsmod | grep nouveau
```

2. 安装显卡驱动

* 若原来通过apt-get方式安装过显卡驱动，则先按以下方式卸载：
```html
sudo apt-get remove --purge nvidia*
sudo apt autoremove
sudo reboot
```

* 安装显卡驱动：
```html
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
ubuntu-drivers devices #列出可选驱动
sudo apt install nvidia-driver-430（Dell）
sudo apt install nvidia-driver-440（2080Ti）
sudo apt install nvidia-driver-440（RTX3000）
```

* 测试指令，若列出独立显卡信息，则安装成功
```html
sudo reboot
nvidia-smi
```

## 4. 更换apt-get源
----------

* 清华apt-get源网址：https:://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/，选择18.04版本，按照官方网址操作：  
* 备份/etc/apt/sources.list  
* 将sources.list内容改为镜像网站中的内容  
* 最后执行
```html
sudo apt-get update  
```

## 5. 更换pip源(pip install)
----------
 
* 清华pip源网址：https://mirrors.tuna.tsinghua.edu.cn/help/pypi/




----------
# 安装配置Win10+Ubuntu18.04.05双系统（双硬盘，台式机）
----------

## 0. 基本同上，注意事项：
----------

* Boot采用UEFI模式。

* Windwos系统采用清华1903英文版，中文版激活失败。

* 安装系统时，显示器插在核显口，2080Ti暂无驱动，否则花屏。

* 若未安装网卡（最好安装一个，避免麻烦），可利用路由器将无线Wifi信号转为有线信号接入网络。电脑联网后，可选择安装USB转无线网卡驱动，具体为：
  * 插入usb转无线网卡，执行lsusb查看型号（例如RTL8812AU）
  * 执行命令安装驱动
```html
sudo apt-get update
sudo apt-get install rtl8812au-dkms
```
* 若UEFI的Security Boot开启，则需要在装完驱动重启时输入MOK，较繁琐。建议在安装显卡驱动前直接禁用Security Boot，2080Ti电脑的禁用Security Boot的方法为：进入Boot单击Advanced Mode双击Secure Boot双击Key Management双击Clear Secure Boot Keys查看Secure Boot状态为Disable保存后退出Boot。

* 安装完显卡驱动后，将显示线接入2080Ti独立显卡。若在启动过程出现PCIE Bus Error字样，则：
  * 启动时，在选择界面光标停留在Ubuntu上，按“e”键进入编辑模式
  * 在以linux开头的行的最后加上pcie_aspm=off，按ctrl+x便可顺利进入图形界面
  * 执行命令sudo gedit /etc/default/grub，编辑文本，在GRUB_CMDLINE_LINUX_DEFAULT行双引号内补充上pcie_aspm=off，并且执行sudo update-grub命令即可。

* 如果要删除原来安装的Ubuntu系统，操作如下：
  * 利用PE系统删除分区，以及Windows引导盘（约300M）中的/efi/ubuntu文件夹
  * 在Windows系统中安装Easy UEFI软件（已下载，《Software01.Ubuntu》文件夹），删除其中对应的Ubuntu启动项





