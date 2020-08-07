---
title: Go 语言 Web 框架 Revel 安装以及使用 GORM 操作 MySQL
toc: true
categories: 与代码
tags: 
	- Go语言
date: 2019-08-05
---

Revel是一个高生产力的 Go 语言 Web 框架。框架源于java的 Play，文件结构是经典的MVC结构，比较清晰，是现在go语言中比较流行的web框架之一。
 
- 官方网站：http://revel.github.io/
- 项目地址：https://github.com/revel/revel
- 中文社区：https://gorevel.cn/
- 中文手册：https://gorevel.cn/docs/manual/index.html

GORM 是一个Golang写的，开发人员友好的ORM库。可以简单直观的对 Mysql，Redis等等一些流行的数据库进行操作。

- 官方网站：https://gorm.io/
- 项目地址：https://github.com/jinzhu/gorm
- 中文手册：https://jasperxu.github.io/gorm-zh/

最近项目中需要一个集中处理id的功能，刚好准备尝试一下使用go，架构师推荐了这个框架，那么就来结合一下GORM做一个小小的使用入门。当然在revel的官方项目中，也给了一个例子：https://github.com/revel/examples/tree/master/orm/gorm

## 安装revel

假定这里你已经成功安装了go，并且已经配置好了 GOROOT 和 GOPATH，GOROOT是go语言编译、工具、标准库等的安装路径，GOPATH是go的工作目录，你的项目都是在这个目录下面。

### 安装框架

**执行如下命令安装revel框架：**
```
$ go get -u -v github.com/revel/cmd/revel
```
**然后可以新建一个revel项目看看是否安装成功：**
```
$ revel new myapp
```
如果提示revel命令找不到，可以找到revel的安装目录放到环境变量中再执行。

**运行项目：**
```
revel run myapp
```
**打开浏览器访问**
```
 http://localhost:9000
```
it works，代表安装运行成功。

`revel new myapp` 命令生成的目录结构如下：

```
myapp                 项目根目录
├── app               MVC框架目录
│   ├── controllers   控制器目录
│   ├── init.go
│   ├── models        模型目录
│   ├── routes
│   ├── tmp
│   └── views         视图目录
├── conf
│   ├── app.conf      配置文件
│   └── routes        路由文件
├── messages          国际化目录
├── public            静态文件目录
└── tests
```

## 安装GROM

### 安装相关包

安装gorm包：
```
go get -u -v github.com/jinzhu/gorm
```

安装mysql驱动包：
```
go get -u -v github.com/go-sql-driver/mysql
```

安装


## 在revel中使用GROM

这里我们使用 https://github.com/revel/examples/tree/master/orm/gorm 官方例子来说明：首先把这个gorm代码拉到工作目录下

### 配置文件

打开`config/app.conf`文件，然后修改其中数据库配置：
```
db.driver=mysql # 数据库类型 ： mysql, postgres, sqlite3
db.host=localhost  # Use dbhost  /tmp/gorm.db is your driver is sqlite
db.name=test #数据库名
db.password=root
db.user=root
```

### model 文件说明

在model文件中添加一个`user.go`文件：
```
package models

import (
	"github.com/jinzhu/gorm"
	"golang.org/x/crypto/bcrypt"
)

// User model
type User `struct` {
	gorm.Model
	Name           string `gorm:"size:255"`
	Email          string `gorm:"type:varchar(100);unique_index"`
	HashedPassword []byte
	Active         bool
	FileName       string `gorm:"size:255"`
}

// SetNewPassword set a new hashsed password to user
func (user *User) SetNewPassword(passwordString string) {
	bcryptPassword, _ := bcrypt.GenerateFromPassword([]byte(passwordString), bcrypt.DefaultCost)
	user.HashedPassword = bcryptPassword
}

```

此文件是用来定义表的model，`struct`结构体中和你的数据表是一一对应的。注意到此model用到了另一个包，需要安装一下：
```
 go get -u -v golang.org/x/crypto
```

### controller 文件说明

在controllers 中添加一个`init.go`文件
```
package controllers

import (
	"github.com/revel/examples/orm/gorm/app/models"
	gorm "github.com/revel/modules/orm/gorm/app"
	"github.com/revel/revel"
)

func initializeDB() {
	gorm.DB.AutoMigrate(&models.User{})
	var firstUser = models.User{Name: "Demo", Email: "demo@demo1.com"}
	firstUser.SetNewPassword("demo")
	firstUser.Active = true
	gorm.DB.Create(&firstUser)
}


func init() {
	revel.OnAppStart(initializeDB)
}
```
这个文件相当于初始化操作，在请求到controller的时候会执行一下，这里是使用的gorm的一个`gorm.DB.AutoMigrate`方法，如果没有就创建表。当然，如果不需要的话是可以去掉。

在controllers 中添加一个`app.go`文件
```
package controllers

import (
	"github.com/revel/examples/orm/gorm/app/models"
	gormc "github.com/revel/modules/orm/gorm/app/controllers"
	"github.com/revel/revel"
)

type App struct {
	gormc.TxnController
}

func (c App) Index() revel.Result {
	var users = []models.User{}
	c.Txn.Find(&users)
	return c.RenderJSON(users)
}

```

这里只是列了一个最基本的find操作，更多的操作可以查看 https://jasperxu.github.io/gorm-zh/ grom中文手册。

### view 文件说明

这里由于是演示，就不使用页面展示了，在 controller 中使用`RenderJSON`，直接以json展示。

运行项目：
```
revel run gorm
```

然后打开：http://localhost:9000/ 查看数据正常展示，打完收工。












