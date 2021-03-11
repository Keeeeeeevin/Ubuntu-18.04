## 1. ROS Installation

### 1.1. 安装
* 按照官网步骤安装Ubuntu对应ROS版本，例如Melodic。
* 注意：安装前，配置Ubuntu的“Software & Updates"，以允许安装来自"main", “restricted” ，“universe，” and “multiverse”的软件。
* 注意：采用默认方式设置sources.list安装特别慢，选择Mirrors中的版本代替！

### 1.2. 问题一
* 安装完ROS后，初始化指令sudo rosdep init失败，出现下列问题: ERROR: cannot download default sources list from: https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/sources.list.d/20-default.list
* 解决办法：修改/etc/hosts
```html
# 打开hosts文件
sudo gedit /etc/hosts

# 在文件末尾添加
151.101.84.133 raw.githubusercontent.com（20210311失败）
或：
185.199.109.133 raw.githubusercontent.com（20210311成功，联通热点，查询ip/域名的网站https://site.ip138.com）

# 保存后退出再尝试（可能需要尝试很多遍才能成功），或者换wifi（移动/联通也有差别！）方式。
```

### 1.3. 问题二
* 安装完ROS后，初始化指令sudo rosdep init失败，提示: rosdep:command not found，按照以下方式解决：
```html
sudo apt install rospack-tools
```

