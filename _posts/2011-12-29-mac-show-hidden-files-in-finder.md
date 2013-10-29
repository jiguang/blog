---
author: jiguang
title: Mac系统隐藏文件（Finder）的两种方法
excerpt:
layout: post
category:
  - Mac
tags: [ ]
---
{% include JB/setup %}

[mac][1]系统的Finder中默认是不显示隐藏文件的，搜索也搜不到。下面介绍两种显示和隐藏文件的办法。

打开终端，通过下面的两行命令可以控制是否显示隐藏文件，注意大小写：

显示Mac隐藏文件的命令：

    defaults write com.apple.finder AppleShowAllFiles -bool true

隐藏Mac隐藏文件的命令：

    defaults write com.apple.finder AppleShowAllFiles -bool false

键入相应命令后回车，然后关闭终端并重启Finder后才能生效。

另外还有一条终端命令可以直接控制文件或文件夹的显隐：

隐藏文件/文件夹命令：

    chflags hidden (文件/文件夹的相对或绝对路径)

例如输入：

    chflags hidden /Users/jiguang/Desktop/test

就可以隐藏桌面上的test文件夹

显示文件/文件夹命令：

    chflags nohidden (文件/文件夹的相对或绝对路径)

例如输入：

    chflags nohidden /Users/jiguang/Desktop/test

就可以显示刚才隐藏掉的test文件夹了。

这两种方法都只是设置Finder中的文件的显隐的，对于会使用终端的同学就不管用了。所以，还是换个地方把[苍井空][2]、[天海翼][3]、[小泽玛利亚][4]等姐妹们好好保管吧。

 [1]: http://jiguang.github.com/index.php/tag/mac/
 [2]: http://www.baidu.com/s?bs=mac%CF%B5%CD%B3%C8%E7%BA%CE%CF%D4%CA%BE%BA%CD%D2%FE%B2%D8%CE%C4%BC%FE&f=8&rsv_bp=1&rsv_spt=3&wd=%B2%D4%BE%AE%BF%D5&inputT=1572
 [3]: http://www.baidu.com/s?bs=%B2%D4%BE%AE%BF%D5&f=3&rsv_bp=1&rsv_spt=3&wd=%CC%EC%BA%A3%D2%ED&oq=%CC%EC%BB%B9%D2%D4&rsp=0&rsv_sug2=0&inputT=3695
 [4]: http://www.baidu.com/s?bs=%CC%EC%BA%A3%D2%ED&f=8&rsv_bp=1&rsv_spt=3&wd=%D0%A1%D4%F3%C2%EA%C0%FB%D1%C7&inputT=2213