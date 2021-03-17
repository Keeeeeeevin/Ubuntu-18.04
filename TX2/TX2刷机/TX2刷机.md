## 0. 官方参考
* 参考：https://developer.download.nvidia.cn/embedded/L4T/r32-3-1_Release_v1.0/jetson_tx2_developer_kit_user_guide.pdf?uN5pWxqTYt5ftJK9ec2lEuSipuLtszT_eLY_b3VwniaWW7ku1g2BuydQDY-pkQE-dE74xEajd-rkj7XImGcKrknQqnassUxF7N6NeDgX17PgVe7rbPw4OVN9Yri1EIX2IIwovm3bjcQan5IswSII_MEVg5SYG5ldcEwUFT5pc3Gpp3bXqPTReA
* 参考：jetson_tx2_developer_kit_user_guide.pdf

## 1. Host Computer 准备

* 在Host Computer上下载并安装SDK Manager（例如：sdkmanager_1.4.1-7402_amd64.deb，已下载）
* 安装方式
```html
sudo apt install ./sdkmanager_[version]-[build#]_amd64.deb 
```
* 启动方式
```html
sdkmanager
```

## 2. 连接Host Computer和TX2

* 官方说明
```html
• Connect an external HDMI display to the carrier board’s HDMI port.
• Connect a USB keyboard and mouse to a USB hub (not included) and connect the hub to the developer kit’s USB Type-A port.  (The USB Micro AB port will be needed for flashing.)
• Insert the Micro-B end of the included USB Micro-B to USB A cable to the carrier board’s USB Micro-AB port. Connect the other end to your Linux host computer.
• Connect the included AC adapter to the carrier board's power jack. Plug the AC adapter into an appropriately rated electrical outlet. 

Use only the supplied AC adapter, as it is appropriately rated for your device.
```

* 步骤： 
```html
• TX2 通过 HDMI 连接显示屏（可不必）
• TX2 通过 USB 分线器连接键盘、鼠标（可不必）
• TX2 通过 套件提供的（USB Micro-B to USB A cable）连接 TX2 和 Host Computer。
• TX2 供电（最好用官方原版供电线）
```

## 3. 使 TX2 进入 Recovery 模式

* 官方说明
```html
1. Connect the developer kit as described above. It must be powered off.
2. Press and hold down the Force Recovery button.
3. Press and hold down the Power button.
4. Release the Power button, then release the Force Recovery button.
```

* 步骤： 
```html
1. 确认关机状态
2. 按住Recovery键，并保持（Recovery键：4个按钮中，离 Power 键最近的那个按钮）
3. 按住Power键
4. 松开Power键后，再松开Recovery键
```

## 4. 利用SDK Manager，刷机
参考：https://docs.nvidia.com/sdk-manager/install-with-sdkm-jetson/index.html

* Step 01: 配置
```html
Product Category: Jetson
Hardware Configuration: Host Machine（建议不打勾，否则在Host Computer上也会安装一套软件）
Hardware Configuration: Target Hardware（选择TX2，打勾）
Jetpack 版本：选择Jetpack 4.4，详细信息（Cuda版本等）查询：https://developer.nvidia.com/jetpack-sdk-44-archive

上述设置完毕后，选择Continue。
```

* Step 02: 下载
```html
选择：download now install later（下载完毕后，在重新打开sdkmanager刷机）

SDK下载相关文件的默认路径为：/home/kevin/Downloads/nvidia/sdkm_downloads
备注：文件下载较慢，已备份Jetpack 4.4的相关文件，看以后是否可重复利用。
```

* Step 03: 安装
```html
1. Flashing...
2. Host Computer的SDK manager在Tx2中烧录完系统后，在TX2的屏幕上需要进行初始化设置，例如计算机名称(=kevin-tx2)、用户名(=kevin)、密码(=1)、键盘布局等；
3. 登入TX2系统，进入系统桌面（此时系统为纯净版，还未安装cuda\opencv等）；
4. Host Computer的SDK manager会弹出登录选项，输入刚才设置的tx2用户名和密码，然后Host Computer的SDK manager会继续在tx2中安装剩余软件；
```

* Step 04: Finalize Setup
```html

```

## 4. 基本软件

### 4.1. 中文输入法
* 搜狗拼音
```html
sudo apt-get install ibus-sunpinyin

# 在命令行执行ibus-setup，弹出ibus设置界面
ibus-setup

# 在设置界面的Input Method中，添加ibus-sunpinyin，或者Intelligent Pinyin（建议）输入法，即可

```

### 4.2. ZED驱动，参考前例
```html
ZED_SDK_Tegra_JP45_v3.4.2.run（for TX2 Jetpack4.5）
```

### 4.3. 不能登录github.com的问题
```html
# 1. 登录：https://www.ipaddress.com/
# 2. 查询 github.com，获取ip，例如“140.82.112.3 github.com”
# 3. 查询 github.global.ssl.fastly.net，获取ip，例如“199.232.69.194 github.global.ssl.fastly.net”
# 4. 修改 sudo gedit /etc/hosts，在文件末尾加上（IP需更新）：
140.82.112.3 github.com
199.232.69.194 github.global.ssl.fastly.net
```


