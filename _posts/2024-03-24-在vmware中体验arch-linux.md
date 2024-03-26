---
layout: post
title: 在VMware中体验Arch Linux
categories: 笔记
tags:
- VMware
- Linux
- Arch
img_path: "/assets/img/posts/arch_in_vmware"
image:
  path: preview.png
  alt: Arch Linux in VMware
date: 2024-03-24 23:18 +0800
---
## 背景介绍

Arch Linux作为一个受人欢迎的Linux发行版，并不隶属于常见的Debian系或者RedHat系，是一个简单、轻量、灵活的滚动发行版.Arch Linux的安装过程相比其他发行版来说更加复杂，很多人可能会因为这个原因而放弃尝试.但是Arch Linux的优点也是显而易见的，比如系统的定制性更高，可以根据自己的需求进行定制，而且软件包的更新速度也更快，可以第一时间体验到最新的软件.为了体验Arch Linux，我们可以在VMware中安装Arch Linux，这样既不会影响到我们的主系统，又可以体验到Arch Linux的魅力.

## 准备工作

- [**VMware Workstation Player**](https://www.vmware.com/content/vmware/vmware-published-sites/us/products/workstation-player/workstation-player-evaluation.html.html){:target="_blank"}
- [**获取安装介质**](https://archlinux.org/download/){:target="_blank"}

> 关于VMware Workstation，免费的Player版功能已经足够我们体验Arch Linux，如果需要更多专业功能，可以考虑了解[**Pro**](https://store-us.vmware.com/compare_workstation){:target="_blank"}版.
{: .prompt-info}

## 具体步骤

### 通过VMware创建虚拟机

![Configurations](Configurations.png)
_虚拟机配置_

> 以上是我的虚拟机配置参考，具体配置可以根据自己平台的实际情况进行调整.
{: .prompt-info}

创建完虚拟机后，可以直接启动虚拟机，如果启动介质选择的没有问题的话，会进入下面的界面：

![systemd](systemd.png)

选择第一项，进入安装介质的Live CD环境.

### （可选）通过SSH连接到Live CD

如果你前面配置虚拟机网络时选择了桥接模式，那么你可以在宿主机上通过SSH连接到Live CD环境，这样可以方便的复制粘贴命令：

1. 修改SSH连接密码：

   ```bash
   passwd
   ```

   根据提示输入两次密码即可.

2. 使用`ip addr`（或`ip a`）查看IP地址：

   ```bash
   root@archiso ~ # ip a
   1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
       link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
       inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
       inet6 ::1/128 scope host noprefixroute
       valid_lft forever preferred_lft forever
   2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
       link/ether 00:0c:29:f9:94:c8 brd ff:ff:ff:ff:ff:ff
       altname enp2s1
       inet 192.168.213.138/24 metric 100 brd 192.168.213.255 scope global dynamic ens33
       valid_lft 3516sec preferred_lft 3516sec
       inet6 2409:8915:205b:23f3:20c:29ff:fef9:94c8/64 scope global dynamic mngtmpaddr noprefixroute
       valid_lft 7120sec preferred_lft 7120sec
       inet6 fe80::20c:29ff:fef9:94c8/64 scope link proto kernel_ll
       valid_lft forever preferred_lft forever
   root@archiso ~ # 
   ```

   上面的`ens33`就是我们的网卡名称，`inet`跟的那串IPv4地址就是通过SSH连接到Live CD时需要的IP地址.

3. 在宿主机通过任意终端输入`ssh root@<Live CD的IP>`，第一次连接会提示是否继续连接，输入`yes`，然后输入前面设置的密码即可连接到Live CD环境.以PowerShell为例：

   ```powershell
   PowerShell 7.4.1
   PS C:\Users\Paspar> ssh root@192.168.213.138
   root@192.168.213.138's password:
   To install Arch Linux follow the installation guide:
   https://wiki.archlinux.org/title/Installation_guide

   For Wi-Fi, authenticate to the wireless network using the iwctl utility.
   For mobile broadband (WWAN) modems, connect with the mmcli utility.
   Ethernet, WLAN and WWAN interfaces using DHCP should work automatically.

   After connecting to the internet, the installation guide can be accessed
   via the convenience script Installation_guide.


   Last login: Sun Mar 24 13:49:38 2024 from 192.168.213.8
   root@archiso ~ #
   ```

   这样就在宿主机上通过SSH连接到了Live CD环境，可以方便的复制粘贴命令.

### 安装Arch Linux

#### 设置网络

虽然我们可能已经通过SSH连接到了Live CD环境，但是为了确保网络正常，但是最好还是用`ping`命令检查一下网络：

```bash
ping archlinux.org -c 3
```

#### （可选）配置镜像源

根据我以往的经验，使用官方源时虽然通过代理也不会出太大问题，但当我尝试换用离自己较近的镜像源时，下载速度直接起飞.

先使用`vim /etc/pacman.d/mirrorlist`打开镜像源配置文件（如果对`vim`不熟悉，可以使用`nano`代替），然后将离自己较近的镜像源贴到文件的最上面，保存退出：

```url
# 清华源
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
# 哈工大源
Server = https://mirrors.hit.edu.cn/archlinux/$repo/os/$arch
# 中科大源
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
# 重庆大学源
Server = https://mirrors.cqu.edu.cn/archlinux/$repo/os/$arch

# 2024/03/24
```

#### 使用`archinstall`脚本安装

1. 更新`archinstall`脚本：

   ```bash
   pacman -Sy archinstall
   ```

2. 配置`archinstall`：

   ![archinstall](archinstall.png)

   使用`archinstall`命令进入安装界面，能看到以上配置界面，下面简单讲讲其中几项配置：

   - `Mirrors`： 如果前面配置了镜像源，则可以留空
   - `Disk configuration`：由于是在虚拟机中进行安装，建议选择`Use a best-effort default partition layout`这一项然后一路使用默认选项即可.(选择磁盘时注意选择含有`VMware`字样的磁盘)
   - `Profile`： 由于出于体验的目的，建议选择`Desktop`并使用`KDE Plasma`桌面环境，其余选项默认即可
   - `Audio`： 选择`PipeWire`
   - `Network Configuration`： 选择`NetworkManager`

   其余选项可以保持默认或按照提示自行配置，配置完成后选择`Install`，根据提示进行安装即可.

   安装时间长短取决于网络速度和硬件性能，安装过程一切正常的话，会询问你是否使用`chroot`进入新系统，选择`no`，然后使用`reboot`重启虚拟机即可进入Acrh Linux登录界面.

   ![登录界面](sddm.png)

## 简单配置和体验

### 中文本地化

1. 安装中文字体

   ```bash
   sudo pacman -S noto-fonts-cjk
   ```

2. 设置中文环境

   使用管理员权限编辑`/etc/locale.gen`（通过你喜欢的编辑器），找到`zh_CN.UTF-8 UTF-8`这一行，将前面的`#`去掉，保存退出.然后执行：

   ```bash
   sudo locale-gen
   ```

   在用户根目录下新建`.xprofile`文件，写入以下内容：

   ```bash
   export LANG=zh_CN.UTF-8
   export LANGUAGE=zh_CN:en_US
   ```

3. 安装中文输入法

   ```bash
   sudo pacman -S ibus ibus-libpinyin
   ```

   然后使用`ibus-setup`启动并配置IBus，根据提示配置即可.

### 窗口分辨率自动适配

未配置此功能时，虚拟机窗口大小调整后，Arch Linux界面并不会自动适配，可以通过安装`open-vm-tools`来解决：

```bash
sudo pacman -S open-vm-tools
```

> 需确保在VMware中开启了自动适配
> VMware Workstation中这一设置项位于：`查看` > `自动调整大小` > `自动适应客户机`
{: .prompt-tip}



> To be continued...
{: .prompt-tip}