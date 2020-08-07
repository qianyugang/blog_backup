---
title: 数据迁移工具对比&介绍
tags:
  - 工具
toc: true
url: 1729.html
id: 1729
categories:
  - 我是修电脑的
date: 2019-11-21 14:40:13
---

1.Tungsten Replicator
=====================

简介 Tungsten Replicator是数据库集群和复制供应商 Continuent 推出的高性能、开源的数据复制引擎，是Continuent最先进的集群解决方案的核心组件之一，特别适合作为异构数据库之间数据迁移的解决方案。 Tungsten replicator被定义为是异构数据库复制框架，可实现不同版本，不同种类数据库之间的数据库复制。根据官方的说明，现在可以实现mysql各版本件，以及mysql与oracle间的数据库复制，但是一些函数上限制还是不能避免。 ![](https://images2015.cnblogs.com/blog/885818/201701/885818-20170107151230331-1412994990.png)

*   官网：[https://code.google.com/p/tungsten-replicator/wiki/Downloads](https://code.google.com/p/tungsten-replicator/wiki/Downloads)

特点

1.  支持高版本MySQL向低版本复制，如5.1->5.0；
2.  支持跨数据库系统的复制，如MySQL->Oracle，并且所支持的数据库不仅包括MySQL、PostgreSQL和Amazon RDS等传统关系型数据库，还包括MongoDB等NoSQL数据库以及Vertica、InfiniDB、Hadoop和Amazon RedShift等数据仓库；
3.  支持多种复制拓扑结构，如Master-Slave、Multi-Master、Direct、Fan-In和Star等。
4.  数据备份与恢复以及数据库基准测试工具等其他辅助功能点

参考链接

*   [https://www.cnblogs.com/fernandolee24/p/6259480.html](https://www.cnblogs.com/fernandolee24/p/6259480.html)
*   [https://cloud.tencent.com/developer/article/1005413](https://cloud.tencent.com/developer/article/1005413)
*   [https://blog.51cto.com/tanzj/595795](https://blog.51cto.com/tanzj/595795)

2\. Maxwell
===========

简介 Maxwell是一个能实时读取MySQL二进制日志binlog，并生成 JSON 格式的消息，作为生产者发送给 Kafka，Kinesis、RabbitMQ、Redis、Google Cloud Pub/Sub、文件或其它平台的应用程序。它的常见应用场景有ETL、维护缓存、收集表级别的dml指标、增量到搜索引擎、数据分区迁移、切库binlog回滚方案等。 maxwell相对于canal的优势是使用简单，它直接将数据变更输出为json字符串，不需要再编写客户端。maxwell可以看作是canal服务端+canal客户端

*   官网：[http://maxwells-daemon.io/](http://maxwells-daemon.io/)
*   项目地址:[https://github.com/zendesk/maxwell](https://github.com/zendesk/maxwell)

特点

*   支持`SELECT*FROM table`的方式进行全量数据初始化
*   支持在主库发生failover后，自动恢复binlog位置(GTID)
*   可以对数据进行分区，解决数据倾斜问题，发送到kafka的数据支持database、table、column等级别的数据分区
*   工作方式是伪装为Slave，接收binlog events，然后根据schemas信息拼装，可以接受ddl、xid、row等各种event

参考链接

*   [https://cloud.tencent.com/developer/article/1403999](https://cloud.tencent.com/developer/article/1403999)
*   [https://laijianfeng.org/2019/03/MySQL-Binlog-%E8%A7%A3%E6%9E%90%E5%B7%A5%E5%85%B7-Maxwell-%E8%AF%A6%E8%A7%A3/](https://laijianfeng.org/2019/03/MySQL-Binlog-%E8%A7%A3%E6%9E%90%E5%B7%A5%E5%85%B7-Maxwell-%E8%AF%A6%E8%A7%A3/)

3.MySQLStreamer
===============

简介 MySQLStreamer 是一个数据库变更捕获和发布系统，它负责捕获数据库的每一条数据变更，将它们打包成消息并发布到Kafka 中。“流表二象性”的概念就是讲述可以重放表中的每一条变更操作来将整张表重建为某个时间点的镜像，或者发送表中的每一条变更来重新生成流。 要理解我们是如何捕获和发布数据变更的，就必须了解我们的数据库基础架构。在 Yelp，我们把绝大多数数据都保存在 MySQL 集群中。为了处理海量访问和管理读请求负载，Yelp 必须维护几十个在地理上按区域分布的读副本。 MySQL 数据复制原理： 要注意的是从库上 SQL 线程执行的事件并不一定是主库二进制文件中的最新事件，这个在上图中表现为复制延迟。Yelp 的 MySQLStreamer 就做为一个从库，把更新操作持久化到 Apache Kafka 中，而不是象普通的 MySQL 从库一样持久化到数据表中。 MySQLStreamer 则负责： 不断从 MySQL 二进制文件中查看最新的日志，读取这两种类型的事件； 根据事件类型不同而进行相应处理，将 DML 事件发布到 Kafka Topic 中； MySQLStreamer 会发布四种不同的事件类型：插入、更新、删除和刷新。前三种对应着相同类型的 DML 操作。刷新事件由我们的初始化 Topic 过程产生 ![](https://static001.infoq.cn/resource/image/d0/44/d05d23406ae3bc3db8bdf43484e05244.jpg)

*   项目地址:[https://github.com/Yelp/mysql_streamer](https://github.com/Yelp/mysql_streamer)

参考链接

*   [https://www.infoq.cn/article/yelp-real-time-data-pipeline-part02](https://www.infoq.cn/article/yelp-real-time-data-pipeline-part02)

4.Camus
=======

简介 linkin开发,目前被 gobblin 取代

*   项目地址:[https://github.com/linkedin/camus](https://github.com/linkedin/camus)

5.Gobblin
=========

简介 Gobblin是一个用于批处理和流系统的分布式大数据集成框架（导入，复制，合规性，保留）。 Gobblin是用来整合各种数据源的通用型ETL框架，在某种意义上，各种数据都可以在这里“一站式”的解决ETL整个过程，专为大数据采集而生，易于操作和监控，提供流式抽取支持 Gobblin具有与Apache Hadoop，Apache Kafka，Salesforce，S3，MySQL，Google等的集成。 ![](https://ask.qcloudimg.com/http-save/yehe-2725853/v74nkx004r.jpeg?imageView2/2/w/1620)

*   项目地址:[https://github.com/apache/incubator-gobblin](https://github.com/apache/incubator-gobblin)
*   官网地址:[https://gobblin.readthedocs.io/en/latest/](https://gobblin.readthedocs.io/en/latest/)

特点 底层支持三种部署方式，分别是standalone，mapreduce，mapreduce on yarn。可以方便快捷的与Hadoop进行集成，上层有运行时任务调度和状态管理层，可以与Oozie，Azkaban进行整合，同时也支持使用Quartz来调度（standalone模式默认使用Quartz进行调度）。对于失败的任务还拥有多种级别的重试机制，可以充分满足我们的需求。再上层呢就是由6大组件组成的执行单元了。这6大组件的设计也正是Gobblin高度可扩展的原因。

*   Source:主要负责将源数据整合到一系列workunits中，并指出对应的extractor是什么。这有点类似于Hadoop的InputFormat。
*   Extractor:则通过workunit指定数据源的信息，例如kafka，指出topic中每个partition的起始offset，用于本次抽取使用。Gobblin使用了watermark的概念，记录每次抽取的数据的起始位置信息。
*   Converter:顾名思义是转换器的意思，即对抽取的数据进行一些过滤、转换操作，例如将byte arrays 或者JSON格式的数据转换为需要输出的格式。转换操作也可以将一条数据映射成0条或多条数据（类似于flatmap操作）。
*   Quality Checker:即质量检测器，有2中类型的checker：record-level和task-level的策略。通过手动策略或可选的策略，将被check的数据输出到外部文件或者给出warning。
*   Writer:就是把导出的数据写出，但是这里并不是直接写出到output file，而是写到一个缓冲路径（ staging directory）中。当所有的数据被写完后，才写到输出路径以便被publisher发布。Sink的路径可以包括HDFS或者kafka或者S3中，而格式可以是Avro,Parquet,或者CSV格式。同时Writer也可是根据时间戳，将输出的文件输出到按照“小时”或者“天”命名的目录中。
*   Publisher:就是根据writer写出的路径，将数据输出到最终的路径。同时其提供2种提交机制：完全提交和部分提交；如果是完全提交，则需要等到task成功后才pub，如果是部分提交模式，则当task失败时，有部分在staging directory的数据已经被pub到输出路径了。

参考链接

*   [https://cloud.tencent.com/developer/article/1351988](https://cloud.tencent.com/developer/article/1351988)

6.Open Replicator
=================

Open Replicator是一个用Java编写的MySQL binlog分析程序。Open Replicator 首先连接到MySQL（就像一个普通的MySQL Slave一样），然后接收和分析binlog，最终将分析得出的binlog events以回调的方式通知应用。Open Replicator可以被应用到MySQL数据变化的实时推送，多Master到单Slave的数据同步等多种应用场景。 Open Replicator目前只支持MySQL5.0及以上版本。 简介

*   项目地址：[https://github.com/whitesock/open-replicator](https://github.com/whitesock/open-replicator)
*   项目主页：[https://code.google.com/archive/p/open-replicator/](https://code.google.com/archive/p/open-replicator/)

参考链接

*   [https://blog.csdn.net/u013256816/article/details/53072560](https://blog.csdn.net/u013256816/article/details/53072560)
*   [https://blog.csdn.net/menergy/article/details/17583823](https://blog.csdn.net/menergy/article/details/17583823)

７.Galera Cluster
================

简介 -　官网：[https://galeracluster.com/](https://galeracluster.com/) 何谓Galera Cluster？就是集成了Galera插件的MySQL集群，是一种新型的，数据不共享的，高度冗余的高可用方案，目前Galera Cluster有两个版本，分别是Percona Xtradb Cluster及MariaDB Cluster，都是基于Galera的，所以这里都统称为Galera Cluster了，因为Galera本身是具有多主特性的，所以Galera Cluster也就是multi-master的集群架构，如图1所示： ![](https://img-blog.csdn.net/20180729102825657?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dvMTgyMzcwOTU1Nzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70) 特性

*   真正的多主集群，Active-Active架构；
*   同步复制，没有复制延迟；
*   多线程复制；行级别并行复制
*   没有主从切换操作，无需使用虚IP；
*   热备份，单个节点故障期间不会影响数据库业务；
*   支持节点自动加入，无需手动拷贝数据；
*   支持InnoDB存储引擎；
*   对应用程序透明，原生MySQL接口；
*   无需做读写分离；
*   部署使用简单。

参考链接

*   [http://www.yunweipai.com/archives/19574.html](http://www.yunweipai.com/archives/19574.html)
*   [https://juejin.im/post/5d36a4f551882537dc64a734](https://juejin.im/post/5d36a4f551882537dc64a734)
*   [https://blog.csdn.net/qq_38125183/article/details/80861925](https://blog.csdn.net/qq_38125183/article/details/80861925)
*   [https://blog.csdn.net/wo18237095579/article/details/81270954](https://blog.csdn.net/wo18237095579/article/details/81270954)

8.Group Replication
===================

简介 MySQL Group Replication（简称MGR）是MySQL官方于2016年12月推出的一个全新的高可用与高扩展的解决方案。组复制是MySQL5.7版本出现的新特性，它提供了高可用、高扩展、高可靠的MySQL集群服务。MySQL组复制分单主模式和多主模式，mysql 的复制技术仅解决了数据同步的问题，如果 master 宕机，意味着数据库管理员需要介入，应用系统可能需要修改数据库连接地址或者重启才能实现。（这里也可以使用数据库中间件产品来避免应用系统数据库连接的问题，例如 mycat 和 atlas 等产品）。组复制在数据库层面上做到了，只要集群中大多数主机可用，则服务可用，也就是说3台服务器的集群，允许其中1台宕机。 MGR提供了single-primary和multi-primary两种模式。其中，single-primary mode(单写模式) 组内只有一个节点负责写入，读可以从任意一个节点读取，组内数据保持最终一致；multi-primary mode(多写模式)，即写会下发到组内所有节点，组内所有节点同时可读，也是能够保证组内数据最终一致性。尤其要注意：一个MGR的所有节点必须配置使用同一种模式，不可混用！

*   在单主模式下, 组复制具有自动选主功能，每次只有一个 server成员接受更新;
*   在多主模式下, 所有的 server 成员都可以同时接受更新;

![](https://images2017.cnblogs.com/blog/695151/201802/695151-20180209163955857-1422391339.png) 参考链接

*   [http://drmingdrmer.github.io/tech/mysql/2018/08/04/mysql-group-replication.html](http://drmingdrmer.github.io/tech/mysql/2018/08/04/mysql-group-replication.html)
*   [https://www.cnblogs.com/kevingrace/p/10260685.html](https://www.cnblogs.com/kevingrace/p/10260685.html)

对比
==

平台

开发厂商

语言

stars

active

Tungsten Replicator

Continuent

Java

Maxwell

zendesk

Java

1.8k

6 days ago

MySQLStreamer

Yelp

Python

332

3 years ago

Camus

linkdin

Java

837

4 years ago

Gobblin

linkdin

Java

1.6k

4 days ago

Open Replicator

person

Java

440

4 years ago

Group Replication

Galera Cluster

相关链接
====

*   [https://www.cnblogs.com/wuchangsoft/p/10390552.html](https://www.cnblogs.com/wuchangsoft/p/10390552.html)
*   [https://dbaplus.cn/news-141-2315-1.html](https://dbaplus.cn/news-141-2315-1.html)
