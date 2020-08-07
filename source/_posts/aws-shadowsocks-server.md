---
title: 使用亚马逊AWS的免费服务搭建Shadowsocks(Ubuntu 16.04)
toc: true
categories: 与代码
tags: 
	- Shadowsocks 
date: 2018-02-28
---

## 亚马逊 AWS 免费服务
首先你需要注册一个免费的亚马逊的 AWS 服务，具体注册教程如下，准备好一张信用卡，根据下面的免费注册教程一步一步的来，就可以得到了一年免费使用的vps。这里就使用这个免费的vps来搭建一下Shadowsocks服务。
- 免费注册教程 [AWS一年免费VPS服务器申请及使用教程](https://www.vpsbuluo.com/youhui/76.html) 
- aws官方首页 [https://aws.amazon.com](https://aws.amazon.com) 
- 免费vps地址 [https://aws.amazon.com/free](https://aws.amazon.com/free)
- VPS管理 [https://console.aws.amazon.com/ec2/home](https://console.aws.amazon.com/ec2/home)

### 注册时候的注意事项
在注册完成后开始选择vps的时候，切记暂停一下，看一下右上角的服务器节点地址，因为很多人直接默认填过去了，导致选择的美国东部俄亥俄等，其实俄亥俄的延时还是比较大的，个人建议选择东京节点，还算是相当快的。虽然还是不如google的台湾节点。可以访问 [http://www.cloudping.info/](http://www.cloudping.info/)这个网址去查看各个节点的速度。

### 安全组设置
在启动了aws的实例之后，记得一定要设置一下安全组策略，不然到后面开启了ss服务会发现依旧无法访问。
在安全组的入站中创建一个自定义 TCP 规则，协议为TCP，然后端口范围就写你下面`shadowsocks.json`文件中的`server_port`的端口，来源就是选择`0.0.0.0/0`，点击添加即可

## 搭建shadowsocks服务

### 安装Shadowsocks
``` bash
$ sudo apt-get update
$ sudo apt-get install python-gevent python-pip python-m2crypto python-wheel python-setuptools
$ sudo pip install shadowsocks
```
在安装中可能会出现以下报错
``` bash
# pip install shadowsocks
Traceback (most recent call last):
  File "/usr/bin/pip", line 11, in <module>
    sys.exit(main())
  File "/usr/lib/python2.7/dist-packages/pip/__init__.py", line 215, in main
    locale.setlocale(locale.LC_ALL, '')
  File "/usr/lib/python2.7/locale.py", line 581, in setlocale
    return _setlocale(category, locale)
locale.Error: unsupported locale setting
```
是因为语言配置错误导致的，解决方案
``` bash
$ export LC_ALL=C

```

### 配置Shadowsocks
``` bash
$ sudo vim /etc/shadowsocks.json
```
写入以下内容
``` bash
{
    "server":"my_server_ip",
    "server_port":8000,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"my_password",
    "timeout":300,
    "method":"rc4-md5"
}
```
然后保存

### 启动和停止

``` bash
$ sudo ssserver -c /etc/shadowsocks.json -d start
$ sudo ssserver -c /etc/shadowsocks.json -d stop
```
在启动的时候有可能提示说找不到`ssserver`，这个时候可能是这个命令没有加入全局变量中，先使用`whereis ssserver`找到命令的位置，然做一个软连接`ln -s 源位置 目标位置`即可。或者直接把命令中的`ssserver`更换为你实际安装路径
如果出现以下报错
``` bash
Traceback (most recent call last):
  File "/bin/ssserver", line 9, in <module>
    load_entry_point('shadowsocks==2.6.8', 'console_scripts', 'ssserver')()
  File "/usr/lib/python2.7/site-packages/shadowsocks/server.py", line 60, in main
    tcp_servers.append(tcprelay.TCPRelay(a_config, dns_resolver, False))
  File "/usr/lib/python2.7/site-packages/shadowsocks/tcprelay.py", line 584, in __init__
    server_socket.bind(sa)
  File "/usr/lib64/python2.7/socket.py", line 224, in meth
    return getattr(self._sock,name)(*args)
socket.error: [Errno 99] Cannot assign requested address
```
如果不想频繁重启来验证是否修改正确，可以使用以下命令查看
``` bash
$ sudo ssserver -c /etc/shadowsocks.json -d status
```
可以尝试把上面的开始命令更换为再次启动
``` bash
$ sudo sslocal -c /etc/shadowsocks.json -d start	
```

### 查看日志
启动和访问的日志位置在`/var/log/shadowsocks.log`，访问日志和报错日志都在这里，如果有启动不了或者访问受限的问题，记得来这里查看并解决。 

## 客户端下载和安装

最后只需要安装好对应的客户端[https://github.com/shadowsocks](https://github.com/shadowsocks)，就可以正常使用ss了

## 疑问和解答

如果在安装或者启动途中有什么报错和问题，可以直接访问[https://github.com/shadowsocks/shadowsocks/issues](https://github.com/shadowsocks/shadowsocks/issues)，即可，应该能解决你大部分的疑问。

