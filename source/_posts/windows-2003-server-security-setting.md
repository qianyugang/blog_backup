---
title: Windows 2003 服务器安全设置
tags:
  - windows
toc: true
url: 445.html
id: 445
categories:
  - 我是修电脑的
date: 2012-11-29 07:41:04
---

# 一、先关闭不需要的端口
我比较小心，先关了端口。只开了3389、21、80、1433，有些人一直说什么默认的3389不安全，对此我不否认，但是利用的途径也只能一个一个的穷举爆破，你把帐号改了密码设置为十五六位，我估计他要破上好几年，哈哈！办法：本地连接–属性–Internet协议（TCP/IP）–高级–选项–TCP/IP筛选–属性–把勾打上，然后添加你需要的端口即可。PS一句：设置完端口需要重新启动！
当然大家也可以更改远程连接端口方法：
Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE \ SYSTEM\ Current ControlSet \ Control \ Terminal Server\WinStations\RDP-Tcp]
“PortNumber”=dword:00002683
保存为.REG文件双击即可！更改为9859，当然大家也可以换别的端口，直接打开以上注册表的地址，把值改为十进制的输入你想要的端口即可！重启生效
还有一点，在2003系统里，用TCP/IP筛选里的端口过滤功能，使用FTP服务器的时候，只开放21端口，在进行FTP传输的时候，FTP 特有的Port模式和Passive模式，在进行数据传输的时候，需要动态的打开高端口，所以在使用TCP/IP过滤的情况下，经常会出现连接上后无法列出目录和数据传输的问题。所以在2003系统上增加的Windows连接防火墙能很好的解决这个问题，不推荐使用网卡的TCP/IP过滤功能。

# 二.关闭不需要的服务打开相应的审核策略
我关闭了以下的服务
Computer Browser 维护网络上计算机的最新列表以及提供这个列表
Task scheduler 允许程序在指定时间运行
Messenger 传输客户端和服务器之间的 NET SEND 和 警报器服务消息
Distributed File System: 局域网管理共享文件，不需要禁用
Distributed linktracking client：用于局域网更新连接信息，不需要禁用
Error reporting service：禁止发送错误报告
Microsoft Serch：提供快速的单词搜索，不需要可禁用
NTLMSecuritysupportprovide：telnet服务和Microsoft Serch用的，不需要禁用
PrintSpooler：如果没有打印机可禁用
Remote Registry：禁止远程修改注册表
Remote Desktop Help Session Manager：禁止远程协助
Workstation关闭的话远程NET命令列不出用户组
把不必要的服务都禁止掉，尽管这些不一定能被攻击者利用得上，但是按照安全规则和标准上来说，多余的东西就没必要开启，减少一份隐患。

在”网络连接”里，把不需要的协议和服务都删掉，这里只安装了基本的Internet协议（TCP/IP），由于要控制带宽流量服务，额外安装了Qos数据包计划程序。在高级tcp/ip设置里–”NetBIOS”设置”禁用tcp/IP上的NetBIOS（S）”。在高级选项里，使用”Internet连接防火墙”，这是windows 2003 自带的防火墙，在2000系统里没有的功能，虽然没什么功能，但可以屏蔽端口，这样已经基本达到了一个IPSec的功能。

在运行中输入gpedit.msc回车，打开组策略编辑器，选择计算机配置-Windows设置-安全设置-审核策略在创建审核项目时需要注意的是如果审核的项目太多，生成的事件也就越多，那么要想发现严重的事件也越难当然如果审核的太少也会影响你发现严重的事件，你需要根据情况在这二者之间做出选择。

推荐的要审核的项目是：
登录事件
账户登录事件
系统事件
策略更改
对象访问
目录服务访问
特权使用

# 三、关闭默认共享的空连接
地球人都知道，我就不多说了！

# 四、磁盘权限设置
C盘只给administrators和system权限，其他的权限不给，其他的盘也可以这样设置，这里给的system权限也不一定需要给，只是由于某些第三方应用程序是以服务形式启动的，需要加上这个用户，否则造成启动不了。
Windows目录要加上给users的默认权限，否则ASP和ASPX等应用程序就无法运行。以前有朋友单独设置Instsrv和temp等目录权限，其实没有这个必要的。

另外在c:/Documents and Settings/这里相当重要，后面的目录里的权限根本不会继承从前的设置，如果仅仅只是设置了C盘给administrators权限，而在All Users/Application Data目录下会出现everyone用户有完全控制权限，这样入侵这可以跳转到这个目录，写入脚本或只文件，再结合其他漏洞来提升权限；譬如利用serv-u的本地溢出提升权限，或系统遗漏有补丁，数据库的弱点，甚至社会工程学等等N多方法，从前不是有牛人发飑说：”只要给我一个webshell，我就能拿到system”，这也的确是有可能的。在用做web/ftp服务器的系统里，建议是将这些目录都设置的锁死。其他每个盘的目录都按照这样设置，每个盘都只给adinistrators权限。
另外，还将：
net.exe NET命令
cmd.exe  CMD 懂电脑的都知道咯~
tftp.exe
netstat.exe
regedit.exe  注册表啦大家都知道
at.exe
attrib.exe
cacls.exe  ACL用户组权限设置，此命令可以在NTFS下设置任何文件夹的任何权限！偶入侵的时候没少用这个….（：
format.exe  不说了，大家都知道是做嘛的
大家都知道ASP木马吧，有个CMD运行这个的，这些如果都可以在CMD下运行..55，，估计别的没啥，format下估计就哭料~~~（：这些文件都设置只允许administrator访问。

