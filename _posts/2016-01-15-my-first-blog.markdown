---
layout: post
title:  “我也只是试试这个怎么用”
date:   2016-01-15 3:51:42
categories: jekyll update
---
关于在jekyll怎么写文章,我也不知道的啦!反正先试试.

Reveal是一个很强大的UI分析工具，可非常直观地查看app的UI布局，不仅限于自己的app，其他app的UI布局也一览无余。下面就是要干这事...
必须要一台越狱手机
越狱教程：http://www.pangu.io
安装openSSH（Cydia源里安装）
在mac终端通过openSSH命令连接越狱机

1.越狱机与mac必须连接同一网络
2.在终端命令中输入ssh root@越狱机网络IP，如：ssh root@192.168.2.2
3.输入您修改过的Root密码，默认：alpine

注意：连接越狱机时，可能出现下面错误（未出错请忽略这块）

RSA host key for 192.168.2.2 has changed and you have requested strict checking.
Host key verification failed.
这是openssh-server重装引起的，执行以下命令即可解决

    ssh-keygen -R 192.168.2.2 (192.168.2.2换成你要连的手机网络IP)
安装MobileSubstrate（Cydia源里安装）
...

安装Reveal（Mac上）
1.官网：http://revealapp.com
2.可下载试用版：download a trial (当然你也可以买滴~~)
3.如果之前下载过又过期了，可以调整系统时间（调前一两年）
1.打开Revela，找到libReveal.dylib、Reveal.framework
<img src="http://upload-images.jianshu.io/upload_images/295346-d5b5d4e236539aec.png" alt="替代文本" title="标题文本" width="400" />
2.拷贝Reveal.framework到越狱机 （注意：重新开个终端，无需连接越狱机）

    scp -r /Users/apple/Desktop/Reveal.framework  root@192.168.2.2:/System/Library/Frameworks
<img src="http://upload-images.jianshu.io/upload_images/295346-5c2de59a14c5f8f1.png" alt="替代文本" title="标题文本" width="400" />
可到越狱机查看注入的文件:


3.拷贝libReveal.dylib到越狱机

    scp -r /Users/apple/Desktop/libReveal.plist root@192.168.2.2:/Library/MobileSubstrate/DynamicLibraries/

4.在本地创建libReveal.plist，编辑libReveal.plist，指定app的Bundle identifier（可以指定多个Bundle identifier），下面添加App Store与简书的Bundle identifier
（注意：如何找app对应的Bundle identifier请看后面）


拷贝libReveal.plist到越狱机

    scp -r /Users/apple/Desktop/libReveal.plist root@192.168.2.2:/Library/MobileSubstrate/DynamicLibraries/

可越狱机查看注入的文件:

    # cd /Library/MobileSubstrate/DynamicLibraries/
    # ls

重启手机（或执行命令killall SpringBoard），打开App Store或简书app，随后在Reveal右上角选择
（注意：有可能白苹果，解决方案请看后面）


App Store

简书app
如何找到app的Bundle identifier？
1.终端连接越狱机后，输入：    cd /private/var/mobile/Containers/Bundle/Application

2.查看手机所有app资源文件输入：ls （列出手机上所有app的资源文件，怎么找？）

ps:查看当前打开的app的bundle id的最简单方法是:在iPhone中关闭掉其他的app,然后在终端输入 ps aux 注意查看一般,最长的那个一般就是当前app的路径.

3.打开iTunes->我的应用程序->右键简书app->在Finder中显示->解压ipa->进入解压文件->Payload->右键应用程序->显示包内容->找到可执行文件(黑色)->复制文件名 (iOS9之后已经不能通过iTunes查看到安装的app了,除非是在iTunes上面安装的store app,所以此方法已经不是那么有效,不过对于越狱后hook store app 给未越狱设备安装还是可以考虑从iTunes安装app.建议在MacBook上安装iTools或者别的iDevice工具,方便管理越狱设备.比如此步骤就可以直接在iTools的程序选项中将app导出ipa文件到本地解压操作.显示包内容之后就可以看到plist文件了,直接用文本工具或者Xcode打开会更加直接)

可执行文件：


查找简书app资源文件路径：find . -name 'Hugo*'


简书app资源文件名：BE1895C3-5CB0-4894-8A9E-FE4EEF4C7989

4.安装iFile（Cydia源里安装）
进入iFile打开iTunesMetadata.plist：/private/var/mobile/Containers/Bundle/Application/BE1895C3-5CB0-4894-8A9E-FE4EEF4C7989/iTunesMetadata.plist

下图红框里的comjianshu.Hugo即为简书的Bundle identifier


5.打开Xcode
如果您是使用的非app store的方式安装的app,比如cydia,25pp助手.你可以直接在Xcode -> Window -> Devices -> 您的设备查看所有安装的app的Bundle identifier.

6.

重启手机后白苹果，无法进入界面
预估原因：libReveal.plist文件格式错误导致

第一种方法：

长按【home键】+【电源键】->在白苹果刚刚出现的时候->按住iPhone的音量增加键->iPhone进入Safe Mode->正常重启->将libReveal.plist文件删掉
第二种方法：

第一步：重新恢复系统
1.打开iTunes，越狱机与电脑连接 -> 长按【home键】+【电源键】6秒强制关机 -> 随后长按【home】键6秒，直到在电脑上看到识别到DFU状态下的USB设备时就进入到DFU模式
2.随后你有两个选择，升级更高系统或恢复出厂状态。(系统都会自动会升级到最高版本9.3.1)
第二步：降低系统，目前越狱只支持9.1以下
1.找到8.4的固件(下载地址: http://jailbreak.25pp.com/gujian/ 固件区分: http://bbs.25pp.com/thread-108715-1-1.html)
2.手机连接mac->打开iTunes->选择设备->摘要->option + 点击更新/恢复iPhone)->选择下载的固件 -> 自动安装直到成功
第三步：越狱
1.用太极软件越狱 (下载地址：http://www.pangu.io)


原文链接：http://www.jianshu.com/p/d172826fe578
