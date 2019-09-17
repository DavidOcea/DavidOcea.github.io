---
layout: post
title: (杂谈)Linux文件夹的结构
date: 2019-06-18 
tags: 操作系统/闪马  
---

## 说明

杂谈主要来源于网络，个大论坛，微信公众号等，好记性不如烂笔头，看一遍记不住，写一遍加深记忆。

### 目录

* [linux下各文件夹的结构说明](#the structure of each folder in linux)

### <a name="the structure of each folder in linux"></a>linux下各文件夹的结构说明

/bin：二进制可执行命令。
/dev：设备特殊文件。
/etc：系统管理和配置文件。
/etc/rc.d：启动的配 置文件和脚本。
/home：用户主目录的基点，比如用户user的主目录就是/home/user，可以用~user表示。
/lib：标准程序设计库，又 叫动态链接共享库，作用类似windows里的.dll文件。
/sbin：系统管理命令，这 里存放的是系统管理员使用的管理程序。
/tmp：公用的临时文件存储 点。
/root：系统管理员的主目 录。
/mnt：系统提供这个目录是 让用户临时挂载其他的文件系统。
/lost+found：这个 目录平时是空的，系统非正常关机而留下“无家可归”的文件就在这里。
/proc：虚拟的目录，是系 统内存的映射。可直接访问这个目录来获取系统信息。
/var：某些大文件的溢出 区，比方说各种服务的日志文件。
/usr：最庞大的目录，要用 到的应用程序和文件几乎都在这个目录。其中包含：
/usr/x11r6：存放x window的目录。
/usr/bin：众多的应用程序。
/usr/sbin：超级用户的一些管理程序。
/usr/doc：linux文档。
/usr/include：linux下开发和编译应用程序所需要的头文件。
/usr/lib：常用的动态链接库和软件包的配置文件。
/usr/man：帮助文档。
/usr/src：源代码，linux内核的源代码就放在/usr/src/linux 里。
/usr/local/bin：本地增加的命令。
/usr/local/lib：本地增加的库根文件系统。

通常情况下，根文件系统所占空间一般应该比较小，因为其中的绝大部分文件都不需要经常改动，而且包括严格的文件和一个小的 不经常改变的文件系统不容易损坏。除了可能的一个叫/vmlinuz标准的系统引导映像之外，根目录一般不含任何文 件。所有其他文件在根文件系统的子目录中。

### /bin目录
/bin目录包含了引导启动所需的命令或普通用户可能用的命令(可能在引导启动后)。这些命 令都是二进制文件的可执行程序(bin是binary的简称)，多是系统中重要的系统文件。

### /sbin目录
/sbin目录类似/bin ，也用于存储二进制文件。因为其中的大部分文件多是系统管理员使用的基本的系统程序，所以虽然普通用户必要且允许时可以使用，但一般不给普通用户使 用。

### /etc目录

/etc目录存放着各种系统配置文件，其中包括了用户信息文件/etc/passwd， 系统初始化文件/etc/rc等。linux正是靠这些文件才得以正常地运行。

### /root目录

/root目录是超级用户的目录。

### /lib目录

/lib目录是根文件系统上的程序所需的共享库，存放了根文件系统程序运行所需的共享文件。 这些文件包含了可被许多程序共享的代码，以避免每个程序都包含有相同的子程序的副本，故可以使得可执行文件变得更小，节省空间。

### /lib/modules目录

/lib/modules目录包含系统核心可加载各种模块，尤其是那些在恢复损坏的系统时重 新引导系统所需的模块(例如网络和文件系统驱动)。

### /dev目录

/dev目录存放了设备文件，即设备驱动程序，用户通过这些文件访问外部设备。比如，用户可 以通过访问/dev/mouse来访问鼠标的输入，就像访问其他文件一样。

### /tmp目录

/tmp目录存放程序在运行时产生的信息和数据。但在引导启动后，运行的程序最好使用/var/tmp来 代替/tmp，因为前者可能拥有一个更大的磁盘空间。

### /boot目录

/boot目录存放引导加载器(bootstrap loader)使用的文件，如lilo，核心映像也经常放在这里，而不是放在根目录中。但是如果有许多核心映像，这个目录就可能变得很大，这时使用单独的 文件系统会更好一些。还有一点要注意的是，要确保核心映像必须在ide硬盘的前1024柱面内。

### /mnt目录

/mnt目录是系统管理员临时安装(mount)文件系统的安装点。程序并不自动支持安装到/mnt 。/mnt下面可以分为许多子目录，例如/mnt/dosa可能是使用 msdos文件系统的软驱，而/mnt/exta可能是使用ext2文件系统的软驱，/mnt/cdrom光 驱等等。

### /proc, /usr, /var, /home目录

其他文件系统的安装点。

目录树可以分为小的部分，每个部分可以在自己的磁盘或分区上。主要部分是根、/usr 、/var 和 /home 文件系统。每个部分有不同的目的。

每台机器都有根文件系统，它包含系统引导和使其他文件系统得以mount所必要的文件，根文件系统应该有单用户状态所必须的足够的内容。还应该包括修复损坏 系统、恢复备份等的工具。

/usr 文件系统包含所有命令、库、man页和其他一般操作中所需的不改变的文件。 
/usr 不应该有 一般使用中要修改的文件。这样允许此文件系统中的文件通过网络共享，这样可以更有效，因为这样节省了磁盘空间(/usr 很容易是数百兆)，且易于管理 (当升级应用时，只有主/usr 需要改变，而无须改变每台机器) 即使此文件系统在本地盘上，也可以只读mount，以减少系统崩溃时文件系统的损 坏。

/var 文件系统包含会改变的文件，比如spool目录(mail、news、打印机等用的)， log文件、 formatted manual pages和暂存文件。传统上/var 的所有东西曾在 /usr 下的某个地方，但这样/usr 就不可能只读安装 了。

/home 文件系统包含用户家目录，即系统上的所有实际数据。一个大的/home 可能要分为若干文件系统，需要在 /home 下加一级名字，如/home/students 、/home/staff 等。

下面详细介绍：

* /etc文件系统

/etc目录包含各种系统配置文件，下面说明其中的一些。其他的你应该知道它们属于哪个程序， 并阅读该程序的man页。许多网络配置文件也在/etc中。

1. /etc/rc或/etc/rc.d或/etc/rc?.d：启动、或改变运行级时运 行的脚本或脚本的目录。

2. /etc/passwd：用户数据库，其中的域给出了用户名、真实姓名、用户起始目 录、加密口令和用户的其他信息。

3. /etc/fdprm：软盘参数表，用以说明不同的软盘格式。可用setfdprm进 行设置。更多的信息见setfdprm的帮助页。

4. /etc/fstab：指定启动时需要自动安装的文件系统列表。也包括用swapon -a启用的swap区的信息。

5. /etc/group：类似/etc/passwd ，但说明的不是用户信息而是组的信息。包括组的各种数据。

6. /etc/inittab：init 的配置文件。

7. /etc/issue：包括用户在登录提示符前的输出信息。通常包括系统的一段短说明 或欢迎信息。具体内容由系统管理员确定。

8. /etc/magic：“file”的配置文件。包含不同文件格式的说 明，“file”基于它猜测文件类型。

9. /etc/motd：motd是message of the day的缩写，用户成功登录后自动输出。内容由系统管理员确定。

常用于通告信息，如计划关机时间的警告等。

10. /etc/mtab：当前安装的文件系统列表。由脚本(scritp)初始化，并由 mount命令自动更新。当需要一个当前安装的文件系统的列表时使用(例如df命令)。

11. /etc/shadow：在安装了影子(shadow)口令软件的系统上的影子口令 文件。影子口令文件将/etc/passwd文件中的加密口令移动到/etc/shadow中，而后者只对超级用户(root)可读。这使破译口令更困 难，以此增加系统的安全性。

12. /etc/login.defs：login命令的配置文件。

13. /etc/printcap：类似/etc/termcap ，但针对打印机。语法不同。

14. /etc/profile 、/etc/csh.login、/etc/csh.cshrc：登 录或启动时bourne或cshells执行的文件。这允许系统管理员为所有用户建立全局缺省环境。

15. /etc/securetty：确认安全终端，即哪个终端允许超级用户(root) 登录。一般只列出虚拟控制台，这样就不可能(至少很困难)通过调制解调器(modem)或网络闯入系统并得到超级用户特权。

16. /etc/shells：列出可以使用的shell。chsh命令允许用户在本文件 指定范围内改变登录的shell。提供一台机器ftp服务的服务进程ftpd检查用户shell是否列在/etc/shells文件 中，如果不是，将不允许该用户登录。

17. /etc/termcap：终端性能数据库。说明不同的终端用什么“转义序列”控 制。写程序时不直接输出转义序列(这样只能工作于特定品牌的终端)，而是从/etc/termcap中查找要做的工作的 正确序列。这样，多数的程序可以在多数终端上运行。

* /dev文件系统

/dev目录包括所有设备的设备文件。设备文件用特定的约定命名，这在设备列表中说明。设备文件在安装时由系 统产生，以后可以用/dev/makedev描述。/dev/makedev.local 是系统管理员为本地设备文件(或连接)写的描述文稿(即如一些非标准设备驱动不是标准makedev 的一部分)。下面简要介绍/dev下 一些常用文件。

1. /dev/console：系统控制台，也就是直接和系统连接的监视器。

2. /dev/hd：ide硬盘驱动程序接口。如：/dev/hda指的是第一个硬 盘，had1则是指/dev/hda的第一个分区。如系统中有其他的硬盘，则依次为/dev /hdb、/dev/hdc、. . . . . .；如有多个分区则依次为hda1、hda2 . . . . . .

3. /dev/sd：scsi磁盘驱动程序接口。如系统有scsi硬盘，就不会访问/dev/had， 而会访问/dev/sda。

4. /dev/fd：软驱设备驱动程序。如：/dev/fd0指 系统的第一个软盘，也就是通常所说的a盘，/dev/fd1指第二个软盘，. . . . . .而/dev/fd1 h1440则表示访问驱动器1中的4.5高密盘。

5. /dev/st：scsi磁带驱动器驱动程序。

6. /dev/tty：提供虚拟控制台支持。如：/dev/tty1指 的是系统的第一个虚拟控制台，/dev/tty2则是系统的第二个虚拟控制台。

7. /dev/pty：提供远程登陆伪终端支持。在进行telnet登录时就要用到/dev/pty设 备。
8. /dev/ttys：计算机串行接口，对于dos来说就是“com1”口。

9. /dev/cua：计算机串行接口，与调制解调器一起使用的设备。

10. /dev/null：“黑洞”，所有写入该设备的信息都将消失。例如：当想要将屏幕 上的输出信息隐藏起来时，只要将输出信息输入到/dev/null中即可。

* /usr文件系统

/usr是个很重要的目录，通常这一文件系统很大，因为所有程序安装在这里。/usr里 的所有文件一般来自linux发行版；本地安装的程序和其他东西在/usr/local下，因为这样可以在升级新版系 统或新发行版时无须重新安装全部程序。/usr目录下的许多内容是可选的，但这些功能会使用户使用系统更加有效。/usr可容纳许多大型的软件包和它们的 配置文件。下面列出一些重要的目录(一些不太重要的目录被省略了)。

1. /usr/x11r6：包含x window系统的所有可执行程序、配置文件和支持文件。为简化x的开发和安装，x的文件没有集成到系统中。x window系统是一个功能强大的图形环境，提供了大量的图形工具程序。用户如果对microsoft windows比较熟悉的话，就不会对x window系统感到束手无策了。

2. /usr/x386：类似/usr/x11r6 ，但是是专门给x 11 release 5的。

3. /usr/bin：集中了几乎所有用户命令，是系统的软件库。另有些命令在/bin或/usr/local/bin中。

4. /usr/sbin：包括了根文件系统不必要的系统管理命令，例如多数服务程序。

5. /usr/man、/usr/info、/usr/doc：这些目录包含所有手册页、 gnu信息文档和各种其他文档文件。每个联机手册的“节”都有两个子目录。例如：/usr/man/man1中包含联机手册第一节的源码(没有格式化的原 始文件)，/usr/man/cat1包含第一节已格式化的内容。联机手册分为以下九节：内部命令、系统调用、库函数、设备、文件格式、游戏、宏软件包、 系统管理和核心程序。

6. /usr/include：包含了c语言的头文件，这些文件多以.h结尾，用来描述c 语言程序中用到的数据结构、子过程和常量。为了保持一致性，这实际上应该放在/usr/lib下，但习惯上一直沿用了这 个名字。

7. /usr/lib：包含了程序或子系统的不变的数据文件，包括一些site – wide配置文件。名字lib来源于库(library); 编程的原始库也存在/usr/lib 里。当编译程序时，程序便会和其中的库进行连接。也有许多程序把配置文件存入其中。

8. /usr/local：本地安装的软件和其他文件放在这里。这与/usr很相似。用户 可能会在这发现一些比较大的软件包，如tex、emacs等。

* /var文件系统

/var包含系统一般运行时要改变的数据。通常这些数据所在的目录的大小是要经常变化或扩充 的。原来/var目录中有些内容是在/usr中的，但为了保持/usr目录的相对稳定，就把那些需要经常改变的目录放到/var中了。每个系统是特定的， 即不通过网络与其他计算机共享。下面列出一些重要的目录(一些不太重要的目录省略了)。

1. /var/catman：包括了格式化过的帮助(man)页。帮助页的源文件一般存在 /usr/man/catman中；有些man页可能有预格式化的版本，存在/usr/man/cat中。而其他的man页在第一次看时都需要格式化，格 式化完的版本存在/var/man中，这样其他人再看相同的页时就无须等待格式化了。(/var/catman经常被 清除，就像清除临时目录一样。)

2. /var/lib：存放系统正常运行时要改变的文件。

3. /var/local：存放/usr/local中 安装的程序的可变数据(即系统管理员安装的程序)。注意，如果必要，即使本地安装的程序也会使用其他/var目录，例如/var/lock 。

4. /var/lock：锁定文件。许多程序遵循在/var/lock中 产生一个锁定文件的约定，以用来支持他们正在使用某个特定的设备或文件。其他程序注意到这个锁定文件时，就不会再使用这个设备或文件。

5. /var/log：各种程序的日志(log)文件，尤其是login (/var/log/wtmplog纪 录所有到系统的登录和注销) 和syslog (/var/log/messages 纪录存储所有核心和系统程序信息)。/var/log 里的文件经常不确定地增长，应该定期清除。

6. /var/run：保存在下一次系统引导前有效的关于系统的信息文件。例如，/var/run/utmp包 含当前登录的用户的信息。

7. /var/spool：放置“假脱机(spool)”程序的目录，如mail、 news、打印队列和其他队列工作的目录。每个不同的spool在/var/spool下有自己的子目录，例如，用户的邮箱就存放在/var/spool/mail 中。

8. /var/tmp：比/tmp允许更大的或需要存在较长时间的临时文件。注意系统管理 员可能不允许/var/tmp有很旧的文件。

* /proc文件系统

/proc文件系统是一个伪的文件系统，就是说它是一个实际上不存在的目录，因而这是一个非 常特殊的目录。它并不存在于某个磁盘上，而是由核心在内存中产生。这个目录用于提供关于系统的信息。下面说明一些最重要的文件和目录(/proc文件系统 在proc man页中有更详细的说明)。

1. /proc/x：关于进程x的信息目录，这x是这一进程的标识号。每个进程在 /proc下有一个名为自己进程号的目录。

2. /proc/cpuinfo：存放处理器(cpu)的信息，如cpu的类型、制造商、 型号和性能等。

3. /proc/devices：当前运行的核心配置的设备驱动的列表。

4. /proc/dma：显示当前使用的dma通道。

5. /proc/filesystems：核心配置的文件系统信息。

6. /proc/interrupts：显示被占用的中断信息和占用者的信息，以及被占用 的数量。

7. /proc/ioports：当前使用的i/o端口。

8. /proc/kcore：系统物理内存映像。与物理内存大小完全一样，然而实际上没有 占用这么多内存；它仅仅是在程序访问它时才被创建。(注意：除非你把它拷贝到什么地方，否则/proc下没有任何东西占用任何磁盘空间。)

9. /proc/kmsg：核心输出的消息。也会被送到syslog。

10. /proc/ksyms：核心符号表。

11. /proc/loadavg：系统“平均负载”；3个没有意义的指示器指出系统当前 的工作量。

12. /proc/meminfo：各种存储器使用信息，包括物理内存和交换分区 (swap)。

13. /proc/modules：存放当前加载了哪些核心模块信息。

14. /proc/net：网络协议状态信息。

15. /proc/self：存放到查看/proc的 程序的进程目录的符号连接。当2个进程查看/proc时，这将会是不同的连接。这主要便于程序得到它自己的进程目录。

16. /proc/stat：系统的不同状态，例如，系统启动后页面发生错误的次数。

17. /proc/uptime：系统启动的时间长度。

18. /proc/version：核心版本。

* /usr/local下一般是你安装软件的目录，这个目录就相当于在windows下的programefiles这个目录 

* /opt这个目录是一些大型软件的安装目录，或者是一些服务程序的安装目录

举个例子：刚才装的测试版firefox，就可以装到/opt/firefox_beta目录下，/opt/firefox_beta目录下面就包含了运 行firefox所需要的所有文件、库、数据等等。要删除firefox的时候，你只需删除/opt/firefox_beta目录即可，非常简单。

* /usr/local

这里主要存放那些手动安装的软件，即 不是通过“新立得”或apt-get安装的软件 。 它和/usr目录具有相类似的目录结构 。让软件包管理器来管理/usr目录，而把自定义的脚本(scripts)放到/usr/local目录下面，我想这应该是个不错的主意。


### 关于原文
文章来源于微信公号：[点击跳转](https://mp.weixin.qq.com/s?__biz=MzIxODM4MjA5MA==&mid=2247489737&idx=2&sn=56a6c334aa4e267f875bd00b5395defb&chksm=97ea32aca09dbbba4626a426f18a161db664540f01b44d328285ea207d1079404a7ea0e8df0d&mpshare=1&scene=1&srcid=0618ZwA3EgDIS1R7P7miLbRo&rd2werd=1#wechat_redirect)