## 1. 环境准备
----------

1. 更换 TX2 的 apt-get源，amd架构的源和笔记本电脑有区别（Jetpack4.4所刷的TX2系统为18.04.04）

* 特别注意：Jetson TX2 的CPU是arm64架构，镜像路径不同，为xxx/ubuntu-ports/

* 清华 ARM64 架构 CPU 的 apt-get 源地址（选择18.04版本）：https://mirror.tuna.tsinghua.edu.cn/help/ubuntu-ports/
* 备份/etc/apt/sources.list

* 复制 ARM64 架构 CPU 的 apt-get 源的镜像地址到/etc/apt/sources.list文件
```html
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-proposed main restricted universe multiverse
```

* 最后执行
```html
sudo apt-get update  
```

2. 安装ROS：参考ROS wiki的步骤即可，遇到问题参考前例（若rosdep部分不成功，可跳过rosdep，问题好像也不大）。


3. 安装IDE: QtCreator
https://ros-qtc-plugin.readthedocs.io/en/latest/
```html
sudo apt-get install qtcreator
sudo apt-get install qt5-default

# 安装Qt串口库：
sudo apt-get install libqt5serialport5-dev
```

3. 安装IDE: Clion（jdk11）:https://blog.csdn.net/zt1091574181/article/details/88899668

4. QtCreator配置ROS：https://blog.csdn.net/u013468614/article/details/88383558

virtualenv 一定要sudointall，不然可能找不到virtualenv命令：bash virtualenv command not found

5. https://www.cnblogs.com/cslxiao/p/5125620.html


## 2. Inmoov Wheel Control
----------
* 新建目录/home/kevin/QtProjects用于放置非ROS的QT工程

* 拷贝备份的InMoov_WheelControl.zip文件至Qt Projects目录，并解压，解压后的目录结构为：
```html
/home/kevin/QtProjects/Inmoov_WheelControl/Inmoov_WheelControl/InMoov_WheelControl.pro # 备注：Inmoov_WheelControl有两级，防止编译文件目录乱
```

* 编译并运行（2个轮毂电机驱动器通过USB转RS485与TX2连接，对应串口应该为：/dev/ttyUSB0）

* 关于/dev/ttyUSB0权限的问题：
```html
# 初始状态，通过”ls -l /dev/ttyUSB0“，可查看其权限，为：/c/rw-/rw-/---/，即：所有者user(rw-=4+2+0)/群组group(rw-=4+2+0)/其他人other(---=0+0+0)
# QtCreator不具备读写该串口的权限，所以打不开（一定不要用管理员权限运行QtCreator，不然后面会很麻烦）

# 解决方式一（重启不失效）：
串口设备文件所在组（group）为”dialout“组，讲当前用户添加到”dialout“组，并重启即可，指令如下：
sudo gpasswd -a $USER dialout
其他设备，可以参照该方法，将用户添加到相应组即可。

# 解决方式二（临时，重启失效）：更改/dev/ttyUSB0的权限，使QtCreator也能读写：
sudo chmod 666 /dev/ttyUSB0
或者：
sudo chmod o+rw /dev/ttyUSB0
```

## 3. Inmoov Body Control


## 4. pip
sudo apt-get install python3-pip

## 4. virtualenv
```html
sudo apt-get install virtualenv
mkdir ~/Environments & cd ~/Environments
# --no-site-packages：安装到系统Python环境中的所有第三方包都不会复制过来，这样，我们就得到了一个不带任何第三方包的“干净”的Python运行环境。xitong python3.6
virtualenv --no-site-packages env_yolov5 --python=python3.6
source env_yolov5/bin/activate
pip3 list
```

## 4. Yolov5
1. pytorch
```html
nvidia forum:https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-8-0-now-available/72048
sudo apt-get install python3-pip libopenblas-base libopenmpi-dev 
source ~/Environments/env_yolov5/bin/activate
pip3 install Cython
pip3 install numpy torch-1.8.0-cp36-cp36m-linux_aarch64.whl
```
2. 


github shangbuqu cankao: xiugai /etc/hosts
cankao :https://www.cnblogs.com/xingxia/p/github_request_problem.html
