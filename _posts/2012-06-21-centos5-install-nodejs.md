---
layout: post
title: CentOS 5.5 上安装 Node.js
category:
  - NodeJs
tags:
  - nodejs
---
{% include JB/setup %}

下面是我的作法，没什么问题，如果在你那里行不通可不要怪我哦。

1， 安装 CentOS 开发工具包：

    sudo yum groupinstall 'Development Tools'

2，安装 Open SSL 开发包 （Node.js必须，用来支持HTTPS）：

    sudo yum install openssl-devel 

3，找个合适的位置下载Node.js包，我一般用根路径（root）：

    cd /root

4，下载Node.js包（请到Node.js官网上获取最新版本）：

    wget http://[nodejs][1].org/dist/v0.6.19/node-v0.6.19.tar.gz

5，解压下载后的压缩包：

    gunzip node-v0.6.19.tar.gz

6，解压node-v0.6.19.tar包：

    tar -xf node-v0.6.19.tar

7，在第六步应该会在你当前的目录中创建一个目录，本例中我在 /root。 CD 到那个目录：

    cd node-v0.6.19

8， 运行配置：

    ./configure

9， 编译 Node.js：

    make

10， 安装编译后的文件：

    make install

现在Node.js应该就可以工作了！我还创建了一个目录 /var/node 方便放置我全部的Node代码。

参考文献：http://stuff.blat.co.za/2011/06/03/installing-node-js-on-centos-5-5/

 [1]: http://jiguang.github.com/index.php/tag/nodejs-2/ "nodejs"