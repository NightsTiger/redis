# redis
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

https://mmbiz.qpic.cn/mmbiz_png/9QbuglRCMtdCsru2H6LaV4RK8WtK406pichH7B190wnBbRKGTMhYQO4lZ54NcObOW1PpNYtH9icI9cQicBI1VWggA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1



## aof


