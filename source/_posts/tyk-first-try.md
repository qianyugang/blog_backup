---
title: Tyk 初探（安装以及使用）
tags:
  - api网关
  - Tyk
url: 1346.html
id: 1346
categories:
  - 我是修电脑的

date: 2018-11-18 12:57:39

---

Tyk介绍
-----

*   官网地址：[https://tyk.io/](https://tyk.io/)
*   官方文档：[https://tyk.io/docs/](https://tyk.io/docs/)
*   项目地址：[https://github.com/TykTechnologies/](https://github.com/TykTechnologies/)

安装介绍
----

tyk依赖mongo以及redis，如果手动安装按照步骤，需要注意先安装好这两个。

docker安装方式：
-----------

官方有提供一个demo，这个demo已经集成了所有的安装项目需要，项目地址 ：[https://github.com/TykTechnologies/tyk-pro-docker-demo](https://github.com/TykTechnologies/tyk-pro-docker-demo)执行命令clone项目下来

```
git clone https://github.com/TykTechnologies/tyk-pro-docker-demo.git
```

然后启动docker，注意，如果你的电脑上已经启动了其他的docker环境，要看看端口是否已经被占用掉了，如果占用了，需要更改docker的配置文件的端口。

启动docker-composer
```
    docker-compose -f docker-compose.yml -f docker-local.yml up
    
Starting tykprodockerdemo\_tyk-redis\_1 ... 
Starting tykprodockerdemo\_tyk-mongo\_1 ... 
Starting tykprodockerdemo\_tyk-mongo\_1
Starting tykprodockerdemo\_tyk-redis\_1 ... done
Starting tykprodockerdemo\_tyk-gateway\_1 ... 
Starting tykprodockerdemo\_tyk-gateway\_1
Starting tykprodockerdemo\_tyk-dashboard\_1 ... 
Starting tykprodockerdemo\_tyk-gateway\_1 ... done
Starting tykprodockerdemo\_tyk-pump\_1 ... 
Starting tykprodockerdemo\_tyk-pump\_1 ... done
```

看到成功启动之后，就可以打开[http://localhost:3000](http://localhost:3000)出现控制面板，此时暂时还不能使用控制面板，需要根据提示去注册一个免费的开发license，注册好了之后可以在邮箱中收到这个license，然后
```
 vim confs/tyk_analytics.conf
```
找到license这一行，把邮件收到的key填入进去。 之后执行
```
sh setup.sh
```
看到一个测试开发版的用户名和密码，登入后面板，即可使用。

常规安装方式
------

参照官方文档安装：[https://tyk.io/docs/get-started/with-tyk-on-premise/installation/](https://tyk.io/docs/get-started/with-tyk-on-premise/installation/)
