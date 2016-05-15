---
layout: post
title:  "iPhone常用终端命令"
date:   2016-05-12 19:47:20
categories: iosre kaupelot
---

相信对Linux、Unix比较熟悉的朋友，在iphone或
ipad越狱后发现通过Cydia可以安装OpenSSH，一定都想安装上并且通过ssh登录上去看看，但是登录后却发现几乎没几个命令可用，也就只有ls、cd等一些常用的命令，至于ifconfig、ping、netstat等都没有。。。

下面就来介绍一下如何让iphone或
ipad拥有Linux、Unix常用的命令。

1、首先你的iphone或 ipad得先越狱，越狱后才有Cydia，才能安装OpenSSH。

 

2、记住在使用Cydia的时候，要选择“Developer”（开发者），如果一开始选择的是“User”，可以进入Cydia->Sources->Settings->Developer进行修改，否则搜索不到这些软件包。

 

3、安装并启动sshd后，通过ssh -l root
IPAD_IP_ADDRESS登录，默认口令是：alpine，这是ios系统默认的root密码，记得及时修改。当然如果可以不用这么启动，其实只要安装openssh后，服务就会默认启动的。如果没有可以像Windows一样重启设备也可以。

 

4、下面就是一些软件包的名字：

adv-cmds #finger,fingerd,last,lsvfs,md,ps

basic-cmds #msg,uudecode,uuencode,write

bc #计算器工具

cURL #就是curl了

Diff Utilities #diff

diskdev-cmds #mount,quota,fsck等，忘记是否默认安装的

file #常用的file命令

file-cmds #chflags，compress

Find Utilites #find

Gawk #awk

grep #grep

inetutils #ftp,inetd,ping,telnet…

less #less

links #links,文本浏览器

lsof #lsof

netcat #nc

network-cmds #arp,ifconfig,route,traceroute

ngrep #ngrep (Network grep).

Nmap #nmap

rsync #rsync

Screen #screen

sed #sed

shell-cmds #killall,mktemp,time,which

system-cmds #iostat,login,sync,sysctl

tcpdump #tcpdump

top #top

unrar #unrar备用

unzip #unzip

VI IMproved #vim

wget #wget

whois #whois

 

注意：以下内容都很重要！

其实ios系统属于unix系统分支BSD系统的一支：“Darwin”系统。

例如我的iphone 4：

login as: root
[root@192.168.91.34's](mailto:root@192.168.91.34's)password:

tutengyidumato-iPhone:~ root# uname
-a

Darwin tutengyidumato-iPhone 11.0.0 Darwin Kernel Version 11.0.0:
Tue Nov 1 20:33:58 PDT 2011;
root:xnu-1878.4.46~1/RELEASE_ARM_S5L8930X iPhone3,1 arm N90AP
Darwin

tutengyidumato-iPhone:~ root# uname -r

11.0.0

tutengyidumato-iPhone:~ root# hostname

tutengyidumato-iPhone

tutengyidumato-iPhone:~ root#信息说明：

以上信息显示，

系统以版本：11.0.0；

系统生成时间：Tue Nov 1 20:33:58 PDT
2011

内核版本：xnu-1878.4.46~1/RELEASE_ARM_S5L8930X
iPhone3,1 arm N90AP Darwin

主机名：tutengyidumato-iPhone。

既然同属于BSD系统，那么就会有其相同特征和命令使用方法，比如使用apt-get命令。这个命令可以再cydia中安装，只要在搜索中输入apt字符，就会显示出关于apt命令的所有软件包，如果是标记命令行软件包的，安装即可，就会安装上apt-get。

安装apt-get后，其实不用再在cydia中搜索以上命令的软件包了。只要使用如下格式：

例如：ipad2上面测试当前网络，无论是3g还是wifi是否可用，该怎么办？其实很简单，像Windows一样使用ping命令进行测试即可：

 

