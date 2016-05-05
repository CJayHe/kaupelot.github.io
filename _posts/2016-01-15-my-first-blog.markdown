---
layout: post
title:  “我也只是试试这个怎么用”
date:   2016-01-15 3:51:42
categories: jekyll update
---
关于在jekyll怎么写文章,我也不知道的啦!反正先试试.

<div class="show-content"><blockquote><p>Reveal是一个很强大的UI分析工具，可非常直观地查看app的UI布局，不仅限于自己的app，其他app的UI布局也一览无余。下面就是要干这事...</p></blockquote>
<h3>必须要一台越狱手机</h3>
<blockquote><p>越狱教程：<a href="http://www.taig.com" target="_blank">http://www.taig.com</a></p></blockquote>
<h3>安装openSSH（Cydia源里安装）</h3>
<p>在mac终端通过openSSH命令连接越狱机</p>
<blockquote><p>1.越狱机与mac必须连接同一网络<br>  2.在终端命令中输入ssh root@<code>越狱机网络IP</code>，如：ssh root@192.168.2.2<br>  3.输入您修改过的Root密码，默认：<code>alpine</code></p></blockquote>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/295346-9cc541e11fda0e54.png?imageMogr2/auto-orient/strip%257CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/295346-9cc541e11fda0e54.png"><br><div class="image-caption"></div>
</div>
<p>注意：连接越狱机时，可能出现下面错误（未出错请忽略这块）</p>
<blockquote><p>RSA host key for 192.168.2.2 has changed and you have requested strict checking.<br>Host key verification failed.</p></blockquote>
<p>这是openssh-server重装引起的，执行以下命令即可解决</p>
<blockquote><p><code>ssh-keygen -R 192.168.2.2</code>  (192.168.2.2换成你要连的手机网络IP)</p></blockquote>
<h3>安装MobileSubstrate（Cydia源里安装）</h3>
<p>...</p>
<h3>安装Reveal（Mac上）</h3>
<blockquote><p>1.官网：<a href="http://revealapp.com" target="_blank">http://revealapp.com</a><br>2.可下载试用版：download a trial (当然你也可以买滴~~)<br>3.如果之前下载过又过期了，可以调整系统时间（调前一两年）</p></blockquote>
<p>1.打开Revela，找到<code>libReveal.dylib</code>、<code>Reveal.framework</code></p>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/295346-d5b5d4e236539aec.png?imageMogr2/auto-orient/strip%257CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/295346-d5b5d4e236539aec.png"><br><div class="image-caption"></div>
</div>
<p>2.拷贝<code>Reveal.framework</code>到越狱机 （注意：重新开个终端，无需连接越狱机）</p>
<pre><code class="objc">scp -r /Users/apple/Desktop/Reveal.framework  root@192.168.2.2:/System/Library/Frameworks</code></pre>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/295346-5c2de59a14c5f8f1.png?imageMogr2/auto-orient/strip%257CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/295346-5c2de59a14c5f8f1.png"><br><div class="image-caption"></div>
</div>
<p>可到越狱机查看注入的文件:</p>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/295346-8fe828830756372b.png?imageMogr2/auto-orient/strip%257CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/295346-8fe828830756372b.png"><br><div class="image-caption"></div>
</div>
<p>3.拷贝<code>libReveal.dylib</code>到越狱机</p>
<pre><code class="objc">scp -r /Users/apple/Desktop/libReveal.plist root@192.168.2.2:/Library/MobileSubstrate/DynamicLibraries/</code></pre>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/295346-51cb0b76ce3a2208.png?imageMogr2/auto-orient/strip%257CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/295346-51cb0b76ce3a2208.png"><br><div class="image-caption"></div>
</div>
<p>4.在本地创建<code>libReveal.plist</code>，编辑<code>libReveal.plist</code>，指定app的<code>Bundle identifier</code>（可以指定多个<code>Bundle identifier</code>），下面添加<code>App Store</code>与<code>简书</code>的<code>Bundle identifier</code><br>（注意：如何找app对应的<code>Bundle identifier</code>请看后面）</p>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/295346-75f59b872bc774cd.png?imageMogr2/auto-orient/strip%257CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/295346-75f59b872bc774cd.png"><br><div class="image-caption"></div>
</div>
<p>拷贝<code>libReveal.plist</code>到越狱机</p>
<pre><code class="objc">scp -r /Users/apple/Desktop/libReveal.plist root@192.168.2.2:/Library/MobileSubstrate/DynamicLibraries/</code></pre>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/295346-654a10e8477e5ea4.png?imageMogr2/auto-orient/strip%257CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/295346-654a10e8477e5ea4.png"><br><div class="image-caption"></div>
</div>
<p>可越狱机查看注入的文件:</p>
<pre><code class="objc"># cd /Library/MobileSubstrate/DynamicLibraries/
# ls</code></pre>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/295346-ce6119859c009e43.png?imageMogr2/auto-orient/strip%257CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/295346-ce6119859c009e43.png"><br><div class="image-caption"></div>
</div>
<h3>重启手机（或执行命令<code>killall SpringBoard</code>），打开<code>App Store</code>或<code>简书app</code>，随后在Reveal右上角选择</h3>
<p>（注意：有可能白苹果，解决方案请看后面）</p>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/295346-cdaee7ee5946b7d9.png?imageMogr2/auto-orient/strip%257CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/295346-cdaee7ee5946b7d9.png"><br><div class="image-caption">App Store</div>
</div>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/295346-01fb09a0fcdcd659.png?imageMogr2/auto-orient/strip%257CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/295346-01fb09a0fcdcd659.png"><br><div class="image-caption">简书app</div>
</div>
<h3>如何找到app的Bundle identifier？</h3>
<p>1.终端连接越狱机后，输入：<code>cd /private/var/mobile/Containers/Bundle/Application</code></p>
<p>2.查看手机所有app资源文件输入：<code>ls</code>  （列出手机上所有app的资源文件，怎么找？）</p>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/295346-2815acc01ddaac2b.png?imageMogr2/auto-orient/strip%257CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/295346-2815acc01ddaac2b.png"><br><div class="image-caption"></div>
</div>
<p>3.打开iTunes-&gt;我的应用程序-&gt;右键<code>简书app</code>-&gt;在Finder中显示-&gt;解压ipa-&gt;进入解压文件-&gt;Payload-&gt;右键应用程序-&gt;显示包内容-&gt;找到<code>可执行文件</code>(黑色)-&gt;复制文件名 (iOS9之后已经不能通过iTunes查看到安装的app了,除非是在iTunes上面安装的store app,所以此方法已经不是那么有效,不过对于越狱后hook store app 给未越狱设备安装还是可以考虑从iTunes安装app.建议在MacBook上安装iTools或者别的iDevice工具,方便管理越狱设备.比如此步骤就可以直接在iTools的程序选项中将app导出ipa文件到本地解压操作.显示包内容之后就可以看到plist文件了,直接用文本工具或者Xcode打开会更加直接)</p>
<p><code>可执行文件</code>：<br></p><div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/295346-4ff43aabc45845ca.png?imageMogr2/auto-orient/strip%257CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/295346-4ff43aabc45845ca.png"><br><div class="image-caption"></div>
</div>
<p>查找<code>简书app</code>资源文件路径：<code>find . -name 'Hugo*'</code></p>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/295346-6e858e92ce60eb16.png?imageMogr2/auto-orient/strip%257CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/295346-6e858e92ce60eb16.png"><br><div class="image-caption"></div>
</div>
<p><code>简书app</code>资源文件名：<code>BE1895C3-5CB0-4894-8A9E-FE4EEF4C7989</code> </p>
<p>4.安装iFile（Cydia源里安装）<br>进入iFile打开iTunesMetadata.plist：<code>/private/var/mobile/Containers/Bundle/Application/BE1895C3-5CB0-4894-8A9E-FE4EEF4C7989/iTunesMetadata.plist</code></p>
<p>下图红框里的<code>comjianshu.Hugo</code>即为简书的<code>Bundle identifier</code></p>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/295346-c935e5d5820d4a98.png?imageMogr2/auto-orient/strip%257CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/295346-c935e5d5820d4a98.png"><br><div class="image-caption"></div>
</div>
5.打开Xcode
如果您是使用的非app store的方式安装的app,比如cydia,25pp助手.你可以直接在Xcode -> Window -> Devices -> 您的设备查看所有安装的app的Bundle identifier.
<h3>重启手机后白苹果，无法进入界面</h3>
<p>预估原因：libReveal.plist文件格式错误导致</p>
<p>第一种方法：</p>
<blockquote><p>长按【home键】+【电源键】-&gt;在白苹果刚刚出现的时候-&gt;按住iPhone的音量增加键-&gt;iPhone进入Safe Mode-&gt;正常重启-&gt;将libReveal.plist文件删掉</p></blockquote>
<hr>
<p>第二种方法：</p>
<blockquote><p><code>第一步：重新恢复系统</code><br>1.打开iTunes，越狱机与电脑连接 -&gt; 长按【home键】+【电源键】6秒强制关机 -&gt; 随后长按【home】键6秒，直到在电脑上看到识别到DFU状态下的USB设备时就进入到DFU模式<br>2.随后你有两个选择，升级更高系统或恢复出厂状态。(系统都会自动会升级到最高版本8.4.1)<br><code>第二步：降低系统，目前越狱只支持8.4以下（8.4.1只能降到8.4）</code><br>1.找到8.4的固件(下载地址: <a href="http://jailbreak.25pp.com/gujian/" target="_blank">http://jailbreak.25pp.com/gujian/</a> 固件区分: <a href="http://bbs.25pp.com/thread-108715-1-1.html" target="_blank">http://bbs.25pp.com/thread-108715-1-1.html</a>)<br>2.手机连接mac-&gt;打开iTunes-&gt;选择设备-&gt;摘要-&gt;option + 点击<code>更新</code>/<code>恢复iPhone</code>)-&gt;选择下载的固件 -&gt; 自动安装直到成功<br><code>第三步：越狱</code><br>1.用太极软件越狱 (下载地址：<a href="http://www.taig.com" target="_blank">http://www.taig.com</a>)  （ps：pp助手，8.4越狱失败）</p></blockquote>
</div>
