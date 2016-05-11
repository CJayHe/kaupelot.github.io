---
layout: post
title:  "iOS越狱开发环境搭建"
date:   2016-04-19 19:38:04
categories: iosre kaupelot
---
前置条件是安装xcode 安装command line tool

### 下载基础代码
    mkdir /opt/
    git clone --recursive https://github.com/kirb/theos.git
由于kirb已经将项目并入theos,所以可以直接使用:
git clone --recursive https://github.com/theos/theos.git

### 修改环境变量:
    vim ~/.zshrc //如果你使用的bash,则为:~/.bash_profile
    export THEOS=/opt/theos
    export PATH=$THEOS/bin:$PATH
    export nic.pl=/opt/theos/bin/nic.pl
 

### 安装ldid
    cd/opt/theos/bin
    curl  http://joedj.net/ldid
    sudo chmod777ldid

### 导入include头文件和非必要的类
下载https://github.com/hbang/headers,然后将所有的头文件复制到/opt/theos/include目录中
#####假设你的越狱iPhone的内网地址是192.168.1.2,开启ssh.从其复制两个文件出来.
    sudo scp root@192.168.1.2:/Library/Frameworks/CydiaSubstrate.framework/Headers/CydiaSubstrate.h $THEOS/include/substrate.h
    sudo scp root@192.168.1.2:/Library/Frameworks/CydiaSubstrate.framework/CydiaSubstrate $THEOS/lib/libsubstrate.dylib

### 安装dpkg (如果没有安装brew,建议参考)
    brew install dpkg
### 安装模板
    git clone https://github.com/DHowett/theos-nic-templates.git
将下载下来的几个tar文件，放入到/opt/theos/templates/ios路径下

 
### 新建项目
    nic.pl
如果出现error,请到其他目录下新建项目。不要在/opt/theos目录下建立
![new](http://zhenhappy.github.io/assets/images/iOS-jailbreak-development-environment-1/1454494321547.jpg)

### 编译和运行
    make
    make package
    make package install
![run](http://zhenhappy.github.io/assets/images/iOS-jailbreak-development-environment-1/1454580724047.jpg)

#### 玩的开心!