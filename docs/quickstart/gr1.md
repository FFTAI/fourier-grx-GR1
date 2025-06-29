---
layout: default
title: GR1 机器人
nav_order: 1.1
parent: 快速开始
has_toc: true
---

# GR1 机器人

* TOC
{:toc}

## 视频教程

以下视频教程演示了如何快速开始使用 GR1 系列机器人。

## 系统开机

当我们拿到机器人后，首先需要将机器人开机。

1. 接通机器人的电池电源开关。
2. 确保机器人的急停按钮处于松开状态。
3. 确认所有关节执行器的指示灯为 <span style="color: purple;">紫色慢闪</span> 状态（正常工作状态）。

## 登录系统

### 本地登录

连接机器人控制电脑的 HDMI 显示器和 USB 键盘鼠标，开机后会自动进入系统桌面。用户信息如下：

- 用户名为 `gr1{type}{month}{serial number}`
    - `type` 为机器人型号，如 `t1` 或 `t2`
    - `month` 为生产月份缩写，如 `ja` 代表 1 月，`fe` 代表 2 月，`mr` 代表 3 月，依次类推
    - `serial number` 为机器人的序列号
- 用户密码为 `fftai2015`

### 远程登录

机器人开机后，系统会自动启动机器人主控系统的热点信号，用户可以通过手机或电脑连接机器人的热点信号。

- 热点名称为 `gr1{type}{month}{serial number}`
    - `type` 为机器人型号，如 `t1` 或 `t2`
    - `month` 为生产月份缩写，如 `ja` 代表 1 月，`fe` 代表 2 月，`mr` 代表 3 月，依次类推
    - `serial number` 为机器人的序列号
- 热点密码为 `66668888`。

连接完成后，可以通过 `ssh` 服务登录到机器人的主控电脑，登录用户名和密码与本地登录方式相同。


> ℹ️ **说明**：
>
> - 部分生产的 Fourier GR1 机器人没有配置自动热点模式，因此搜不到热点信号，此时可以考虑用有线网络方式连接机器人。
> - 机器人主控电脑的有线网口 IP 地址为 `192.168.137.220`, ssh 登录信息与本地登录方式相同。


> ℹ️ **说明**：
>
> - 部份旧款 GR1 机器人使用 IP 地址为 `192.168.137.200`，更新固件后会被自动更新到 `192.168.137.220`。安装更新过程如果出现 ssh 断联，需要重新连接并重新运行安装程序。
> - 部份旧款 GR1 机器人使用 Ubuntu 为 20.04 版本，需要更新到 Ubuntu 22.04 才能使用 `fourier-grx` 工具。 Ubuntu 系统升级方法可以参考 [Ubuntu系统升级](/docs/usage#ubuntu系统升级)。

## 程序启动

GR1 机器人出厂时，默认是没有安装 `fourier-grx` 软件工具。
需要安装对应 SDK 程序，可以去 [固件更新](/docs/update) 页面查看下载和安装流程。

该软件工具提供了 `fourier-grx start` 命令用于机器人控制程序启动。

```bash
# 在机器人主控电脑上：
# 1. 准备好手柄，连接到机器人主控电脑的 USB 端口。
# 2. 启动 fourier-grx 主程序
fourier-grx start
```

程序启动完成后，即可使用手柄控制机器人完成相应的任务。（图片为 XBOX 键位手柄，具体按键功能对应关系与所用手柄种类相关）

![joystick.jpg](/assets/images/joystick.jpg)

---

## 二次开发环境配置

除了启动控制程序的功能，`fourier-grx` 工具还提供了 `fourier-grx setup_conda` 命令用于一键配置 conda 开发环境用于机器人二次开发。

```bash
# 在机器人主控电脑上：
fourier-grx setup_conda

# 程序运行完成后，会搭建出一个名为 `fourier-grx` 的 conda 环境，可以通过以下命令激活该环境
conda activate fourier-grx

# 如果希望自主搭建开发环境，可以在 $HOME/fourier-grx/whl 中找到依赖库文件进行手动安装。
```

## 示例程序运行

当我们安装好 conda 开发环境后，可以通过 git 同步机器人的二次开发接口示例程序。

```bash
git clone https://github.com/FFTAI/Wiki-GRx-Deploy.git --branch=FourierGR1
```

建议同步到 `$HOME` 目录下，同步完成后，可以通过 `cd $HOME/Wiki-GRx-Deploy` 进入该目录查看。

然后，我们可以通过以下命令启动示例程序：

```bash
# 在机器人主控电脑上打开 Terminal
# 1. 启动 fourier-grx 主程序
# 激活 conda 环境
conda activate fourier-grx

# 启动 fourier-grx 主程序 （不同机型使用不同配置文件参数）
python $HOME/fourier-grx/whl/run.py --config=$HOME/fourier-grx/config/FourierGR1/config_GR1_T1_sdk.yaml  # GR1T1 机型
python $HOME/fourier-grx/whl/run.py --config=$HOME/fourier-grx/config/FourierGR1/config_GR1_T2_sdk.yaml  # GR1T2 机型

# 当看到提示信息 ”You can start playing with the robot right now.“ 时，表示程序启动成功。
```

```bash
# 在机器人主控电脑上或与机器人同局域网内的任意一台电脑上打开 Terminal
# 1. 启动 user 接口示例
# 激活 conda 环境
conda activate fourier-grx  

# 启动示例程序，进入准备状态，使能全部执行器
python $HOME/Wiki-GRx-Deploy/user/demo_ready_state.py

# ctrl + c 退出程序

# 启动示例程序，进入行走状态，可以用手柄控制机器人行走
python $HOME/Wiki-GRx-Deploy/user/demo_walk.py
```

程序启动后，可以通过手柄控制机器人完成相应的任务。

至此，我们已经完成了机器人的快速开始。接下来，我们可以通过 [示例代码](/fourier-grx-GR1/docs/examples) 来了解更多的机器人各项功能。🎆🎆🎆