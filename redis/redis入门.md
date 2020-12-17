`redis `的 作用 

1. 当一个网页或`APP` 的用户量增大，请求数量也随之增大，数据库的压力过大。严重会导致数据库崩溃。

2. 多台服务器之前，数据不同步.

3. 多台服务器中的锁，已经不存在互斥性。

   ![image-20201123221124299](G:\笔记\redis\图片\image-20201123221124299.png)

`NOSQL ` 非关系型数据库 `not only SQL` 

1. key - value  `redis` 
2. 文档性 
3. 面向列
4. 图形化

`Redis `

```java
// 意大利人，在开发一个统计界面，发现MYsql 的性能不好，所以开发出了一款非关系性数据库，Redis
// Redis （Remote Dictionary Server） 远程字典服务
// Redis 是基于内存存储的，Redis 还提供了多种持久化的机制防止数据丢失
// Redis 的性能非常快
```

安装`Redis` 

```yml
version: '3.1'
services:
  redis:
     image: daocloud.io/library/redis:5.0.7
     restart: always
     container_name: redis
     environment:
        -TZ: Asia/Shanghai
     ports:
        -6379: 6379
```

