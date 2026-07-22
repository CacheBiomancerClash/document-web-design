---
sidebar_position: 2
split_by_h2: true
unlisted: true
sidebar_label: Wiki 教程
title: Robo3588 机器人主板 Wiki 教程
---

# Robo3588机器人主板

旨在开发者快速熟悉使用该产品。

##  1. 硬件清单

- 贝启Robo3588机器人主板
- 12V DC电源
- USB Type-A 转 Type-C 数据线
- 串口小板

## 2. PC环境配置

我们会提供驱动安装包以及烧录工具包、以及烧录固件

下载方式：联系我们公司客服获取

下载完成后解压。

### 	2.1 ADB工具安装

hdc的工具在bin目录下，打开系统设置环境变量到该目录即可使用hdc工具。

![image-20260703153900640](./images/image-1.webp)

### 	2.2 RockChip烧录驱动安装

![image-20260703145838332](./images/image-20260703154041402.webp)

![image-20260703154138678](images/image-20260703154138678.webp)

为避免驱动安装出现问题，请先点击驱动卸载，再点击驱动安装，驱动成功安装后，如下所示

![image-20260703154211142](images/image-20260703154211142.webp)

### 	2.3 RockChip烧录软件安装

解压后直接运行即可（最好解压到全英文路径下面）

![image-20260703154224989](images/image-20260703154224989.webp)

## 3. 固件烧录

使用USB Type-A 转 Type-C 数据线连接设备OTG口和PC的USB口

![image-20260703154437621](images/image-2.webp)

如何进入烧写模式(Loader)

1. 按住设备的 recovery 键不要松开，然后按一下 reset 键系统复位，大约两秒后松开 recovery 键。
2. 连接完成OTG口后，上电设备，在PC的powershell输入 hdc shell 进入到设备终端，执行`reboot loader`命令

这时候进入到烧录工具可以看到下面这个现象

![image-20260703154928499](images/image-20260703154928499.webp)

导入配置

![image-20260703155038288](images/image-20260703155038288.webp)

![image-20260703155105076](images/image-20260703155105076.webp)

导入完成后需要根据列出的项名字选择对应的img文件

例如

![image-20260703155221333](images/image-20260703155221333.webp)

选择完成后勾选点击执行岂可。

![image-20260703155312653](images/image-27.webp)

## 4. 基础功能验证

### 4.1 USB功能测试


接上鼠标/键盘/U盘等常见usb设备能够正常使用

### 4.2 usb-otg

执行wim+r 输入cmd 打开终端，输入adb shell可以进入

![image-20260703155312653](images/image-3.webp)

![image-20260703155312653](images/image-4.webp)

### 4.4 BUZZER

进入hdc shell 界面(或者串口界面)执行以下命令可以实现蜂鸣器的开关
```
echo 0 > /sys/devices/platform/leds/leds/buzzer_gpio/brightness (关闭)
echo 0 > /sys/devices/platform/leds/leds/buzzer_gpio/brightness (开启)
```

### 4.5 hdmi-out

接hdmi外接屏，屏幕正常显示

![image-20260703155312653](images/image-5.webp)


### 4.6 有线网络

设备连接网线，进入hdc shell 界面(或者串口界面)执行以下命令可以看到ip，通过ping baidu.com 能够正常联网

```
ifconfig
ping baidu.com
```
![image-20260703155312653](images/image-6.webp)
![image-20260703155312653](images/image-7.webp)

### 4.7 CPU-FAN

接上风扇外设，能够正常转动

### 4.8 RTC

进入hdc shell 界面(或者串口界面)执行以下命令可以看到rtc时间

```
hwclock
```
![image-20260703155312653](images/image-8.webp)

### 4.9 485

设备通过485串口和上位机进行通信设备，对应的串口是/dev/ttyS7

![image-20260703155312653](images/image-9.webp)

### 4.10 232

设备通过232串口和上位机进行通信设备，对应的串口是/dev/ttyS0、/dev/ttyS5、/dev/ttyS9

![image-20260703155312653](images/image-10.webp)
![image-20260703155312653](images/image-11.webp)
![image-20260703155312653](images/image-12.webp)

