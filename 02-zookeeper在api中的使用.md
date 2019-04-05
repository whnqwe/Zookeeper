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

