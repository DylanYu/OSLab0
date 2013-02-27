#Lab0 Tutorial

##Windows下使用wubi安装Ubuntu

0.	假定我们将Ubuntu安装到E盘，保证E盘有10G剩余空间。不需要格式化E盘，原有的件也不需要删除。 
1.	到[这里](：http://zijingbt.njuftp.org/stats.html?id=52080 "紫荆BT")下载ubuntu12.04。下载完后我们将里面的ubuntu-12.04-desktop-i386.iso拷贝到E盘根目录。
2.	用winrar打开ubuntu-12.04-desktop-i386.iso，找到wubi.exe件，将其解压出来也放到E盘根目录（仅仅解压这一个exe件即可）。
3.	打开控制台cmd，键入如下指令：

			E:\wubi.exe --force-wubi

	就启动了wubi.exe，如图（图片来源于http://www.drv5.cn/）：
	
	![图片](http://www.drv5.cn/sfinfo/UPic/2012-5/20125188104134041.gif)

	目标驱动器选为E，安装大小10G（意为分配给Ubuntu的磁盘空间，你可以选择更大一些），桌面环境选为Ubuntu，用户名和口令即为Ubuntu系统的用户名和口令，按你的习惯设置。然后点击安装，会有一个拷贝的过程，结束之后询问你是否重启系统，选择重启。
4.  重启后会直接引导进入Ubuntu的安装过程，这其中你不需要做任何事，安装结束后会重启，现在你的启动项里会增加一个`ubuntu`选项，选择即可进入我们刚刚安装好的系统。
5.  Done!

##Reference:

[1] https://wiki.ubuntu.com/WubiGuide

[2] http://askubuntu.com/questions/125015/can-i-install-12-04-inside-windows
