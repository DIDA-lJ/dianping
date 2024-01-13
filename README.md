# Dianping
一款基于 Java 开发的店铺点评 APP，实现了找店铺 =>写点评 => 看热评 => 点赞关注=>关注 Feed 流的完整业务流程

## 实现功能
<ul>
    <li>店铺查询</li>
    <li>短信登录</li>
    <li>优惠券秒杀</li>
    <li>发布探店笔记</li>
    <li>查看热点评论</li>
    <li>点赞关注实现</li>
    <li>粉丝信息推送(推 Feed 流实现)</li>
    <li>粉丝信息查询(滚动分页优化)</li>
    <li>附近商户查询</li>
    <li>签到功能实现</li>
    <li>百万数据UV统计</li>
</ul>

## 项目架构流程图
![image](https://github.com/DIDA-lJ/dianping/assets/97254796/555f583c-5eed-4b4c-8d24-a663cda173f6)

## 目录代码结构介绍

```
D:.
├─sql                             代码数据库文件
├─src                             源码
│  ├─main                         main 包，主要代码文件包
│  │  ├─java                      java 代码
│  │  │  └─com                   
│  │  │      └─linqi
│  │  │          ├─config        配置文件目录，存放项目依赖相关配置
│  │  │          ├─constants     常量文件目录，存放代码中公共常量
│  │  │          ├─controller    controller 层，存放 Restful 风格的 API 接口
│  │  │          ├─dto           dto 目录，存放业务封装类
│  │  │          ├─entity        项目实体目录，存放和数据库对应的 Java POJO 实体类
│  │  │          ├─interceptors  拦截器，主要存放两个拦截器，一个是用户登录拦截器和token刷新拦截器
│  │  │          ├─mapper        mapper 层，主要用于存放操作数据库的代码
│  │  │          ├─service       Service 层。主要用于存放拉业务逻辑处理代码
│  │  │          │  └─impl       实现接口，主要实现 Service 层下的接口
│  │  │          └─utils
│  │  └─resources                资源目录
│  │      ├─data                 数据资源，存放一些过程中需要的数据
│  │      ├─db                   数据库创建 SQL 
│  │      └─mapper               存放一些 SQL 语句
   └─test                        测试目录


```


## 项目实现
1. 短信登录功能：使用 Redis 实现分布式 Session,解决了集群件登录态同步的问题，并且使用 Hash 代替 String 来存储用户信息，节省了 10 % 左右的内存，且有利于单字段的修改，最后改进成了使用 Redis + Token 机制实现单点登录。
2. 店铺查询功能：使用 Redis 实现了对于高频访问店铺进行缓存，降低数据库压力的同时，提升了接近 90% 的数据查询性能。
3. 为方便其他业务后续使用缓存，使用泛型 + 函数式编程实现了通用缓存访问静态方法，并且解决了缓存雪崩、缓存穿透等问题。
4.使用常量类全局管理 Reids Key 前缀、TTL 等内容，保证了键空间的业务隔离，减少冲突。
5.使用 Redis 的 Geo + Hash 数据结构分类存储附近商户，并且使用 Geo Search 命令实现高性能的商户查询以及距离排序功能；
6.使用 Redis List 数据结构存储用户点赞信息，并且基于 Zset 实现 TopN 点赞排行，其相比于数据库查询，性能提升了 10 %;
7.使用 Redis Set 数据结构实现用户关注以及共同关注功能（交集），实际测量结果与数据库查询对比，提升了 17 %;
8.使用 Redis BitMap 实现用户连续签到功能，与传统数据库相比，节省了大约 24 % 的内存以及提升了 11 %的查询性能；
9.项目基于推模式实现了 Feed 流，保证了新点评消息的及时可达，大大减少了用户访问的等待时间；
10：优惠券秒杀：使用 Redis + Lua 脚本实现库存预检，并且通过 Stream 队列实现订单的异步可达，解决了超卖问题，实现一人一单功能。
   
## 项目功能展示 
功能演示：<a href="https://github.com/DIDA-lJ/Dianping/blob/main/Presentation.md">功能演示</a>


