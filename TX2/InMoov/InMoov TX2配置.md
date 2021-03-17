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


## 2. IDE 配置
----------

1. IDE QtCreator

* 命令行安装QtCreator
```html
sudo apt-get install qtcreator
sudo apt-get install qt5-default
```
* Qt串口库：
```html
sudo apt-get install libqt5serialport5-dev
```

2. IDE PyCharm/Clion:

* TX2上运行PyCharm和Clion需要java jdk
```html
sudo apt-get install oepnjdk-11-jdk

# 设置环境变量
gedit ~/.bashrc
# java 11
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-arm64
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
# end java 11
source ~/.bashrc

# 然后，按照之前的Clion和Pycharm例子配置即可。
```

3. QtCreator配置ROS：https://blog.csdn.net/u013468614/article/details/88383558

4. https://www.cnblogs.com/cslxiao/p/5125620.html


## 3. Inmoov Wheel Control
----------
* 新建目录/home/kevin/QtProjects用于放置非ROS的QT工程

* 拷贝备份的InMoov_WheelControl.zip文件至Qt Projects目录，并解压，解压后的目录结构为：
```html
/home/kevin/QtProjects/Inmoov_WheelControl/Inmoov_WheelControl/InMoov_WheelControl.pro # 备注：Inmoov_WheelControl有两级，防止编译文件目录乱
```

* 用QtCreator打开工程，编译并运行（2个轮毂电机驱动器通过USB转RS485与TX2连接，对应串口应该为：/dev/ttyUSB0）

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


## 4. Virtualenv

* 安装pip
```html
sudo apt-get install python3-pip
```

* 安装virtualenv
```html
sudo pip3 install virtualenv
mkdir ~/Environments & cd ~/Environments
virtualenv env_yolov5 --python=python3.6（系统中的python3.6）

# 进入虚拟环境
source env_yolov5/bin/activate

# 测试
pip3 list（应该只有少数几个包）
```

## 5. Yolov5

### 5.1. 依赖项配置

1. 依赖项pytorch
```html
# nvidia forum: https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-8-0-now-available/72048

sudo apt-get install python3-pip libopenblas-base libopenmpi-dev 
source ~/Environments/env_yolov5/bin/activate

pip3 install Cython
# numpy1.18.5需要building wheel，安装时间比较长（5min左右）
pip3 install numpy==1.18.5(注意，目前直接pip3 install numpy有问题，安装的是1.19.5版本，import后会出现illegal instruction (core dumped) 的问题)
# 安装 torch-1.7.0（已下载）
pip3 torch-1.7.0-cp36-cp36m-linux_aarch64.whl
```

2. 依赖项torchvision
```html
sudo apt-get install libjpeg-dev zlib1g-dev libpython3-dev libavcodec-dev libavformat-dev libswscale-dev
git clone --branch v0.8.1 https://github.com/pytorch/vision torchvision  # (torchvision版本和pytorch版本要对应，若不能下载，参考github不能登录的解决办法)
cd torchvision
export BUILD_VERSION=0.8.1  # where 0.x.0 is the torchvision version  
python3 setup.py install
```

3. 测试 pytorch 和 torchvision
```html
python3
import torch
import torchvision

# 若报错“No module named 'PIL' ”，则“pip3 install Pillow”安装对应库即可
print(torch.__version__)
print(torchvision.__version__)
```

4. 依赖项cv2
```html
# 搜索在系统中已经刷过的opencv
sudo find / -name cv2

# 建立软链接
ln -s /usr/lib/python3.6/dist-packages/cv2/python-3.6/cv2.cpython-36m-aarch64-linux-gnu.so ~/Environments/env_yolov5/lib/python3.6/site-packages/cv2.cpython-36m-aarch64-linux-gnu.so

# 测试
python3
import cv2
cv2.__version__
```

5. 其他依赖包
```html
pip3 install matplotlib（3.3.4，安装3.2.2版本失败）
pip3 install Pillow
pip3 install PyYAML==5.4.1
pip3 instlal scipy（默认 1.5.4）
pip3 install tensorboard（默认 2.4.1）
pip3 install tqdm（默认 4.59.0）

pip3 install seaborn(0.11.1)
pip3 install pandas(1.15.1)

pip3 install thop(0.0.31)
pip3 install pycocotools(2.0.2)
```

### 5.2. 命令行测试
```html
python detect.py --source data/images --weights yolov5s.pt --conf 0.25

# 如果出现：The 'opencv-python>=4.1.2' distribution was not found and is required by the application
# 解决办法：可暂时将requirements中的opencv项注释掉
```

### 5.3. Yolov5 in Pycharm

* 打开Pycharm IDE（可先配置.desktop文件，便于启动）
* 新建 Project
```html
1. 选择 + NewPorject
2. 项目目录可设定为/home/jp45/PycharmProjects/yolov5
3. Python解释器设定为virtualenv中配置好的python3.6，即：/home/jp45/Environments/env_yolov5/bin/python3.6
4. 去掉“Create a main.py welcome script”前的勾
5. 点击Create
6. 将备份的yolov5文件里的内容复制到项目文件夹/home/jp45/PycharmProjects/yolov5中即可
7. 运行项目主程序
```



