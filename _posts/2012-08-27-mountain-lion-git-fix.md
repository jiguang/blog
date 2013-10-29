---
layout: post
title: Mountain Lion 中 Git “Command Not Found” 错误的解决办法
category:
  - DevTools
tags: [Git, Mac]
---
{% include JB/setup %}

如果在之前的 Lion 系统中安装了 [git][1]，且升级到了 Mountain Lion 的话，就会出现 git 命令丢失的错误：

    &quot;-bash: git: command not found&quot;

当在终端中执行 git 命令时则会出现下图所示：

![Git not found][2]

不过不用担心，修复这个问题只需要两步（不过对于国内用户时间可能比较漫长）。

Step 1

打开 App Store 下载/更新 XCode，使其为最新版，[笔者][3]这里显示为 4.4.1（4F1003）。

![Install command line tools][5]

Step 2

安装 Xcode 并运行，使用 Command + , 快捷键呼出 Preference 面板，转到 Downloads 面板并找到 Command Line Tools ，然后点击 Install。

![Install xcode][4]

这时可能会要求你输入一个 Apple Developer ID，如果没有的话注册一个即可。

![Login with apple dev id][6]

Step 3

好吧，其实到这步已经完成了，再次打开终端，输入 git 就可以看到熟悉的文字啦。

![Test git in terminal][7]

PS：上面几步看上去比较简单，但是对于国内用户来说 XCode 的下载和更新真是煎熬啊~

 [1]: http://jiguang.github.com/index.php/tag/git/ "git"
 [2]: http://jiguang.github.com/content/uploads/2012/08/git-not-found.jpeg "git-not-found.jpeg"
 [3]: http://jiguang.github.com "笔者"
 [4]: http://jiguang.github.com/content/uploads/2012/08/install-command-line-tools.jpeg "install-command-line-tools.jpeg"
 [5]: http://jiguang.github.com/content/uploads/2012/08/install-xcode.jpeg "install-xcode.jpeg"
 [6]: http://jiguang.github.com/content/uploads/2012/08/login-with-apple-dev-id.jpeg "login-with-apple-dev-id.jpeg"
 [7]: http://jiguang.github.com/content/uploads/2012/08/test-git-in-terminal.jpeg "test-git-in-terminal.jpeg"