# TiDB数据库

## TiDB-功能与特点

具有同时支持OLTP和OLAP业务的能力。

能够同时存储一份数据的行存版本和列存版本，并保证一致性读取。

TiDB数据可需要<mark>通过数据复制的方式（搭建从库）</mark>才能保证数据的高可用性。

<mark>只有在云原生模式下</mark>，TiDB数据库能够实现水平扩容或者缩容。

## TiDB 体系架构

水平扩容或者缩容

金融级高可用

实时HTAP

云原生的分布式数据库

兼容MySQL 5.7协议

## TiDB Server

### TiDB Server-模块

Executor 模块负责执行SQL执行计划。

PD Client 模块<mark>只是负责</mark>  TiDB Server 向PD请求 TSO，并接受返回的 TSO。

TiKV Client 模块<mark>只负责</mark>执行 Coprocessor request，比如，过滤，聚合或列投影等等，<mark>点查操作由KV模块直接发给TiKV</mark>。

DistSQL  模块负责将包含  JOIN,  SUBQUERY 等复杂算子和涉及很多的表的SQL 抽象为只涉及到单个表数据的多个操作，并将这些操作<mark>直接发给 TiKV</mark>。

### TiDB Server-功能

TiDB Server 在开始执行 SQL 语句时，会从PD 节点获取当前的 TSO。

TiDB Server 负责SQL 的解析和编译，而<mark>PD 负责</mark>在关系型数据和KV 存储间相互转换。

TiKV 的元数据，在数据库启动后会<mark>全部载入到TiDB Server 的缓存</mark>中，加快查询效率。

TiDB Server 的缓存中除了有表的元数据外<mark>还会储存Online DDL 的 job队列</mark>。

### TiDB Server-SQL 编译

物理优化发生在编译（Compile）操作中。

逻辑优化发生在编译（Compile）操作中。

<mark>编译（Compile）</mark>操作的产物是AST 语法树。

SQL 语句的TSO 获取一般在解析和编译操作<mark>之前完成</mark>。

### TiDB Server -将关系型数据（含有主键，并希望key中含有主键）转化为key-value 数据

将主键（Primary Key）单独分离出来，与表的 Table ID 拼接，作为key序列

主键（Primary Key）对应的单行数据作为 value ，形成 key-value 对

将这些 key-value 存储在一个 region 中

如果 region 大小超过 96MB，则分裂为2个

### TiDB Server-GC 机制

GC 会被自动触发 ，默认是10分钟一次。

每次GC 操作过后，修改时间在 safe point 之前的旧版本数据快照可能会被删除掉，之后的不会。

如果有多个 TiDB Server ，则这些 TiDB Server 可以<mark>并行</mark>来执行 GC操作。

GC life time 是<mark>无法手工调整</mark>的， 由系统根据数据库压力情况自动调整。

### TiDB Server -缓存

复杂的SQL 的查询中间结果，往往会存储在 TiDB Server 的缓存中。

通过 tidb-men-quota-query 参数能够控制每个查询的缓存使用量。

oom-action 参数用来设置 SQL 语句超过内存使用阈值后的行为。

information schema 和表的统计信息都在存储在 <mark>TiDB Server 的持久化存储</mark>中，启动后载入 TiDB Server 的缓存中。

## TiKV

### TiKV 构架和作用

数据持久化

分布式一致性

MVCC

分布式事务

Coprocessor

### TiKV -功能和特点

执行被下推的类似数据过滤、部分聚合等SQL计算，目的是减少 TiDB Server 与 TiKV 的网络交互成本和 TiDB Server 的CPU 和缓存负载。

存储锁信息到RockDB 实例中。

TiKV 中的 RocksDB 实例负责存储 key-value 数据。

一部分过滤、聚合和投影等SQL 计算工作。

将锁信息和事务的提交信息进行存储。

<mark>store ID</mark> ，region ID ，table ID 和 index ID 等全局 ID 和分布式事务的事务 ID 生成。>> PD

<mark>Region 各个副本所在 TiKV （store） 路由信息的存储。</mark>  >>PD

