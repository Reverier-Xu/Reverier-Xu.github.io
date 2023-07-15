---
title: GU604VI折腾日记
date: 2023-06-22T18:40:19+08:00
tags: [Development]
categories: [Development]
---

## 前情提要

接[上文](./20230514.md)，新电脑有了，攒了一整年用来毕业旅行的钱换的。

## 电脑配置

```plaintext
MODEL: ROG Zephyrus M16 GU604VI_GU604VI 1.0

CPU: 13th Gen Intel i9-13900H (20) @ 5.200GHz
GPU: NVIDIA GeForce RTX 4070 Max-Q / Mobile
MEM: 32GB
DISK 1: HFS001TEJ9X101N 1TB
DISK 2: ZHITAI Ti7100 2TB
```

挺不错的，打算当主力机用个四五年，本文主要记录一下在这个本上装 Arch Linux 所做出的一些努力。

## 安装过程

总体来说没出什么幺蛾子，照着 [Wiki](https://wiki.archlinux.org/title/Installation_guide) 配就可以了。如果在 archiso 那里就卡显卡了，需要在内核参数里加上 `nouveau.modeset=0`，然后重新启动。

桌面环境这里我继续选用了 KDE Plasma on Wayland，但是安装完成之后怎么都没法启动，一直以为是显卡问题…… 排查到最后发现是因为没装 `XWayland`，装上之后就好了，检查 `journalctl -b -1` 查看上次开关机 log，相关日志如下：

```log
Jun 21 13:39:13 Reverier-Arch dbus-daemon[825]: [session uid=1000 pid=825] Activating via systemd: service name='org.freedesktop.impl.portal.desktop.kde' unit='plasma-xdg-desktop-portal-kde.service' requested by ':1.6' (uid=1000 pid=849 comm="/usr/lib/xdg-desktop-portal")
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: No backend specified, automatically choosing drm
Jun 21 13:39:13 Reverier-Arch dbus-daemon[572]: [system] Successfully activated service 'org.freedesktop.UPower'
Jun 21 13:39:13 Reverier-Arch systemd[1]: Started Daemon for power management.
Jun 21 13:39:13 Reverier-Arch baloo_file[835]: QDBusConnection: name 'org.freedesktop.UDisks2' had owner '' but we thought it was ':1.31'
Jun 21 13:39:13 Reverier-Arch baloo_file[835]: QDBusConnection: name 'org.freedesktop.UPower' had owner '' but we thought it was ':1.34'
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: OpenGL vendor string:                   Intel
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: OpenGL renderer string:                 Mesa Intel(R) Graphics (RPL-P)
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: OpenGL version string:                  4.6 (Core Profile) Mesa 23.1.2
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: OpenGL shading language version string: 4.60
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: Driver:                                 Intel
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: GPU class:                              Unknown
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: OpenGL version:                         4.6
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: GLSL version:                           4.60
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: Mesa version:                           23.1.2
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: Linux kernel version:                   6.3.8
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: Requires strict binding:                no
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: GLSL shaders:                           yes
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: Texture NPOT support:                   yes
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: Virtual Machine:                        no
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: kwin_core: Parse error in tiles configuration for monitor "32593100-170c-5758-be7c-15262ca65916" : "illegal value" Creating default setup
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: kwin_xkbcommon: XKB: inet:323:58: unrecognized keysym "XF86EmojiPicker"
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: kwin_xkbcommon: XKB: inet:324:58: unrecognized keysym "XF86Dictate"
Jun 21 13:39:13 Reverier-Arch kwin_wayland[836]: kwin_xwl: Xwayland process failed to start
Jun 21 13:39:22 Reverier-Arch systemd[1]: Stopping User Manager for UID 973...
Jun 21 13:39:23 Reverier-Arch systemd[641]: Activating special unit Exit the Session...
```

## 内核和显卡驱动

最开始我直接使用的官方 Linux 内核，log 里小问题，不断，于是按照 [ASUS Linux](https://asus-linux.org/) 这里的提供的内核安装了 `linux-g14`，以及一系列配套工具 `asusctl` 等等，并开启了相关服务。显卡驱动使用 `nvidia-dkms`，显卡调度等功能使用了 [ASUS Linux](https://asus-linux.org/) 提供的 `supergfxctl`，这是我目前见到过的最完美的显卡方案，你不用显卡他真的就不转。

## 电源管理

电源管理依旧使用ASUS Linux解决方案，不过这个工具会默认给电池最大充电量限制到80%，这样可以延长电池寿命。于是我没怎么动，在实际使用体验中，80%电量大概可以用4个小时，也算舒心。

## 声音

这台电脑有四个扬声器，两个是低音扬声器两个高音扬声器，其中两个低音扬声器需要额外靠电池供电，高音扬声器直接靠声卡供电，这就导致了在 Linux 默认声卡之下，只有两个高音扬声器能用，而且声音很小，低音扬声器完全没声音。我在网上搜了搜，似乎大伙在装了 [ASUS Linux](https://asus-linux.org/) 的内核之后，声音就能用了，但是我这里并没有，找来找去找到一个[帖子](https://forums.linuxmint.com/viewtopic.php?t=394616)，原因可能单纯是这设备太新了，所以暂时没有适配方案。楼主最终自己patch了BIOS和内核，我差点以为我也要这么干，但最终在ASUS Linux的Discord群里问了几句，发现`linux-g14`其实已经带有这个patch了，我只需要修改一下ACPI tables即可。

根据 [Load custom ACPI tables](https://gist.github.com/lamperez/d5b385bc0c0c04928211e297a69f32d7) 这里的过程，我最终构建出了一个 `patched_acpi_tables.cpio`，并将其加到grub引导参数中，这样就能让内核加载这个补丁了。

可能是由于型号不太匹配的原因，这么做之后音响能听了，但是音质很奇怪，听起来就好像大过年去姥姥家吃年夜饭，隔壁有个上世纪的收音机在放春晚一样。不过我大部分时间都戴着耳机，音响能响就行。我看linux主线也在推进 ALC 285 芯片相关驱动，没准等一段时间上游就给修好了。

## “掉盘”风波

新电脑自带了一块1T的海力士，如果双系统的话不太够用。正好赶上长江消防队给固态价格完全干下来了，于是打算支持一下国货，入了ZHITAI TiPlus 7000 2TB版本。这块固态没有缓存，在网上查了查风评甚好，就入了。

结果三天掉了两次盘，具体表现是用着用着系统突然死了，无法新建任何进程，机器上的硬盘灯直接灭了。journal log由于需要持久化存储到硬盘上，因此log也没看见。我以为我是那个掉盘倒霉蛋，但其实并不是，我只要长按电源键重启电脑，硬盘是可以直接识别的。这跟掉盘的表现不太一样，掉盘的话再次开机应该会直接不识盘，等待主控自行修复完成之后才能正常挂载开机。但是我这里只要强制重启了就一定能跑，这就很奇怪了。最后找到了[这个](https://lore.kernel.org/all/82fa489d-a14b-58d9-7bd9-67418a02a0d3@nvidia.com/t/)和[这个](https://www.spinics.net/lists/stable/msg645104.html)，确认了是硬盘深度休眠结果直接睡死了。

遂禁用掉最深的休眠状态，问题就消失了。

## 容器

本来是想继续用Docker的，后来看了看podman，无守护进程的设计感觉还不错，于是就搞了podman，甚至还装了一个k3s（但是一次也没用到）。

目前用起来唯一的问题是podman没有守护进程，因此每次开机之后无法恢复之前的容器运行状态，得手动启动一下。对于一些必要服务，倒也有解决方案：用systemd来启动。例如：

```shell
podman generate systemd --new --name 服务名 -f
```

可以生成一个systemd服务文件，复制到systemd文件夹下然后作为用户自启任务即可。
