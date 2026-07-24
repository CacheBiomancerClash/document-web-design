---
sidebar_position: 4
split_by_h2: true
slug: /_split-source/terminal/rk3506-industrial-panel/OpenHarmony
unlisted: true
sidebar_label: OpenHarmony 系统 Wiki 教程
title: RK3506 工控屏 OpenHarmony 系统 Wiki 教程
---

# RK3506工控屏

旨在开发者快速熟悉使用该产品。

##  1. 硬件清单

- 贝启RK3506工控屏
- 12V DC电源
- 双头USB数据线
- 串口小板

## 2. PC环境配置

我们会提供驱动安装包以及烧录工具包、以及烧录固件

下载方式：联系我们公司客服获取

下载完成后解压。

### 	2.1 ADB工具安装

adb的工具在bin目录下，打开系统设置环境变量到该目录即可使用adb工具。

![image-20260703153900640](images/image-20260703153900640.png)

### 	2.2 RockChip烧录驱动安装

![image-20260703145838332](images/image-20260703154041402.png)

![image-20260724092920834](images/image-20260724092920834.png)

为避免驱动安装出现问题，请先点击驱动卸载，再点击驱动安装，驱动成功安装后，如下所示

![image-20260724092949352](images/image-20260724092949352.png)

### 	2.3 RockChip烧录软件安装

解压后直接运行即可（最好解压到全英文路径下面）

![image-20260724093008031](images/image-20260724093008031.png)

## 3. 固件烧录

用双头USB连接OTG口和PC的USB口

![image-20260703154437621](images/image-20260703154437621.webp)

如何进入烧写模式(Loader)

1. 按住设备的 recovery 键不要松开，然后按一下 reset 键系统复位，大约两秒后松开 recovery 键。
2. 连接完成OTG口后，上电设备，在PC的powershell输入 adb shell 进入到设备终端，执行`reboot loader`命令

这时候进入到烧录工具可以看到下面这个现象

![image-20260724093026455](images/image-20260724093026455.png)

导入配置

![image-20260724093142905](images/image-20260724093142905.png)

![image-20260724093206781](images/image-20260724093206781.png)

导入完成后需要根据列出的项名字选择对应的img文件

例如

![image-20260703155221333](images/image-20260724093218187.png)

选择完成后勾选点击执行岂可。

![image-20260703155312653](images/image-20260724093228554.png)

## 4. 基础功能验证

### 4.1 USB功能测试

![image-20260703174227425](images/image-20260724093244957.png)

插入U盘到USB-HOST，使用adb shell进入设备终端可以查看当前U盘是否被挂载

### 4.2 DODI功能测试

![image-20260703181123161](images/image-20260724093923678.png)

```
IN1节点 /sys/devices/platform/gpios_dido/DIN1
IN2节点 /sys/devices/platform/gpios_dido/DIN2
OUT1节点 /sys/devices/platform/gpios_dido/DOUT1
OUT2节点 /sys/devices/platform/gpios_dido/DOUT2

OUT1与OUT2 echo 1为输出高电平，echo 0 为输出低电平
参考命令 echo 1 > /sys/devices/platform/gpios_dido/DOUT1

IN1与IN2使用cat查看输入，返回值为0为输入低电平，返回值为1为输入高电平
参考命令 cat /sys/devices/platform/gpios_dido/DIN1
```

### 4.3 LED功能测试

![image-20260703181226587](images/image-20260724093941364.png)

```
LED1控制节点 /sys/class/leds/led-1/brightness
LED2控制节点 /sys/class/leds/led-2/brightness
LED3控制节点 /sys/class/leds/led-3/brightness
LED4控制节点 /sys/class/leds/led-4/brightness
节点echo 1 打开LED灯，echo 0 关闭LED灯
参考命令 echo 1 > /sys/class/leds/led-1/brightness
```

### 4.4 CAN功能测试

![image-20260724094002244](images/image-20260724094002244.png)

![image-20260724094022837](images/image-20260724094022837.png)

```
CAN测试方法
adb push ip /bin/
adb push cansend /bin/
adb push candump /bin/


adb shell
进入ADB命令行



执行ip 需要进入bin目录
./ip link set can1 down
./ip link set can0 down
./ip link set can0 type can bitrate 250000 sample-point 0.8 dbitrate 250000 sample-point 0.8 fd on
./ip link set can1 type can bitrate 250000 sample-point 0.8 dbitrate 250000 sample-point 0.8 fd on
./ip link set can1 up
./ip link set can0 up

can1发送
cansend can1 123#1122334455667788
can1接收
candump can1
```

### 4.5 GPS功能测试

![image-20260724094038783](images/image-20260724094038783.png)

```
执行microcom -s 115200 /dev/ttyS5 可以看见GPS报文
```

### 4.6 RS232 RS485

![image-20260724094054085](images/image-20260724094054085.png)

```
485_1 UART1 /dev/ttyS1   485_2 UART2 /dev/ttyS2
232_1 UART3 /dev/ttyS3   232_2 UART4 /dev.ttyS4
测试使用echo “xxxx” 发送 cat节点接收
```

## 5. OpenHarmony

### 5.1 HDC功能使用

1.编译hdc工具
编译命令：./build.sh --product-name ohos-sdk   #在源码根路径下执行获取路径：成功编译后，可在路径下./out/sdk/ohos-sdk/linux/toolchains/ #下载hdc_std到本地
注：可将hdc_std重命名为hdc
2.在系统环境变量里面配置hdc工具的路径
3.打开cmd，执行hdc -v，输出版本号（例如：Ver: 1.1.1e）hdc工具即可生效
4.执行hdc shell即可开启hdc终端

### 5.2 开机桌面显示

DC座子接12V电源，上电后显示开机logo与开机动画，最后进入桌面，进入桌面后如图所示

![image-20260703181516335](images/20260724092411_75_65.jpg)

### 5.3 WIFI功能使用

点击桌面上的setting hap，进入后点击WIFI选项，点击扫描即可扫描当前附件WIFI热点，点击WIFI名称进入输入密码，按回车输入完成进行连接

![image-20260703181516335](images/20260724092403_73_65.jpg)

### 5.4 视频播放

使用如下命令 替换视频路径，即可全屏播放视频， -r为调节视频播放方向，-c为播放后自动结束播放进程，-W和-H调整播放显示区域，区域参数可不变动
rkadk_player_test -i /user/xxxxxx.mp4 -v -a 0 -s 0 -I 1 -P 0 -l 1 -c 1 -r 90 -W 800 -H 1280

### 5.5 AB分区 OTA升级

以下为AB分区OTA升级命令，--image_url后指定ota升级镜像路径，添加--reboot升级完成后自动重启
updateEngine --partition=0x3EFC00 --image_url=/storage/update-ab.img --update --reboot
