---
sidebar_position: 2
split_by_h2: true
slug: /_split-source/terminal/rk3588-edge-computing-industrial-gateway/wiki-tutorial-openeuler
unlisted: true
sidebar_label: OpenEuler 系统 Wiki 教程
title: RK3588 边缘计算工业网关 OpenEuler 系统 Wiki 教程
---

# RK3588边缘计算工业网关

RK3588工业控制主板面向工业控制、边缘计算、设备网关、机器视觉等应用场景，提供 RS485、RS232、DI/DO、USB、Type-C、RTC、Wi-Fi、以太网、SSD、SATA、CAN、4G、SFP、MIPI Camera 等常用工业接口。

## 一、快速上手

### 1. 接口概览

开发板整体接口如下图所示，具体接口定义以板卡丝印和随货资料为准。

![图片](wiki-tutorial-assets/picture1.webp)

![图片](wiki-tutorial-assets/picture2.webp)

### 2. 准备工作

| 类别 | 说明 |
| --- | --- |
| 主板 | RK3588工业控制主板 |
| 电源 | 12V 电源适配器 |
| 烧写线材 | Type-C 数据线 |
| 调试工具 | USB 转串口线、串口调试软件 |
| 显示输入 | HDMI 显示器、键盘、鼠标 |
| 可选外设 | USB 转 485、RS232/RS485 设备、CAN 设备、SSD/SATA 硬盘、4G 模块、SFP 光模块、MIPI 摄像头 |

### 3. 固件烧写

开发板一般采用 Loader 模式烧写固件。如果无法进入 Loader 烧写模式，可进入 MaskRom 模式进行烧写。

#### 3.1 进入 Loader 烧写模式

1. 使用 12V 电源适配器给开发板供电。
2. 使用 Type-C 数据线连接开发板和电脑 USB 接口。
3. 按住主板 `Recovery` 按键不放。
4. 上电后短按 `Reset` 按键。
5. 工具识别到 Loader 设备后松开按键。

#### 3.2 进入 MaskRom 烧写模式

1. 使用 12V 电源适配器给开发板供电。
2. 使用 Type-C 数据线连接开发板和电脑 USB 接口。
3. 按住主板 `Maskrom` 按键不放。
4. 上电后短按 `Reset` 按键。
5. 工具识别到 MaskRom 设备后松开按键。

#### 3.3 Linux 主机烧写

使用 `edge` 工具查询烧写状态：

```shell
./edge flash -q
```

状态说明：

| 状态 | 说明 |
| --- | --- |
| `none` | 未识别到烧写设备 |
| `loader` | 已进入 Loader 烧写模式 |
| `maskrom` | 已进入 MaskRom 烧写模式 |

烧写全部镜像：

```shell
./edge flash -a
```

按分区烧写：

```shell
# 烧写 U-Boot 相关镜像
./edge flash -u

# 烧写 Kernel 相关镜像
./edge flash -k

# 烧写 misc 镜像
./edge flash -m

# 烧写 rootfs 文件系统镜像
./edge flash -r

# 查看帮助
./edge flash -h
```

#### 3.4 Windows 主机烧写

1. 安装 RK USB 驱动。
2. 打开 `RKDevTool.exe`。
3. 确认开发板已进入 Loader 或 MaskRom 烧写模式。
4. 勾选需要烧写的镜像。
5. 建议勾选 `Loader` 和 `Parameter`，其他镜像按需选择。
6. 点击“执行”，等待烧写完成。

### 4. 串口调试

串口调试用于查看启动日志、登录系统和执行底层调试命令。将 USB 转串口线连接到开发板 Debug 口。

| 参数 | 配置 |
| --- | --- |
| 调试口 | Debug 口 |
| 波特率 | 150000 |
| 数据位 | 8 |
| 停止位 | 1 |
| 奇偶校验 | 无 |
| 流控制 | 无 |

## 二、用户与密码

### 1. 默认用户

OpenEuler 系统已预设 `root` 用户，可用于串口登录和系统调试。

| 项目 | 默认值 |
| --- | --- |
| 用户名 | `root` |
| 密码 | `123` |

### 2. 创建普通用户

如需创建自己的 OpenEuler 用户，请连接 HDMI 显示器、键盘和鼠标，进入系统桌面后，根据系统引导或用户设置界面创建普通用户并设置密码。

创建完成后，可使用新建用户进入桌面系统。涉及串口、GPIO、CAN、Camera 等硬件接口测试时，建议切换到 `root` 用户或使用具备管理员权限的账户执行命令。

### 3. SSH 登录

开发板接入网络后，可在设备端查看 IP 地址：

```shell
ifconfig
```

如果当前固件已开启 root 用户 SSH 登录，可在 PC 端使用 `root` 用户登录：

```shell
ssh root@<开发板IP>
```

