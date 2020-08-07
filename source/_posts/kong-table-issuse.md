---
title: 在Kong的插件中使用自建数据表的问题
toc: true
categories: 与代码
tags: 
	- Kong
	- Kong中文文档
	- Kong教程
date: 2019-04-11
---

目前正在开发的kong 插件中需要用到自己新建表，然后对其进行读写，但是由于kong的版本升级原因，有一些问题在官方文档中没有提到，如果按照官方文档开发，很容易掉坑里,导致一直读取失败，这里把其中一些可能会遇到的问题列一下，有一些代码是参考了升级之后，官方自带的插件中的代码，主要注意的点如下：

## 1.迁移文件的差异

### kong的版本>= 1.0
迁移文件的名字为`000_base_xxx.lua`，其中格式格式如下：

```
 return {
    postgres = {
        up = [[
          CREATE TABLE IF NOT EXISTS "xxxxx" (
            "id"	uuid	NOT NULL	DEFAULT uuid_generate_v4 ()	PRIMARY KEY,
            "xx"	int		NOT NULL,
            "xx"	bool,
            "xx"	json,
            "xx"	json
          );
        ]],
    },
    cassandra ={
        up =[[

        ]]
    }
}

```
 
### kong的版本小于1.0版本
迁移文件的名字为`postgres.lua`和`cassandra.lua`，其中格式如下：
此为postgres.lua

```
 return {
  {
    name = "2015-07-31-172400_init_keyauth",
    up = [[
      CREATE TABLE IF NOT EXISTS keyauth_credentials(
        id uuid,
        consumer_id uuid REFERENCES consumers (id) ON DELETE CASCADE,
        key text UNIQUE,
        created_at timestamp without time zone default (CURRENT_TIMESTAMP(0) at time zone 'utc'),
        PRIMARY KEY (id)
      );

      DO $$
      BEGIN
        IF (SELECT to_regclass('public.keyauth_key_idx')) IS NULL THEN
          CREATE INDEX keyauth_key_idx ON keyauth_credentials(key);
        END IF;
        IF (SELECT to_regclass('public.keyauth_consumer_idx')) IS NULL THEN
          CREATE INDEX keyauth_consumer_idx ON keyauth_credentials(consumer_id);
        END IF;
      END$$;
    ]],
    down = [[
      DROP TABLE keyauth_credentials;
    ]]
  }
}
```
 
## 2.daos.lua的差异
daos.lua的格式也是有差异的，但是这部分的差异在文档中是没有体现的
 
### kong的版本>= 1.0

```
local typedefs = require "kong.db.schema.typedefs"

local xxx = {
    primary_key = { "id" },
    name = "app_config",
    fields = {
        { id = typedefs.uuid },
        { xx = { type = "string", required = false, unique = true }, },
        { xx = { type = "string", required = false, unique = true } },
        { xx = { type = "string", required = false } },
        { xx = { type = "string", required = false } },
        { xx = { type = "string", required = false } }
    }
}
return {
    zz = xxx,
}
```

 
### kong的版本小于1.0版本
 
```
-- daos.lua
local SCHEMA = {
  primary_key = {"id"},
  table = "keyauth_credentials", -- the actual table in the database
  fields = {
    id = {type = "id", dao_insert_value = true}, -- a value to be inserted by the DAO itself (think of serial ID and the uniqueness of such required here)
    created_at = {type = "timestamp", immutable = true, dao_insert_value = true}, -- also interted by the DAO itself
    consumer_id = {type = "id", required = true, foreign = "consumers:id"}, -- a foreign key to a Consumer's id
    key = {type = "string", required = false, unique = true} -- a unique API key
  }
}
return {keyauth_credentials = SCHEMA} -- this plugin only results in one custom DAO, named 'keyauth_credentials'

```

尤其注意`fields`字段的结构差异，不然很容易弄混

## 3.调用差异

### kong的版本>= 1.0

```
-- 通过某个字段查询
local app_configs, err = kong.db.app_config:select_by_app_key(app_key)
-- 通过主键查询
local app_group_configs, err = kong.db.app_group_config:select({ id = group_id })
```

### kong的版本小于1.0版本

```
local app_configs, err = kong.dao.app_config:find()
local app_configs, err = kong.dao.app_config:find_all()
```



