---
layout: post
title: "Git超简洁教程"
category:
  - DevTools
tags:
  - Git
  - 教程
  - 翻译
---
{% include JB/setup %}

原文链接：<http://rogerdudler.github.com/git-guide/>

[git][1]超简洁教程 ——只是初学者的简单教程，没有神马高深的。

**安装**

下载OSX平台的git：[Download git for OSX][2]

下载Windows平台的git：[Download git for Windows][3]

下载Linux平台的git：[Download git for Linux][4]

**创建新分支**

创建一个新目录，打开目录并执行

    git init

来创建一个新的资源库（repository）。

**检出分支**

通过以下命令创建一个本地资源库（local repository）的工作副本（working copy）：

    git clone /path/to/repository

当使用远程服务器时，命令则为：

    git clone username@host:/path/to/repository

**工作流程**

你的本地分支包含三个由git维护的“树”，第一个是你的**工作目录（Working Directory），**它保存着实际的文件；第二个是**索引（Index）**，它的行为像是一个临时区域；最后一个是头（HEAD），它指向你的最后一次提交。

[![][6]][6]

**添加和提交**

你可以使用下面的命令执行更改（添加到索引Index）：

    git add <filename>
    git add *

这只git基本工作流的第一步，想确实提交这些更改要使用：

    git commit -m "Commit message"

现在文件会被提交到头（HEAD），但是还没有到远程资源库（remote repository）中。

**推入变更**

你的更改现在在本地工作副本的头（HEAD）文件中，若想把更改发送到远程资源库，执行：

    git push origin master

master可以改为任何你想要推入文件的资源库的名字。

如果你还没有克隆（clone）任何已存在的资源库，并且希望本地资源库连接到远程服务器，你需要用下面命令添加：

    git remote add origin <server>

现在，你可以把你的更改推入选定的远程服务器了。

**分支**

分支（branch）是用来开发特性相互独立的部分，当创建一个资源库时，主（*master*）分支是默认的分支。可以使用其它分支进行开发，并在开发结束后合并到主分支。

![主分支][7]]

创建一个名为“feature_x”的分支，并切换到该分支可以使用：

    git checkout -b feature_x

可以再切回到主分支：

    git checkout master

并且删除刚才创建的分支：

    git branch -d feature_x

如果你不把分支推入到远程资源库的话，别人是无法使用该分支的：

    git push origin &lt;branch&gt;

**更新与合并**

想要更新你的本地资源库到最新的提交，可以在你的本地资源库执行：

    git pull

来抓取（*fetch*）和合并（merge）远程的更改。

要想合并其它分支到你的当前分支（active branch），比如主分支，使用：

    git merge &lt;branch&gt;

在这两种情况下，git都会尝试自动合并更改。不幸的是，有时候这是不可能的，而且会导致冲突（*conflicts*）。这时你需要通过修改git所列出的文件来手动合并冲突，修改后，你需要将它们标记为已合并：

    git add &lt;filename&gt;

在合并更改之前，也可以通过下列命令进行预览：

    git diff &lt;source_branch&gt; &lt;target_branch&gt;

**标签**

我们最好为软件的版本创建标签（tag），这是已知的概念，在SVN中也存在。你可以通过执行下面的命令创建一个建一个名为1.0.0的新标签：

    git tag 1.0.0 1b2e1d63ff

这个1b2e1d63ff 代表了你希望标签所引用的提交id（commit id）的前10个字符，你可以通过：

    git log

来获取提交id，你也可以使用更少的字符作为提交id，只不过它必须是唯一的。

**替换本地更改**

万一你做错了什么（这肯定不会发生），你可以用下面的命令替换本地更改：

    git checkout -- &lt;filename&gt;

这会用头（HEAD）文件中最后的内容来替换本地工作树（working tree）中的更改。已经添加到索引中的更改以及新文件都会被保留。

反之，如果你想放弃所有本地的更改和提交，可以抓取服务器最新的历史并将本地的主分支指向它，像这样：

    git fetch origin
    git reset --hard origin/master

**有用的提示**

内置的git图形界面（GUI）：

    gitk

使用彩色的git输出：

    git config color.ui true

以每次提交单独一行的格式显示日志：

    git config format.pretty oneline

使用交互的添加操作：

    git add -i

**链接及资源**

图形客户端

*   [GitX (L) (OSX, open source)][7]
*   [Tower (OSX)][8]
*   [Source Tree (OSX, free)][9]
*   [GitHub for Mac (OSX, free)][10]

指南及手册

*   [Git Community Book][11]
*   [Pro Git][12]
*   [Think like a git][13]
*   [GitHub Help][14]
*   [A Visual Git Guide][15]

 [1]: http://jiguang.github.com/index.php/tag/git/
 [2]: http://code.google.com/p/git-osx-installer/downloads/list?can=3
 [3]: http://code.google.com/p/msysgit/downloads/list?can=3
 [4]: http://book.git-scm.com/2_installing_git.html
 [5]: http://jiguang.github.com/content/uploads/2012/01/trees1.png
 [6]: http://jiguang.github.com/content/uploads/2012/01/branches.png
 [7]: http://gitx.laullon.com/
 [8]: http://www.git-tower.com/
 [9]: http://www.sourcetreeapp.com/
 [10]: http://mac.github.com/
 [11]: http://book.git-scm.com/
 [12]: http://progit.org/book/
 [13]: http://think-like-a-git.net/
 [14]: http://help.github.com/
 [15]: http://marklodato.github.com/visual-git-guide/index-en.html