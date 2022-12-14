# Redis实战-分布式缓存

## 缓存数据库双写不一致问题

##### 先删缓存再更数据库的问题

1. 线程2的查询数据库及更新缓存操作，在线程1删掉缓存与更新数据库操作之间发生。 线程2还是缓存的旧数据；

   导致：缓存不一致时间：缓存过期，或者下次删除缓存；

##### Cache Aside Pattern存在的问题

1. 线程2查询数据库操作，在线程1更新数据库及删除缓存之前，但是线程2更新缓存的操作，在线程1更新数据库及删除缓存之后。

​         导致：缓存不一致时间：缓存过期，或者下次删除缓存；

解决办法：

- ###### 强一致性

  加锁（分布式锁）：有性能问题，降低并发量，影响系统吞吐量，高并发下不适用。

  加锁优化：针对于读多写少场景，使用分布式读写锁，redisson.getReadWriteLock()方法，对key设置一个mode属性实现读写锁；

- ###### 最终一致性

  “Cache Aside Pattern + 延迟双删” 无锁方案：只能保证在高并发前提下尽可能减少不一致的可能，也是AP模式下BASE的最终一致性体现。



###### 总结：

要保证缓存与数据库强一致，最好的办法就是使用分布式锁，但无法保证并发量；

“Cache Aside Pattern + 延迟双删” 无锁方案只能保证在高并发前提下尽可能减少不一致的可能，也是AP模式下BASE的一种体现。

对于读多写多场景没必要使用缓存；

## redis读写锁实现