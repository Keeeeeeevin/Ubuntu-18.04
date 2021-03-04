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
* 将安装包在~/Software目录下解压，运行其中的~/Software/clion-2020.3.3/bin/clion.sh文件即可，需要chmod +x，赋予执行权限。
* 建立~/WS文件夹，复制ORB-SLAM3-master到~/WS文件夹；打开Clion，在Clion中打开~/WS/ORB_SLAM3-master/CMakeLists.txt文件（Open as Project），即可打开ORB-SLAM工程。

## 2.2. 编译
* 设置Clion的编译方式（默认编译方式有问题）：
```html
File-->Settings-->Build, Execution, Deployment-->Toolchains: 设置Name=kevin，CMake=cmake，Make=默认，C Compiler=/usr/bin/gcc，C++ Compiler=/usr/bin/g++，Debugger=/usr/bin/gdb，完成后Apply
File-->Settings-->Build, Execution, Deployment-->CMake: 设置Build type=Release，Toolchain=kevin，build options=-- -j 12
```
* 编译第三方库及解压词典（参考ORB-SLAM3给出的build.sh文件）：
```html
echo "Configuring and building Thirdparty/DBoW2 ..."

cd Thirdparty/DBoW2
mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
make -j

cd ../../g2o

echo "Configuring and building Thirdparty/g2o ..."

mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
make -j

cd ../../../

echo "Uncompress vocabulary ..."

cd Vocabulary
tar -xf ORBvoc.txt.tar.gz
cd ..

echo "Configuring and building ORB_SLAM3 ..."

mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
make -j
```
* 编译ORB-SLAM3
```html
Build-->Build All in "Release-kevin"
# 备注：若出现“the working directory doesn't exist clion”等问题，需重新CMake，即File-->Reload CMake Project，然后再Build即可。
```

* 运行（以双目惯性视觉SLAM为例）：
```html
Run-->Edit Configurations：设置Target=stereo_inertial_euroc，Executable=stereo_inertial_euroc，Program arguments为：

/home/kevin/ORB_SLAM3/Vocabulary/ORBvoc.txt /home/kevin/ORB_SLAM3/Examples/Stereo-Inertial/EuRoC.yaml /home/kevin/Documents/MH_05_difficult /home/kevin/ORB_SLAM3/Examples/Stereo-Inertial/EuRoC_TimeStamps/MH05.txt dataset-MH05_stereoi

# 上述数据集已下载，设置完成后，Run-->Run 'stereo_inertial_euroc'即可运行。
```

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

# 节点运行例子：
roslaunch zed_wrapper zedm.launch
# 更多应用参考对应github仓库
```
* 安装ZED ROS Examples：https://github.com/stereolabs/zed-ros-examples
```html
cd ~/catkin_zed/src
git clone https://github.com/stereolabs/zed-ros-examples.git
cd ../
rosdep install --from-paths src --ignore-src -r -y
catkin_make -DCMAKE_BUILD_TYPE=Release
source ./devel/setup.bash

# 节点运行例子：
roslaunch zed_display_rviz display_zed.launch
roslaunch zed_rtabmap_example zed_rtabmap.launch
# 更多应用参考对应github仓库
```


----------

