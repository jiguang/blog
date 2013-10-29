---
author: jiguang
title: Wordpress不能安装插件或主题
layout: post
category:
  - Wordpress
tags: [ ]
---
{% include JB/setup %}

今天在折腾 WordPress 的时候突然发现不能安装插件了，主题也同样安装不了，在 laserij 解决问题的过程中遇到几个问题，也查找了一些资料，这里整理出来希望能对大家有所帮助。

首先，我的 WordPress 在插件安装页面出现了以下错误提示：

    An Unexpected HTTP Error occurred during the API request.

大致意思是“在发送API请求时发生了一个未知的HTTP错误”，那么既然是“未知”错误，我们就要想办法一探究竟了！

首先打开 WordPress 的调试功能，找到 wp-config.php 这个文件，将 WP_DEBUG 设为 true：

    define('WP_DEBUG', true);

然后就可以看到报错信息了。不过这个错误信息也很神奇，信息内容如下：

Notice: has_cap was called with an argument that is deprecated since version 2.0! Usage of user levels by plugins and themes is deprecated. Use roles and capabilities instead. in /Users/mikkel/Dropbox/Safe/annikalundgren/wp-includes/functions.php on line 3466

这个信息大致的意思是“has_cap调用时的一个参数在2.0版本以后就已经过时了！主题或插件中使用用户级别已经过时了。使用角色和对应的能力替代。错误代码在/Users/mikkel/Dropbox/Safe/annikalundgren/wp-includes/functions.php 中第 3466 行”。

这个错误说明我们安装的某个插件或主题过时了，使用了旧的API接口，所传的参数不正确。那么接下来第一步就是要确定是哪个插件或主题出错了。按照我之前的操作来看，应该是安装的某个插件出了问题，所以先禁止全部插件再说！果然，禁止全部插件后问题消失了。那么接下来就要用鸟枪法挨个试验了，最后确定了居然是“Syntax Highlighter Compress”出错了。

下面要在插件的文件中找到类似 add\_submenu\_page 这样的函数，幸好 Intellij IDEA 有个 Find in Path 功能，可以在路径中查找内容，瞬间就找到了函数位置：

    add_submenu_page('options-general.php', __('Syntax Highlighter ComPress Options', 'SHC'), $menutitle, 8, basename(__FILE__), 'shc_options_subpanel');

这里这个参数 8 就是用户权限级别的数字，在2.0版本之后，这里应该传入一个 String 类型的参数，用来表示用户的能力，那么我们将这个参数修改为 “read”，即可解决问题了。即代码为：

    add_submenu_page('options-general.php', __('Syntax Highlighter ComPress Options', 'SHC'), $menutitle, 'read', basename(__FILE__), 'shc_options_subpanel');

关于用户的角色和能力，请参看：[Roles and Capabilities][1] 。

下面返回插件安装界面，再次刷新，发现 DEBUG 信息消失了，说明程序应该没有出现问题，但是：

    An Unexpected HTTP Error occurred during the API request.

的提示仍然存在，还是安装不了插件。继续搜寻解决方案，有网友提到 HTTP 请求的超时时间的问题，先试一下再说！方法如下：

找到 /wp-includes/http.php，如果是 WordPress3.0以上，则在 /wp-includes/class-http.php 中，Ctrl+F 搜索，找到如下一行：

    'timeout' => apply_filters( 'http_request_timeout', 5),

后面的数字5表示请求超时的时间，将这个数值改大一点，比如100，然后将修改后的文件更新到FTP服务器。

再次刷新插件安装界面，仍然提示错误…我倒…

又继续研究了一下，据说可能是服务器限制超时时间的原因，于是我咨询了我的服务器提供商，得到的答案是，服务器超时时间为 120 秒，这说明不是服务器的原因。那么，到底是怎么个回事呢？继续搜索…

看到有的牛人提供了一个可供调试的插件，据说是 WordPress 开发必备之工具，称之为：Core Control <http://wordpress.org/extend/plugins/core-control/>，可以借助这个工具来查看许多信息。由于 [laser][2] 目前不能在线安装插件，只好把这个插件下载下来，手动 FTP 传上去。

选择 HTTP Access module ，然后保存，打开页面上方新出现的 HTTP Tab，在这里可以看到当前 WordPress 正在使用哪种方法进行 HTTP 传输，从给出的报表可以看到，该服务器此时的 PHP Stream 和 PHP fsockopen() 都是 Not Available （不可用）的。于是我把截图给了技术支持的客服，估计他们是不想麻烦了，说给我换服务器，等20分钟。

经过一番周折后，换了服务器，改了IP，再次进行了尝试，发现偶尔有某个插件可以下载成功，大部分还是下载一半就停止，不过之前的错误总算消失了，这说明服务器方面是有一定原因的，这个问题也算告一段落了。最后，记得关掉 WordPress 的调试模式。

    define('WP_DEBUG', false);

在整个解决问题的过程中，任何一个解决步骤都解决了一大批人的问题，可是在我这里一直折腾到最后也还没有很好的解决。那个HTTP错误还是偶尔会出现，所以，我还是有点怀疑是不是 WordPress 本身的 API 服务器现在已经承受不了这么大的负载，偶尔就扛不住挂掉了，或者是API接口的连接数到达上限了，而我们恰好被踢了下来，所以才有这么多人遇到这个问题。

在网速不是很快的情况下，建议大家还是先下载再用FTP工具上传吧，Wordpress在线下载的貌似不支持断点续传，中间一旦出了什么问题终止了，就得重新再安装。而且在服务器上留下了一些临时文件，还要手动删除。

如果你也遇到了同样的问题，或者你有什么好的解决办法，欢迎留言交流：）

参考文献：

<http://www.clickonf5.org/5802/solved-an-unexpected-http-error-wordpress/>

<http://www.antonkoekemoer.com/wordpress/solution-an-unexpected-http-error-occurred-during-the-api-request/>

<http://wordpress.org/support/topic/an-unexpected-http-error-occurred-during-the-api-request-on-wordpress-3>

 [1]: http://codex.wordpress.org/Roles_and_Capabilities
 [2]: http://jiguang.github.com