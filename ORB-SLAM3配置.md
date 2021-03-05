## 1. 安装Prerequisites

### 1.1. Pangolin
* ORB_SLAM3利用Pangolin进行界面显示和交互。  
* Pangolin Github仓库：https://github.com/stevenlovegrove/Pangolin
* 需要安装Pangolin仓库中的Required Dependencies。

### 1.2. OpenCV 3.2.0
* ORB-SLAM3 Tested with OpenCV 3.2.0，特别注意系统中多个版本OpenCV冲突的问题。
* 安装OpenCV可参考官网说明：
```html
# 3.2.0
https://docs.opencv.org/3.2.0/d7/d9f/tutorial_linux_install.html

# OpenCV CMake configuration reference
https://docs.opencv.org/master/db/d05/tutorial_config_reference.html

# 3.4.13
https://docs.opencv.org/3.4.13/d7/d9f/tutorial_linux_install.html
```
* 备注：opencv 3.2.0和opencv-contrib 3.2.0已下载，不用再git clone。
```html
cd ~/Software
git clone https://github.com/opencv/opencv.git
git clone https://github.com/opencv/opencv_contrib.git
cd opencv-3.2.0
mkdir build & cd build
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local -OPENCV_EXTRA_MODULES_PATH=/home/kevin/Software/opencv_contrib-3.2.0/modules -WITH_CUDA=ON ..
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
File-->Settings-->Build, Execution, Deployment-->Toolchains: 选择Default，设置Name=Default，CMake=cmake(/usr/bin/cmake)，Make=Detected:/usr/bin/make，C Compiler=/usr/bin/gcc，C++ Compiler=/usr/bin/g++，Debugger=gdb(/usr/bin/gdb)，完成后Apply
File-->Settings-->Build, Execution, Deployment-->CMake: 选择Release，设置Name=Release，Build type=Release，Toolchain=Default，build options=-- -j 8
File-->Settings-->Build, Execution, Deployment-->CMake: 选择Debug，  设置Name=Debug，  Build type=Debug，  Toolchain=Default，build options=-- -j 8
```
* 编译第三方库及解压词典（参考ORB-SLAM3给出的build.sh文件，也可直接在Terminal中按照build.sh文件编译，而不采用IDE）：
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

## 2.3. 运行（Run）（以双目惯性视觉SLAM为例）：
```html
Run-->Edit Configurations：设置Target=stereo_inertial_euroc，Executable=stereo_inertial_euroc，Program arguments为：

/home/kevin/ORB_SLAM3/Vocabulary/ORBvoc.txt /home/kevin/ORB_SLAM3/Examples/Stereo-Inertial/EuRoC.yaml /home/kevin/Documents/MH_05_difficult /home/kevin/ORB_SLAM3/Examples/Stereo-Inertial/EuRoC_TimeStamps/MH05.txt dataset-MH05_stereoi

# 上述数据集已下载，设置完成后，Run-->Run 'stereo_inertial_euroc'即可运行。
```

## 2.4. 运行（Debug）：
```html
Run-->Debug 'stereo_inertial_euroc'
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

## 3.2. 方式1：Terminal下按照build_ros.sh编译并运行的方法
* 更改~/.bashrc，在其中加上以下内容并source，使得pakage目录在ROS_PACKAGE_PATH中：
```html
export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:/home/kevin/ORB_SLAM3/Examples/ROS
```
* 然后编译：
```html
chmod +x build_ros.sh
./build_ros.sh
```
* 参考：文件build_ros.sh内容：
```html
cd Examples/ROS/ORB_SLAM3
mkdir build
cd build
cmake .. -DROS_BUILD_TYPE=Release
make -j

# 备注，若cmake发现错误"ModuleNotFoundError: No Module Named 'rospkg'"，直接pip install rospkg即可。
```

* 运行：
```html
# Terminal 1（ROS Core）
roscore

# Terminal 2（package: ORB-SLAM3 --> node: Stereo_Inertial）
cd /home/kevin/ORB_SLAM3
rosrun ORB_SLAM3 Stereo_Inertial Vocabulary/ORBvoc.txt Examples/Stereo-Inertial/EuRoC.yaml true

# Terminal 3（rosbag play，Once ORB-SLAM3 has loaded the vocabulary, press space in the rosbag tab.）
rosbag play --pause V1_02_medium.bag /cam0/image_raw:=/camera/left/image_raw /cam1/image_raw:=/camera/right/image_raw /imu0:=/imu
```

## 3.2. 方式2：CLion中编译并运行的方法，需修改CMakeLists.txt文件和文档结构（推荐，可调式）
* CLion开发ROS程序的官方教程：
```html
https://www.jetbrains.com/help/clion/ros-setup-tutorial.html
```
 
```html
# Create and build a ROS workspace:
mkdir -p ~/ros_ws/src
cd ros_ws
catkin_make

