#### api使用

##### java api的使用

###### 导包

```xml
<dependency>

<groupId>org.apache.zookeeper</groupId>

<artifactId>zookeeper</artifactId>

<version>3.4.8</version>

</dependency>


```

###### 使用

```java
public class ZooKeeper {
      public ZooKeeper(String connectString, int sessionTimeout, Watcher watcher)
        throws IOException
    {
        this(connectString, sessionTimeout, watcher, false);
    }
}





```

```java
public interface Watcher {

  
    public interface Event {
        
        public enum KeeperState {
            @Deprecated
            Unknown (-1),

          
            Disconnected (0),

        
            @Deprecated
            NoSyncConnected (1),

            SyncConnected (3),
            
            AuthFailed (4),

   
            ConnectedReadOnly (5),

 
            SaslAuthenticated(6),


            Expired (-112);

            private final int intValue; 
                                           

            KeeperState(int intValue) {
                this.intValue = intValue;
            }

            public int getIntValue() {
                return intValue;
            }

            public static KeeperState fromInt(int intValue) {
                switch(intValue) {
                    case   -1: return KeeperState.Unknown;
                    case    0: return KeeperState.Disconnected;
                    case    1: return KeeperState.NoSyncConnected;
                    case    3: return KeeperState.SyncConnected;
                    case    4: return KeeperState.AuthFailed;
                    case    5: return KeeperState.ConnectedReadOnly;
                    case    6: return KeeperState.SaslAuthenticated;
                    case -112: return KeeperState.Expired;

                    default:
                        throw new RuntimeException("Invalid integer value for conversion to KeeperState");
                }
            }
        }

        public enum EventType {
            None (-1),
            NodeCreated (1),
            NodeDeleted (2),
            NodeDataChanged (3),
            NodeChildrenChanged (4);

            private final int intValue;    
                                            

            EventType(int intValue) {
                this.intValue = intValue;
            }

            public int getIntValue() {
                return intValue;
            }

            public static EventType fromInt(int intValue) {
                switch(intValue) {
                    case -1: return EventType.None;
                    case  1: return EventType.NodeCreated;
                    case  2: return EventType.NodeDeleted;
                    case  3: return EventType.NodeDataChanged;
                    case  4: return EventType.NodeChildrenChanged;

                    default:
                        throw new RuntimeException("Invalid integer value for conversion to EventType");
                }
            }           
        }
    }

    abstract public void process(WatchedEvent event);
}
```



```java

  public String create(final String path, byte data[], List<ACL> acl,
            CreateMode createMode)
      
  public void delete(final String path, int version)
      throws InterruptedException, KeeperException
      
  public Stat setData(final String path, byte data[], int version)
      throws KeeperException, InterruptedException
        
    public byte[] getData(final String path, Watcher watcher, Stat stat)
        throws KeeperException, InterruptedException
```



###### ACL权限模型

权限的控制模型

scheme  : 授权对象

ip		ip地址

digest	username:password

world          开放式的权限控制模式，数据节点的访问权限对所有用户开放，world:anyone

super         超级用户，可以对zk上的数据节点进行操作



###### 连接状态

KeeperStat.Expired  在一定时间内客户端没有收到服务器的通知， 则认为当前的会话已经过期了。

KeeperStat.Disconnected  断开连接的状态

KeeperStat.SyncConnected  客户端和服务器端在某一个节点上建立连接，并且完成一次version、zxid同步

KeeperStat.authFailed  授权失败



###### 事件类型

NodeCreated  当节点被创建的时候，触发

NodeChildrenChanged  表示子节点被创建、被删除、子节点数据发生变化

NodeDataChanged    节点数据发生变化

NodeDeleted        节点被删除

None   客户端和服务器端连接状态发生变化的时候，事件类型就是None





##### zkclient

###### 导包

```xml
 <dependency>
    <groupId>com.101tec</groupId>
    <artifactId>zkclient</artifactId>
    <version>0.11</version>
</dependency>

```

###### 使用

- 基本操作

- 递归创建

- 递归删除

  

###### 事件订阅

> 





##### curator

###### 优点

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

###### 使用

基本操作

异步操作

###### 事务操作

###### 事件操作

事件类型

- pathcache  监视一个路径下子节点的创建、删除、节点数据更新
- nodecache 监视一个节点的创建、更新、删除
- treecache   pathcache +nodecache  的合体，路径下的创建、更新、删除，并且缓存路径下子节点的数据

###### curator连接的重试策略

> ExponentialBackoffRetry()衰减重试

> RetryNTimes 指定最大重试次数

> RetryOneTime 仅重试一次

> RetryUnitilElapsed一直重试知道规定的时间