如果无法连接 SSH，请先确认网络连接状态，并检查 SSH 服务是否正常；若系统禁止 root 用户远程登录，可使用已创建的普通用户登录，或通过串口进入系统后调整 SSH 配置。

## 三、硬件接口使用

### 1. RS485

RS485 接口位置如下图所示。

![图片](wiki-tutorial-assets/picture3.webp)

#### 1.1 硬件连接

根据 USB 转 485 线的针脚定义，将 `A`、`B`、`GND` 分别连接到开发板 RS485 接口的 `A`、`B`、`GND`。

![图片](wiki-tutorial-assets/picture4.webp)

![图片](wiki-tutorial-assets/picture5.webp)

具体引脚可查看板卡背面丝印。

#### 1.2 串口连接

使用 USB 转串口线连接开发板 Debug 串口，并按本文前面的串口参数打开终端。

![图片](wiki-tutorial-assets/picture6.webp)

![图片](wiki-tutorial-assets/picture7.webp)

#### 1.3 收发测试

RS485 使用 `uart0`，系统节点通常为 `/dev/ttyS0`。

![图片](wiki-tutorial-assets/picture8.webp)

发送测试：

```shell
stty -F /dev/ttyS0 115200
echo "beiqi" > /dev/ttyS0
```

接收测试：

```shell
cat /dev/ttyS0
```

### 2. RS232

#### 2.1 硬件连接

RS232 接口连接方式如下图所示。

![图片](wiki-tutorial-assets/picture9.webp)

#### 2.2 收发测试

RS232 使用 `uart5`，系统节点通常为 `/dev/ttyS5`。

![图片](wiki-tutorial-assets/picture10.webp)

发送测试：

```shell
stty -F /dev/ttyS5 115200
echo "beiqifz" > /dev/ttyS5
```

接收测试：

```shell
cat /dev/ttyS5
```

### 3. DI/DO

DI/DO 接口位置如下图所示。

![图片](wiki-tutorial-assets/picture11.webp)

#### 3.1 DO 测试

DO 接口如下图所示。

![图片](wiki-tutorial-assets/picture12.webp)

查看和设置 DO1：

```shell
# 查看 DO1 当前电平
cat /sys/devices/platform/gpios_in/DOUT1

# 设置 DO1 输出高电平
echo 1 > /sys/devices/platform/gpios_in/DOUT1

# 设置 DO1 输出低电平
echo 0 > /sys/devices/platform/gpios_in/DOUT1
```

#### 3.2 DI 测试

DI 接口如下图所示。

![图片](wiki-tutorial-assets/picture13.webp)

读取 DI 输入：

```shell
cat /sys/devices/platform/gpios_in/DIN1
cat /sys/devices/platform/gpios_in/DIN2
```

#### 3.3 DI/DO 回环测试

如无万用表或其他测试工具，可将 `DIN1` 与 `DOUT1` 连接起来做回环测试。

![图片](wiki-tutorial-assets/picture14.webp)

输出高电平并读取输入：

```shell
echo 1 > /sys/devices/platform/gpios_in/DOUT1
cat /sys/devices/platform/gpios_in/DIN1
```

![图片](wiki-tutorial-assets/picture15.webp)

输出低电平并读取输入：

```shell
echo 0 > /sys/devices/platform/gpios_in/DOUT1
cat /sys/devices/platform/gpios_in/DIN1
```

![图片](wiki-tutorial-assets/picture16.webp)

### 4. USB

USB 接口位置如下图所示。

![图片](wiki-tutorial-assets/picture17.webp)

测试方法：

1. 插入鼠标或键盘，确认系统可正常识别并使用。
2. 接入 HDMI 显示器后，移动鼠标，确认屏幕中的鼠标指针可正常跟随移动。

### 5. Type-C

Type-C 接口主要用于固件烧写和 ADB 调试。

![图片](wiki-tutorial-assets/picture18.webp)

使用 ADB 前，请确认 PC 端已安装 ADB 工具并加入系统环境变量。连接 Type-C 数据线后，在 PC 端执行：

```shell
adb shell
```

成功进入设备命令行后，即可进行调试。

![图片](wiki-tutorial-assets/picture19.webp)

### 6. RTC

RTC 电池接口位置如下图所示。

![图片](wiki-tutorial-assets/picture20.webp)

测试方法一：

1. 确认已连接 RTC 电池。
2. 连接网络进行时间同步。
3. 断开 Wi-Fi 与网线。
4. 关机等待几分钟。
5. 再次开机，确认系统时间是否保持准确。

测试方法二：

```shell
hwclock
```

多次执行后，观察时间是否正常递增。

![图片](wiki-tutorial-assets/picture21.webp)

### 7. 网络连接

#### 7.1 Wi-Fi

外接 HDMI 显示器进入 OpenEuler 桌面后，可在系统设置中连接 Wi-Fi。

![图片](wiki-tutorial-assets/picture22.webp)

也可以使用命令行：

