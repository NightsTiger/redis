
## rdb

##### **一、手动触发(save、bgsave)**

###### 1.save

- 会造成阻塞

- Redis 不能执行其他命令

###### 2.bgsave

- fork一个子进程，后台异步执行快照操作
- fork期间发生阻塞

##### 二、自动触发 (redis.conf 配置save规则)

​		Redis 服务器周期操作函数 `serverCron`默认每个 100 毫秒就会执行一次，检查配置的save规则是否有一项被满足。被满足，执行bgsave

> 手动触发和自动触发流程图




## aof




> 1.所有写命令追加到到aof缓冲区
>
> 2.aof缓冲区根据对应的策略将数据刷新到aof文件
>
> 3.aof文件大 --> 触发重写
>
> 4.重启，创建伪客户端，从aof文件文件分析并执行命令，写入到redis。



##### 一、命令写入

​		appendOnlyFile = yes。aof 开启后，每次写命令追加到aof缓冲区末尾。

##### 二、文件的写入和同步

- 每次结束一个事件循环，都会调用flushAppendOnlyFile()函数，判断是否需要将aof缓冲区数据同步到aof文件。
- flushAppendOnlyFile()函数根据redis.conf配置文件中 appendfsync 配置来决定是否要同步，appendfsync有三个可选值
  - no
  - everysec
  - always
- 延迟写 fsync

##### 三、aof文件恢复

​		fake client 

##### 四、aof重写

- 从数据库中读取现有的键值，用一条命令记录下来，代替之前多条记录。

##### 五、aof后台重写

1. 命令写到aof重写缓冲区和aof缓冲区
2. aof重写缓冲区开子进程
3. 当子进程完成aof重写工作后，向父进程发送一个信号
4. 父进程接收到信号后，aof缓冲区内所有内容写入到新aof文件中。
5. 新aof文件原子覆盖现有aof文件
6. 继续处理客户端请求



​		 























1.rdb参考:https://mp.weixin.qq.com/s/NpUV-7bvXTD3iu0_2aRssQ?from=timeline

2.aof参考:https://juejin.im/post/5d405370e51d4561fa2ebfe8

