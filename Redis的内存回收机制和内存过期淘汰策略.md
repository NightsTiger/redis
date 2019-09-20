# Redis的内存回收机制和内存过期淘汰策略

过期策略和淘汰策略不是同一个概念。

## 过期策略

> 删除过期时间的key值

##### **一、定时删除**

​		每个key创建一个定时器，到过期时间立即删除。耗cpu，redis默认不使用

##### 二、惰性删除

​		每次访问的时候，判断该key是不是已过期，已过期则删除，可能有些没有被访问的数据占用大量内存。	

##### 三、定期删除

​		定期扫描expires字典中一定数量的key，删除其中已经过期的，如果删除的key占这批的25%以上，则重复这个步骤。



## 淘汰策略

> 内存使用到达maxmemory上限时触发内存淘汰数据

- lru

  - 最近最少使用
  - LinkedHashMap
  - maxmeory-samples

- 策略(maxmemory-policy)

  内存达到最大内存后，采取什么策略来回收。

  - noeviction 报错
  - allKeys-lru 最近最少使用
  - allKeys-random 随机
  - **volatile-lru** 在带有expire的key中，移除最近最少使用的key
  - **volatile-random** 在带有expire的key中，随机移除一个key
  - **volatile-ttl** 在带有expire的key中，移除更早的key



参考：https://juejin.im/post/5c966300e51d451bf425fbe2
