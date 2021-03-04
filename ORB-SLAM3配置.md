## 1. 安装Prerequisites

### 1.1. Pangolin
* ORB_SLAM3利用Pangolin进行界面显示和交互。  
* Pangolin Github仓库：https://github.com/stevenlovegrove/Pangolin
* 需要安装Pangolin仓库中的Required Dependencies。

### 1.2. OpenCV 3.4.13
* ORB-SLAM3 Tested with OpenCV 3.2.0，实测目前OpenCV3最新的OpenCV 3.4.13也能适配。
* OpenCV 3.4.13的安装可参考官网说明：
```html
https://docs.opencv.org/3.4.13/d7/d9f/tutorial_linux_install.html
https://docs.opencv.org/master/db/d05/tutorial_config_reference.html
```
* 备注：opencv 3.4.13和opencv-contrib 3.4.13已下载，不用再git clone。
```html
cd ~/<my_working_directory>
git clone https://github.com/opencv/opencv.git
git clone https://github.com/opencv/opencv_contrib.git
cd opencv-3.4.13
mkdir build & cd build
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local -OPENCV_EXTRA_MODULES_PATH=/home/kevin/Software/opencv_contrib-3.4.13/modules -WITH_CUDA=ON ..
make -j7
sudo make install
```

### 1.3. Eigen 3.3.7
* ORB-SLAM3 Tested with Eigen 3.1.0，实测Eigen 3.3.7也能适配。
```html
cd ~/<my_eigen_directory>
mkdir build & cd build
cmake ..
sudo make install
```
* 安装后，头文件安装在/usr/local/include/eigen3/

### 1.4. ROS
* 参考ROS Installation部分。

----------

# 2. ORB-SLAM3 Test

## 2.1. IDE Clion




----------

# 3. ORB-SLAM3 Test with ROS

## 3.1. ZED驱动
* 官网下载ZED SDK，选择Cuda对于版本：https://www.stereolabs.com/developers/release/
* 特别注意，严格确认Ubuntu所安装的Cuda和Cudnn版本：
```html
cat /usr/local/cuda/version.txt
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
```
* 安装ZED_SDK：
```html
chmod +x ZED_SDK_Ubuntu18_cuda10.0_v3.4.1.run
.ZED_SDK_Ubuntu18_cuda10.0_v3.4.1.run

# 安装完后，可运行例子：
cd /usr/local/zed/tools
./ZED_Depth_Viewer
```
* 安装ZED ROS Wrapper：https://github.com/stereolabs/zed-ros-wrapper
```html
mkdir -p ~/catkin_zed/src
cd ~/catkin_zed/src
git clone https://github.com/stereolabs/zed-ros-wrapper.git
cd ../
rosdep install --from-paths src --ignore-src -r -y
catkin_make -DCMAKE_BUILD_TYPE=Release
source ./devel/setup.bash

# 运行节点：
roslaunch zed_wrapper zedm.launch
# 更多应用参考对应github仓库
```



----------

