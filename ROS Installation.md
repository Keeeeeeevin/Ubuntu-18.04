（1） 安装完ROS后，初始化指令sudo rosdep init失败，提示: sudo: rosdep:command not found
按照以下方式解决：

sudo apt install rospack-tools


（2）在sudo 
ERROR: cannot download default sources list from: https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/sources.list.d/20-default.list

(base) kevin@kevin-Precision:~$ sudo rosdep init
ERROR: cannot download default sources list from:
https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/sources.list.d/20-default.list
Website may be down.



#打开hosts文件
sudo gedit /etc/hosts
#在文件末尾添加
151.101.84.133 raw.githubusercontent.com
#保存后退出再尝试

duoshijici,yekeyichenggongde jihui
换网络
关闭VPN？
