---
layout: post
title: 彻底删除 Cydia 中删不掉的无效源
category:
  - LifeStyle
tags:
  - iOS
  - iPhone
---
{% include JB/setup %}

在为 Cydia 添加源时偶尔可能手误输入错误，比如 [laser][1] 这次就是不小心把百度搜索结果的地址粘贴到里面，结果还悲剧的添加成功了。然后在点“编辑”就死活没有作用了。

如果你也遇到这种情况，有几种解决方案可以尝试（试验机 iOS 5.1.1）：

1，如果你点“编辑”时，错误的源可以直接删除，那你就太幸运了！

2，使用 iFile 打开 **/etc/apt/sources.list.d/** 目录，找到 **Cydia.list **这个文件，用文本编辑器打开，编辑里面的内容，将出错的无效源删除并保存，然后打开 Cydia ，如果无效源被删除了，那你也是比较幸运的。

3，如果上面两种都没有解决问题的话，那么就要进行如下操作了：

*   首先，同第一步操作，将无效源删除
*   然后到  **/var/lib/apt/list/** 目录中，将跟无效源有关的都删除，或者完全删除
*   进到 partial 文件夹进行同样操作，注意，如果删除了 **partial** 文件夹可能会导致 Cydia 闪退
*   最后，找到  **/var/lib/cydia/metadata.plist** 这个文件并删除

完成这些操作以后，再打开 Cydia，应该可以看到问题已经解决了。另外，还有个专门修复错误源的软件包叫做 CyFix ，laser 没有去尝试，感兴趣的同学可以试试。

-

**附录：常用 Cydia 中文源**

**1、178源—号称中文第一源**

源地址：apt.178.com

包含内容：Cydia软件、铃声、字体

支持设备：iPhone、iPad等iOS设备（不包括Apple TV）

源管理员：丛林狼 [微博@ta][2]

推荐理由：中文，能看懂，更新快，资料全，并且是Baidu输入法和搜狗输入法的官方镜像地址，安全性没的说，每日更新，服务器速度快，基本不会卡Cydia。不愧是成为第一中文源。

使用小技巧：

快捷查看清单：<http://apt.178.com/applist/category/>

每日更新软件：<http://apt.178.com/new/>

**2、威锋源**

源地址：[apt.weiphone.com/][3]

包含内容：Cydia软件、铃声、某些ipa软件的deb格式

支持设备：iPhone、iPad等iOS设备

管理员：weiphone管理

推荐理由：背靠大山威锋论坛，技术支持能力很强，源内容更新速度较快，凭借强大的论坛提供的CDN加速，基本不卡Cydia，不过美中不足的是感觉比较乱，更新软件不太及时，不过依然很不错，推荐和178一起添加，有很多互补的地方。

**3、Saurik源**

源地址：apt.saurik.com

包含软件：系统级Cydia软件，Cydia整个程序，Linux移植到iOS程序

支持设备：所有iOS设备

管理员：[saurik][4]

推荐理由：没有理由，Cydia自带的源，少了还真不行，里面简直是个大宝库，各种系统级软件保证让你的iOS变成Linux服务器。

小技巧：直接下载deb文件，请到：[http://apt.saurik.com/debs/][5]，后直接用系统自带搜索，开足马力迅雷下载。都是deb文件，直接安装。

结余：按说这三个就够了，不过对于发烧级iOS用户肯定是不够的，所以以后会慢慢更新这个文章的，敬请关注吧。

**4、hackulo源**

说到这个源，大家都很陌生吧，但是我告诉你这个源的一款软件，名字叫做[AppSync][6]，大家肯定不会陌生了吧。没错，我们装机率最高的这个cydia软件就是Hackulo这个源里开发的。与此同时这个源还有一款叫做[installous][7]的软件。如果您是从iPhone 3G时代过来的，我想您一定不会陌生吧？顶级的XX软件中心，堪比现在的同步推那玩意儿，绝对是D版天堂的开山鼻祖，在国外臭名昭著，不过在国内一段时间曾是香饽饽呢。

源地址：cydia.hackulo.us

包含软件：[AppSync][8]、installous 、[crackulous][9]

管理员:hackulo论坛（很大的一个外国论坛，堪比威风）

推荐理由：曾经的龙头老大地位，必装[补丁][10]源地址，资源大宝库。不过目前在国内比更加凶猛的威锋源和178源取代了位置。

小技巧：同样开放了deb文件直接下载功能，链接到这里，您可以直接下载那些[deb工具][11]离线安装。

**5、Dev Team源**

这个不多说了吧，有锁机iPhone用户必备的cydia源，因为解锁要到这里下载ultrasn0w（中文名：超雪），这款解锁软件。用来解基带锁的。国行机用户可无视（国行无所）。

源地址：repo666.ultrasn0w.com

包含软件：ultrasn0w

管理员：Dev team

推荐理由：有锁机的最爱。

**6、BigBoss源**

重量级的综合型大软件源，支持cydia商店购买，都是纯正版的。

源地址：apt.thebigboss.org/repofiles/cydia/

包含软件：多了去了，都是正版，有各种主题，N多常用软件的官方地址。

管理员：Bigboss论坛

推荐理由：真正的大型软件源，cydia app的仓库，可惜是英文，不过看懂应该没问题。

**7、ModMyi源**

也是一个综合性的大cydia源，不过更多的是偏重于主题啊，字体啊，还有Sbsetting的主题等等，应该是文艺青年的最爱。

源地址：apt.modmyi.com

包含软件：多种多样，主题，美化，铃声为主

管理员：ModMyi

推荐理由：花样多，内容丰富，不过要注意了，收费软件极多，国外很注重版权的。也同时提醒国人并不是cydia软件都是免费的，他们也是有版权的。使用需购买。

 [1]: http://jiguang.github.com "姬光"
 [2]: http://weibo.com/178apt?s=6cm7D0
 [3]: http://apt.weiphone.com/ "http://apt.weiphone.com/"
 [4]: http://www.saurik.com/
 [5]: http://apt.saurik.com/debs/ "http://apt.saurik.com/debs/"
 [6]: http://www.52cydia.com/cydia-course/www.52cydia.com/debs/55.html
 [7]: http://cydia.hackulo.us/installous-4.3.deb
 [8]: http://www.52cydia.com/debs/145.html "AppSync 5+"
 [9]: http://cydia.hackulo.us/crackulous-1.0.0.5.deb
 [10]: http://www.52cydia.com/tag/patch/ "更多有关补丁的Cydia插件"
 [11]: http://cydia.hackulo.us/