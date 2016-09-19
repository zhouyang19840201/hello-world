# hello-world
在中国网络环境下从源代码安装Go1.6到CentOS 7
http://www.golangtc.com/t/56f3c1f4b09ecc66b9000182

http://www.cnblogs.com/fang8206/p/4939781.html centos7 桌面系统打造
https://seisman.info/linux-environment-for-seismology-research.html 用 CentOS 7 打造合适的科研环境

 CentOS 作为服务器的操作系统是很常见的，但是因为需要稳定而没有很时髦的更新，所以很少做为桌面环境。在服务器上通常不需要安装桌面环境，最小化地安装 CentOS（也就是 minimal CentOS） 就可以了。不过在最小化安装的 CentOS 中通过 YUM 来安装桌面环境也是非常方便的。

单位的那台服务器上就让我安装了最小化的 CentOS 操作系统。但是同事说操作不方便，所以我就试了试，顺便记录这个安装方法。使用 yum groupinstall 指令很容易就能安装上图形界面的桌面系统。
1. yum 的 group 指令

yum 可以以程序组的模式来安装成套的软件包。支持的软件包可以通过，

# yum grouplist

查询到。在 group 软件包中，Desktop、Desktop Platform、KDE Desktop、X Window System 是主要的桌面环境。

软件包列表根据系统使用的语言来显示，支持简体中文文件名。所以安装前最好用上述指令查询一下看看。如果系统使用了简体中文，而安装指令使用英文，可能会导致查询不到软件包这样的错误。下面的安装指令用的都是英语。
2. 图形桌面环境

要安装 KDE 桌面环境，执行指令，

# yum groupinstall "X Window System" "KDE Desktop" Desktop

即可，同时安装了 3 个软件包。注意，因为 KDE Desktop 和  X Window System 两个软件包名称中间都包含空格，需要用引号引起来才行。

要安装 Gnome 桌面环境，执行指令，

# yum groupinstall "X Window System" "Desktop Platform" Desktop

即可，也是同时安装了 3 个软件包，其中 X Window System 是必须的，不管是 Gnome 还是 KDE。

既然是桌面环境，可能还需要诸如字体、管理工具之类的，如，

# yum -y groupinstall "Graphical Administration Tools"
# yum -y groupinstall "Internet Browser"
# yum -y groupinstall "General Purpose Desktop"
# yum -y groupinstall "Office Suite and Productivity"
# yum -y groupinstall "Graphics Creation Tools"

3. 启用

从命令行直接启动图形桌面环境，

# startx

这样就会启动默认的 Gnome 或者 KDE 桌面环境。如果有人喜欢同时安装 Gnome 和 KDE，切换方法可以参考 CentOS 文档。

如果希望启动时自动启动到图形桌面，需要修改 /etc/inittab，将 id:3:initdefault: 改为 id:5:initdefault:。参考。直接用 sed 会很方便，

sed -i 's/id:3:initdefault:/id:5:initdefault:/' /etc/inittab

启动图形界面后，如果希望从图形界面切换到命令行界面，可以用 Ctrl + Alt + F6（实际上 F1 到 F6 都行，不过它们代表 Linux 中不同的控制台），或者反过来 Ctrl + Alt + F7 回到刚才的图形界面。