<mark>将 region 的元信息进行存储。</mark> >>PD

### TiKV -RocksDB

持久化机制，同时保证性能和安全性。

性能随CPU 数量线性提升。

RocksDB 使用 LSM 存储引擎。

支持 key-value、<mark>json、xml </mark>和图数据的存储。

### TiKV-RocksDB 读写

写入操作（增删改）的数据最开始都是保存在内存中。

immutable MemTable 不支持继续写入。

Level-0 层的SST 文件是 immutable MemTable 的转储。

Column Families 共享 WAL 文件。

WAL 的作用是为了<mark>加速磁盘的写入速度</mark>，将随机写变为顺序写。

读取时，内存中只读 Block Cache，<mark>不会去读 memtable</mark>，目的是不会影响写的效率。

读取时，如果内存中没有找到的key <mark>一定不会</mark>出现在level0 的SST 文件中。

Column Families <mark>共享 SST 文件</mark>。

### TiKV -Raft 与 Multi Raft

raft log 利用 region ID 和日志的顺序 ID 来进行唯一标识。

candidate 角色的TiKV 节点在达到某种条件后，会自动将状态变为 leader。

在raft 中，同一时间 （一个 term 内），可以有<mark>多个 leader </mark>在集群中。

同一个 region 的多个副本<mark>可以</mark>在一个TiKV节点上。

### TiKV -MVCC

在TiDB 数据库中，一旦数据被修改并且事务被提交，新的读取（select 操作并且tidb_snapshot=“”）则无法读取到修改之前的任何版本。

MVCC 机制<mark>只存在于</mark>支持分布式事务的数据库中。

<mark>只有</mark>当隔离级别为 snapshot isolation 或者 repeatable read 时候，MVCC才会生效。

TiDB 数据库中存储了数据从写入至当前的<mark>所有版本</mark>。

### TiKV -分布式事务

Default cf 和 Write cf 中都有可能储存写入的数据。

TiDB 数据库中分布式事务的实现参考了 Google 的Percolator。

分布式事务一般需要拿到2个 TSO。

Percolator 的 commit 阶段需要将<mark>所有行的锁删除掉</mark>，才算成功。

### TiKV- 读写

单条 select 语句读取到的任何数据都是已经从 raft 日志中 apply 到 RocksDB KV中的。

最开始从 PD 中读取到的路由信息中的 leader 所在 TiKV 节点，不一定是最终读取数据的节点。

follower read <mark>不可能</mark>比 leader read 先读到数据。

当集群中<mark>所有的 follower 节点</mark>都收到了这个日志（raft log），我们才认为这个（raft log）是commited。

## PD

### PD -功能和特点

SQL 向 PD 请求 TSO 后，并不会被阻塞，而是继续进行解析和编译，所以是异步获取。

TSO 分配中，时间窗口的一个目的是减少 PD 的IO 负载。

PD 会以一定的时间间隔<mark>向所有的 region 发送心跳</mark>，并拉取 region 当前的最新信息。

全局事务ID ， store ID ，regio ID 是由 PD 分配的，<mark>Table ID和 Index ID 是由执行 SQL 的</mark>  <mark>TiDB Server 根据表的元数据信息生成的</mark>。

### PD-TSO

TSO 的组成： 物理时钟 + 逻辑时钟。

TSO 的申请者是通过 PD Client 模块来向 PD 申请TSO 的。

SQL 语句的解析和编译可以与 TSO的获取异步执行。

PD 发出TSO 只会递增不会递减。

当PD Client 模块过于繁忙时， PD会<mark>直接将TSO 返回给申请者</mark>。

### PD -label

label 的配置需要在 PD 和TiKV 上进行操作。

如果不使用 label ，PD 仅仅保证 region 的多个副本不会存储在同一个 TiKV 实例上。

Label 的作用之一是控制 region 副本的存放位置，比如 host ，rack 或者 DC。

Zone <mark>必须对应</mark>一个数据中心（DC），不能对应一个机柜。

### PD -调度

