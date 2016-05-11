---
layout: post
title:  "终端命令行基本命令"
date:   2016-04-09 11:20:17
categories: iosre kaupelot linux
---

# 一、Linux基本命令

关于执行文件路径的变量：

echo 有显示打印出的意思、

在root下列出目前显示的路径:

    echo $PATH 
    
    [root@HadoopMaster ~]# echo $PATH
    /usr/lib64/qt-3.3/bin:
    /usr/local/sbin:/usr/local/bin:
    /sbin:/bin:/usr/sbin:/usr/bin:
    /usr/local/jdk1.8.0_11/bin:
    /usr/local/jdk1.8.0_11/jre/bin:
    /root/bin
    

================================================================
文件目录管理

查看文件与目录：

    ls 
    ls -a   全部文件，
     -d   仅列出目录本身，而不是目录内文件的内容（常用）  
    -f   直接 列出结果，而不进行排序：ls默认
    -F   根据文件，目录等信息给予附加数据结构
    -h   以人类较易读取的方式列出来
    -i   列出inode号码
    -l   列出长数据串包含文件的属性与权限等数据（常用）
    
    [root@HadoopMaster Desktop]# ls -i
    138916 Eclipse.desktop            138942 nginx.txt~
    132103 nginx+redis+tomcat         139507 Nginx瀹瑁
    缃畘
    139438 nginx+redis+tomcat???鼐???  139404 redis瀹瑁
    缃畘
    138922 Nginx+tomcat+redis????     138882 ??钥.txt
    

================================================================
复制，删除，与移动 ：cp rm mv 

要复制文件，使用cp 

功能: 复制文件或目录

说明: cp指令用于复制文件或目录，如同时指定两个以上的文件或目录，且最后的目的地是一个已经存在的目录，

则它会把前面指定的所有文件或目录复制到此目录中。若同时指定多个文件或目录，而最后的目的地并非一个已存在的目录，则会出现错误信息

示例:
    .复制文件，只有源文件较目的文件的修改时间新时，才复制文件
     cp -u -v file1 file2

    .将文件file1复制成文件file2
     cp file1 file2
    
    .采用交互方式将文件file1复制成文件file2
     cp -i file1 file2
    
    .将文件file1复制成file2，因为目的文件已经存在，所以指定使用强制复制的模式
     cp -f file1 file2
    
    .将目录dir1复制成目录dir2
     cp -R file1 file2
    
    .同时将文件file1、file2、file3与目录dir1复制到dir2
    
    cp -R file1 file2 file3 dir1 dir2
    
    .复制时保留文件属性
     cp -p a.txt tmp/
    .复制时保留文件的目录结构
     cp -P  /var/tmp/a.txt  ./temp/
    .复制时产生备份文件
     cp -b a.txt tmp/
    .复制时产生备份文件，尾标 ~1~格式
     cp -b -V t   a.txt /tmp    
    .指定备份文件尾标    
      cp -b -S _bak a.txt /tmp
    

===============================================================

查看进程

    ps -ef | grep tomcat
    ps -ef 进程号
    
    kill -9 #进程号 一律干掉
    
    find xxxx | grep java.lang