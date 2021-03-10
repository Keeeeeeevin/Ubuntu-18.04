## 0. 参考
* 参考：https://developer.download.nvidia.cn/embedded/L4T/r32-3-1_Release_v1.0/jetson_tx2_developer_kit_user_guide.pdf?uN5pWxqTYt5ftJK9ec2lEuSipuLtszT_eLY_b3VwniaWW7ku1g2BuydQDY-pkQE-dE74xEajd-rkj7XImGcKrknQqnassUxF7N6NeDgX17PgVe7rbPw4OVN9Yri1EIX2IIwovm3bjcQan5IswSII_MEVg5SYG5ldcEwUFT5pc3Gpp3bXqPTReA
* 参考：jetson_tx2_developer_kit_user_guide.pdf

## 1. Host Computer

### 1.1. 下载并安装 SDK Manager
* 在Host Computer上下载并安装SDK Manager（例如：sdkmanager_1.4.1-7402_amd64.deb，已下载）
* 安装方式
```html
sudo apt install ./sdkmanager_[version]-[build#]_amd64.deb 
```
* 启动方式
```html
sdkmanager
```

### 1.2. OpenCV 3.2.0
* ORB-SLAM3 Tested with OpenCV 3.2.0，特别注意系统中多个版本OpenCV冲突的问题。
* 安装OpenCV可参考官网说明：
```html