# 五、防火墙、杀毒软件的安装
关于这个东西的安装其实我也说不来，反正安装什么的都有，建议使用卡巴，卖咖啡。用系统自带的防火墙，这个我不专业，不说了！大家凑合！

# 六、SQL2000 SERV-U FTP安全设置
SQL安全方面
1、System Administrators 角色最好不要超过两个
2、如果是在本机最好将身份验证配置为Win登陆
3、不要使用Sa账户，为其配置一个超级复杂的密码
或修改sa用户名：
update sysxlogins set name=’xxxx’ where sid=0×01
update sysxlogins set sid=0xE765555BD44F054F89CD0076A06EA823 where name=’xxxx’
4、删除以下的扩展存储过程格式为：
use master
sp_dropextendedproc ‘扩展存储过程名’
xp_cmdshell：是进入操作系统的最佳捷径，删除
访问注册表的存储过程，删除
Xp_regaddmultistringXp_regdeletekeyXp_regdeletevalueXp_regenumvalues
Xp_regread Xp_regwrite Xp_regremovemultistring
OLE自动存储过程，不需要，删除
Sp_OACreate Sp_OADestroySp_OAGetErrorInfoSp_OAGetProperty
Sp_OAMethodSp_OASetPropertySp_OAStop
5、隐藏 SQL Server、更改默认的1433端口。
右击实例选属性-常规-网络配置中选择TCP/IP协议的属性，选择隐藏 SQL Server 实例，并改原默认的1433端口。
6.为数据库建立一个新的角色，禁止改角色对系统表的的select等权限，防止sql注入时利用系统表。
　　serv-u的几点常规安全需要设置下：
选中”Block “FTP_bounce”attack and FXP”。什么是FXP呢？通常，当使用FTP协议进行文件传输时，客户端首先向FTP服务器发出一个”PORT”命令，该命令中包含此用户的IP地址和将被用来进行数据传输的端口号，服务器收到后，利用命令所提供的用户地址信息建立与用户的连接。大多数情况下，上述过程不会出现任何问题，但当客户端是一名恶意用户时，可能会通过在PORT命令中加入特定的地址信息，使FTP服务器与其它非客户端的机器建立连接。虽然这名恶意用户可能本身无权直接访问某一特定机器，但是如果FTP服务器有权访问该机器的话，那么恶意用户就可以通过FTP服务器作为中介，仍然能够最终实现与目标服务器的连接。这就是FXP，也称跨服务器攻击。选中后就可以防止发生此种情况。

# 七、IIS安全设置
IIS的安全：
1、不使用默认的Web站点，如果使用也要将IIS目录与系统磁盘分开。
2、删除IIS默认创建的Inetpub目录（在安装系统的盘上）。
3、删除系统盘下的虚拟目录，如：_vti_bin、IISSamples、Scripts、IIShelp、IISAdmin、IIShelp、MSADC。
4、删除不必要的IIS扩展名映射。
右键单击“默认Web站点→属性→主目录→配置”，打开应用程序窗口，去掉不必要的应用程序映射。主要为.shtml、.shtm、 .stm。
5、更改IIS日志的路径
右键单击“默认Web站点→属性-网站-在启用日志记录下点击属性
6、如果使用的是2000可以使用iislockdown来保护IIS，在2003运行的IE6.0的版本不需要。
八、其它
1、系统升级、打操作系统补丁，尤其是IIS 6.0补丁、SQL SP3a补丁，甚至IE 6.0补丁也要打。同时及时跟踪最新漏洞补丁；
2、停掉Guest 帐号、并给guest 加一个异常复杂的密码，把Administrator改名或伪装！

3、隐藏重要文件/目录
可以修改注册表实现完全隐藏：“HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\ Current-Version\Explorer\Advanced\Folder\Hi-dden\SHOWALL”，鼠标右击 “CheckedValue”，选择修改，把数值由1改为0。
4、启动系统自带的Internet连接防火墙，在设置服务选项中勾选Web服务器。
5、防止SYN洪水攻击。
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters
新建DWORD值，名为`SynAttackProtect`，值为2。
6、禁止响应ICMP路由通告报文
HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet\ Services\Tcpip\Parameters\Interfaces\interface
新建DWORD值，名为PerformRouterDiscovery 值为0。
7、防止ICMP重定向报文的攻击
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters
将EnableICMPRedirects 值设为0
8、不支持IGMP协议
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters
新建DWORD值，名为IGMPLevel 值为0。
9、禁用DCOM：
运行中输入Dcomcnfg.exe。回车，单击“控制台根节点”下的“组件服务”。打开“计算机”子文件夹。

> 原文：https://www.cnblogs.com/Spring/archive/2008/03/09/1097890.html
