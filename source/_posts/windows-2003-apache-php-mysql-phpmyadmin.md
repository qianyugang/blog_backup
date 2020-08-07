---
title: windows2003手动配置apache+php+Mysql+phpMyAdmin完全教程
tags:
  - windows
url: 334.html
id: 334
categories:
  - 我是修电脑的
date: 2012-11-03 14:35:10
---

最近一直在折腾服务器配置，怎么在windows2003操作系统下手动配置apache+php+mysql+phpMyAdmin，因为需要修改的配置文件很多，所以很多人容易在其中出错误，我也是，折腾了我好几天，终于今天把这个东西配置好了。有一些小细节的东西真的很难掌控，所以我在这里尽量把所有的可能出现的问题和我自己出现的问题都在里面说一下，以免碰到更多的问题。 好的，废话不多说了，直接进入安装开始吧，安装的时候照着步骤一步一步来，不要着急，慌张也没用，看完步骤之后我会在后面列出一般会出现的问题，慌慌张张的配置好很容易出现问题的；淡定一点好。 首先在各大官方网站上面下载好apache、php、mysql、phpMyAdmin，然后放到手边上备用。 

Apache 的安装 

1 点击apahce安装包 ![](http://su.100steps.net/xxblog/uploadfiles/php-0.gif "点击apahce安装包") 2 接受协议 ![](http://su.100steps.net/xxblog/uploadfiles/php-1.gif "接受协议") 3 下面3项随便填即可 apache 监听80 端口.如果iis用了80端口,修改iis端口或者apache端口,以免冲突 ![](http://su.100steps.net/xxblog/uploadfiles/php-2.gif "3") 4 这里默认的是 typical,建议选custom,点击next. ![](http://su.100steps.net/xxblog/uploadfiles/php-3.gif "这里默认的是 typical,建议选custom,点击next. ") 5 这里可用修改安装路径,注意安装路径不能含中文. ![](http://su.100steps.net/xxblog/uploadfiles/php-4.gif "这里可用修改安装路径,注意安装路径不能含中文. ") 6 点击install开始安装 ![](http://su.100steps.net/xxblog/uploadfiles/php-5.gif "点击install开始安装 ") 7 安装完成后在浏览器里打入 http://localhost 或者 http://127.0.0.1 如果可以看到这个页面,证明apache安装成功,已经可以解释静态页面了 ![](http://su.100steps.net/xxblog/uploadfiles/php-6.gif "安装成功")

**MySQL 的安装** 1 点击MySQL安装包 ![](http://su.100steps.net/xxblog/uploadfiles/php-7.gif "1") 2 默认是Typical,如果想修改安装路径的话也可以选择custom. 注意:安装mysql的路径中,不能含有中文! ![](http://su.100steps.net/xxblog/uploadfiles/php-8.gif "2") 3 点击intall开始安装 ![](http://su.100steps.net/xxblog/uploadfiles/php-9.gif "3") 4 跳过注册 ![](http://su.100steps.net/xxblog/uploadfiles/php-10.gif "4")  5 是否现在就配置 MySQL.也可以之后在开始菜单的configuration wizard 进行配置. 这里是现在进行配置 ![](http://su.100steps.net/xxblog/uploadfiles/php-11.gif "5") 6 选择Detailed Configuration(详细设置),点Next继续 ![](http://su.100steps.net/xxblog/uploadfiles/php-12.gif "6")  7 下面这个选项是选择mysql应用于何种类型,第一种是开发服务器,将只用尽量少的内存,第二种是普通WEB服务器,将使用中等数量内存,最后一种是这台服务器上面只运行MySQL数据库,将占用全部的内存.用户可根据自己的需求,选择选项.这里只选择开发服务器,点Next继续 ![](http://su.100steps.net/xxblog/uploadfiles/php-13.gif "7") 8 下面是选择数据库用途,第一种是多功能用途,将把数据库优化成很好的innodb(事务)存储类型和高效率的myisam(非事务)存储类型,第二种是只用于事务处理类型,最好的优化innodb,但同时也支持myisam,只有myisam才支持全文索引最后一种是简单的网络开发,适合于简单的应用,只有不支持事务的myisam类型是被支持的.一般选择第一种多功能的. ![](http://su.100steps.net/xxblog/uploadfiles/php-14.gif "8")  9 下面是选择InnodDB的数据存放位置,一般默认好了,不需要改动(如果要修改数据保存路径,也可用在安装后修改my.ini的datadir的值) ![](http://su.100steps.net/xxblog/uploadfiles/php-15.gif "9") 10 下面是选择MySQL允许的最大连接数,第一种是最大20个连接并发数,第二种是最大550个并发连接数,最后 一种是自定义,你可以根据自己的需要选择.这里选择第一个 ![](http://su.100steps.net/xxblog/uploadfiles/php-16.gif "10") 11 下面是是否运行网络链接.这里选择复选框.数据库监听的端口,一般默认是3306,如果改成其他端口,以后连接数据库的时候都要记住修改的端口,否则不能连接mysql数据库,比较麻烦,这里不做修改,用mysq的默认端口：3306 ![](http://su.100steps.net/xxblog/uploadfiles/php-17.gif "11") 12 这一步设置mysql的默认编码,默认是latin1,也是标准的编码.第二种是UTF8,第三种是手动设置.编码是版本4.1以上引入的.如果要用原来数据库的数据,最好能确定原来数据库用的是什么编码,如果这里设置的编码和原来数据库数据的编码不一致,在使用的时候可能会出现乱码.建议使用latin1标准编码 ![](http://su.100steps.net/xxblog/uploadfiles/php-18.gif "12") 13 这一步是是否要把mysql设置成windows的服务,一般选择设成服务,这样以后就可以通过服务中启动和关闭mysql数据库了.推荐：下面的复选框也勾选上,这样,在cmd模式下,不必非到mysql的bin目录下执行命令.在命令行下咨询可以执行文件 ![](http://su.100steps.net/xxblog/uploadfiles/php-20.gif "13") 14 这一步是设置mysql的超级用户密码,这个超级用户非常重要,对mysql拥有全部的权限,请设置好并牢记超级用户的密码, 下面有个复选框是表示创建一个匿名账号,这会使数据库系统不安全.如果有这个需求,也请勾选. ![](http://su.100steps.net/xxblog/uploadfiles/php-21.gif "14") 15 点击 Execute进行安装 ![](http://su.100steps.net/xxblog/uploadfiles/php-22.gif "15") 16 点击finish完成安装 ![](http://su.100steps.net/xxblog/uploadfiles/php-23.gif "16") 如果在第15步,出现下图提示 ![](http://su.100steps.net/xxblog/uploadfiles/php-24.gif "17") 是因为你之前装过mysql.卸载时还保留了一些配置文件. 点击retry看看是否可以通过。否则点击 cancel 退出.然后点击开始菜单的 MySQL Server Instance Config Wizard 重新配置 mysql 重复之前的操作.第14步将会出现界面是像下面这样 ![](http://su.100steps.net/xxblog/uploadfiles/php-25.gif "18") 有三个输入密码的地方,你原来装过mysql. 你在第一个文本框输入原来root的密码,后面两个文本框输入root的新密码就可以了 如果还是不行那就重装一次MySQL。 重装注意事项：最好删除原来的所有文件,必要的话,可以清一下注册表, 如果你机器上没有其它mysql相关的程序.而且一定记得不要保留原有的my.ini文件. 还有就是删除原来安装路径下的文件,并删除数据目录下面的ibdata1文件. **PHP 的安装** 由于php是一个zip文件(非install版),安装较为简单 解压就行.把解压的 php-5.2.1-Win32 重命名为 php5.并复制到C盘目录下.即安装路径为 c:\\php5 1 找到php目录下的 php.ini.recommended (或者php.ini-dist)文件,重命名为 php.ini并复制到系统盘的windows目录下(以c:\\windows为例). 2 再把php目录下的php5ts.dll,libmysql.dll复制到目录 c:\\windows\\system32下. 3 把php5\\ext目录下的php\_gd2.dll,php\_mysql.dll,php\_mbstring.dll文件复制到c:\\windows\\system32下注意:不要把 php\_mysql.dll 和 php\_mssql.dll 混淆如果没有加载 php\_gd2.dll php将不能处理图像.没有加载php\_mysql.dll php将不支持mysql函数库php\_mbstring.dll在后面使用phpmyadmin时支持宽字符 **配置php并关联MySQL** 打开c:\\windows\\php.ini文件 **1 设置扩展路径** 查找 extension\_dir 有这么一行 extension\_dir = "./" 将此行改成 extension\_dir = "C:\\php5\\ext" 其中C:\\php5是你安装php的路径.路径不正确将无法加载dll (注意:有些php版本是 ;extension\_dir = "./" 要把前面的分号去掉) **2 分别查找** ;extension=php\_mbstring.dll ;extension=php\_gd2.dll ;extension=php\_mysql.dll 把上面3项前面的分号去掉,这样apache启动时就可以加载这些dll了 注意不要把 ;extension=php\_mysql.dl 和 ;extension=php\_mssql.dl 混淆 当然前面我们也把这些dll复制到system32下了.(大家在安装的过程中都注意到如何把一些dll加载入来了. 以后要加载一些dll,比如说php\_mysqli.dll,也就懂得怎么加载了) **3 设置会话保存路径** 查找session.save\_path 有这么一行 ; session.save\_path = "N;/path" 在此行后加入一行(注意是加入一行,不是加到后面) session.save_path = "C:\\WINDOWS\\Temp" 保存到你的临时目录下,这里完全可以保存到windows临时目录Temp下 **4 还有比较值得注意的是 short\_open\_tag .**有一些php版本默认是Off的.也就是说 php不能使用短标记如 必须使用由于短标记使用方便,并且很多程序也是用短短标记来写,如discuz等，如果不把 short\_open\_tag 改成On将出现的症状将很难判断是上面原因,这里建议修改 查找 short\_open\_tag = Off 改为 short\_open\_tag = On **5 是否显示错误 display_errors** 出于安全性考虑,display\_errors 有些版本也默认为 Off.就是说在调试时,如果php代码有误,就只出现一个空白页.而不会显示出错原因和出错行数.这样调试起来将非常不便,建议根据自己需要修改 查找 display\_errors = Off (注意不是 ; - display\_errors = Off \[Security\]) 改成 display\_errors = On **6 显示NOTICE敬告提示** 第五步虽然打开了出错提示,但出错报告还受到 error\_reporting 的控制.php5默认关闭NOTICE敬告提示,如果是在本地调试,建议打开NOTICE敬告提示. 查找 error\_reporting = E\_ALL 改成 error\_reporting = 7 另外提示一下,在程序中也可以通过error_reporting()控制错误报告输出,具体怎么用大家参考下手册. **7 register_globals** 出于安全性考虑它默认也是Off 当register\_globals=Off的时候,下一个程序接收的时候应该用$\_POST\['user\_name'\]和$\_POST\['user\_pass'\]） 当register\_globals=On的时候,下一个程序可以直接使用$user\_name和$user\_pass来接受值. 建议根据自己需要修改,为了兼容问题,我还是把它改成On了. **8 php5时差问题** 时间相差八小时 为什么呢?PHP5系列版本新增了时区设置,默认为格林威治时间,与中国所在的东8区正好相差8个小时 查找date.timezone有这么一行 ;date.timezone = 将;去掉,改成 date.timezone = PRC 其中PRC：People's Republic of China 中华人民共和国, **9 php5上传文件问题** a. 一般的文件上传,除非文件很小.就像一个5M的文件,很可能要超过一分钟才能上传完. 但在php中,默认的该页最久执行时间为 30 秒.就是说超过30秒,该脚本就停止执行. 这就导致出现 无法打开网页的情况.这时我们可以修改 max\_execution\_time 在php.ini里查找 max\_execution\_time 默认是30秒.改为 max\_execution\_time = 0 0表示没有限制 另一种方法是可以在php程序中加入 set\_time\_limit(); 来设定页面最久执行时间. set\_time\_limit(0);//0表示没有限制 b. 修改 post\_max\_size 设定 POST 数据所允许的最大大小。此设定也影响到文件上传。 php默认的post\_max\_size 为2M.如果 POST 数据尺寸大于 post\_max\_size $\_POST 和 $\_FILES superglobals 便会为空. 查找 post\_max\_size .改为 post\_max\_size = 150M c. 很多人都会改了第二步.但上传文件时最大仍然为 8M. 为什么呢.我们还要改一个参数upload\_max\_filesize 表示所上传的文件的最大大小。 查找upload\_max\_filesize,默认为8M改为 upload\_max\_filesize = 100M 另外要说明的是,post\_max\_size 大于 upload\_max\_filesize 为佳. **Apache整合PHP** **1 打开apache配置文档，**以作者的电脑为例：D:\\myphp\\apache2.2\\conf\\httpd.conf **2 修改网站根目录** 查找DocumentRoot有这么一行 DocumentRoot "C:/Program Files/Apache Software Foundation/Apache2.2/htdocs" 这就是你网站的根目录,你可以修改,也可以用默认的.如果改,还要修改下面这项,否则可能会出现 403 错误 查找 This should be changed to whatever you set DocumentRoot to 在它下面两行有 把上面两项的 C:/Program Files/Apache Group/Apache2/htdocs 改成你想要的目录 **3 查找 DirectoryIndex index.html** 修改成 DirectoryIndex index.html index.html.var index.php 这样index.php 可以充当默认页面了 **4 Apache中模块化安装php** 查找 # LoadModule foo\_module modules/mod\_foo.so 在此行后加入一行 LoadModule php5\_module C:/php5/php5apache2\_2.dll (其中C:/php5/php5apache2\_2.dll是你安装php的相应路径. 注意不要把php5apache2\_2.dll,php5apache2.dll和php5apache.dll混淆.php5apache.dll只适用于apache 版本1的. PHP5压缩包里的php5apache2.dll只适用于apache2.0.*版本,如果是2.2.*以上版本,必须使用php5apache2\_2.dll.否则就可能会出现 "Cannot load C:/php/php5apache2.dll into server: The specified module could not be found." 或者: "The requested operation has failed" 的情况. 不过php5apache2\_2.dll出来之后也就没有多少参考价值了) **5 查找 AddType application/x-gzip .gz .tgz** 在此行后加入一行 AddType application/x-httpd-php .php 这样apache就可以解释php文件了 到这里配置基本完成了 **6 重启apache，**在网站根目录下创建一个 phpinfo.php 文件 在浏览器中打开。如果能正常看到php的信息，则说明php已经配置好了。 然后，把你下载好的phpMyAdmin，放到apache下的网站目录下，然后进入http://localhost/phpMyAdmin，就可以用图形管理界面管理你的数据库了。

**下面说一说可能出现的问题**
----------------

The requested operation has failed，其实这个报错很笼统，你根本就无从下手，但是在windows下，你就可以再cmd中使用一个命令来解决，具体的报错原因或者解决方法是： 原因一：80端口占用例如IIS，另外就是迅雷。我的apache服务器就是被迅雷害得无法启用！ 原因二：软件冲突装了某些软件会使apache无法启动如Dr.com 你打开网络连接->TcpIp属性->高级->WINS标签 把netbios的lmhosts对勾去掉，禁用tcp/ip的netbios. 然后再启动应该就可以了。 原因三：httpd.conf配置错误如果apache的配置文件httpd.conf搞错了，在windows里启动它，会提示the requested operation has failed，这是比较郁闷的事，因为查错要看个半天。 其实可以用命令行模式启动apache，并带上参数，apache会提示你哪句有误，然后就可以针对性的解决。 原因四：我遇到的就是扩展插件开启的过多，可能互相有影响，所以我们只开启自己需要的插件，不需要的插件就不要开启，因为这样很容易无法保障的，我再这里列个表出来，说说每个扩展模块的作用，方便大家做取舍。 检查错误方法:进入cmd 然后进入 Apache安装目录(具体为你自己的安装目录)\\bin> httpd.exe -w -n "Apache2" -k start (引号中的Apache2修改为你的Apache服务名,我的是2.2.4版,服务名就是Apache2,可以到计算机服务里找)

**扩展库                                     说明 **

**php_bz2.dll bzip2        压缩函数库 **

**php_calendar.dll         历法转换函数库 自 ****PHP 4.0.3 起内置 **

**php_cpdf.dll Clib         PDF 函数库 **

**php_crack.dll                密码破解函数库 **

**php_ctype.dll                ctype 家族函数库 自 PHP 4.3.0 起内置 **

**php_curl.dll                    CURL    客户端 URL 库函数库 需要：libeay32.dll，ssleay32.dll（已附带） **

**php_cybercash.dll    网络现金支付函数库 PHP <= 4.2.0 **

**php\_db.dll                       DBM          函数库 已废弃。用 DBA 替代之（php\_dba.dll） **

**php_dba.dll                    DBA        数据库（dbm 风格）抽象层函数库 **

**php_dbase.dll               dBase 函数库 **

**php_dbx.dll dbx          函数库 **

**php_domxml.dll         DOM XML 函数库 PHP <= 4.2.0 需要：libxml2.dll（已附带），PHP >= 4.3.0 需要：iconv.dll（已附带） **

**php_dotnet.dll               .NET 函数库 PHP <= 4.1.1 **

**php\_exif.dll EXIF        函数库 需要 php\_mbstring.dll。并且在 php.ini 中，**

**php\_exif.dll                    必须在 php\_mbstring.dll之后加载。 **

**php_fbsql.dll                  FrontBase 函数库 PHP <= 4.2.0 **

**php_fdf.dll FDF            表单数据格式化函数库 需要：fdftk.dll（已附带） **

**php_filepro.dll              filePro 函数库 只读访问 **

**php_ftp.dll FTP            函数库 自 PHP 4.0.3 起内置 **

**php\_gd.dll GD               库图像函数库 在 PHP 4.3.2 中删除。此外注意在 GD1 中不能用真彩色函数，用 php\_gd2.dll 替代。 **

**php_gd2.dll GD            库图像函数库 GD2 **

**php\_gettext.dll             Gettext 函数库 PHP <= 4.2.0 需要 gnu\_gettext.dll（已附带），PHP >= 4.2.3 需要 libintl-1.dll，iconv.dll（已附带）。 **

**php\_hyperwave.dll HyperWave 函数库 php\_iconv.dll ICONV 字符集转换 需要：iconv-1.3.dll（已附带），PHP >=4.2.1 需要 iconv.dll **

**php_ifx.dll                       Informix 函数库 需要：Informix 库 **

**php_iisfunc.dll             IIS 管理函数库 **

**php_imap.dll                  IMAP，POP3 和 NNTP 函数库 **

**php\_ingres.dll               Ingres II 函数库 需要：Ingres II 库 php\_interbase.dll InterBase functions 需要：gds32.dll（已附带） **

**php_java.dll                    Java 函数库 PHP <= 4.0.6 需要：jvm.dll（已附带） **

**php_ldap.dll                   LDAP 函数库 PHP <= 4.2.0 需要 libsasl.dll（已附带），PHP >= 4.3.0 需要 libeay32.dll，ssleay32.dll（已附带） **

**php_mbstring.dll       多字节字符串函数库 **

**php_mcrypt.dll            Mcrypt 加密函数库 需要：libmcrypt.dll **

**php_mhash.dll             Mhash 函数库 PHP >= 4.3.0 需要：libmhash.dll（已附带） **

**php\_mime\_magic.dll Mimetype 函数库 需要：magic.mime（已附带） **

**php_ming.dll                 Ming 函数库（Flash） **

**php_msql.dll                  mSQL 函数库 需要：msql.dll（已附带） **

**php_mssql.dll                MSSQL 函数库 需要：ntwdblib.dll（已附带） **

**php_mysql.dll               MySQL 函数库 PHP >= 5.0.0 需要 libmysql.dll（已附带） **

**php_mysqli.dll              MySQLi 函数库 PHP >= 5.0.0 需要 libmysql.dll（PHP <= 5.0.2 中是 libmysqli.dll）（已附带） **

**php_oci8.dll                    Oracle 8 函数库 需要：Oracle 8.1+ 客户端库 **

**php_openssl.dll            OpenSSL 函数库 需要：libeay32.dll（已附带） **

**php_oracle.dll               Oracle 函数库 需要：Oracle 7 客户端库 **

**php_overload.dll         对象重载函数库 自 PHP 4.3.0 起内置 **

**php_pdf.dll                       PDF 函数库 **

**php_pgsql.dll                  PostgreSQL 函数库 **

**php_printer.dll             打印机函数库 **

**php_shmop.dll              共享内存函数库 **

**php_snmp.dll                 SNMP 函数库 仅用于 Windows NT！ **

**php\_soap.dll                    SOAP 函数库 PHP >= 5.0.0 php\_sockets.dll Socket 函数库 **

**php\_sybase\_ct.dll        Sybase 函数库 需要：Sybase 客户端库 **

**php\_tidy.dll                     Tidy 函数库 PHP >= 5.0.0 php\_tokenizer.dll Tokenizer 函数库 自 PHP 4.3.0 起内置 **

**php\_w32api.dll              W32api 函数库 php\_xmlrpc.dll XML-RPC 函数库 PHP >= 4.2.1 需要 iconv.dll（已附带） **

**php_xslt.dll                      XSLT 函数库 PHP <= 4.2.0 需要 sablot.dll，expat.dll（已附带）。PHP >= 4.2.1 需要 sablot.dll，expat.dll，iconv.dll（已附带）。 **

**php_yaz.dll                       YAZ 函数库 需要：yaz.dll（已附带） **

**php_zip.dll                        Zip 文件函数库 只读访问 **

**php_zlib.dll                      ZLib 压缩函数库 自 PHP 4.3.0 起内置**
