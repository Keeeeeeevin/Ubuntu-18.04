## 1. 环境准备
----------

1. 更换 TX2 的 apt-get源，和电脑有区别（Jetpack4.4所刷的TX2系统为18.04.04）

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

2. 安装IDE: QtCreator
```html
sudo apt-get install qtcreator
sudo apt-get install qt5-default

# 安装Qt串口库：
sudo apt-get install libqt5serialport5-dev
```

## 2. Inmoov Wheel Control
----------
* 新建目录/home/kevin/QtProjects用于放置非ROS的QT工程

* 拷贝备份的InMoov_WheelControl.zip文件至Qt Projects目录，并解压，解压后的目录结构为：
```html
/home/kevin/QtProjects/Inmoov_WheelControl/Inmoov_WheelControl/InMoov_WheelControl.pro # 备注：Inmoov_WheelControl有两级，防止编译文件目录乱
```

* 编译并运行（2个轮毂电机驱动器通过USB转RS485与TX2连接，对应串口应该为：/dev/ttyUSB0）
备注：qt可能不具备打开串口权限，可尝试：sudo chmod 777 /dev/ttyUSB0


## 3. Qt ROS: https://blog.csdn.net/u013468614/article/details/88383558



