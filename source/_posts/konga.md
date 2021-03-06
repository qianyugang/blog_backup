---
title: 使用 konga 来管理微服务 API 网关 kong
toc: true
categories: 与代码
tags: 
	- Kong
	- Kong中文教程

date: 2019-07-04
---

微服务网关kong有比较多个后台管理面板，比如比较简单的kong-dashboard，还有konga，之前在初探kong的时候，使用的就是比较简单的kong-dashboard，很多功能都没有，而且最近由于kong官方更新比较频繁，1.0之后的kong-dashboard就已经不兼容了，频繁报错，所有今天我就来使用一下另一款kong的后台管理面板：konga

- 官方网址：https://pantsel.github.io/konga/
- 项目地址：https://github.com/pantsel/konga
- 项目demo：http://139.59.145.231:1337/

## 安装konga

在开始安装之前，需要准备的有：

- 一个已经安装好的kong环境
- Nodejs >= 8 (推荐8.11.3 LTS)
- npm

关于这三者的安装，我这里就不赘述了，kong的安装可以在博客的[相关文章](http://www.102no.com/archives/tag/kong)查看。在做好准备工作之后，就开始安装：

### 安装npm依赖
```
$ git clone https://github.com/pantsel/konga.git
$ cd konga
$ npm i
```

在执行`npm i`之后，由于会引入很多的npm包，所以可能会有报错，比如我这里就遇到了一些权限问题，比如：
```
Unable to save binary /home/qianyugang/soft/konga/node_modules/node-sass/vendor/linux-x64-64 : { Error: EACCES: permission denied, mkdir '/home/thinkpad/soft/konga/node_modules/node-sass/vendor'

```
如果出现了如上错误，可以把最后一个命令修改为
```
sudo npm i --unsafe-perm=true --allow-root
```

执行之后，可能会提示：
```
bower bootstrap-switch extra-resolution Unnecessary resolution: bootstrap-switch#~3.3.4
added 69 packages from 77 contributors and audited 3120 packages in 4.654s
found 275 vulnerabilities (125 low, 24 moderate, 126 high)
run `npm audit fix` to fix them, or `npm audit` for details
```

意思是有一些包没有执行好，那么我们就按照它的提示执行一下：
```
sudo npm audit fix --unsafe-perm=true --allow-root
```
基本就可以解决npm依赖包的问题了。总之这里是有可能出现各种各样的npm问题，依次解决即可。

### 初始化数据库

npm的依赖都安装完成之后，就需要复制一下konga目录下的`.env_example`文件
```
cp .env_example env
```
然后把其中的一些配置项目都填写上去。具体的配置项可以查看 https://github.com/pantsel/konga#environment-variables 。这里就主要把以下三项配置好：
```
DB_ADAPTER=postgres
PORT=1337
DB_URI=postgresql://kong_user:kong_pass@localhost:5432/konga
```

注意这个DB_URI一定要填写完整，这里的数据库我用的是postgres，konga其实还支持mysql，mongo等多种数据库，这里就不赘述了。配置完成之后，需要登上你自己的postgres数据库，然后执行如下命令新建数据库：
```
CREATE DATABASE "konga" WITH ENCODING='UTF8';
```

新建了数据库之后，，执行如下命令，来初始化数据库：
```
node ./bin/konga.js prepare --adapter postgres --uri postgresql://kong_user:kong_pass@localhost:5432/konga
```
出现如下提示之后，数据库这一块就完成：
```
Preparing database...
debug: Hook:api_health_checks:process() called
debug: Hook:health_checks:process() called
debug: Hook:start-scheduled-snapshots:process() called
debug: Hook:upstream_health_checks:process() called
debug: Hook:user_events_hook:process() called
debug: Seeding User...
debug: User seed planted
debug: Seeding Kongnode...
debug: Kongnode seed planted
debug: Seeding Emailtransport...
debug: Emailtransport seed planted
debug: Database migrations completed!
```

### 启动konga
执行如下命令：
```
npm start
```
看到如下小帆船的图，成功启动
```

   Sails              <|    .-..-.
   v0.12.14            |\
                      /|.\
                     / || \
                   ,'  |'  \
                .-'.-==|/_--'
                `--'-------' 
   __---___--___---___--___---___--___
 ____---___--___---___--___---___--___-__

```

最后，打开浏览器输入`http://localhost:1337/`，就可以进入konga的管理界面了。会出现一个让你注册的界面，注册登录一下，然后配置一下kong的admin-api链接，大功告成，打完收工。


