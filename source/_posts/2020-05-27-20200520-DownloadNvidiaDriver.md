---
title: ubuntu16.04下NVIDIA显卡安装
date: 2020-05-27 09:43:34
tags:
description: 自己台式机显卡总是会爆炸，因此在这里好好记录一下，以至于下次爆炸之后就不用百度了
---

# 显卡安装

实在受不了了每次显卡都会莫名其妙的爆炸，在这边稍稍记录一下安装显卡的过程。

其中，主要的教程参照[这篇博客](https://blog.csdn.net/xunan003/article/details/81665835)



## 禁用nouveau

1. 编辑文件blacklist.conf

```shell
sudo vim /etc/modprobe.d/blacklist.conf
```

2. 在文件最后加入以下部分

```
blacklist nouveau

options nouveau modeset=0
```

3. 更新并重启

```shell
sudo update-initramfs -u
reboot
```

4. 验证是否启用

```shell
lsmod | grep nouveau
```

没有输出的话说明已经禁用了

## 下载驱动

直接到NVIDIA官网下载对应显卡的驱动即可

## 安装驱动

1. 按ctrl+alt+f1进入命令行界面
2. 关闭图形界面

```shell
sudo service lightdm stop 
```

3. 卸载原有的驱动

```shell
sudo apt-get remove nvidia-* 
```

4. 给.run文件权限并安装，这里注意修改文件的路径

```shell
sudo ./NVIDIA-Linux-x86_64-396.18.run -no-x-check -no-nouveau-check -no-opengl-files
```

5. 安装过程中的选项

```
1. The distribution-provided pre-install script failed! Are you sure you want to continue? 选择 yes 继续。
2. Would you like to register the kernel module souces with DKMS? This will allow DKMS to automatically build a new module, if you install a different kernel later?  选择 No 继续。
3. Nvidia's 32-bit compatibility libraries? 选择 No 继续。
4. Would you like to run the nvidia-xconfigutility to automatically update your x configuration so that the NVIDIA x driver will be used when you restart x? Any pre-existing x confile will be backed up.  选择 Yes 继续

```

6. 挂载并检查驱动情况

```shell
modprobe nvidia
nvidia-smi
```

如果出现提示，则安装成功，reboot或者

```shell
sudo service lightdm start
```

