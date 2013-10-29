---
author: jiguang
title: AppleScript实现保护视力小程序
layout: post
category:
  - Mac
tags:
  - AppleScript
---

每天用电脑不知时光飞逝，一不小心就在电脑前坐了几个小时不动，对身体实在不好。于是[laser][1]在网上搜索类似[mac][2]下的保护视力的软件，收获不多。刚好今天了解了一下AppleScript，于是就想到用AppleScript折腾一段小代码出来。原理很简单，就是每隔45分钟弹出一个窗口，提示你该休息了，至于你到底休不休息那还得看自觉了。

代码中的各个参数不言自明，可以任意修改时间间隔，提示话语等。

另外AppleScript还有很多强大的功能，以后再慢慢发掘，我这里恐怕是大材小用了。先看看代码吧：

    on reminder()
    	--time to close the dialog
    	set closeDelay to 5
    
    	--words to say
    	set tips to "Honey, it's time to rest your eyes"
    
    	--say someting
    	say tips
    
    	--show dialog to remind you
    	display dialog tips giving up after closeDelay
    
    end reminder
    
    on idle
    
    	--interval to remind
    	set interval to 45
    
    	--repeat
    	reminder()
    
    	--set interval time
    	return interval * 60
    
    end idle
    
    on quit
    
    	display dialog "You sure wanna quit?" buttons {"Yes", "No"}
    
    	if button returned of result = "Yes" then
    
    		continue quit
    
    	end if
    
    end quit

其中on xxx就相当于定义了一个函数，on idle是比较特殊的，可以每隔一段时间执行一次，而on quit显而易见，是退出时进行的操作了。这段代码没有多少好解释的，只是一个小练习而已，感兴趣的同学可以搜索一下AppleScript的相关资料，真的很强大的。

这里还有打包后的脚本，与Mac上的其他App一样，解压后双击即可使用 [timeReminder][3] 。

另外，也可以打开Mac的Date&Time Preferences，选择Announce the time即可开启整点报时的功能，每隔一个固定时间就会有一个声音提示你当前时间，声音还可以自定义，唯一的不足就是只能选择整点或半点等几个固定选项。

 [1]: http://jiguang.github.com
 [2]: http://jiguang.github.com/index.php/tag/mac/
 [3]: http://blog.laserji.com/content/uploads/2011/12/timeReminder.zip