# In the workspace, create a package:
cd src
catkin_create_pkg slam_pkg roscpp rospy std_msgs
```

* 将src/slam_pkg中的文档删除，替换为备份文档（文件结构变化，以便于编译、调试等），例如CMakeLists.txt文档变为：
```html
cmake_minimum_required(VERSION 2.8)
project(slam_pkg) # ZQ

IF(NOT CMAKE_BUILD_TYPE)
  SET(CMAKE_BUILD_TYPE Release)
ENDIF()

MESSAGE("Build type: " ${CMAKE_BUILD_TYPE})

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall   -O3")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall   -O3")
set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -march=native")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -march=native")

# set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wno-deprecated -O3 -march=native ")
# set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wno-deprecated -O3 -march=native")

# Check C++11 or C++0x support
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
   add_definitions(-DCOMPILEDWITHC11)
   message(STATUS "Using flag -std=c++11.")
elseif(COMPILER_SUPPORTS_CXX0X)
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
   add_definitions(-DCOMPILEDWITHC0X)
   message(STATUS "Using flag -std=c++0x.")
else()
   message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules)

find_package(OpenCV 3)
if(NOT OpenCV_FOUND)
   find_package(OpenCV 2.4.3 QUIET)
   if(NOT OpenCV_FOUND)
      message(FATAL_ERROR "OpenCV > 2.4.3 not found.")
   endif()
endif()

MESSAGE("OPENCV VERSION:")
MESSAGE(${OpenCV_VERSION})

find_package(Eigen3 3.1.0 REQUIRED)
find_package(Pangolin REQUIRED)

find_package(catkin REQUIRED COMPONENTS # ZQ
  cv_bridge
  dynamic_reconfigure
  geometry_msgs
  sensor_msgs
  std_msgs
  std_srvs
  message_generation
  message_filters
  roscpp
  rospy
  roslib
)

include_directories(
${PROJECT_SOURCE_DIR}
${PROJECT_SOURCE_DIR}/include
${PROJECT_SOURCE_DIR}/include/CameraModels
${PROJECT_SOURCE_DIR}/src # ZQ
${EIGEN3_INCLUDE_DIR}
${Pangolin_INCLUDE_DIRS}
)

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)

set(ORBSLAM3_SOURCE_FILES # ZQ
src/System.cc
src/Tracking.cc
src/LocalMapping.cc
src/LoopClosing.cc
src/ORBextractor.cc
src/ORBmatcher.cc
src/FrameDrawer.cc
src/Converter.cc
src/MapPoint.cc
src/KeyFrame.cc
src/Atlas.cc
src/Map.cc
src/MapDrawer.cc
src/Optimizer.cc
src/PnPsolver.cc
src/Frame.cc
src/KeyFrameDatabase.cc
src/Sim3Solver.cc
src/Initializer.cc
src/Viewer.cc
src/ImuTypes.cc
src/G2oTypes.cc
src/CameraModels/Pinhole.cpp
src/CameraModels/KannalaBrandt8.cpp
src/OptimizableTypes.cpp
src/MLPnPsolver.cpp
include/System.h
include/Tracking.h
include/LocalMapping.h
include/LoopClosing.h
include/ORBextractor.h
include/ORBmatcher.h
include/FrameDrawer.h
include/Converter.h
include/MapPoint.h
include/KeyFrame.h
include/Atlas.h
include/Map.h
include/MapDrawer.h
include/Optimizer.h
include/PnPsolver.h
include/Frame.h
include/KeyFrameDatabase.h
include/Sim3Solver.h
include/Initializer.h
include/Viewer.h
include/ImuTypes.h
include/G2oTypes.h
include/CameraModels/GeometricCamera.h
include/CameraModels/Pinhole.h
include/CameraModels/KannalaBrandt8.h
include/OptimizableTypes.h
include/MLPnPsolver.h
include/TwoViewReconstruction.h
src/TwoViewReconstruction.cc)

# ZQ add_subdirectory(Thirdparty/g2o)

set(LIBS # ZQ
        ${OpenCV_LIBS}
        ${EIGEN3_LIBS}
        ${Pangolin_LIBRARIES}
        ${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib/libDBoW2.so
        ${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o.so
        -lboost_serialization
        -lcrypto
)

include_directories( # ZQ
  ${catkin_INCLUDE_DIRS}
)

# Build examples

add_executable( # ZQ
    stereo_ros_inertial Examples/ros_stereo_inertial.cc ${ORBSLAM3_SOURCE_FILES})

target_link_libraries( # ZQ
    stereo_ros_inertial ${catkin_LIBRARIES} ${LIBS} )
```

# Source the workspace: 
cd ~/ros_ws
source ./devel/setup.bash

# launch CLion in the same terminal:
# 在source的terminal中，打开clion.sh
sh ~/Software/clion-2020.3.2/bin/clion.sh



```

----------