操作如下：

    zhouzhoumato-iPad:~ root# uname -a
    Darwin zhouzhoumato-iPad 11.0.0 Darwin Kernel Version 11.0.0: Tue Nov 1 20:34:16 PDT 2011; root:xnu-1878.4.46~1/RELEASE_ARM_S5L8940X iPad2,1 arm K93AP Darwin

    zhouzhoumato-iPad:~ root# uname -r
    11.0.0

    zhouzhoumato-iPad:~ root# hostname -sh: hostname: command not found
    zhouzhoumato-iPad:~ root#

上面信息显示：当前的ipad2设备连hostname都没有，所以首先安装一个hostname命令测试一下：

    zhouzhoumato-iPad:~ root# apt-get install hostname
    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    Note, selecting inetutils instead of hostname
    The following NEW packages will be installed:
    inetutils
    0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
    Need to get 0B/212kB of archives.
    After this operation, 889kB of additional disk space will be used.
    Selecting previously deselected package inetutils.
    (Reading database ...

    dpkg: serious warning: files list file for package
    `com.chronic-dev.greenpois0n.corona' missing, assuming package has
    no files currently installed. 2261 files and directories currently installed.)
    Unpacking inetutils (from .../inetutils_1.6-8_iphoneos-arm.deb) ...
    Setting up inetutils (1.6-8) ...
    zhouzhoumato-iPad:~ root#

 

安装ping命令：

zhouzhoumato-iPad:~ root# apt-get install
ping

Reading package lists... Done

Building dependency tree

Reading state information... Done

Note, selecting inetutils instead of ping

The following NEW packages will be installed:

inetutils

0 upgraded, 1 newly installed, 0 to remove and 0 not
upgraded.

Need to get 0B/212kB of archives.

After this operation, 889kB of additional disk space will be
used.

Selecting previously deselected package inetutils.

(Reading database ...

dpkg: serious warning: files list file for package
`com.chronic-dev.greenpois0n.corona' missing, assuming package has
no files currently installed.

2261 files and directories currently installed.)

Unpacking inetutils (from .../inetutils_1.6-8_iphoneos-arm.deb)
...

Setting up inetutils (1.6-8) ...

 

测试ping命令：

    zhouzhoumato-iPad:~ root# ping
    ping: missing host operand
    Try `ping --help' or `ping --usage' for more information.
    zhouzhoumato-iPad:~ root#
说明命令已经安装成功了。

 

使用ping命令测试网络：

    zhouzhoumato-iPad:~ root# ping[www.baidu.com](http://www.baidu.com/)

    PING [www.a.shifen.com](http://www.a.shifen.com/) (119.75.218.77): 56 data bytes

    64 bytes from 119.75.218.77: icmp_seq=0 ttl=52 time=31.919 ms

    64 bytes from 119.75.218.77: icmp_seq=1 ttl=52 time=40.037 ms

    ^C--- [www.a.shifen.com](http://www.a.shifen.com/) ping statistics ---

    2 packets transmitted, 2 packets received, 0% packet loss

    round-trip min/avg/max/stddev = 31.919/35.978/40.037/4.059 ms

    zhouzhoumato-iPad:~ root#

 

使用apt-get的一些操作：

所以使用apt-get去执行一些相关的安装、更新、删除软件的动作很方便。这样更新安装后iphone或ipad就可以像完整的linux系统一样工作了。

apt-get的安装： apt-get install
软件包名

        apt-get的更新：apt-get update 软件包名

        apt-get的删除：apt-get remove 软件包名

 

总结：

既然可以如此操作iphone、或ipad，那么如何在命令行模式下对iphone或ipad进行启动、关闭，播放等操作呢？

 

引申：

对其它各种软件各种操作。。。如何实现？

本人对ios系统没有深入研究，在网络上搜索了很多资料，无论是中文的还是英文的，都没有人如此实现过，等待后续测试操作。
http://blog.sina.com.cn/s/blog_51d3553f0100xrxz.html