### 4.11 wifi

在桌面打开设置，选择第一个点击进入WLAN进入选择所需网络连接，能够正常上网

![image-20260703155312653](images/image-13.webp)
![image-20260703155312653](images/image-14.webp)
![image-20260703155312653](images/image-15.webp)

### 4.12 bluetooth

在桌面打开设置，选择第二个蓝牙点击进入选择连接对应蓝牙设备，能够正常使用

![image-20260703155312653](images/image-16.webp)
![image-20260703155312653](images/image-17.webp)
![image-20260703155312653](images/image-18.webp)

### 4.13 nvme

设备接上nvme外设，在/dev/block下面能够识别到，并且通过mount，能够实现对nvme的读写

![image-20260703155312653](images/image-19.webp)

挂载和卸载命令

```
# cd dev/block/
# ls
by-name  loop5         mmcblk0p1   mmcblk0p15  mmcblk0p7  nvme0n1p3
loop0    loop6         mmcblk0p10  mmcblk0p2   mmcblk0p8  platform
loop1    loop7         mmcblk0p11  mmcblk0p3   mmcblk0p9  ram0
loop2    mmcblk0       mmcblk0p12  mmcblk0p4   nvme0n1    zram0
loop3    mmcblk0boot0  mmcblk0p13  mmcblk0p5   nvme0n1p1
loop4    mmcblk0boot1  mmcblk0p14  mmcblk0p6   nvme0n1p2
# mkdir test
#mount nvme0n1p1 test
# ls test/
EFI                           SKY1-EVB-PCIEX11.DTB
GRUB                          SKY1-EVB-PCIEX2.DTB
IMAGE                         SKY1-EVB-PCIEX4.DTB
ROOTFS.CPIO.GZ                SKY1-EVB-PCIEX8.DTB
SKY1-BATURA.DTB               SKY1-EVB-PCIE_WIDTH_X1.DTB
SKY1-CLOUDBOOK.DTB            SKY1-EVB-PCIE_WIDTH_X2.DTB
SKY1-CRB.DTB                  SKY1-EVB-PCIE_WIDTH_X4.DTB
SKY1-EMU-PM.DTB               SKY1-EVB-SOF-ALC5682-ALC1019.DTB
SKY1-EMU-SMP.DTB              SKY1-EVB-USB4_5.DTB
SKY1-EMU.DTB                  SKY1-EVB.DTB
SKY1-EVB-DISPLAY.DTB          SKY1-FPGA.DTB
SKY1-EVB-HDA-ALC256.DTB       SKY1-ORANGEPI-6-PLUS-40PIN-PWM.DTB
SKY1-EVB-ISO.DTB              SKY1-ORANGEPI-6-PLUS-40PIN.DTB
SKY1-EVB-ISP.DTB              SKY1-ORANGEPI-6-PLUS.DTB
SKY1-EVB-LT7911UXC-AUDIO.DTB  SKY1-ORION-O6-40PIN.DTB
SKY1-EVB-NPU-RESMEM.DTB       SKY1-ORION-O6.DTB
SKY1-EVB-PCIEX10.DTB          initrd.img-6.6.89-cix-build-generic
# umount test
```

### 4.14 NPU

设备接上RK1828外设，执行lspci能够识别设备

![image-20260703155312653](images/image-20.webp)
![image-20260703155312653](images/image-21.webp)


### 4.15 NPU-Fan

设备接上风扇能够正常运行

### 4.16 npu 代码测试

进入hdc shell 执行下列测试命令

```
vendor/bin/rknn_yolov5_deme
```
![image-20260703155312653](images/image-22.webp)

### 4.17 Audio

设备接上耳机或喇叭，点击桌面音乐图标播放音乐能够正常放声

![image-20260703155312653](images/image-23.webp)

通过命令安装对应测试hap，点击录音，录音文件能够正常播放

```
hdc install C:\Users\19037\Desktop\recorder.hap
```
![image-20260703155312653](images/image-24.webp)
![image-20260703155312653](images/image-25.webp)
![image-20260703155312653](images/image-26.webp)