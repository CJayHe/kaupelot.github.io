---
layout: post
title:  "不到100块,就可以知道你家方圆50米收到了什么短信"
date:   2016-04-13 22:56:17
categories: iosre kaupelot
---

最近新闻比较多伪基站的案子,看到腾讯安全的一篇文章说是不到100块钱就可以弄一个伪基站,看完我很激动,开始在网上找短信嗅探和伪基站之类的东西.然后发现这些早在12年就被大神们玩剩下了.表示心有戚戚,看设备的时候发现使用到的摩托罗拉c118就是我人生的第一部手机.于是决定来一发.跑到闲鱼上面看到一部35块钱,入手了,再到淘宝买了c2102的接口和,c118的耳机线.然后给parallels上面的kali安装环境.终于在原料到齐全的第一天嗅探到了周边的短信,3个小时内收到了12条短信.没办法,我的移动号码,在这经常没有信号,所以gsm能够嗅探到时十分正常的了.不过是看不到收信人的信息的,发信人和短信内容清清楚楚.
下面是教程.

之前看到[那篇GSM_SMS嗅探文章](http://bbs.pediy.com/showthread.php?t=182574)后就开始试验了，但是按照文中的方法进行总是有错误，可能是我操作有问题吧。在这里，把路上总结的一些资料发出来，希望其他人别走弯路了。

网络上找得到的几处资料都或多或少存在一些错误或者没讲清楚的地方，可能原作者在修正错误后没有把过程写进去。无法找到某些资料原作者是谁了，只能在此表示感谢！

关于系统，虽然几个主流系统都可以，但是还是觉得Kali舒服，之前用别的系统出了好几个莫名其妙的错误。我这里用的是Win7_x64下VMware_10.0.0运行Kali_x64的VMware版镜像。
(一)环境配置

(1)添加源。vim看着就不舒服，用Leafpad直接开终端

代码:

    leafpad /etc/apt/sources.list
    

把这三行复制到后面，保存。

代码:

    deb http://mirrors.ustc.edu.cn/kali kali main non-free contrib
    deb-src http://mirrors.ustc.edu.cn/kali kali main non-free contrib
    deb http://mirrors.ustc.edu.cn/kali-security kali/updates main contrib non-free
    

![图片1](http://bbs.pediy.com/attachment.php?attachmentid=91032&amp;thumb=1&amp;d=1406449296)
![图片2](http://bbs.pediy.com/attachment.php?attachmentid=91032&amp;d=1406449296)

(2)乱七八糟的都配置好。最好不要一次都复制进去执行，尤其是apt-get和wget可能会因为网络原因出错。

代码:

    apt-get update
    aptitude install libtool shtool autoconf git-core pkg-config make gcc
    apt-get install build-essential libgmp3-dev libmpfr-dev libx11-6 libx11-dev texinfo flex bison libncurses5 \
    libncurses5-dbg libncurses5-dev libncursesw5 libncursesw5-dbg libncursesw5-dev zlibc zlib1g-dev libmpfr4 libmpc-dev
    wget -c http://bb.osmocom.org/trac/raw-attachment/wiki/GnuArmToolchain/gnu-arm-build.2.sh
    chmod +x gnu-arm-build.2.sh
    mkdir build install src
    cd src/
    

(3)下载3个包。直接用迅雷下好拖进src文件夹也行。

代码:

    wget http://www.gnuarm.com/bu-2.16.1_gcc-4.0.2-c-c++_nl-1.14.0_gi-6.4_x86-64.tar.bz2
    wget http://ftp.gnu.org/gnu/binutils/binutils-2.21.1a.tar.bz2
    wget ftp://sources.redhat.com/pub/newlib/newlib-1.19.0.tar.gz
    

![名称:  2.png
查看次数: 0
文件大小:  10.9 KB](http://bbs.pediy.com/attachment.php?attachmentid=91033&amp;thumb=1&amp;d=1406449296)
(5)下面也是很多步骤都需要漫长的等待，不要着急，最好不要一次复制太多行进去

代码:

    cd ..
     ./gnu-arm-build.2.sh
    echo "export PATH=\$PATH:/root/install/bin">/root/.bashrc
    source /root/.bashrc
    cd ~ 
    git clone git://git.osmocom.org/libosmocore.git
    cd libosmocore/
    autoreconf -i
    ./configure
    make
    make install
    cd ..
    .ldconfig
    git clone git://git.osmocom.org/osmocom-bb.git
    cd ~/osmocom-bb 
    git checkout --track origin/luca/gsmmap
    cd src
    git pull --rebase
    

准备好了，编译！

代码:

    make
    

顺利的话环境配置到这里就结束了。

(6)出现错误？(我没遇到任何错误，下面收集两个来自网络的错误)

A错误:

代码:

    /root/osmocom-bb/src/target/firmware/include/asm/swab.h: Assembler messages:
    /root/osmocom-bb/src/target/firmware/include/asm/swab.h:32: Error: no such instruction: `eor %edx,%ecx,%ecx,ror’
    make[4]: *** [gsmtap_util.lo] 错误 1
    make[4]: Leaving directory `/root/osmocom-bb/src/shared/libosmocore/build-target/src’
    make[3]: *** [all] 错误 2
    make[3]: Leaving directory `/root/osmocom-bb/src/shared/libosmocore/build-target/src’
    make[2]: *** [all-recursive] 错误 1
    make[2]: Leaving directory `/root/osmocom-bb/src/shared/libosmocore/build-target’
    make[1]: *** [all] 错误 2
    make[1]: Leaving directory `/root/osmocom-bb/src/shared/libosmocore/build-target’
    make: *** [shared/libosmocore/build-target/src/.libs/libosmocore.a] 错误 2
    

A解决：

代码:

    tar xf bu-2.16.1_gcc-4.0.2-c-c++_nl-1.14.0_gi-6.4_x86-64.tar.bz2
    mv gnuarm-* ~/gnuarm
    export PATH=~/gnuarm/bin:$PATH
    

B错误:

代码:

    make[1]: *** [board/compal_e88/hello_world.compalram.elf] 错误 1
    make[1]: Leaving directory `/root/osmocom-bb/src/target/firmware’
    make: *** [firmware] 错误 2
    

B解决:

代码:

    git clean -dfx
    make
    

(二)硬件配置
(1)准备需要的东西：

代码:

    MOTOROLA_C118手机
    CP201X(USB to TTL)
    数据线(2.5mm耳机头转杜邦线)如果自制有的需要用刀把接头处切掉一圈才能完全插进去
    

(2)连线

模块上有3V3、TXD、RXD、GND、+5V，只用中间的三根

我这里是：红TXD白RXD黄GND，直接修了一张简图在图右侧标识出来。
![图片3](http://bbs.pediy.com/attachment.php?attachmentid=91034&amp;thumb=1&amp;d=1406449296)

![图片4](http://bbs.pediy.com/attachment.php?attachmentid=91035&amp;thumb=1&amp;d=1406449296)

(3)识别
手机不用插卡，不用开机。把USB插进去，在虚拟机->可移动设备下面会有
![图片5](http://bbs.pediy.com/attachment.php?attachmentid=91036&amp;d=1406449296)

点连接(断开与 主机 的连接)

在Kali里面开个终端输入

代码:

    lsmod | grep usb
    

![图片6](http://bbs.pediy.com/attachment.php?attachmentid=91037&amp;d=1406449296)

(三)嗅探过程
(1)打开4个终端窗口ABCD

(2)在A中粘贴命令

代码:

    cd ~/osmocom-bb/src/host/osmocon/ 
    ./osmocon -m c123xor -p /dev/ttyUSB0 ../../target/firmware/board/compal_e88/layer1.compalram.bin
    

然后按下手机红色开机键

有人说这是在刷固件，但事实上它只不过把这一段程序写到手机中运行而已，并未影响手机本身的固件。
![图片7](http://bbs.pediy.com/attachment.php?attachmentid=91038&amp;d=1406449340)

(3)切换到B终端，粘贴命令

代码:

    cd ~/osmocom-bb/src/host/layer23/src/misc/
    ./cell_log -O
    

漫长的等待之后会显示出扫描到的基站信息。

拿自己的手机测试，苹果手机可以直接*3001#12345#*然后拨出

在Field Test中点GSM Cell Environment，进入GSM Cell Info，再进入GSM Serving Cell，查看当前ARFCN的值

如果不用自己的手机测试，直接在扫描到的列表里随便选一个ARFCN好了。

(4)切换到C终端，粘贴命令

代码:

    cd ~/osmocom-bb/src/host/layer23/src/misc/
    ./ccch_scan -i 127.0.0.1 -a 刚才的ARFCN
    

第二行命令后面-a后加一个空格，然后输入记录的ARFCN值

可以看到控制台疯狂刷屏

(5)切换到D终端，粘贴命令

代码:

    wireshark -k -i lo -f 'port 4729'
    

在打开的Wireshark上修改成gsm_sms过滤，回车。
![图片8](http://bbs.pediy.com/attachment.php?attachmentid=91039&amp;d=1406449340)

到这里就基本结束了，至于抓uplink，需要拆掉一个RX滤波器，手头没工具，以后再说吧。

关于脚本，@木桩 的原文写的已经相当详细了，没有必要重新发一遍。

原文链接：http://bbs.pediy.com/showthread.php?t=182574