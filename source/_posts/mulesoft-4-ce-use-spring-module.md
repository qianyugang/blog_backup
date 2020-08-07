---
title: 在 mulesoft 4 CE版本 使用 Spring Module
toc: true
comments: true
categories: 与代码
tags: 
	- iPaaS
	- Mulesoft

date: 2020-08-06
---

在使用 mulesoft 4 的时候，需要引入 spring 来使用，根据官方文档发现有直接的教程可以使用，官方的教程是针对Mulesoft Enterprise edition(EE收费版)的，如果要在Mulesoft Community edition（CE开源免费版）中使用，还是需要填一些坑的，这篇文章就整理了一下引入的步骤，以及在引入过程中发现的问题和解决方案。
相关文档以及步骤如下:

## 相关文档：

- [Spring Module Release Notes - Mule 4](https://docs.mulesoft.com/release-notes/connector/spring-module-release-notes#1-3-3)
- [Migrating the Spring Module](https://docs.mulesoft.com/mule-runtime/4.3/migration-module-spring)


## 主要步骤如下

### 一. 添加Spring Module到项目中

在 Anypoint Studio 7 中，默认配置中已经提供了Spring Module 相关配置。

在Mule Palette，直接在搜索框中输入“spring”就可以找到相关组件并拖入到流程中。

如果只使用XML编写项目，从Mule Palette中添加Spring模块到你的应用程序，或者在`pom.xml`文件中添加以下依赖项:

```
<dependency>
  <groupId>org.mule.modules</groupId>
  <artifactId>mule-spring-module</artifactId>
  <version>RELEASE</version>
  <classifier>mule-plugin</classifier>
</dependency>
```

以上是EE版本，经过我的使用发现，CE版本是在Mule Palette中是找不到spring模块的，所以直接使用在`pom.xml`文件中添加依赖项。


### 二. 配置Spring Module

#### 1. 添加以下配置

```
<?xml version="1.0" encoding="UTF-8"?>

<mule xmlns:spring="http://www.mulesoft.org/schema/mule/spring"
      xmlns="http://www.mulesoft.org/schema/mule/core"
      xmlns:doc="http://www.mulesoft.org/schema/mule/documentation"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.mulesoft.org/schema/mule/core
        http://www.mulesoft.org/schema/mule/core/current/mule.xsd
        http://www.mulesoft.org/schema/mule/spring
        http://www.mulesoft.org/schema/mule/spring/current/mule-spring.xsd">

    <spring:config name="springConfig" files="beans.xml" />

</mule>
```

#### 2. 把`beans.xml`文件放入`src/main/resources`中。

spring配置必须是有效的。以下是一个示例`bean.xml`文件:
```
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:jdbc="http://www.springframework.org/schema/jdbc"

    xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
      http://www.springframework.org/schema/jdbc
      http://www.springframework.org/schema/jdbc/spring-jdbc-4.2.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context-4.2.xsd
      http://www.springframework.org/schema/security
      http://www.springframework.org/schema/security/spring-security-4.2.xsd">

    <jdbc:embedded-database id="dataSource" type="DERBY">
        <jdbc:script location="classpath:db/sql/create-db.sql" />
        <jdbc:script location="classpath:db/sql/insert-data.sql" />
    </jdbc:embedded-database>

</beans>
```

#### 3. 添加`mule-artifact.json`

必须使用

如果使用的是Anypoint Studio 7.2或更高版本，不需要手动更新`mule-artifact.json`文件。

因为mule也需要一个描述符的Spring bean声明，因此Studio中的嵌入式打包程序会自动生成一个从项目中获取的所有资源。

下面是一个在`mule-artifact.json`中声明`bean .xml`文件的例子：
```
{
    "name": "MyApp",
    "minMuleVersion": "4.2.1",
    "classLoaderModelLoaderDescriptor": {
        "id": "mule",
        "attributes": {
            "exportedResources": [
                "/org/mule/myapp/beans.xml"
            ]
        }
    }
  }
}
```


### 三. 添加Spring Module到项目中

使用Spring对象在Mule配置组件（例如,指向Derby数据源创建Spring bean），如下:

```
<?xml version="1.0" encoding="UTF-8"?>

<mule xmlns:db="http://www.mulesoft.org/schema/mule/db"
      xmlns:spring="http://www.mulesoft.org/schema/mule/spring"
      xmlns="http://www.mulesoft.org/schema/mule/core"
      xmlns:doc="http://www.mulesoft.org/schema/mule/documentation"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.mulesoft.org/schema/mule/core
        http://www.mulesoft.org/schema/mule/core/current/mule.xsd
        http://www.mulesoft.org/schema/mule/spring
        http://www.mulesoft.org/schema/mule/spring/current/mule-spring.xsd
        http://www.mulesoft.org/schema/mule/db
        http://www.mulesoft.org/schema/mule/db/current/mule-db.xsd">

    <spring:config name="springConfig" files="beans.xml" />

    <db:config name="derbyConfig" doc:name="Database Config"  >
        <db:derby-connection database="datasource" create="true" />
    </db:config>
</mule>
```



### 问题 & 解决

做完以上操作，应该是可以使用spring了，至少在Mulesoft Enterprise edition(EE收费版)是可行的，但是当我把它挪到Mulesoft Community edition（CE开源免费版）的时候，却怎么样也不能引入，一直在报错`[beans.xml] cannot be opened because it does not exist`，但是我明明引入了已经。

最后是发现，其实在开源版本中有一个BUG，也就是`mule-artifact.json`和你的flew xml文件有冲突，所以只需要把`mule-artifact.json`文件中，和`beans.xml`引入相关的删掉，就可以使用了。flew xml文件中的

```
<spring:config name="springConfig" files="beans.xml" />
```

这一句还是必须要的，而且还是必须把`beans.xml`文件放入`src/main/resources`中，才可以完成最终的spring 模块引入。


