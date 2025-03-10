---
tags:
  - redis
---
# Redis学习笔记
SQL 关系型数据库
NoSQL 非关系型数据库
## Redis数据类型
![image.png](https://raw.githubusercontent.com/GQLiu1001/mytc/master/img/20250310094815127.png)
### String（字符串）
 **特点**：
- 最基本的数据类型，所有数据最终都会以字符串形式存储。
- 最大存储 512MB 的字符串。
- 适用于缓存、计数器、分布式锁等场景。
### List（列表）
**特点**：
- 基于双向链表实现，可以实现队列（FIFO）或栈（LIFO）。
- 适用于消息队列、任务队列、排行榜等场景。
### Hash（哈希）
**特点**：
- 适用于存储对象（类似于 Java 的 `Map<String, Object>`）。
- 可以高效地存储和获取某个字段的值，节省空间。
- 适用于存储用户信息、对象属性等场景。
### Set（集合）
**特点**：
- 元素无序、唯一，基于 `hashtable` 实现。
- 适用于去重、标签管理、共同好友等场景。
### Geo（地理位置）
**特点**：
- 存储地理坐标（经纬度），支持附近查询。
- 适用于 LBS（Location-Based Service）场景，如查找附近的商家/用户。
## SpringDataRedis
### maven依赖
```xml
<!-- https://mvnrepository.com/artifact/org.springframework.data/spring-data-redis -->
<dependency>
<groupId>org.springframework.data</groupId>
    <artifactId>spring-data-redis</artifactId>
    <version>3.4.3</version>
</dependency>

<!-- 连接池https://mvnrepository.com/artifact/org.apache.commons/commons-pool2 -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
    <version>2.12.1</version>
</dependency>
```
