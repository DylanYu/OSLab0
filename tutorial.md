#Lab0 Tutorial

##Windows下使用wubi安装Ubuntu

0.	假定我们将Ubuntu安装到E盘，保证E盘有10G剩余空间。不需要格式化E盘，原有的文件也不需要删除。 
1.	到[这里](http://zijingbt.njuftp.org/stats.html?id=52080 "ZiJingBT")下载ubuntu12.04。下载完后我们将里面的ubuntu-12.04-desktop-i386.iso拷贝到E盘根目录。
2.	用winrar打开ubuntu-12.04-desktop-i386.iso，找到wubi.exe文件，将其解压出来也放到E盘根目录（仅仅解压这一个exe文件即可）。
3.	打开控制台cmd，键入如下指令：

			E:\wubi.exe --force-wubi

	就启动了wubi.exe，如图（图片来源于http://www.drv5.cn/）：
	
	![图片](http://www.drv5.cn/sfinfo/UPic/2012-5/20125188104134041.gif)

	目标驱动器选为E，安装大小10G（意为分配给Ubuntu的磁盘空间，你可以选择更大一些），桌面环境选为Ubuntu，用户名和口令即为Ubuntu系统的用户名和口令，按你的习惯设置。然后点击安装，会有一个拷贝的过程，结束之后询问你是否重启系统，选择重启。
4.	重启后会直接引导进入Ubuntu的安装过程，这其中你不需要做任何事，安装结束后会重启。现在你的启动项里会增加一个`ubuntu`选项，选择即可进入我们刚刚安装好的系统。
5.	Done!

##配置你的Ubuntu

接下来我们需要对新安装的Ubuntu进行一些必要的配置，为后续的实验做准备。

###设置更新源
进入系统后，在键盘上按下Ctrl+Alt+T，可以调出一个叫终端(Terminal)的窗口，这和Windows下的cmd控制台很像，用于键入命令执行操作。以下操作如果需要输入口令，使用登录密码即可。

在终端上依次执行以下操作：

    cd /etc/apt
    sudo cp sources.list sources.list.backup
    sudo gedit sources.list

这下会弹出一个文本编辑窗口，把里面的内容全部删掉，然后把下面这段复制进去

    deb http://ftp.ipv6.heanet.ie/mirrors/ubuntu precise main restricted universe multiverse
    deb http://ftp.ipv6.heanet.ie/mirrors/ubuntu precise-security main restricted universe multiverse
    deb http://ftp.ipv6.heanet.ie/mirrors/ubuntu precise-updates main restricted universe multiverse
    deb-src http://ftp.ipv6.heanet.ie/mirrors/ubuntu precise main restricted universe multiverse
    deb-src http://ftp.ipv6.heanet.ie/mirrors/ubuntu precise-security main restricted universe multiverse
    deb-src http://ftp.ipv6.heanet.ie/mirrors/ubuntu precise-updates main restricted universe multiverse
    
保存退出文本编辑器，这样我们就配置好了更新源。如果你想尝试其他更新源，请参考[这里](http://my.oschina.net/rockbaby/blog/14711)。

###获取必要的工具

下面我们会从刚刚配置好的更新源下载实验需要的工具软件。回到终端，保证你的PC处于联网状态，输入
    
    sudo apt-get update
    
会执行一大堆操作，这是在向更新源询问我们的系统有哪些可以安装或更新的软件包，看到`Reading package lists... Done`就表示完成了询问过程。接下来输入：

    sudo apt-get install build-essential

这是在安装编译需要的库

然后安装虚拟机软件qemu：

    sudo apt-get install qemu
    
如果你想知道更多qemu相关的知识可以参考[这里](http://zh.wikipedia.org/wiki/QEMU)或者google一下

接下来安装git：

    sudo apt-get install git

安装好git之后我们就要使用它来获取实验的示范代码。依次输入：

    mkdir ~/OSLab
    cd ~/OSLab
    git clone https://github.com/jiangyy/OSLab0.git
    
询问你yes or no的时候输入yes，经过几秒钟的下载，我们就获得了示例代码。然后输入：

    cd OSLab0
    ls

看到几个文件和文件夹没，就是它们了。

##制作小游戏！

经过一番折腾，我们终于要进入正题了。首先浏览一遍示例代码吧，boot文件夹里是启动相关代码，
include和src文件夹里分别是游戏相关的头文件和源文件。我们先尝试编译一下，输入：

    make

竟然出错了！编译器告诉我们有几个函数找不到定义，查看发生错误的地方，发现是在main.c中调用了game.h里声明的三个未经定义的函数，那就定义它们吧！

    cd src
    vi game.c

复制以下内容到game.c

    #include "common.h"
    #include "game.h"
    #include "device/video.h"
    #include "string.h"
    
    void timer_event() {
        ;
    }
    
    void keyboard_event(int scan_code) {
        srand(scan_code);
        draw_string(itoa(scan_code), rand() % 180, rand() % 300, 15);
    }
    
    void main_loop() {
        while(TRUE) {
            draw_string("What a game!", 100, 100, 10);
        }
    }

保存退出。然后我们再来编译运行：
    
    cd ..
    make play
    
会出现这样一个窗口

![图片](http://t1.qpic.cn/mblogpic/bf4fa30ef37bb992bd2a/460.jpg)

我们的小游戏已经运行起来了！在键盘上随便敲几下试试，有没有出现一些杂乱分布的数字？这就是游戏的雏形了：响应外部事件，然后在屏幕上渲染出效果。

好了，教程就到这里，开始丰富你的代码吧。

Enjoy！

##Reference:

[1] https://wiki.ubuntu.com/WubiGuide

[2] http://askubuntu.com/questions/125015/can-i-install-12-04-inside-windows
