---
layout: post
title: 使用别名alias简化输入
category:
  - Mac
tags: [ ]
---
{% include JB/setup %}

在Linux/[mac][1]等终端输入命令时经常要输入很多字符，如果每次都手动输入的话效率很低，这时可以使用alias命令为常用的命令设置别名。

网上有很多类似的教程，具体修改方法不一，其原因是按照不同的修改方法修改后覆盖的范围不同。

如果单次使用（在终端不关闭的情况下），可以直接在终端输入：

    alias ［alias-name=’original-command’］

其中 alias-name 是你希望取的别名，而 original-command 是原始的命令及参数，注意等号的两端不能有空格。

例如，如果经常需要输入：

    $ cd /usr/X11/lib/X11

的话，那么可以在bash提示符下输入：

    $ alias goconfig=’cd /usr/X11/lib/X11’

![Alias1][2]

然后在执行 goconfig 就可以直接进入 /usr/X11/lib/X11 目录了。

如果要删除这个别名的话，可以使用 unalias 命令，例如：

    $ unalias goconfig

那么如果希望别名永久生效的话，则需要编辑用户目录下的 .bash_profile 文件了，这样每次重启终端后也可以直接使用了。

例如，我会把[Apache服务器][3]的目录设置一个别名 gowebroot，当位于你的用户目录时可以使用下面命令打开文件：

    vi .bash_profile

然后将光标移到末尾，输入你的别名设置，然后保存退出即可。

![Alias2][4]

 [1]: http://jiguang.github.com/index.php/tag/mac/ "mac"
 [2]: http://jiguang.github.com/content/uploads/2012/03/alias1.png "alias1.png"
 [3]: http://jiguang.github.com/index.php/2011/11/mac-lion-apache-php-environment/
 [4]: http://jiguang.github.com/content/uploads/2012/03/alias2.png "alias2.png"