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



1. 可能需要
2. sudo chmod 777 /dev/ttyUSB0

2. qt ros:https://blog.csdn.net/u013468614/article/details/88383558



