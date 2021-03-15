---
title: Home
type: book  # Do not modify.
toc: true
---

Welcome to gyanliang's notes.

# 复习列表

## 基础

- [x] String 类能不能被继承？为什么？
- [x] 实现单例设计模式（懒汉，饿汉）
- [x] == 和 equals() 的区别？
- [ ] 简述 BIO, NIO, AIO 的区别
- [x] 简述 Java 的反射机制及其应用场景
- [x] 什么是内存泄漏，怎么确定内存泄漏？
- [x] 简述动态代理与静态代理
- [x] java的异常体系介绍一下
- [x] Java 是如何实现线程安全的，哪些数据结构是线程安全的？
- [x] Java 中接口和抽象类的区别
- [x] hashcode 和 equals 方法的联系
- [x] 简述常见的工厂模式以及单例模式的使用场景
- [x] 简述生产者消费者模型
- [x] Java 如何高效进行数组拷贝
- [ ] 成员变量和方法的区别？


## 并发

- [x] volatile 关键字解决了什么问题，它的实现原理是什么？
- [x] synchronized 关键字底层是如何实现的？它与 Lock 相比优缺点分别是什么？
- [x] 简述 Synchronized，Volatile，可重入锁（ReetrantLock） 的不同使用场景及优缺点？
- [x] ThreadLocal 实现原理是什么？
- [x] Java 常见锁有哪些？ReetrantLock 是怎么实现的？
- [ ] Java 线程和操作系统的线程是怎么对应的？Java线程是怎样进行调度的?
- [x] Java 中 sleep() 与 wait() 的区别
- [x] Java 线程池里的 arrayblockingqueue 与 linkedblockingqueue 的使用场景和区别
- [x] 线程池是如何实现的？简述线程池的任务策略
- [x] Java 线程间有多少通信方式？
- [x] CAS 实现原理是什么？
- [x] 谈谈对AQS的理解
- [ ] 简述乐观锁以及悲观锁的区别以及使用场景
- [ ] 什么情况下会发生死锁，如何解决死锁？


## 集合

- [x] Java 容器有哪些？ 哪些是同步容器？ 哪些是并发容器？
- [ ] HashMap 与 ConcurrentHashMap 的实现原理是怎样的？ConcurrentHashMap 是如何保证线程安全的？
- [x] 集合类中的 List 和 Map 的线程安全版本是什么，如何保证线程安全的？
- [ ] 简述 HashMap 和 TreeMap 的实现原理以及常见操作的时间复杂度
- [ ] 简述 ArrayList 与 LinkedList 的底层实现以及常见操作的时间复杂度
- [ ] hashmap 和 hashtable 的区别是什么？
- [ ] HashMap 实现原理，为什么使用红黑树？
- [ ] hashMap 1.7 / 1.8 的实现区别


## JVM

[JVM 基础](./java/jvm)
[JVM 垃圾回收](./java/jvm_gc)


## 数据库

### Mysql

