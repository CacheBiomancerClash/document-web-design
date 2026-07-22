---
sidebar_position: 2
split_by_h2: true
sidebar_label: OpenHarmony 用户手册
title: RK3576 工业控制主板 OpenHarmony 用户手册
---

# RK3576 工控板Openharmony系统用户手册

## 第一章 RK3576开发板介绍

### 1.1 RK3576开发板简介

- 本开发板基于瑞芯微 RK3576 高性能处理器设计，集成 CPU、GPU、NPU 等多元计算单元，具备 6.0 TOPs 算力，可满足工业控制、边缘计算、智能终端等场景的应用需求。板载资源丰富，提供双千兆网口、SFP 光口、HDMI 8K 输出、多种串口及扩展接口，支持 4G/5G 和 WiFi 6 等无线通信。

  本手册主要分为三部分：

  1. **产品整体概述**  

     简要介绍开发板接口资源、性能等。

  2. **环境搭建**

     介绍相关环境的安装以及烧录方法。

  3. **功能测试**  

     通过命令行对开发板的各项接口和系统功能进行测试。

---

## 第二章 烧写系统

RK3576 目前支持 OTG 烧写方式。在用户资料中提供了相应的烧写工具。

### 2.1 准备工作：安装相关驱动

驱动路径：

```
5-工具\DriverAssitant_v5.13.zip`
```

操作步骤：

1. 将 `DriverAssitant_v5.13.zip` 解压到任意目录；
2. 以管理员权限运行 `DriverInstall.exe`；
3. 为避免驱动安装异常，建议先点击 **驱动卸载**；
4. 再点击 **驱动安装**；
5. 出现安装成功提示后，表示驱动安装完成。

> 注意：驱动安装需要管理员权限。

***

<img src="../images/1.png" alt="1" style={{ zoom: '50%' }} />

为避免驱动安装出现问题，请先点击驱动卸载，再点击驱动安装。

<img src="../images/8.png" alt="8" style={{ zoom: '75%' }} />

驱动成功安装后如下所示：

<img src="../images/9.png" style={{ zoom: '90%' }} />

***

### 2.2 烧录操作：烧写固件到设备

固件和烧写工具：

~~~
固件：由贝启提供
烧写工具：RKDevTool_Release.zip
~~~

RKDevTool 是瑞芯微提供的开发工具。使用前建议将工具解压到**全英文路径**下。

烧写前准备：

1. 接上电源适配器，开发板上电；

2. 使用 Type-C 线连接开发板 `	TYPE-C` 口和电脑主机；

3. 按住开发板上的 `recovery` 键不要松开；
4. 按一下 `reset` 键，使系统复位；
5. RKDevTool 工具上会提示发现 `LOADER` 设备或 `MASKROM` 设备，再松开 `	recovery`按键。

> 注意：识别设备时，开发板上电过程中需要保持 `recovery` 按键按下状态。

> 注意：理论上 RKDevTool 解压目录可以随意，但如果工具打开后界面异常，建议将其解压到全英文目录下。

烧写步骤：

1. 打开 RKDevTool；

2. 确认工具显示：`发现一个 LOADER 设备` 或 `发现一个 MASKROM 设备`；

3. 清空工具所有项

   <img src="../images/11.png" alt="11" style={{ zoom: '75%' }} />

4. 清空完成后，点击 **导入配置**；

   <img src="../images/12.png" alt="12" style={{ zoom: '75%' }} />

5. 选择镜像目录下的 `config.cfg` 文件；

   <img src="../images/2.png" alt="2" style={{ zoom: '75%' }} />

   <img src="../images/3.png" alt="3" style={{ zoom: '75%' }} />

6. 依次选择工具所勾选的镜像文件；

   <img src="../images/4.png" alt="4" style={{ zoom: '75%' }} />

   <img src="../images/5.png" alt="5" style={{ zoom: '75%' }} />

   <img src="../images/6.png" alt="6" style={{ zoom: '75%' }} />

7. 点击 **执行** 按钮开始升级。

   <img src="../images/13.png" alt="13" style={{ zoom: '75%' }} />

   等待工具左侧显示烧录完成即可。

   注意：如果 rk3576 使用的是 hdmi 显示，需要将 resource 选择为 resource_hdmi.img ，boot选择为：boot_linux_hdmi.img，

---

## 第三章 RK3576 平台界面功能使用及测试

### 3.1 桌面功能测试

开发板启动后，进入系统桌面。可通过桌面图标和系统应用检查基础显示、触控或鼠标操作是否正常。

测试重点：

- 桌面是否正常显示；
- 应用图标是否正常加载；
- MIPI屏幕触摸操作是否正常；
- 应用是否能够正常打开;

开发板启动桌面显示如下：

<img src="../images/14.png" alt="桌面" style={{ zoom: '55%' }} />

***

### 3.2 WIFI 测试

> 说明：由于网络环境不同，进行 Wi-Fi 测试时需要根据实际网络情况进行设置。用户也可以使用手机热点进行测试。

测试步骤：

1. 打开系统 **设置**；

2. 进入 **WLAN**；

3. 打开 WLAN 开关；

4. 查看是否能够扫描到 Wi-Fi 设备；

5. 选择目标 Wi-Fi；

6. 输入密码并点击连接；

   <img src="../images/15.png" alt="15" style={{ zoom: '75%' }} />

7. 连接成功后，打开命令行窗口hdc shell 进入Openharmony系统后，进入系统后可使用如下命令查看网络信息：

   ```bash
   # ifconfig
   wlan0 	Linkencap:Ethernet HWaddrc0:f5:35:32:68:da Driverbcmsdh_sdmmc
           inetaddr:192.168.0.132 Bcast:192.168.0.255 Mask:255.255.255.0
           inet6addr: fe80::c2f5:35ff:fe32:68da/64Scope:Link
           UPBROADCASTRUNNINGMULTICAST MTU:1500 Metric:1
           RXpackets:411errors:0dropped:0overruns:0frame:0
           TXpackets:172errors:0dropped:0overruns:0carrier:0
           collisions:0txqueuelen:1000
   ```

***

### 3.3 网口测试

1. 开发板网口插上以太网；

2. 打开cmd 命令窗口 ，输入 hdc shell 进入系统

3. 使用命令查看网络信息：

   ```bash
   #ifconfig
   eth0 	Linkencap:Ethernet HWaddraa:78:e9:09:aa:c4 Driverrk_gmac-dwmac
           inetaddr:192.168.0.133 Bcast:192.168.0.255 Mask:255.255.255.0
           inet6addr: fe80::a878:e9ff:fe09:aac4/64Scope:Link
           UPBROADCASTRUNNINGMULTICAST MTU:1500 Metric:1
           RXpackets:59errors:0dropped:0overruns:0frame:0
           TXpackets:47errors:0dropped:0overruns:0carrier:0
           collisions:0txqueuelen:1000
   ```

网络连通性测试：

```bash
ping qq.com -c 3
```

测试结果：

```text
PING qq.com(123.150.76.218)56(84) bytes of data.64 bytes from 123.150.76.218(123.150.76.218): icmp_seq=1ttl=53 time=43.8ms64 bytes from 123.150.76.218(123.150.76.218): icmp_seq=2ttl=53 time=42.4ms64 bytes from 123.150.76.218(123.150.76.218): icmp_seq=3ttl=53 time=48.7ms---qq.compingstatistics--3 packets transmitted,3received,0%packet loss, time 3005ms
```

如果出现 `0% packet loss`，说明网络通信正常。LAN1测试步骤一致，不再重复。

***

### 3.4 系统信息查询

查看内核和 CPU 信息，输入如下命令：

```shell
# uname -a
```

示例输出：

```text
Linux localhost 6.6.22 #1 SMP Tue Nov 11 15:14:30 CST 2025 aarch64
```

查看环境变量信息：

```shell
# env
```

示例输出：

```text
_=/bin/envHOME=/
PULSE_STATE_PATH=/data/data/.pulse_dir/state
rcu_nocbs=all
UV_THREADPOOL_SIZE=16
TMP=/data/local/mtp_tmp/
PULSE_RUNTIME_PATH=/data/data/.pulse_dir/runtime
TERM=linux
default_boot_device=2a330000.mmc
normal_no_read_back=0
TMPDIR=/data/local/tmp
PATH=/usr/local/bin:/bin:/usr/bin
hardware=rk3576
UBSAN_OPTIONS=print_stacktrace=1:print_module_map=2:log_exe_name=1
DOWNLOAD_CACHE=/data/cache
OHOS_SOCKET_hdcd=11
```

***

### 3.5 UART 485 测试

#### 3.5.1 准备工具和环境

测试前需要准备：

- 485 、232串口连接工具；
- PC端串口助手；

> 说明：也可以使用资料中的 `demo -> bq_uart` 应用进行测试。应用使用说明位于网盘资料的 `demo` 文件夹下。

串口硬件接口介绍：

<img src="../images/w1.jpg" alt="485 接口" style={{ zoom: '60%' }} />

测试工具介绍：

1. 485串口测试工具

   <img src="../images/17.png" alt="17" style={{ zoom: '85%' }} />

2. 232串口测试工具

   <img src="../images/18.png" alt="18" style={{ zoom: '110%' }} />

3. PC端串口助手

   <img src="../images/19.png" alt="19" style={{ zoom: '110%' }} />

   

***

#### 3.5.2 RS485_UART2发送测试

```shell
# microcom -s 115200 /dev/ttyS2
```

输入命令后，在命令行下方输入数据即可。串口助手这边接收到数据。

<img src="../images/20.png" style={{ zoom: '67%' }} />

#### 3.5.3 RS485_UART2 接收测试

```shell
# microcom -s 115200 /dev/ttyS2
```

串口助手发送数据后，终端这边接收到数据。

<img src="../images/j3.png"  />

***

### 3.6 UART 232 测试

#### 3.6.1 准备工具和环境

使用工具连接开发板上的 232 串口、以及打开所提供的资料中的串口助手。

<img src="../images/18.png" alt="232 接口" style={{ zoom: '110%' }} />

串口引脚示意图（丝印标识与 TX、RX、GND 对应位置）：

<img src="../images/w2.jpg" alt="串口引脚示意图" style={{ zoom: '60%' }} />

**设置串口助手：**

<img src="../images/19.png" alt="串口助手设置" style={{ zoom: '110%' }} />

***

#### 3.6.2 RS232_UART5 发送测试

```shell
# microcom -s 115200 /dev/ttyS5
```

输入命令后，在命令行下方输入 `beiqi`，串口助手这边接收到数据。

<img src="../images/20.png" style={{ zoom: '67%' }} />

***

#### 3.6.3 RS232_UART5 接收测试

```shell
# microcom -s 115200 /dev/ttyS5
```

串口助手发送数据后，终端这边接收到数据。

<img src="../images/22.png" style={{ zoom: '67%' }} />

***

### 3.7 USB 鼠标测试

将 USB 鼠标接入开发板 USB 接口，使用鼠标进行点击测试。

测试判断：

- 鼠标能够正常移动；
- 点击无误触；
- 坐标位置正确；
- 桌面或应用能够正常响应。

满足以上条件，说明 USB 鼠标功能正常。

如图所示：

<img src="../images/23.png" alt="USB 鼠标测试" style={{ zoom: '50%' }} />

***

### 3.8 USB 测试

USB 端口可连接以下设备进行测试：

- USB 鼠标；
- USB 键盘；
- U 盘；
- 其他 USB 外设。

测试重点：

- 插入设备后是否能识别；
- 鼠标、键盘是否能正常操作；
- U 盘是否能正常识别；
- 是否支持热插拔。

插入设备后测试正常即可。

***

### 3.9 蓝牙测试

测试步骤：

1. 确认蓝牙天线已正确连接；

   <img src="../images/w3.jpg" alt="蓝牙天线接线图" style={{ zoom: '60%' }} />

2. 打开系统 **设置**；

3. 进入 **蓝牙**；

4. 打开蓝牙开关；

5. 点击扫描；

6. 选择目标蓝牙设备；

7. 点击连接；

8. 确认连接成功。

<img src="../images/24.png" alt="蓝牙连接" style={{ zoom: '60%' }} />

测试判断：

- 能扫描到蓝牙设备；
- 能够正常连接；
- 连接状态显示正常。
- 打开音乐应用播放音频，通过已连接的蓝牙耳机确认声音输出是否正常。

***

### 3.10 耳机测试

测试步骤：

1. 找到开发板上丝印标识为 `PHONE` 的接口；

   <img src="../images/w4.jpg" alt="耳机接线图" style={{ zoom: '60%' }} />

2. 插入耳机；

3. 打开桌面音乐播放器；

   <img src="../images/25.png" alt="音乐播放器" style={{ zoom: '60%' }} />

4. 点击播放音乐；

5. 确认耳机是否有声音输出。

测试判断：

- 耳机有声音输出，说明耳机接口功能正常；
- 如果无声音，需要检查耳机、音量、播放器以及接口接触情况。

***

### 3.11 音量键测试

测试步骤：

1. 按下 `音量 +` 按键；

   <img src="../images/w6.jpg" alt="音量按键" style={{ zoom: '60%' }} />

3. 查看桌面音量状态是否有更新；

4. 确认系统音量条是否正常显示并变化。

测试判断：

- 音量状态随按键变化，说明音量键功能正常；
- 系统桌面出现音量弹窗显示。

<img src="../images/26.png" alt="音量测试效果" style={{ zoom: '60%' }} />

***

### 3.12 MIC 测试

测试步骤：

1. 在网盘中下载多录音播放的 hap：

   通过网盘分享的文件：RK3588_Openharmony

   链接: https://pan.baidu.com/s/1sHKpNJbwwqLKPEmCpmjfuQ 提取码: j6imdemo->bq_recorder.hap

   <img src="../images/j1.png" style={{ zoom: '70%' }} />

   MIC 接线图

   <img src="../images/w5.jpg" alt="相机接口示意图" style={{ zoom: '60%' }} />

2. 安装录音播放应用

3. 点击打开应用后，点击录音，录音完毕后，点击播放，插上耳机或扬声器，测试是否正常

<img src="../images/j2.png" style={{ zoom: '70%' }} />

测试判断：  

- 录音正常录制；
- 录音顺利播放

***

### 3.13 喇叭测试

测试步骤：

1. 点击打开音乐应用，找到丝印 SPK 处插上喇叭测试左右声道喇叭是否正常。

<img src="../images/W.jpg" style={{ zoom: '60%' }} />



测试判断：

- 喇叭输出声音正常

***

### 3.14 MIPI 相机测试

测试步骤：

1. 打开系统相机查看相机成像。

<img src="../images/W7.jpg" style={{ zoom: '60%' }} />

2. 依次接好 mipi 相机，打开系统相机应用进行测试，测试相机是否正常工作

测试判断：

- 相机成像正常

***

