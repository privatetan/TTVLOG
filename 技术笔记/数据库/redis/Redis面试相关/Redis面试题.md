# Redis面试题

## 一、Redis单线程，为什么会这么快？

- 内存存储：Redis是使用内存(in-memeroy)存储,没有磁盘IO上的开销；

- 单线程实现：Redis使用单个线程处理请求，避免了多个线程之间线程切换和锁资源争用的开销；

- **非阻塞IO**：Redis使用多路复用IO技术，在poll，epool，kqueue选择最优IO实现；
- 优化的数据结构：Redis有诸多可以直接应用的优化数据结构的实现，应用层可以直接使用原生的数据结构提升性能；

## 二、内存耗尽redis会发生什么？

### 内存回收

Redis的每种数据对象都是由对象结构redisObject和对应编码的数据结构组成；

- ###### refcount属性：

  redisObject中的refcount属性，是对象的引用计数，计数0那么就是可以回收；

- ###### lru属性：

  redisObject的lru属性：记录了对象最后一次被命令程序访问的时间；

  **空转时长**：当前时间减去键的值对象的lru时间，就是该键的空转时长；

  maxmemory选项：服务器用于回收内存的算法为volatile-lru或者allkeys-lru，那么当服务器占用的内存数超过了maxmemory选项所设置的上限值时，空转时长较高的那部分键会优先被服务器释放，从而**回收内存**。

### 设置过期时间

- expire key ttl：将 `key` 值的过期时间设置为 `ttl` **秒**。
- pexpire key ttl：将 `key` 值的过期时间设置为 `ttl` **毫秒**。
- expireat key timestamp：将 `key` 值的过期时间设置为指定的 `timestamp` **秒数**。
- pexpireat key timestamp：将 `key` 值的过期时间设置为指定的 `timestamp` **毫秒数**。

ps：1. 过期时间底层使用 pexpireat 命令实现；

​         2.移除过期时间：persist <key>命令；        

### 过期策略

将一个过期的键删除，我们一般都会有三种策略：

- ###### 定时删除

  为每个键设置一个定时器，一旦过期时间到了，则将键删除；

  这种策略对内存很友好，但是对 `CPU` 不友好，因为每个定时器都会占用一定的 `CPU` 资源。

- ###### 惰性删除

  不管键有没有过期都不主动删除，等到每次去获取键时再判断是否过期，如果过期就删除该键，否则返回键对应的值；

  这种策略对内存不够友好，可能会浪费很多内存。

- ###### 定期扫描

  系统每隔一段时间就定期扫描一次，发现过期的键就进行删除。

  这种策略相对来说是上面两种策略的折中方案，需要注意的是这个定期的频率要结合实际情况掌控好；

  定期扫描只会扫描设置了过期时间的键，因为设置了过期时间的键 `Redis` 会单独存储，所以不会出现扫描所有键的情况；

  使用这种方案有一个缺陷就是可能会出现已经过期的键也被返回。

ps：在 `Redis` 当中，其选择的是策略 `2` 和策略 `3` 的综合使用。

### 8种淘汰策略



LRU算法



LFU算法

## 三、Redis运用场景