---
layout: post
title:  "树莓派2代试玩"
date:   2016-03-23 22:19:54
categories: jekyll update
---
有时间,闲鱼上面入手了一部树莓派b+,卖家给包装的不错,快递送过来直接点亮了,系统是完好的,配了显示器,十分好上手,就差更深入的学习了.

我的树莓派又要重新开机了。又重新买了一张 SD 卡。那么开始吧！

## 准备烧好镜像的 SD 卡

决定下载似乎对新手更友好的 NOOBS：[Download NOOBS for Raspberry Pi](https://www.raspberrypi.org/downloads/noobs/)

下载好之后记得看一下 sha1 对不对，Mac 下只要 `$ shasum NOOBS_v1_8_0.zip` 一下就行。

跟着 Setup Guide 走不吃亏：[Raspberry Pi NOOBS Setup](https://www.raspberrypi.org/help/noobs-setup/)

## 启动

还是依照上面的 Setup Guide，因为公司显示器什么的多，所以就省点力气直接接显示器、键盘鼠标来弄了。会顺利启动，然后直接安装 Raspbian 要等几分钟。（记得之前是直接 `dd` 把 Raspbian 的镜像烧到 SD 卡的，会更快一些。不过这次试试这个 NOOBS。）

现在的版本不用配置开机之后默认就会到图形界面了，蛮好的，那这一步就提前完成了。

## 配置

目标是：树莓派工作于命令行模式，用 SSH 可以登录，加入需要图形的话也可以连接 VNC。

接下来配置一些东西：

- 
给树莓派分配固定 IP

- 
设置树莓派的 SSH 和 VNC

- 
重启并测试

- 
配置系统语言

- 
用 zsh

### 分配 IP

现在树莓派上 `$ ifconfig` 到自己的 IP，然后去找路由器的设置，在 DHCP 里面添加对应的绑定就好了。

### SSH 和 VNC

一共三个文档可以参考：

- 
基础的 [SSH](https://www.raspberrypi.org/documentation/remote-access/ssh/) 配置

- 
省去输入密码的 [Passwordless SSH access](https://www.raspberrypi.org/documentation/remote-access/ssh/passwordless.md)

- 
图形界面的 [VNC](https://www.raspberrypi.org/documentation/remote-access/vnc/README.md)

这三个可以按顺序一个一个来。

SSH 是默认开启的不用担心。顺便发现图形界面里的 **Raspberry Pi Configuration** 和命令行工具里的内容差不多。之后设置语言的时候可能可以直接在这里设置。

Passwordless SSH access 因为之前才服务器上弄过，所以这个也轻松设置上了，没弄过的话就好好按教程来吧。

    vncserver :1 -geometry 1400x900 -depth 24

这样开启的 VNC 端口是 `5901`（参照[这里](http://elinux.org/RPi_VNC_Server)的解释）。我用的是 Chrome 的一个 VNC 插件连接的。想了想 VNC 不需要开机自启，反正主要是用 SSH 的，那就简单把上面的命令写在一个脚本文件里好了。

### 重启并测试

先要知道关机和重启的命令：

    # shutdown
    $ sudo shutdown -h now
    # reboot
    $ sudo shutdown -r now

发现图形界面里面修改的启动方式不生效，那么重新再命令行里设置吧：`$ sudo sudo raspi-config`。把启动改成 Console 且不自动登陆用户，现在还主要靠 SSH，之后可能要用上自动登陆。

经测试，开机后 45s 可以用 SSH 链接。

### 配置系统语言

在 SSH login 和开启 VNC 的时候会分别出现下面的错误提示：

    -bash: warning: setlocale: LC_ALL: cannot change locale (en_US.UTF-8)
    -bash: warning: setlocale: LC_ALL: cannot change locale (en_US.UTF-8)
    -bash: warning: setlocale: LC_ALL: cannot change locale (en_US.UTF-8)

    perl: warning: Setting locale failed.
    perl: warning: Please check that your locale settings:
        LANGUAGE = (unset),
        LC_ALL = "en_US.UTF-8",
        LC_CTYPE = "UTF-8",
        LANG = "en_GB.UTF-8"
        are supported and installed on your system.
    perl: warning: Falling back to a fallback locale ("en_GB.UTF-8").

语言暂且先全部设置成 "en_US.UTF-8" 吧。首先在 `raspi-config` 里的 Configuring locales 把下面两个语言打上勾：

    [*] en_US.UTF-8 UTF-8
    [*] zh_CN.UTF-8 UTF-8

Default Locale 选择 `en_US.UTF-8`。

这个设置完之后就解决了！应该是一开始勾选的问题吧，勾的是 en_GB，但用的是 en_US。

### 用 zsh

- 
install zsh

- 
config git user

- 
oh my zsh

OK， 至此树莓派又成功的跑起来了！

## 暂且存在的问题

- 
showdown 里面的 halt、poweroff 到底指什么

- 
locale 到底是什么概念

## 参考

有关 Linux 的关机命令（还是不太明白那个 halt 是什么，还有这些命令能不能和 GUI 里面的按钮对应上？）：