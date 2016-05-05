---
layout: post
title:  “我也只是试试这个怎么用”
date:   2016-01-15 3:51:42
categories: jekyll update
---
关于在jekyll怎么写文章,我也不知道的啦!反正先试试.
<table>
    <tr>
        <td>Foo</td>
    </tr>
</table>

ImageMagick-exploit-hack.png
ImageMagick是一款广泛流行的图像处理软件，有无数的网站使用它来进行图像处理，但在本周二，ImageMagick披露出了一个严重的0day漏洞，此漏洞允许攻击者通过上传恶意构造的图像文件，在目标服务器执行任意代码。Slack安全工程师Ryan Hube发现了这一0day漏洞。


如果你在网站中使用了ImageMagick去识别，裁剪或者调整用户上传的图像，你必须确认已经使用了这些缓解措施，并且调整你的代码只接受有效的图像文件，沙盒ImageMagick也是一个不错的主意。

在这个安全漏洞公布之后，这一漏洞的EXP也随即被发布，并被命名为：ImageTragick。漏洞的EXP已经通过邮件和论坛广泛传播，所以如果你使用了ImageMagick去处理用户输入，请立即采取相应的缓解措施。

ImageMagick被许多编程语言所支持，包括Perl，C++，PHP，Python和Ruby等，并被部署在数以百万计的网站，博客，社交媒体平台和流行的内容管理系统(CMS)，例如WordPress和Drupal。

该漏洞的利用十分简单，通过上传一个恶意图像到目标Web服务器上，攻击者就可以执行任意代码，窃取重要信息，用户帐户等。

换句话说，只有采用了ImageMagick，且允许用户上传图像的网站，才会受到影响。

ImageMagick团队已经承认了此漏洞，称：

最近发布的漏洞报告……包含可能存在的远程代码执行。
虽然该团队还没有公布任何安全补丁，但它建议网站管理者应该在配置文件中添加几行代码去阻止攻击，至少在某些情况下可以防御。

Web管理员同时被建议在文件发送给ImageMagick处理前，检查文件的magic bytes。Magic bytes是一个文件的前几个字节，被用于识别图像类型，例如GIF，JPEG和PNG等。

为了让你更好地了解你将要面对的漏洞，下面提供一个可以瞒过ImageMagick的示例文件：

push graphic-context

viewbox 0 0 640 480

fill ‘url(https://example.com/image.jpg“|ls “-la)’

pop graphic-context
将其保存为任意的扩展名，例如expoit.jpg，然后通过ImageMagick去运行它

convert exploit.jpg out.png
是的，ImageMagick将会去执行嵌入的代码：ls -l命令。

将这条命令替换为其它的恶意命令，将会直接威胁到目标机器，不过你可能会触犯一些法律。

该漏洞将在ImageMagick 7.0.1-1和6.9.3-10版本中被修补，这些新版本预计将在周末前被公布。

预警：

ImageMagick的这个远程代码执行漏洞也将波及Wordpress博客网站以及Discuz论坛！有使用imageMagic模块来处理图片业务的公司&站长请注意：头像上传、证件上传、资质上传等方面的点尤其是使用到图片（批量）裁剪的业务场景！

漏洞描述：

据ImageMagick官方，目前程序存在一处远程命令执行漏洞（CVE-2016-3714），当其处理的上传图片带有攻击代码时，可远程实现远程命令执行，进而可能控制服务器，此漏洞被命名为ImageTragick。

ImageMagick是一款开源图片处理库，支持PHP、Ruby、NodeJS和Python等多种语言，使用非常广泛。包括PHP imagick、Ruby rmagick和paperclip以及NodeJS imagemagick等多个图片处理插件都依赖它运行。

可能的影响范围包括各类流行的内容管理系统（CMS）。

影响影响范围：

1、调用ImageMagick的库实现图片处理和渲染的应用。

ImageMagick 为多种语言提供了api。

2、很多流行的内容管理系统（CMS）使用了ImageMagick ，例如 WordPress 的图片处理插件已被证实存在远程命令执行的漏洞（Author 及以上权限用户执行）。

其他例如MediaWiki、phpBB和vBulletin 使用了ImageMagick 库生成缩略图，还有一些程序如LyX使用ImageMagick转换图片格式。以上应用可能受到此漏洞影响。

3、如果通过shell 中的convert 命令实现一些图片处理功能，也会受到此漏洞影响。

漏洞等级：

高危

解决方案：官方方案

通过配置策略文件暂时禁用ImageMagick，可在 “/etc/ImageMagick/policy.xml” 文件中添加如下代码：

<policymap>
  <policy domain="coder" rights="none" pattern="EPHEMERAL" />
  <policy domain="coder" rights="none" pattern="URL" />
  <policy domain="coder" rights="none" pattern="HTTPS" />
  <policy domain="coder" rights="none" pattern="MVG" />
  <policy domain="coder" rights="none" pattern="MSL" />
</policymap>
更多信息：

imagetragick.com ： ImageMagick Is On Fire — CVE-2016–3714 TL;DR

漏洞本地检测POC ：GitHub

 * 原文链接：thehackernews，theregister，watcher编译，转载请注明来自FreeBuf黑客与极客（FreeBuf.COM）