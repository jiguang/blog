---
author: jiguang
title: Mac Lion系统配置php环境
layout: post
category:
  - Mac
tags:
  - Apache
---
{% include JB/setup %}

这里介绍的是利用 [mac][1] 系统自带的 Apache 和 PHP 进行配置，而不使用如 [MAMP][2] 或者 [XAMPP][3] 这样的集成环境。关于集成环境的安装非常简单，可以仔细阅读一下官方的文档，这里不再赘述。

Mac OS X 系统自带就有 Apache 和 PHP ，可以直接使用。这里有两种方式可以启动 Apache：

第一种：打开“System Preferences -> Sharing ”，勾选“Web Sharing”；

第二种：打开“终端（terminal）”，运行“sudo apachectl start”，再输入你的管理员密码即可运行 Apache了。

关于 apachectl 这个命令，它是Apache超文本传输协议服务器的前端程序。 其设计意图是帮助管理员控制Apache httpd 后台的功能，你可以在终端中输入：

    man apachectl

来查看命令手册，或者[在网上搜索 apachectl ][4]这个命令。

此时，你可以继续在终端中输入如下命令，来查看localhost的响应：

    curl localhost

如果有响应内容，说明 Apache 服务器已经成功启动，或者直接打开浏览器，输入 <http://localhost> ，如果出现了 “It Works！”就说明启动成功了，文件目录就在“/Library/WebServer/Documents/”目录中，这个目录是Apache的默认根目录。

下面配置 PHP 环境，打开终端，运行：

    sudo vi /etc/apache2/httpd.conf

使用vi命令 “/php5” 搜索到这样一句：

    #LoadModule php5_module libexec/apache2/libphp5.so

将这句前面的＃号删掉，然后 Esc -> :w! -> Enter -> :q 退出 vi 编辑返回终端即可。

下面还要新建一个 PHP 的配置文件，方便以后进行个性化的配置，运行：

    sudo cp /etc/php.ini.default /etc/php.ini

这样就创建了一个配置文件，目录如命令中所示，你一定可以找到它。

最后一步，重启 Apache 服务器：

    sudo apachectl restart

至此， Apache 和 PHP 的环境就配置完成了，下面就可以在 /Library/WebServer/Documents/ 这个目录放置你的PHP文件进行测试了。

PS: 写完本文后才发现[秦歌][5]已经写过一篇非常详尽的文章[《在Mac OS X中配置Apache ＋ PHP ＋ MySQL》][6]，网上大部分都是抄袭他的博客的内容。大家可以参照这篇文章里的步骤安装 MySQL 和 PhpMyAdmin 等，我就不再赘述了。

 [1]: http://jiguang.github.com/index.php/tag/mac/ "mac"
 [2]: http://www.mamp.info/en/index.html
 [3]: http://www.apachefriends.org/en/xampp-macosx.html
 [4]: http://www.google.com.hk/search?hl=zh-CN&newwindow=1&safe=strict&q=apachectl&oq=apachectl&aq=f&aqi=&aql=&gs_sm=e&gs_upl=1853533l1857914l0l1858055l18l11l0l0l0l0l0l0ll0l0
 [5]: http://dancewithnet.com/about/
 [6]: http://dancewithnet.com/2010/05/09/run-apache-php-mysql-in-mac-os-x/