---
layout: post
title: 使用 archinstall 安装 Arch Linux
categories: 笔记
tags:
- Arch
- Linux
- Tech
date: 2024-03-03 23:10 +0800
---
## 背景介绍

众所周知，Arch Linux由于其安装过程相较于其他常见的发行版更为复杂、繁琐而闻名，因此，为了简化安装过程，Arch Linux官方提供了一个名为 [archinstall](https://wiki.archlinux.org/title/Archinstall){:target="_blank"} 的工具，它可以帮助用户快速、简单地安装Arch Linux。本文将介绍如何使用 `archinstall` 命令安装 Arch Linux。

## 准备工作

- [**获取安装介质**](https://archlinux.org/download/){:target="_blank"}
- [**制作安装介质**](https://wiki.archlinuxcn.org/wiki/%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97#%E5%87%86%E5%A4%87%E5%AE%89%E8%A3%85%E4%BB%8B%E8%B4%A8){:target="_blank"}
- **启动到Live环境**

> 不同平台的启动方式不同，请自行查阅相关资料。
{: .prompt-tip }

## 具体步骤

### 确认联网状态

   确保您的计算机已经连接到互联网。您可以使用 `ping` 命令来测试网络连接：

   ```bash
   ping -c 3 archlinux.org
   ```

   如果网络连接正常，您将会收到来自 `archlinux.org` 的回复。
> 若未连接到互联网，请参考 [**Connect_to_the_internet**](https://wiki.archlinux.org/title/Installation_guide#Connect_to_the_internet){:target="_blank"} 配置网络。
{: .prompt-tip }

### 使用 `archinstall` 命令进入安装脚本配置界面

   在终端中输入以下命令：

   ```bash
   archinstall
   ```

   然后按 `Enter` 键。

### 按照提示进行安装

   `archinstall` 将向您展示一系列提示和选项，以配置安装过程。根据您的偏好和系统设置选择适当的选项。这些选项包括：

   - **选择键盘布局**
   - **选择时区**
   - **配置磁盘**
   - **设置主机名**
   - **创建用户并设置密码**
   - **安装引导程序**
   - **等等······**

   您可以根据提示输入相应的信息，或者按 `Enter` 键接受默认值。

### 安装完成

   安装过程完成后，`archinstall` 将提示您重新启动计算机。重新启动后，您将可以使用新安装的 Arch Linux 系统。
