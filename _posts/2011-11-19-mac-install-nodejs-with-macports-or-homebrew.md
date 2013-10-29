---
author: jiguang
title: Mac下安装 nodejs 的几种方法
layout: post
category:
  - NodeJs
tags:
  - Git
---
{% include JB/setup %}

在 [mac][1] 下安装 [nodejs][2] 相对来说是比较方便的，如果你之前安装过类似 [Macports][3] 或者 [homebrew][4] 这样的工具，只需要简单的一句话就可以安装。如果使用的是 Macports，那么在终端执行如下命令即可：

    brew install node

如果使用的是 homebrew，则执行下面的命令即可安装：

    sudo port install nodejs

如果你没有使用过这两个工具，建议去尝试使用一下，可以像 linux 下面的 apt-get 一样安装软件，非常方便。

如果你还没有使用过这两个工具，那么就只能手动安装了。Mac 下默认没有安装 gcc，无法直接从源码编译安装，[laser][5] 尝试了单独安装 gcc 的方法，没有成功。最好的建议是直接从 AppStore 下载安装 Xcode，安装之后常用的开发工具包就都有了。不过下载 Xcode 时间比较长，网速比较好的话可能也要 4 个小时以上。

安装好 Xcode 后，系统就有了 gcc 的功能，下面可以继续安装 [git][6]，从 Git clone 下来源码进行编译安装。关于 Git 是什么和如何安装 Git，网上已经有无数的资料，laser 这里就不再赘述了，推荐去看 [Github 的帮助文档][7]，讲解很详细，只不过是英文的。从 Git 安装的步骤如下：

    git clone https://github.com/joyent/node.git nodejs
    cd nodejs
    ./configure
    make
    sudo make install

等待命令成功执行即安装完成了。

如果你没有安装 Git，那么也可以直接[下载 nodejs 的源码 ][8]，解压缩后放到合适的路径，然后进入该文件夹，仍然执行：

    cd nodejs
    ./configure
    make
    sudo make install

即可安装。

 [1]: http://jiguang.github.com/index.php/tag/mac/ "mac"
 [2]: http://jiguang.github.com/index.php/tag/nodejs-2/ "nodejs"
 [3]: http://www.macports.org/install.php
 [4]: http://mxcl.github.com/homebrew/
 [5]: http://jiguang.github.com "姬光"
 [6]: http://jiguang.github.com/index.php/tag/git/ "git"
 [7]: http://help.github.com/win-set-up-git/
 [8]: http://nodejs.org/#download