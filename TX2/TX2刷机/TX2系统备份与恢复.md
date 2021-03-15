## 1. TX2系统备份

* 参考TX2刷机文档，连接Host Computer和TX2，并使TX2进入Recovery模式。
* 参考TX2刷机文档，在Host Computer上启动sdkmanager。

* 软件目录说明：
```html
1. sdkmanager 软件下载目录：/home/kevin/Downloads/nvidia/sdkm_downloads
2. sdkmanager 软件安装目录：/home/kevin/nvidia/nvidia_sdk/JetPack_4.5_Linux_JETSON_TX2/Linux_for_Tegra
```

* 进入Host PC 的 sdkmanager 软件安装目录。
```html
cd ~/nvidia/nvidia_sdk/JetPack_4.5_Linux_JETSON_TX2/Linux_for_Tegra
```

* 从TX2备份镜像到Host PC，时长大概30分钟。
```html
# 生成的备份文件 tx2_img_backup@XX_XX_XX.img 暂时放在 Linux_for_Tegra 文件夹
sudo ./flash.sh -r -k APP -G tx2_img_backup@`date +%Y-%m-%d`.img jetson-tx2 mmcblk0p1
```

## 2. TX2系统恢复

* 参考TX2刷机文档，连接Host Computer和TX2，并使TX2进入Recovery模式。
* 参考TX2刷机文档，在Host Computer上启动sdkmanager。

* 进入Host PC的Linux_for_Tegra目录
```html
cd ~/nvidia/nvidia_sdk/JetPack_4.5_Linux_JETSON_TX2/Linux_for_Tegra
```
* 替换镜像：
sudo mv tx2_img_backup@XX_XX_XX.img bootloader/system.img

* Host PC向TX2烧录镜像
```html
sudo ./flash.sh -r  jetson-tx2 mmcblk0p1
```

