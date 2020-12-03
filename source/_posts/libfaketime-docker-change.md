---
title: 使用 libfaketime 修改 docker 容器时间
toc: true
categories: 与代码
tags: 
	- docker
	- faketime

date: 2019-11-28
---

## 容器的时间问题：

如果想要直接进入容器，使用`date -s`修改日期，则会出现一个

```
date: cannot set date: Operation not permitted
```

的错误，而且也不会成功。

这是由于docker容器的隔离是基于Linux的Capability机制实现的, Linux的Capability机制允许你将超级用户相关的高级权限划分成为不同的小单元。目前Docker容器默认只用到了以下的Capability.

- CHOWN,
- DAC_OVERRIDE,
- FSETID,
- FOWNER,
- MKNOD,
- NET_RAW,
- SETGID,
- SETUID,
- SETFCAP,
- SETPCAP,
- NET_BIND_SERVICE,
- SYS_CHROOT,
- KILL,
- AUDIT_WRITE
    
而要修改系统时间需要有SYS_TIME权限。使用 `--cap-add`, `--cap-drop` 可以添加或禁用特定的权限。`--privileged`参数也可以达到开放权限的作用, 与`--cap-add`的区别就是, `--privileged`是将所有权限给容器.

docker使用`--privileged`, `--cap-add`, `--cap-drop` 来对容器本身的能力进行开放或限制。

那么使用如下命令就可以直接改变时间了：
```
docker run -it --cap-add SYS_TIME --name centos centos /bin/bash
```
接着进入容器实行date命令修改时间，如果没有修改成功，，那么可能就是因为宿主机做了共享主机的localtime（比如laradock就做了）：
```
docker run --name <name> -v /etc/localtime:/etc/localtime:ro  .... 
docker cp /etc/localtime:【容器ID或者NAME】/etc/localtime
```

如果修改成功一会就又恢复了，那么就可能要查看一下宿主机是否做了定时校准的任务。

但是如此执行之后，那就是容器时间变更为5月28日之后，宿主机的时间也跟着变更了，
因为上边操作的 `--cap-add SYS_TIME`是为了将宿主机的内核时间挂载进来与容器共享，因此容器时间更改了，宿主机时间也会跟着更改。


## 使用 libfaketime

由此可见，直接修改docker容器的时间是比较危险的，所以选择如下方案。

首先在宿主机上安装：libfaketime
```
git clone https://github.com/wolfcw/libfaketime.git
cd libfaketime  && make install
```

安装完成之后，把安装后的库文件拷贝到docker中：
```
docker cp /usr/lib/x86_64-linux-gnu/faketime/libfaketime.so.1 e6e239e5fba7:/usr/local/lib/
```

然后再进入docker中执行命令改变环境变量：

```
export LD_PRELOAD=/usr/local/lib/libfaketime.so.1 FAKETIME="-5d"
```

改变环境变量之后，再执行脚本会发现时间已经改变。若想要恢复，直接把环境变量修改为空即可：
```
export LD_PRELOAD=
```

> LD_PRELOAD是Linux系统的一个环境变量，它可以影响程序的运行时的链接（Runtime linker），它允许你定义在程序运行前优先加载的动态链接库。这个功能主要就是用来有选择性的载入不同动态链接库中的相同函数。通过这个环境变量，我们可以在主程序和其动态链接库的中间加载别的动态链接库，甚至覆盖正常的函数库。一方面，我们可以以此功能来使用自己的或是更好的函数（无需别人的源码），而另一方面，我们也可以以向别人的程序注入程序，从而达到特定的目的。

如果需要修改容器中的各种web服务的时间，只需要在改变环境变量之后，重启服务即可。但是注意，镜像必须使用比较基础的镜像，因为如果直接使用服务的镜像（例如php镜像），会在重启的时候，整个容器会退出，faketime就会修改无效。

例如：
```
pkill php-fpm ; /usr/local/php/sbin/php-fpm 
```

## 修改 faketime 步骤

第一步：需要通过dockerfile把libfaketime拷贝部分制作到基础镜像当中。
第二步：通过`uuid`来寻找要执行的pod。
第三步：修改pod的yaml文件（容器启动），把

```export LD_PRELOAD=/usr/local/lib/libfaketime.so.1 FAKETIME="-5d"```

加入yaml，重启pod（会自动重启相应服务php、kong、nginx、go等），此时faketime生效。


## 参考

- https://www.jianshu.com/p/3cfd7e4d8c76
- http://www.eryajf.net/2888.html
- https://blog.csdn.net/u012375924/article/details/100068898