- [x] MySQL 为什么使用 B+ 树来作索引，对比 B 树它的优点和缺点是什么？
- [x] 数据库的事务隔离级别有哪些？各有哪些优缺点？
- [x] 数据库有哪些常见索引？
- [x] 简述什么是最左匹配原则
- [x] 数据库索引的实现原理是什么？
- [x] 聚簇索引和非聚簇索引有什么区别？什么情况用聚集索引？
- [x] 唯一索引与普通索引的区别是什么？使用索引会有哪些优缺点？(插入速度，查询速度)
- [x] MySQL 的索引什么情况下会失效？
- [x] 简述数据库中的 ACID 分别是什么？
- [x] 数据库设计的范式是什么？
- [x] 简述脏读和幻读的发生场景，InnoDB 是如何解决幻读的？
- [x] MySQL 中 varchar 和 char 的区别是什么？
- [x] 并发事务会引发哪些问题？如何解决？
- [ ] 简述 MySQL 三种日志的使用场景
- [ ] 简述 undo log 和 redo log 的作用，区别是什么，分别有什么用？
- [x] 简述 MySQL MVCC 的实现原理
- [ ] SQL优化的方案有哪些，如何定位问题并解决问题？
- [ ] 简述数据库中什么情况下进行分库，什么情况下进行分表？
- [ ] 数据库查询中左外连接和内连接的区别是什么？
- [ ] 数据库的读写分离的作用是什么？如何实现？
- [ ] 什么是 SQL 注入攻击？如何防止这类攻击？
- [ ] 什么是数据库事务，MySQL 为什么会使用 InnoDB 作为默认选项
- [x] 简述 MySQL 的间隙锁，如何实现
- [ ] MySQL 有什么调优的方式？
- [ ] 简述 MySQL 的主从同步机制，如果同步失败会怎么样？
- [x] Mysql 索引重建
- [ ] Mysql 事务实现的底层原理
- [x] Mysql 在可重复读隔离级别下会不会有幻读的情况，为什么？
- [x] 共享锁和排他锁的使用场景
- [ ] 数据库中得连接方式有哪些
- [x] Mysql 如何查看你的 sql 执行情况
- [ ] checkpoint机制
- [x] Innodb 和 MyISAM 的区别是什么
- [ ] Innodb 的默认加锁方式是什么，是怎么实现的
- [ ] Mysql 什么情况会造成慢查，如何查看慢查询
- [ ] 如何处理慢查询，你一般是怎么处理慢查询的
- [ ] 慢查询的解决方式
- [ ] 如何高效处理大库 DDL
- [ ] 关系型数据库和非关系数据库的优缺点
- [ ] 数据库外键的优缺点
- [ ] 有没有使用过数据库的视图
- [ ] Mysql 中插入数据使用自增 id 好还是使用 uuid，为什么？
- [ ] Mysql 有哪些数据类型，使用的时候有没有什么注意点
- [ ] Mysql 集群有哪几种方式，分别适用于什么场景
- [ ] Mysql 主从模式如何保证主从强一致性
- [ ] Mysql 集群如何保证主从可用性

- [Mysql 基础架构](./Mysql/mysql.md)
- [Mysql 增删改查操作](./Mysql/mysql-index-operation.md.md)
- [Mysql 索引](./Mysql/mysql-index.md)
- [Mysql 索引 增删改查操作](./Mysql/mysql-index-operation.md)
- [Mysql 锁](./Mysql/mysql-lock.md)
- [Mysql 事务](./Mysql/mysql-trans.md)
- [Mysql MVCC 多版本并发控制](./Mysql/mysql-mvcc.md)
- [Mysql 日志](./Mysql/mysql-logs.md)
- [Mysql 其他](./Mysql/other.md)
- [Mysql 问题列表](./Mysql/questions.md)


### Redis

- [ ] 简述 Redis 持久化中 rdb 以及 aof 方案的优缺点
- [ ] Redis 序列化有哪些方式？
- [ ] 简述 Redis 的哨兵机制
- [ ] Redis 如何实现分布式锁？
- [ ] 简述 Redis 中如何防止缓存雪崩和缓存击穿
- [ ] 简述 Redis 的过期机制和内存淘汰策略
- [ ] 简述 Redis 中常见类型的底层数据结构
- [ ] 简述 Redis 中跳表的应用以及优缺点
- [ ] Redis 有几种数据结构？Zset 是如何实现的？
- [ ] Redis的缓存淘汰策略有哪些？
- [ ] 为什么 Redis 在单线程下能如此快？
- [ ] Redis 如何实现延时队列，分布式锁的实现原理


## Kafka