```shell
# 查看 Wi-Fi 列表
nmcli device wifi list

# 重新扫描 Wi-Fi
nmcli device wifi rescan
```

![图片](wiki-tutorial-assets/picture23.webp)

连接 Wi-Fi 并查看状态：

```shell
nmcli device wifi connect "<SSID>" password "<密码>"
nmcli device status
```

![图片](wiki-tutorial-assets/picture24.webp)

获取 IP：

```shell
ifconfig
```

![图片](wiki-tutorial-assets/picture25.webp)

#### 7.2 以太网

接入网线后，可在 OpenEuler 桌面设置中查看 Network 状态和 IP 地址。

![图片](wiki-tutorial-assets/picture26.webp)

也可以使用命令查看：

```shell
ifconfig
```

![图片](wiki-tutorial-assets/picture27.webp)

### 8. SSD

#### 8.1 硬件连接

SSD 安装位置如下图所示。安装或拆卸 SSD 前，请先断电。

![图片](wiki-tutorial-assets/picture28.webp)

#### 8.2 测试方法

进入 OpenEuler 桌面文件管理器，查看是否识别到额外硬盘。

![图片](wiki-tutorial-assets/picture29.webp)

也可以使用命令查看：

```shell
lsblk
```

![图片](wiki-tutorial-assets/picture30.webp)

### 9. SATA

#### 9.1 硬件连接

SATA 接口连接方式如下图所示。

![图片](wiki-tutorial-assets/picture31.webp)

#### 9.2 测试方法

进入 OpenEuler 桌面文件管理器，查看是否识别到额外硬盘。

![图片](wiki-tutorial-assets/picture32.webp)

也可以使用命令查看：

```shell
lsblk
```

![图片](wiki-tutorial-assets/picture33.webp)

### 10. CAN

#### 10.1 硬件连接

CAN 接口连接方式如下图所示。

![图片](wiki-tutorial-assets/picture34.webp)

#### 10.2 获取测试工具

从资料包中获取 `cansend` 和 `candump` 工具，并将这两个工具拷贝到设备根目录中。

![图片](wiki-tutorial-assets/picture35.webp)

![图片](wiki-tutorial-assets/picture36.webp)

#### 10.3 收发测试

主机端或 CAN 分析仪端配置波特率为 `250000`。

![图片](wiki-tutorial-assets/picture37.webp)

RK3588 端配置 CAN：

```shell
ip link set can0 down
ip link set can0 type can bitrate 250000
ip link set can0 up
```

3588 侧使用命令进行发送和接收：

```shell
# 发送数据
./cansend can0 123#1122334455667788

# 接收数据
./candump can0
```

![图片](wiki-tutorial-assets/picture38.webp)

### 11. 4G

#### 11.1 硬件连接

4G 模块安装位置如下图所示。安装模块和 SIM 卡前，请先断电。

![图片](wiki-tutorial-assets/picture39.webp)

#### 11.2 测试方法

装上 4G 模块并插入 SIM 卡后，等待系统识别并自动获取 IP。

![图片](wiki-tutorial-assets/picture40.webp)

使用 `ping` 命令验证网络连通性。

![图片](wiki-tutorial-assets/picture41.webp)

### 12. SFP

#### 12.1 硬件连接

SFP 测试需要两台设备、两个 SFP 光模块和一根光纤。

![图片](wiki-tutorial-assets/picture42.webp)

![图片](wiki-tutorial-assets/picture43.webp)

#### 12.2 测试方法

光纤连接后，在两台设备上分别配置同网段 IP，然后互相 `ping` 验证连通性。

示例：

```shell
ifconfig eth0 192.168.0.5
```

![图片](wiki-tutorial-assets/picture44.webp)

![图片](wiki-tutorial-assets/picture45.webp)

### 13. Camera

#### 13.1 硬件连接

MIPI Camera 接口位置如下图所示。

![图片](wiki-tutorial-assets/picture46.webp)

摄像头模组示例：

![图片](wiki-tutorial-assets/picture47.webp)

连接方式如下图所示。

![图片](wiki-tutorial-assets/picture48.webp)

> 注意：FPC 排线不能插反，插拔前请先断电。

#### 13.2 查看设备节点

查看摄像头设备：

```shell
v4l2-ctl --list-devices
```

找到 `rkisp_mainpath` 对应的视频节点。示例中使用 CSI0 时，Camera 节点为 `/dev/video22`。

![图片](wiki-tutorial-assets/picture49.webp)

#### 13.3 gstream 测试摄像头

打开摄像头预览：

```shell
gst-launch-1.0 v4l2src device=/dev/video22 ! video/x-raw,format=NV12,width=1920,height=1080 ! videoconvert ! autovideosink
```

预览效果如下图所示。

![图片](wiki-tutorial-assets/picture50.webp)

## 四、常见问题

### FAQs

Todo

&gt; 可通过beiqi@beiqicloud.com联系我们!
