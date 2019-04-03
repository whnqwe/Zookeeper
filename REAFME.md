# zookeeper

#### 初步认识zookeeper

##### zookeeper是什么

> 分布式数据一致性的解决方案

##### zookeeper的特性

###### 顺序一致性

> 从同一个客户端发起的事务请求，最终会严格按照顺序被应用到zookeeper中

###### 原子性

> 所有的事务请求的处理结果在整个集群中的所有机器上的应用情况是一致的，也就是说，要么整个集群中的所有机器都成功应用了某一事务、要么全都不应用

###### 可靠性

> 一旦服务器成功应用了某一个事务数据，并且对客户端做了响应，那么这个数据在整个集群中一定是同步并且保留下来的

###### 实时性

> 一旦一个事务被成功应用，客户端就能够立即从服务器端读取到事务变更后的最新数据状态；（zookeeper仅仅保证在一定时间内，近实时）

#### zookeeper安装

##### 单机环境安装

1. 下载zookeeper的安装包

>  http://apache.fayea.com/zookeeper/stable/zookeeper-3.4.10.tar.gz

2. 解压zookeeper

> tar -zxvf zookeeper-3.4.10.tar.gz

3. cd到 ZK_HOME/conf  , copy一份zoo.cfg

> cp  zoo_sample.cfg  zoo.cfg

4. sh zkServer.sh

> {start|start-foreground|stop|restart|status|upgrade|print-cmd}

5. sh zkCli.sh -server  ip:port

##### 集群环境

###### 集群环境的角色

zookeeper集群, 包含三种角色: leader / follower /**observer**

 observer

observer 是一种特殊的zookeeper节点。可以帮助解决zookeeper的扩展性

> 如果大量客户端访问我们zookeeper集群，需要增加zookeeper集群机器数量。从而增加zookeeper集群的性能。 导致zookeeper写性能下降， zookeeper的数据变更需要半数以上服务器投票通过。造成网络消耗增加投票成本

- observer不参与投票。 只接收投票结果。

- 不属于zookeeper的关键部位。

###### 配置步骤

第一步： 修改配置文件

server.id=host:port1:port2

id:id的取值范围： 1~255； 用id来标识该机器在集群中的机器序号

port1:表示follower节点与leader节点交换信息的端口号

port2:如果leader节点挂掉了,需要一个端口来重新选举。

 ```properties
server.1=192.168.11.129:2888:3181

server.2=192.168.11.131:2888:3181

server.3=192.168.11.135:2888:3181

 ```

第二步：创建myid

在每一个服务器的dataDir目录下创建一个myid的文件，文件就一行数据，数据内容是每台机器对应的server ID的数字

第三步：启动zookeeper

###### 配置observer

如果需要增加observer节点

zoo.cfg中 增加 ;peerType=observer

server.1=192.168.11.129:2888:3181

server.2=192.168.11.135:2888:3181   

server.3=192.168.111.136:2888:3181:observer



#### zoo.cfg配置文件分析

###### tickTime=2000  

>  zookeeper中最小的时间单位长度 （ms）

###### initLimit=10

> follower节点启动后与leader节点完成数据同步的时间

######  syncLimit=5

> leader节点和follower节点进行心跳检测的最大延时时间

######  dataDir=/tmp/zookeeper

表示zookeeper服务器存储快照文件的目录

######  dataLogDir

表示配置 zookeeper事务日志的存储路径，默认指定在dataDir目录下

######  clientPort

表示客户端和服务端建立连接的端口号： 2181



#### 节点类型

##### 持久化节点  

> 节点创建后会一直存在zookeeper服务器上，直到主动删除

##### 持久化有序节点

> 每个节点都会为它的一级子节点维护一个顺序

##### 临时节点

> 临时节点的生命周期和客户端的**会话**保持一致。当客户端会话失效，该节点自动清理

##### 临时有序节点

> 在临时节点上多勒一个顺序性特性

#### zookeeper中的一些概念

##### 数据模型

zookeeper的数据模型和文件系统类似，每一个节点称为：znode.  是zookeeper中的最小数据单元。每一个znode上都可以保存数据和挂载子节点。 从而构成一个层次化的属性结构

##### 会话

###### 会话状态

- 未连接
- 连接中
- 已连接
- 关闭

##### Watcher

zookeeper提供了分布式数据发布/订阅,zookeeper允许客户端向服务器注册一个watcher监听。当服务器端的节点触发指定事件的时候

**会触发watcher。服务端会向客户端发送一个事件通知 watcher的通知是一次性，一旦触发一次通知后，该watcher就失效**

##### ACL

zookeeper提供控制节点访问权限的功能，用于有效的保证zookeeper中数据的安全性。避免误操作而导致系统出现重大事故。CREATE /READ/WRITE/DELETE/ADMIN

#### zookeeper的命令操作

##### create

create [-s] [-e] path data acl

> -s表示节点是否有序

> -e表示是否为临时节点

> 默认情况下，是持久化节点

##### get 

get path [watch]

> 获得指定 path的信息

##### set 

set path data [version]

修改节点 path对应的data

乐观锁的概念

数据库里面有一个 version字段去控制数据行的版本号

##### delete 

delete path [version]

删除节点

#### stat信息

cZxid = 0x500000015

> 节点被创建时的事务ID

pZxid = 0x500000015

> 节点最后一次被更新的事务ID

mZxid = 0x500000016

> 当前节点下的子节点最后一次被修改时的事务ID

ctime = Sat Aug 05 20:48:26 CST 2017

mtime = Sat Aug 05 20:48:50 CST 2017

cversion = 0

> 子节点的版本号

dataVersion = 1

> 表示的是当前节点数据的版本号

aclVersion = 0

> 表示acl的版本号，修改节点权限

ephemeralOwner = 0x0

> 创建临时节点的时候，会有一个sessionId 。 该值存储的就是这个sessionid

dataLength = 3

> 数据值长度

numChildren = 0

> 子节点数

#### spi使用

##### java api的使用

###### 导包

```xml
<dependency>

<groupId>org.apache.zookeeper</groupId>

<artifactId>zookeeper</artifactId>

<version>3.4.8</version>

</dependency>

```

##### zkclient

###### 导包

```xml
 <dependency>
    <groupId>com.101tec</groupId>
    <artifactId>zkclient</artifactId>
    <version>0.11</version>
</dependency>
```

##### curator

> Curator本身是Netflix公司开源的zookeeper客户端；
>
> curator提供了各种应用场景的实现封装
>
> curator-framework 提供了fluent风格api
>
> curator-replice 提供了实现封装

###### 导包

```xml
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>4.2.0</version>
</dependency>
```

```xml
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>4.2.0</version>
</dependency>
```

###### curator连接的重试策略

> ExponentialBackoffRetry()衰减重试

> RetryNTimes 指定最大重试次数

> RetryOneTime 仅重试一次

> RetryUnitilElapsed一直重试知道规定的时间



##### zookeeper能做什么

###### 数据的发布/订阅

> 统一配置管理（disconf）

###### 负载均衡

> dubbo利用了zookeeper机制实现负载均衡

###### 命名服务

###### master选举

> kafka

###### 分布式队列

###### 分布式锁

redis

zookeeper

数据库

###### ID生成器

##### 