可以将 Region 均匀地分散在集群中的所有 Store 上。

对于写热点，热点调度会同时尝试打散点 Region 的 Peer 和 Leader。

<mark>目前无法</mark>通过调度把相邻的小 Region 合并，需要手动完成。

<mark>目前无法</mark>将待下线节点上的 Region 迁移至其他节点，需要手动完成。

信息收集包括：副本分布、节点数据量、节点的剩余空间；不包括：<mark>TiDB Server 的并发度</mark>

## 数据写入-必须步骤

raft log 被复制到 follower。

raft log 被持久化。

scheduler 模块利用 latch 来进行协调写请求。

将已经写入成功的数据所对应的 raft log 存储到<mark> binlog 日志</mark>中。

## online DDL -必须步骤

TiDB Server 进行解析（Parse）和编译（Compile），生成执行计划。

特定的 worker 进程会从 job queue 中取出第一个未执行的 job 去执行。

start job 模块将 DDL 操作封装。

<mark>将job 队列中的 DDL 语句对应的表全部锁住为只读状态</mark>。

## Online DDL

Online DDL 的job 队列被持久化在 TiKV 中， 不是TiDB Server 中。

Online DDL 操作指的是 DDL 操作并不影响线上业务，对于<mark>性能监控也是无感知的</mark>。

如果有多个 TiDB Server ，所有 TiDB Server 中的 worker 模块会<mark>并行处理</mark>添加索引的操作，加快进度。

接收到 DDL SQL 的 TiDB Server ，会<mark>先启动自己的 worker 模块处理DDL </mark>， 之后根据负载情况可能转移给owner 角色的 TiDB Server 中的worker 模块处理。

## 满足 HTAP 的场景

同时支持 OLTP 和 OLAP 两种业务。

承担着实时数据写入的行存，并且能够实时将数据变化同步到列储存。

能够支持PB 级<mark>数据分析</mark>。

能够支持<mark>高并发</mark>的交易场景，且保证<mark>强一致性</mark>。

## HTAP 的特性

TiFlash 实现了 TiKV 的列存特性。

CSO 优化器可以智能选择查询在 TiKV 还是在 TiFlash 上执行。

MPP 架构实现了在TiFlash 上的聚合和连接操作的运算加速，补充了TiDB 的分析运算能力。

<mark>可以将聚合算子下推到存储引擎中完成。</mark>

## TiDB 数据库的MMP 功能特性

聚合查询也可以通过 MMP 功能提速。

TiFlash 节点的内存承担了 MPP 中的计算功能。

MPP 功能对于大表连接有提高效率的作用。

<mark>将行存数据转为列存。</mark>

## HTAP 典型场景

承载OLTP 在线业务（由 TiKV 完成），同时 TiFlash 列存引擎对实时分析类业务进行分流和加速。

分析库需要具有实时性，能够同步在线业务数据的变化。

无法接受ETL 同步数据的时间。

<mark>支持高并发访问的同时可以进行横向扩展，提高处理能力。</mark>

## TiFlash

### TiFlash 架构特点

TiDB v6.1 版本中，TiFlash 目前不支持数据的写入。

无论是不是分区装，同一个 region 在TiKV 中的副本和TiFlash 中的副本数据都是相同的。

TiFlash 中的 region 副本不参与 raft group的投票。

写入数据时，<mark>必须保证</mark>在TiFlash 中的 region 副本有成功写入节点，才算写入成功，以确保强一致性。

### TiFlash 功能特点

只要 TiKV 中数据不丢失，就可以随时恢复 TiFlash 的副本。

用户可以手动选择 SQL 在TiFlash 节点上执行。

在同一查询内混合使用 TiKV 和 TiFlash 加快查询速度。

TiFlash 中的 region 是 TiKV 的列存版本，但是其上的读取<mark>不支持</mark>MVCC。

### TiFlash -功能

TiFlash 节点不支持写数据操作，所以无法帮助 TiKV 节点分担写压力。

TiKV 节点宕机以后，TiFlash 节点将<mark>无法读取数据</mark>。

