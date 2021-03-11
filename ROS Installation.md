## 1. ROS Installation

### 1.1. 安装
* 按照官网步骤安装Ubuntu对应ROS版本，例如Melodic。
* 注意：采用默认方式设置sources.list安装特别慢，选择Mirrors中的版本代替！

### 1.2. 问题一
* 安装完ROS后，初始化指令sudo rosdep init失败，提示: rosdep:command not found，按照以下方式解决：
```html
sudo apt install rospack-tools
```

### 1.3. 问题二
* 安装完ROS后，初始化指令sudo rosdep init失败，出现下列问题: ERROR: cannot download default sources list from: https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/sources.list.d/20-default.list
```html
# 打开hosts文件
sudo gedit /etc/hosts
# 在文件末尾添加
151.101.84.133 raw.githubusercontent.com
# 保存后退出再尝试（可能需要尝试几十遍才能成功），或者换wifi等方式。
```

