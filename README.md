# QSM603FCP EVK Linux 开发指南

 [![Document Version](https://img.shields.io/badge/docs-v1.0.1-blue)]() [![Status](https://img.shields.io/badge/status-stable-green)]() [![Platform](https://img.shields.io/badge/platform-Linux-orange)]() 

> **适用硬件：** Quectel QSM603FCP EVK
>
> **最后更新：** 2026-10-28
>
> **适用对象：** Linux BSP / 驱动 / 应用开发人员
>
> **核心内容：** 快速上手 · 系统镜像下载 · 镜像烧录 · 功能验证

# 目录

- [文档概览](#文档概览)
- [快速上手](#快速上手)
  - [开发板介绍](#开发板介绍)
  - [开发准备](#开发准备)
  - [获取系统镜像包](#获取系统镜像包)
  - [镜像烧录与系统启动](#镜像烧录与系统启动)
- [专项开发文档](#专项开发文档)
- [FAQ](#FAQ)
- [修订记录](#修订记录)

---

# 文档概览

本文档介绍 **QSM603FCP EVK** 的基本信息、系统镜像获取、烧录及功能验证方法，为开发人员进行 QSM603FCP 平台开发及功能验证提供参考。

> [!IMPORTANT]
>
> 该套件不提供 SDK，请使用板端环境进行编译开发。系统镜像由 Quectel 统一发布。

> [!TIP]
>
> 开发板已预烧录系统镜像，可直接上电使用。预烧录镜像用于基本功能体验，具体功能支持及软件版本请以当前发布的系统镜像版本为准。

### 开始之前

首次使用 QSM603FCP EVK 时，建议按以下顺序操作：

**准备硬件 → 下载系统镜像包 → 安装烧录工具 → 烧录镜像 → 启动系统 → 验证功能**

# 快速上手

本章节用于完成 QSM603FCP EVK 的首次使用。完成后，开发板应能够正常启动 Ubuntu 系统，并可通过调试串口查看系统启动日志。

## 开发板介绍

### 基本信息

QSM603FCP EVK 是基于 MTK MT8371 应用处理器设计的评估开发平台，用于验证 Linux BSP、显示、多媒体、无线连接、工业控制及边缘 AI 应用开发。

开发板集成 Quectel QSM603FCP 核心模组，支持 Wi-Fi、Bluetooth、以太网、USB、HDMI/LVDS 显示等外设接口，可满足工业 HMI、智能网关、边缘计算、视频分析及 IoT 终端等应用场景的开发需求。

套件预览图：

![KIT 套件及配件](./media/board.png)

### 主要特性

| 类别 | 特性 |
| --- | --- |
| 处理器 | MTK MT8371 |
| 操作系统 | Ubuntu 24.04 |
| 无线 | Wi-Fi + Bluetooth |
| 有线网络 | 1 x Gigabit Ethernet + 2 x 100 Mbps Ethernet |
| 显示 | HDMI、LVDS |
| USB | USB Host / Device |
| 存储 | UFS Flash |
| 工业接口 | CAN FD |
| 音频 | Audio Codec |

> [!NOTE]
>
> 上表为主要特性概览，接口能力及软件支持情况请以当前发布的系统镜像版本为准。

## 开发准备

### 硬件准备

| 设备 | 数量 | 用途 | 必需 |
| --- | ---: | --- | :---: |
| QSM603FCP-KIT 套件 | 1 | QSM603FCP EVK 开发及功能验证 | 是 |
| 开发主机 | 1 | 镜像烧录及开发调试 | 是 |
| 网线 | 可选 | 以太网连接及网络功能测试 | 否 |
| SIM 卡 | 可选 | 蜂窝拨号上网及通话验证 | 否 |

### 软件准备

QSM603 系列开发主要需要以下软件资源：

- 系统镜像包
- 镜像烧录工具
- 板端编译环境（用于驱动程序及应用开发）

## 获取系统镜像包

点击 [QSM603FCP-UP24.04-FW.zip](https://developer.quectel.com/downloads/QSM603FCP-UP24.04-FW.zip) 下载系统镜像包。

## 镜像烧录与系统启动

> [!CAUTION]
>
> 烧录会覆盖板卡现有镜像，操作前请确认已备份必要数据。

> [!NOTE]
>
> 完整烧录流程及常见问题请参考[《Quectel_QSM603FCP_Ubuntu_编译&烧录指导》](./files/Quectel_QSM603FCP_Ubuntu_编译&烧录指导_V1.0.0_Preliminary_20261026.pdf)。

### Windows 环境烧录

#### 步骤 1：安装烧录工具

1. 从仓库 `tools/` 目录下获取 `android_usb_110_64_1228.exe` 并执行安装（USB 驱动）。
2. 从 https://www.python.org/downloads/ 下载并安装 Python 3.12.x。
3. 安装 MTK 烧录工具 genio-tools：

```bash
py -3.12 -m pip uninstall -y genio-tools
# 若genio-tools首次安装失败，建议重复安装一次
py -3.12 -m pip install genio-tools==1.7.0
```

> [!NOTE]
>
> `genio-flash.exe` 默认安装于 Python 安装目录的 `Scripts` 子目录，例如 `C:\Users\<用户名>\AppData\Local\Programs\Python\Python312\Scripts\genio-flash.exe`，请根据实际环境调整路径。

#### 步骤 2：执行烧录命令

启动 CMD 并切换至前面下载解压的镜像目录，在CMD中启动烧写工具：

```bash
C:\Users\<用户名>\AppData\Local\Programs\Python\Python312\Scripts\genio-flash.exe
```

#### 步骤 3：设置设备进入下载模式

使用 USB 线将 PC 与 EVB 的type C接口相连，将 EVB 板上丝印为 `BOOT` 的触点短接到 GND。然后给板子上电。烧写工具会自动发现设备并开始烧写。


##### 烧写进度1：等待设备接入
![](./media/burn-1.png)

##### 烧写进度2：设备接入后
![](./media/burn-2.png)

##### 烧写进度3：烧写结束
![](./media/burn-3.png)


> [!NOTE]
>
> 烧录过程约需 5 分钟，不同 PC 的烧录速度可能略有差异。当工具日志正确识别到 COM 口并出现 `flashing` 等关键字时，表示已进入烧录状态。

#### 步骤 4：启动系统

- 烧录开始后即可断开 `BOOT` 触点与 GND 的连接；烧录完成后板卡系统会自动启动。若未正常启动，可重新给板卡上电。
- 若已接入 1920×1080 分辨率的 HDMI 屏幕，屏幕将显示 Ubuntu 24.04 桌面。
- 可通过 Type-C 接口与 PC 连接，并使用 `adb` 工具登录板卡系统，以判断系统启动状态。adb 使用方法请参考[《Quectel_QSM603FCP_Ubuntu_软件用户手册》](./files/Quectel_QSM603FCP_Ubuntu_软件用户手册_V1.0.0_Preliminary_20261008.pdf)。
- 电源接口旁的 26 pin 接口中，Pin 15 为 TX、Pin 17 为 RX、Pin 19 为 GND，电压域3.3V，波特率为 921600。也可通过该接口查看系统启动日志，判断板卡系统的启动状态。

# 专项开发文档

QSM603FCP EVK 提供外设、摄像头、传感器、音频、多媒体等功能。各功能的详细配置及使用方法请参考对应专项开发文档。

根据你的开发目标选择对应文档：

| 🎯 如果你想…… | 📖 推荐查看 | 适用内容 |
| --- | --- | --- |
| 👆 了解系统使用与常用操作 | [《Quectel_QSM603FCP_Ubuntu_软件用户手册》](./files/Quectel_QSM603FCP_Ubuntu_软件用户手册_V1.0.0_Preliminary_20261008.pdf) | 系统登录、调试串口、adb、存储及常用命令 |
| 🚀 烧录系统镜像 | [《Quectel_QSM603FCP_Ubuntu_编译&烧录指导》](./files/Quectel_QSM603FCP_Ubuntu_编译&烧录指导_V1.0.0_Preliminary_20261026.pdf) | 镜像烧录、系统启动及常见问题 |
| 🔌 开发通用外设 | [《Quectel_QSM603FCP_Ubuntu_外设驱动_开发指导》](./files/Quectel_QSM603FCP_Ubuntu_外设驱动_开发指导_V1.0.0_Preliminary_20261014.pdf) | GPIO、I2C、SPI、UART、USB、Ethernet、CAN |
| 📷 开发 Camera | [《Quectel_QSM603FCP_Ubuntu_摄像头驱动_开发指导》](./files/Quectel_QSM603FCP_Ubuntu_摄像头驱动_开发指导_V1.0.0_Preliminary_20261019.pdf) | 摄像头驱动、Device Tree 及调试 |
| 🎛️ 开发传感器 | [《Quectel_QSM603FCP_Ubuntu_传感器驱动_开发指导》](./files/Quectel_QSM603FCP_Ubuntu_传感器驱动_开发指导_V1.0.0_Preliminary_20261021.pdf) | 加速度计、陀螺仪等传感器驱动 |
| 🔊 开发音频 | [《Quectel_QSM603FCP_Ubuntu_音频驱动_开发指导》](./files/Quectel_QSM603FCP_Ubuntu_音频驱动_开发指导_V1.0.0_Preliminary_20261019.pdf) | Audio Codec、音频通路配置及调试 |
| 🎬 开发多媒体应用 | [《Quectel_QSM603FCP_Ubuntu_多媒体应用指导》](./files/Quectel_QSM603FCP_Ubuntu_多媒体应用指导_V1.0.0_Preliminary_20261023.pdf) | 音视频编解码、播放及显示应用 |

# FAQ

参考文档：[FAQ](./FAQ.md)

# 修订记录

| 版本 | 日期 | 修订内容 |
| --- | --- | --- |
| V1.0.0 | 2026-10-27 | 初始版本 |
| V1.0.1 | 2026-10-28 | 提供系统镜像包下载链接 |