- Kafka 发送消息是如何保证可靠性的？
- [Kafka中的基本概念](https://gyl-coder.top/kafka/kafka-concept/)
- [Kafka中的ISR、AR又代表什么？ISR的伸缩又指什么？](https://gyl-coder.top/kafka/kafka-concept/)
- [Kafka中的HW、LEO、LSO等分别代表什么？](https://gyl-coder.top/kafka/kafka-concept/)
- [kafka如何保证数据可靠性和数据一致性](https://gyl-coder.top/kafka/kafka-reliability/)
- [Kafka Rebalance机制分析](https://gyl-coder.top/kafka/kafka-rebalance/)
- [kafka 主题管理](https://gyl-coder.top/kafka/kafka-topic-manage/)
- [Kafka有哪几处地方有分区分配的概念？简述大致的过程及原理](https://gyl-coder.top/kafka/kafka-partition-alocation/)



## 网络

- [ ] 简述 TCP 三次握手以及四次挥手的流程。为什么需要三次握手以及四次挥手？
- [ ] RestFul 与 RPC 的区别是什么？RestFul 的优点在哪里？
- [ ] RestFul 是什么？RestFul 请求的 URL 有什么特点？
- [ ] 简述 RPC 的调用过程
- [ ] HTTP 与 HTTPS 有哪些区别？
- [ ] 简述 HTTPS 的加密与认证过程
- [ ] 从输入 URL 到展现页面的全过程？
- [ ] TCP 怎么保证可靠传输？
- [ ] TCP 与 UDP 在网络协议中的哪一层，他们之间有什么区别？
- [ ] TCP 中常见的拥塞控制算法有哪些？
- [ ] DNS 查询服务器的基本流程是什么？DNS 劫持是什么？
- [ ] Cookie 和 Session 的关系和区别是什么？
- [ ] 简述 JWT 的原理和校验机制
- [ ] HTTP 的方法有哪些？
- [ ] 简述常见的 HTTP 状态码的含义（301，304，401，403）
- [ ] TCP的拥塞控制具体是怎么实现的？UDP有拥塞控制吗？
- [ ] 简述 OSI 七层模型，TCP，IP 属于哪一层？
- [ ] 什么是跨域，什么情况下会发生跨域请求？
- [ ] 简述 WebSocket 是如何进行传输的
- [ ] 从系统层面上，UDP如何保证尽量可靠？
- [ ] 简述 HTTP 1.0，1.1，2.0 的主要区别
- [ ] 简述 TCP 的 TIME_WAIT
- [ ] 简述 TCP 滑动窗口以及重传机制
- [ ] TCP 四次挥手的时候 CLOSE_WAIT 的话怎么处理？
- [ ] TCP 的 keepalive 了解吗？说一说它和 HTTP 的 keepalive 的区别？
- [ ] HTTP 的方法有哪些？
- [ ] 什么是 TCP 粘包和拆包？
- [ ] TCP四次挥手过程以及所处状态，为什么还需要有 time_wait？
- [ ] 简述 TCP 协议的延迟 ACK 和累计应答
- [ ] 简述对称与非对称加密的概念
- [ ] 什么是中间人攻击？如何防止攻击？
- [ ] TCP 中 SYN 攻击是什么？如何防止？
- [ ] TCP 四次挥手的时候 CLOSE_WAIT 的话怎么处理？
- [ ] 为什么需要序列化？有什么序列化的方式？


## Spring

- [ ] Spring mvc 流程
- [ ] 简述 Spring AOP 的原理
- [ ] 简述 Spring 的初始化流程


## 其他

- 简述一致性哈希算法的实现方式及原理
- [x] 手写生产者消费者模型



## 项目介绍（Phoenix）

Phoenix是一个有状态的响应式微服务框架。它提供了高可用、动态伸缩、分布式事务、内存管理、实时监控等关键能力。擅长解决金融领域中业务复杂，性能要求高的场景。

Phoenix是一套响应式微服务开发框架，致力于打造有状态服务。在应用系统架构方面，提供了包括客户端开发、服务端开发、消息通讯架构、服务端状态数据存储和恢复、计算层和消息层监控、服务端运维等架构方案。

而在业务开发方面，Phoenix提供了一套领域驱动设计模式(DDD)的实现模型，业务开发中的领域模型定义以聚合根(Aggregate Root)的形式作为业务计算的基本单元，在整个业务计算的流程中，聚合根对象之间仅以消息(Message)作为数据交互接口。框架对聚合根对象的状态管理和多层次交互的实现进行了封装。Phoenix可以支持用无状态的方式来开发高价值的有状态场景。



简介（初衷，优势，为什么造轮子）

你们微服务怎么做服务间通讯？

​				基于消息中间件、异步调用

基于消息通讯怎么保证消息不丢？

​				自己实现的消息可靠投递机制，定时重试。

你们数据可靠性和一致性怎么做？

​				可靠性：事件存储依赖数据库
​						一致性：事件版本管理，数据库做唯一键检查

KAFKA怎么保证消息不丢？

​				发送端ACK机制，服务端冗余设计，消费端commit offset控制

发送端和消费端的要求？

​				同上

消费端消费后失败，怎么进行重试？
						目前是超时重试。

怎么做幂等？

​				基于msgId和业务自定义幂等。

分布式事务的了解？

TCC、SAGA、二阶段提交

TCC的CC失败怎么处理？

回答目前没有方案处理

内存快照如何保证一致性

可靠性

消息不丢配置

分布式事务

mq选型

分片

cqrs

内存大小，es时间

有状态/无状态的理解

团队规模，公司规模，公司业务

phoenix里有什么展现java能力的亮点

phx优点是什么，使用场景是什么，单线程模型如何使用cpu的多核特性

phx和db的关系

phx如何保证内存可靠性和灾备

phx遇到洪峰流量碰到的问题是什么？最先遇到瓶颈的可能是啥？如何解决

phx里你主要负责的模块？ 多少人做这个框架，成员怎么分布

介绍下actor模型

介绍下akka-cluster和akka-sharding

介绍akka里的actor的生命周期

介绍phx的模型，水平扩容如何实现，提升易用性和性能是什么意思，phx里akka的用法

介绍下saga模型，介绍下事务应该怎么编排



## 算法

- [x] [470. 用 Rand7() 实现 Rand10()](https://leetcode-cn.com/problems/implement-rand10-using-rand7/)
- [x] [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)
- [x] [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
- [x] [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)
- [x] [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)
- [x] [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)
- [x] [437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)
- [x] [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)
- [x] [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)
- [ ] [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)
- [ ] [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
- [ ] [剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)
- [ ] [225. 用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)
- [ ] [232. 用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)
- [ ] [剑指 Offer 31. 栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)
- [ ] [189. 旋转数组](https://leetcode-cn.com/problems/rotate-array/)
- [ ] [300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)
- [ ] [594. 最长和谐子序列](https://leetcode-cn.com/problems/longest-harmonious-subsequence/)
- [ ] [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)
- [ ] [516. 最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)
- [ ] [1218. 最长定差子序列](https://leetcode-cn.com/problems/longest-arithmetic-subsequence-of-given-difference/)
- [ ] [673. 最长递增子序列的个数](https://leetcode-cn.com/problems/number-of-longest-increasing-subsequence/)
- [ ] [521. 最长特殊序列 Ⅰ](https://leetcode-cn.com/problems/longest-uncommon-subsequence-i/)
- [ ] [522. 最长特殊序列 II](https://leetcode-cn.com/problems/longest-uncommon-subsequence-ii/)
- [ ] [674. 最长连续递增序列](https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/)
- [ ] [718. 最长重复子数组](https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/)



- [ ] 常用的限流算法有哪些？简述令牌桶算法原理
- [ ] 有序链表插入的时间复杂度是多少？
- [ ] 实现快速排序
- [ ] 10亿个数中如何高效地找到最大的一个数以及最大的第 K 个数
- [ ] AVL 树和红黑树有什么区别？
- [ ] 给定一个包含 40亿 个无符号整数的大型文件，使用最多 1G 内存，对此文件进行排序
- [ ] 10亿条数据包括 id，上线时间，下线时间，请绘制每一秒在线人数的曲线图
- [ ] 如果通过一个不均匀的硬币得到公平的结果？
- [ ] Hash 表常见操作的时间复杂度是多少？遇到 Hash 冲突是如何解决的？



