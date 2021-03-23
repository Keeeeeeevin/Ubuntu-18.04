# 系统备份
----------

1. 获取管理员权限
```html
sudo su
```

2. 更改目录
```html
cd /home/kevin
```

3. 压缩备份
```html
tar -cvpzf ubuntu_backup@`date +%Y-%m-%d`.tar.gz --exclude=ubuntu_backup@`date +%Y-%m-%d`.tar.gz --exclude=/proc --exclude=/tmp --exclude=/boot --exclude=/lost+found --exclude=/media --exclude=/mnt --exclude=/run /
```
* 参数说明： 
  * -c： 新建一个备份文档 
  * -v： 显示详细信息 
  * -p： 保存权限，并应用到所有文件 
  * -z： 用gzip压缩备份文档，减小空间 
  * -f： 指定备份文件的路径 
  * -exclude： 排除指定目录，不进行备份



----------
# 系统还原
----------

1. 进入ubuntu启动盘系统

2. 进入试用的Ubuntu启动盘系统，获取root权限
```html
sudo su
```

3.挂载

* 切换目录
```html
cd /
```

* 在根目录下新建一个文件夹，如backup，用来挂载系统硬盘。
```html
mkdir backup
```

* 挂载
```html
mount /dev/sda_Num /backup
```
  * 其中/dev/sda_Num可能是sda0,sda1...如果固态硬盘可能是/dev/nvme_Num，可使用fdisk -l查看硬盘号

4. 进入backup文件夹查看是否挂载成功，若挂载成功，应有对应文件。

5. 记录新硬盘的UUID号（如果是迁移到新硬盘一定要做）
```html
cd /backup/etc/
gedit fstab
```
UUID号在fstab里面，例如可能有四个UUID号，/swap,/,/boot/efi,/home，等下用于替换备份压缩包中的UUID

6. 将备份的压缩包解压到backup里，将替换掉原来的文件
```html
tar -xvpzf /media/myDisk/ubuntu_boot_backup@2019-10-1.tar.gz -C /backup
```

7. 打开fstab修改UUID号
```html
cd /backup/etc/
gedit fstab
```

参考：：https://blog.csdn.net/Netooo/article/details/86618279