同一条 SQL 语句，经过智能选择，<mark>不可能同时</mark>从TiKV 和TiFlash 中读取数据。

由于 TiKV 节点的 Coprocessor 功能，所以<mark>只有</mark> TiKV 节点具有数据计算功能。

## Placement Rules in SQL

### Placement Rules in SQL 使用的必须步骤

区域、机柜和服务器配置标签。

用 create table 或者 alter table 来为指定 PLACEMENT POLICY。

设置 PLACEMENT POLICY。

<mark>在PLACEMENT POLICY 中指定副本数</mark>。

### Placement Rules in SQL 之前

跨地域部署的集群，无法本地访问。

无法根据业务隔离资源。

难以按照业务等级配置资源和副本数。

### Placement Rules in SQL 之后

跨地域部署的集群，支持本地访问。

根据业务隔离资源。

按照业务等级配置资源和副本数。

### 可以通过Placement Rules in SQL 功能进行解决的

把热点数据的 leader 放在高性能的TiKV 实例上。

某些数据表比较重要，希望增加副本数。

查询较少的历史数据希望只占用低性能节点分布。

<mark>将读取热点数据缓存在 TiDB Server 的缓存中</mark>。

## 热点小表缓存

表的数据量不大( <64MB)。

只读表或者修改不频繁的表。

表的访问很频繁。

### 热点小表缓存- 原理

租约到期，数据过期。

写操作不再被阻塞。

读写直接到TiKV 节点上执行。

数据更新完毕，租约继续开启。

## 缓存表功能描述

往缓存表写入数据时，有可能出现写入延迟。

当对缓存表中的表做<mark> DDL 操作</mark>时，写被阻塞， 读可以继续进行。

如果一张表基本不会修改，我们可以<mark>减少</mark> tidb_table_cache_lease参数，保持读取缓存表性能的稳定和高效。

如果一张表大小为<mark>512 MB </mark>，那么必须将其变为分区表，且每个分区保证不超过 64 MB，才可以使用缓存表。

## 关于悲观锁、乐观锁和内存悲观锁的说法

内存悲观锁无需将锁信息复制到其他 follower 角色的副本上。

内存悲观锁依然会将锁信息写入 TiKV 节点但只写入到内存中。

悲观锁和乐观锁<mark>都不需要</mark>在 commit 阶段将锁信息写入到TiKV 的持久化存储中。

内存悲观锁实现了在事务未提交时就让其他事务能感知到其锁信息，乐观锁和<mark>悲观锁</mark>则不能。

## Top SQL之后

指定 TiDB 及 TiKV 实例。

正在执行的 SQL 语句。

CPU 开销最多的TOP 5类SQL。

每秒请求数、平均延迟等信息。

## TopSQL 的应用

当某个 TiKV 实例的CPU 使用率过高，可以监控到其上 CPU负载最高的5类SQL语句。

监控某个 TiDB Server 正在执行的5类 CPU 负载最高的SQL 语句。

监控到某个 TiKV 上 CPU 使用率最高的5类SQL 语句。

监控到某个 TiDB Server 上<mark>消耗内存</mark>最高的5类SQL 语句。

## TiDB Enterprise Manager （TiEM）的功能

集群升级。

集群部署。

参数管理。

<mark>监控 Top 5 SQL ，并给出优化建议</mark>。

## TiDB Cloud

### TiDB Cloud 相关知识点

TiDB Cloud 可以定义为提供基于 TiDB 数据库的 DBaaS。

TiDB Cloud 中的TiDB 集群包含 TiDB Server、TiKV 、不包含PD。

在TiDB Cloud 中，数据库中的数据所有者是用户本身。（不是公有云提供商，更不是公开的）。

TiDB Cloud 目前分为Developer Tier 和 Dedicated Tier ，Developer Tier 包含 TiFlash。

### TiDB Cloud 备份

支持自动备份和手动备份。

已经删除的集群数据，可以从回收站（Recycle Bin）中恢复。

正在运行的手动备份任务可以删除。

启动自动备份后，<mark>手动备份无效</mark>。


