## 1. 安装Prerequisites
参考：https://developer.download.nvidia.cn/embedded/L4T/r32-3-1_Release_v1.0/jetson_tx2_developer_kit_user_guide.pdf?uN5pWxqTYt5ftJK9ec2lEuSipuLtszT_eLY_b3VwniaWW7ku1g2BuydQDY-pkQE-dE74xEajd-rkj7XImGcKrknQqnassUxF7N6NeDgX17PgVe7rbPw4OVN9Yri1EIX2IIwovm3bjcQan5IswSII_MEVg5SYG5ldcEwUFT5pc3Gpp3bXqPTReA
参考：

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

