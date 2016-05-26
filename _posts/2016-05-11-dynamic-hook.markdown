---
layout: post
title:  "在模拟器中学习tweak"
date:   2016-05-10 12:58:50
categories: iosre kaupelot
---

https://gist.github.com/friggog/03a2483b8acb9abdcd34





前几天才发现可以在 iOS 模拟器上执行 mobilesubstrate 的 tweak

对开发者来说真的是很方便

因此参考 [这篇文章](http://sharedinstance.net/2013/10/running-tweaks-in-simulator/) 写了一些记录

不过似乎只限于 hook SpringBoard 的 tweak

其他部分我还没有去测试

![http://i.imgur.com/Mburd3r.png](http://i.imgur.com/Mburd3r.png)

- - - -

以下为过程记录，搭配 theos

下载 MobleSubstrate

$ curl -OL http://apt.saurik.com/debs/mobilesubstrate_0.9.4001_iphoneos-arm.deb

解压缩然后在本机配置 library 

$ dpkg-deb -x mobilesubstrate_0.9.4001_iphoneos-arm.deb substrate

$ sudo mv substrate/Library/Frameworks/CydiaSubstrate.framework /Library/Frameworks

$ sudo mv substrate/Library/MobileSubstrate /Library/MobileSubstrate

$ sudo mv substrate/usr/lib/* /usr/lib

接着要修改模拟器来于执行 SpringBoard 时注入 tweak

由于 iOS7 之后的模拟器是由 launchd_sim 来启动 因此旧有的方法无法使用

不过还是可以透过修改 SDK 中 LaunchDaemon 的 plist 达成

先来到

/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator7.0.sdk/System/Library/LaunchDaemons

然后将 com.apple.SpringBoard.plist 备份到别处 

( 放在这目录会造成 SpringBoard 被执行两次 )

编辑 com.apple.SpringBoard.plist 新增一个名为 EnvironmentVariables 的 Dictionary

并在 EnvironmentVariables 下新增一个 Key 名为 DYLD_INSERT_LIBRARIES

内容为你自己的 tweak 的 dylib 路径 ( 不是 MobileSubstrate.dylib )

例如

/Library/MobileSubstrate/DynamicLibraries/SpotLock7.dylib

![http://i.imgur.com/0ovXHEz.png](http://i.imgur.com/0ovXHEz.png)

- - - -

再来 需要修改 theos

开启 $THEOS/makefiles/targets/Darwin/simulator.mk

找到这行

_TARGET_OSX_VERSION_FLAG = -mmacosx-version-min=$(if $(_TARGET_VERSION_GE_4_0), 10.6,10.5)

取代为

_TARGET_VERSION_GE_7_0 = $(call __simplify,_TARGET_VERSION_GE_7_0,$(shell $(THEOS_BIN_PATH)/vercmp.pl $(_THEOS_TARGET_SDK_VERSION) ge 7.0))

_TARGET_OSX_VERSION_FLAG = $(if $(_TARGET_VERSION_GE_7_0),-miphoneos-version-min=7.0,-mmacosx-version-min=$(if $(_TARGET_VERSION_GE_4_0),10.6,10.5))

- - - - 

再来处理 linking 部分

libsubstrate.dylib 并没有 x86 的 slice ，因此要用这个带有 x86 slice 的版本来取代原本的

$ curl -O http://cdn.hbang.ws/dl/libsubstrate_ios7sim.dylib

$ mv $THEOS/lib/libsubstrate.dylib libsubstrate.dylib.bak

$ mv ./libsubstrate_ios7sim.dylib $THEOS/lib/libsubstrate.dylib

- - - -

最后来修改一下我们 tweak 的 makefile ，使其可以正确编译出给 simulator 执行的 dylib 

先加上

export IPHONE_SIMULATOR_ROOT=/Applications/Xcode.app/Contents/Developer/Platfor

ms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator7.0.sdk

然后指定 TARGET 为 simulator

TARGET = simulator

执行 make install 之后 tweak 就会被安装到 simulator 上

若要重启 SpringBoard 必须自己手动 killall -9 SpringBoard

( 原来模拟器的 SpringBoard 运行在本机 真酷 )

- - - - 

不过 theos 的 make install 可能会遇到奇怪的权限问题 喷一些error 看起来很不爽

我自己的懒人解决方式:

在 makefile 加上

sim:

 cp ./obj/iphone_simulator/SpotLock7.dylib $(IPHONE_SIMULATOR_ROOT)/Library/MobileSubstrate/DynamicLibraries/

 cp ./SpotLock7.plist $(IPHONE_SIMULATOR_ROOT)/Library/MobileSubstrate/DynamicLibraries/

 killall -9 SpringBoard

( 记得先修改模拟器目录权限 )

之后执行 make sim 即可