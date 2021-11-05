# Redis高可用-集群机制

## 一、Redis集群

redis集群是redis提供的分布式数据库方案；

集群通过分片（Sharding）来进行数据共享，并提供复制和故障转移功能。

## 二、节点

一个节点就是一个运行在集群模式下的Redis服务器；

一个Redis集群通常由多个节点（node）组成；

连接各个节点使用cluster meet命令来完成：

```c
cluster meet <ip> <port>
```

使用cluster meet 命令可以让node节点与ip和port指定的节点进行握手，握手成功，node节点就会将指定节点添加至当前所在集群。

### 启动节点

redis服务器启动时候，会根据cluster-enabled配置选项是否为yes来决定是否开启服务器的集群模式；

运行在集群模式下的节点，会继续使用所有在单机模式中使用的服务器组件：

```
1. 节点会继续使用文件事件来处理命令请求和返回命令回复；
2. 节点会继续使用时间事件来执行serverCron函数，而serverCron函数又会调用集群模式特有的clusterCron函数；
3. 节点会继续使用数据库来保存键值对数据，键值对依然会是各种不同类型的对象；
4. 节点会继续使用RDB、AOF持久化模块来执行持久化工作；
5. 节点会继续使用发布与订阅模块来执行PUBLISH、SUBSCRIBE等命令；
6. 节点会继续使用复制模块来进行节点的复制功能；
7. 节点会继续使用Lua脚本环境来执行客户端输入的Lua脚本。
```

clusterCron函数：负责执行在集群模式下需要执行的常规操作，如向集群中的其他节点发送Gossip消息，检查节点是否下线，检查是否需要对下线的节点进行自动故障转移。

### 集群数据结构

##### clusterNode结构

每个节点都会使用clusterNode结构保存自身信息，如节点的创建时间、节点名字等；

clusterNode结构如下：

```c
typedef struct clusterNode {
    //节点创建时间
    mstime_t ctime; 
    //节点的名字，由40个十六进制字符组成
    char name[CLUSTER_NAMELEN]; 
    //节点标识
    //使用各种不同的标识值记录节点的角色（主节点、从节点）
    //以及节点目前所处的状态（在线、下线）
    int flags;    
    //节点当前配置纪元，用于实现故障转移
    uint64_t configEpoch; 
    //槽
    unsigned char slots[CLUSTER_SLOTS/8]; 
     ....
    //节点的IP地址
    char ip[NET_IP_STR_LEN];  
     ....
   //连接节点所需的有关信息 （IP、port）
    clusterLink *link; 
} clusterNode;
```

##### clusterLink结构

clusterNode结构的link属性是一个clusterLink结构，该结构保存了连接节点所需的有关信息，比如套接字描述符，输入缓冲区和输出缓冲区；

clusterLink结构如下：

```c
typedef struct clusterLink {
    //连接创建时间
    mstime_t ctime;   
    //连接
    connection *conn; 
   //输出缓冲区，保存着等待发送给其他节点的消息（message ）
    sds sndbuf; 
    //输入缓冲区，保存从其他节点接收到的消息
    char *rcvbuf; 
    //输入缓冲区大小（已使用容量）
    size_t rcvbuf_len; 
    //输入缓冲区最大容量
    size_t rcvbuf_alloc;
    //与这个连接相关联的节点，如果没有的话就为NULL
    struct clusterNode *node;
} clusterLink;
```





### cluster meet命令的实现