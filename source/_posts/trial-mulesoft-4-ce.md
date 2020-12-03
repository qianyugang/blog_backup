---
title: Mulesoft 4 CE 社区版本试用
toc: true
comments: true
categories: 与代码
tags: 
	- iPaaS
	- Mulesoft

date: 2020-07-08
---

## 部署说明

目前在测试服务器中，使用的是多个项目运行在同一个docker容器中的方式，不同项目使用不同的端口映射，使用docker-compose 管理。其中`/opt/mule/apps`作为项目运行目录，在`docker-compose.yaml`做如下映射：

```
version: '3'
services:
  mule:
    image: "javastreets/mule:latest"
    ports:
      - "8081-8089:8081-8089"
    volumes:
      - ./apps:/opt/mule/apps
    expose:
      - "8082"
      - "8083"
      - "8084"
      - "8085"
      - "8086"
      - "8087"
      - "8088"
      - "8089"
    ulimits:
      nproc: 65535
      nofile:
        soft: 655350
        hard: 655350
    user: root
```

可以选择多个项目运行在一个rumtime docker container，也可以开启更多并行rumtime docker container。对于后一种方式可以复制目前的 `docker-compose.yaml`、规划和修改下其中的端口和目录即可。

## 操作步骤
 
以 https://github.com/manikmagar/mule4-examples/blob/master/say-hello-mule4-docker 为例来介绍一下使用步骤。

### 1. 准备环境
在本地安装 Mulesoft CE 4.2.0 社区版本 runtime，下载地址：https://developer.mulesoft.com/download-mule-esb-runtime ，下载完成后解压，执行如下命令启动 runtime 。
```
./mule-standalone-4.2.0-hf1/bin/mule
```

### 2. 本地打包

在 clone 了 say-hello-mule4-docker 项目之后，需要修改一些文件才能进行本地打包。首先需要修改`pom.xml`文件，`<plugin>`中添加`<configuration>`配置如下：
```
<configuration>
	<standaloneDeployment>
  		<muleHome>/xxxx/mule-standalone-4.2.0-hf1</muleHome>
  		<muleVersion>4.2.0</muleVersion>
	</standaloneDeployment>
</configuration>
```

其中`<muleHome>`的地址需要替换为步骤1中本地运行的 mule-runtime 地址。如果在之前已经运行的项目中有端口冲突，需要把
```
say-hello-mule4-docker/src/main/mule/say-hello-mule4-docker.xml
```

文件中的端口号修改为可用端口号，例如可将`8081`修改为`8083`。
在打包过程中需要保证 runtime 是正在运行的，然后执行如下命令打包：`mvn clean package`
打包完成后将在`target`目录得到`xxxxxx.jar`文件。

### 3. 上传服务器

把步骤2中打包完成后的`xxxxxx.jar`文件上传到服务器的`/opt/mule/apps`目录中：
```
scp xxxxx.jar root@10.249.2.23:/opt/mule/apps
```
    
### 4. 重启runtime的docker容器

```
ssh root@10.249.2.23 "cd /opt/mule; docker-compose restart"
```
### 5. 访问

访问链接`http://10.249.2.23:8083/test/hello`查看是否成功。

## 遗留问题

1. 目前最新的 Anypoint Stuido 7 不支持 Mulesoft CE 社区开源 4.x版本，所以项目编辑器目前只能使用本文编辑。我们依旧在持续探索更好的IDE方案。
