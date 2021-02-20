## 1. ROS Installation

* 按照官网步骤安装Ubuntu对应ROS版本，例如Melodic。
* 注意：采用默认方式设置sources.list安装特别慢，选择Mirrors中的版本代替！
* 安装完ROS后，初始化指令sudo rosdep init失败，提示: rosdep:command not found，按照以下方式解决：
```html
sudo apt install rospack-tools
```

* 安装完ROS后，初始化指令sudo rosdep init失败，出现下列问题: ERROR: cannot download default sources list from: https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/sources.list.d/20-default.list
```html
# 打开hosts文件
sudo gedit /etc/hosts
# 在文件末尾添加
151.101.84.133 raw.githubusercontent.com
# 保存后退出再尝试
```

