---
author: jiguang
title: Intellij IDEA配置自动同步到FTP服务器
layout: post
category:
  - DevTools
tags:
  - IDEA
---
{% include JB/setup %}

[Intellij IDEA][1] 是一款非常强大到跨平台的IDE，可以在任何主流操作系统中使用并且保证一致的操作习惯。但是这款优秀的IDE的相关中文教程却少得可怜，大部分使用者都是在慢慢摸索、互相学习才得以熟练使用。

这里[laser][2]介绍一下 FTP 服务器自动同步的配置和一些注意事项，希望能对你有所帮助。

首先前提是你已经创建了一个本地的工程，然后假设我在工程目录里新建一个文件夹叫做wordpress，我们将用它来同步我的 FTP 服务器上的文件。

打开“Tools -> Deployment -> Configuration…”，然后点击左上角的小加号新建一个服务器，在“Connection”页卡下填好各项参数。

填写完“FTP host”和“User name”、“Password”之后，可以选择“Test FTP connection…”测试一下设置是否正确，如果提示“Successfully connected to xx.xx.xx.xx”则说明连接成功。

还有一项“Root path”可以选择“Autodetect…”，但要注意一定要在连接测试成功以后才能点，你也可以点击旁边的省略号按钮，手动指定“Root path”。

注意：这里有的FTP服务器无法自动检测，当点击旁边的省略号按钮时也无法列出服务器文件目录，这时你需要点击“Advanced options…”，然后将“Passiv mode”勾选上即可。

填完这些以后我们就有了一个有效的服务器连接了，下面转到“Mappings”页卡，这里有三个输入框，第一个“Local path”当然就是你本地的路径了，你可以点省略号按钮，选择刚刚创建的wordpress目录。

第二个输入框是“Deployment path on server ‘xxx’”，这个就是你希望部署在服务器上的哪个目录，一般跟“Connection”页卡下的“Root path”是相同的，如果是默认的根目录就是一个斜杠“/”。当然你也可以部署到其他目录。这个目录部署后，本地的 wordpress 文件夹里面的文件，在上传时就会保存到这个目录中。

第三个输入框是“Web path on server ‘xxx’”，这个是通过你的网址可以访问到的路径，因为有的时候部署的路径不一定是实际的访问路径。如果部署的路径与实际的访问路径相同，则此处只填一个英文句点“.”即可，代表当前路径。

当完成这些以后，点击下方的“Apply”，然后点“Ok”关闭对话框。

这时，在Project列表中右键点击创建的wordpress文件夹，应该会多出一个“Deployment”选项，里面的子选项即为上传和下载文件或目录，这说明本地的文件夹已经和 FTP 服务器上的路径建立了联系。

如果想要设置在本地保存后自动同步到 FTP 服务器，可以先左键单击 wordpress 目录，然后选择“Tools -> Deployment -> Configuration…”打开刚才设置服务器的对话框，在左侧选择此服务器，然后单击上方四个小图标最右侧服务器模样的图标，点击后可以看到服务器的名字加粗了，这样就将该服务器设置成了wordpress目录首要使用的服务器连接，完成后关闭对话框。

再次打开“Tools -> Deployment ”，单击下方的“Automatic Upload”，这样就完成了自动同步的设置，当本地的 wordpress 目录中有文件修改并保存时，将自动同步到服务器（前提是本地与服务器上目录结构一致且文件名相同）。

 [1]: http://www.jetbrains.com/idea/
 [2]: http://jiguang.github.com