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
### 操作示例
```java
package com.example.redis.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.geo.*;
import org.springframework.data.redis.core.*;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.concurrent.TimeUnit;

/**
 * RedisService - 提供对 Redis 的 String、Set、Hash、Geo 和 List 数据类型的基本操作。
 */
@Service
public class RedisService {

    @Autowired
    private RedisTemplate<String, Object> redisTemplate; // Redis 通用操作

    @Autowired
    private StringRedisTemplate stringRedisTemplate; // 专门用于 String 类型的操作

    /**
     * ========== String（字符串）操作 ==========
     */

    public void setString(String key, String value, long timeout) {
        stringRedisTemplate.opsForValue().set(key, value);
        if (timeout > 0) {
            stringRedisTemplate.expire(key, timeout, TimeUnit.SECONDS);
        }
    }

    public String getString(String key) {
        return stringRedisTemplate.opsForValue().get(key);
    }

    public void deleteString(String key) {
        stringRedisTemplate.delete(key);
    }

    /**
     * ========== Set（集合）操作 ==========
     */

    public void addToSet(String key, String... values) {
        redisTemplate.opsForSet().add(key, (Object[]) values);
    }

    public Set<Object> getSet(String key) {
        return redisTemplate.opsForSet().members(key);
    }

    public boolean isMemberOfSet(String key, String value) {
        return Boolean.TRUE.equals(redisTemplate.opsForSet().isMember(key, value));
    }

    public void removeFromSet(String key, String... values) {
        redisTemplate.opsForSet().remove(key, (Object[]) values);
    }

    /**
     * ========== Hash（哈希表）操作 ==========
     */

    public void setHashValue(String key, String field, Object value) {
        redisTemplate.opsForHash().put(key, field, value);
    }

    public Object getHashValue(String key, String field) {
        return redisTemplate.opsForHash().get(key, field);
    }

    public Map<Object, Object> getAllHashValues(String key) {
        return redisTemplate.opsForHash().entries(key);
    }

    public void deleteHashField(String key, String field) {
        redisTemplate.opsForHash().delete(key, field);
    }

    /**
     * ========== List（列表）操作 ==========
     */

    /**
     * 从左侧插入一个或多个元素到列表
     *
     * @param key    列表键
     * @param values 要插入的元素
     */
    public void leftPushList(String key, String... values) {
        redisTemplate.opsForList().leftPushAll(key, (Object[]) values);
    }

    /**
     * 从右侧插入一个或多个元素到列表
     *
     * @param key    列表键
     * @param values 要插入的元素
     */
    public void rightPushList(String key, String... values) {
        redisTemplate.opsForList().rightPushAll(key, (Object[]) values);
    }

    /**
     * 从左侧弹出一个元素
     *
     * @param key 列表键
     * @return 弹出的元素
     */
    public Object leftPopList(String key) {
        return redisTemplate.opsForList().leftPop(key);
    }

    /**
     * 从右侧弹出一个元素
     *
     * @param key 列表键
     * @return 弹出的元素
     */
    public Object rightPopList(String key) {
        return redisTemplate.opsForList().rightPop(key);
    }

    /**
     * 获取列表中指定范围的元素
     *
     * @param key   列表键
     * @param start 起始索引（包含）
     * @param end   结束索引（包含 -1 代表最后一个元素）
     * @return 列表中的元素
     */
    public List<Object> getListRange(String key, long start, long end) {
        return redisTemplate.opsForList().range(key, start, end);
    }

    /**
     * 移除列表中的某个值
     *
     * @param key   列表键
     * @param count 移除的个数（正数从头移除，负数从尾移除，0 移除所有）
     * @param value 需要移除的值
     */
    public void removeFromList(String key, long count, String value) {
        redisTemplate.opsForList().remove(key, count, value);
    }

    /**
     * ========== Geo（地理位置）操作 ==========
     */

    public void addGeoLocation(String key, double longitude, double latitude, String member) {
        redisTemplate.opsForGeo().add(key, new Point(longitude, latitude), member);
    }

    public Point getGeoLocation(String key, String member) {
        List<Point> points = redisTemplate.opsForGeo().position(key, member);
        return points != null && !points.isEmpty() ? points.get(0) : null;
    }

    public Distance getGeoDistance(String key, String member1, String member2, Metric unit) {
        return redisTemplate.opsForGeo().distance(key, member1, member2, unit);
    }

    public GeoResults<GeoLocation<Object>> getNearbyLocations(String key, double longitude, double latitude, double radius) {
        Circle circle = new Circle(new Point(longitude, latitude), new Distance(radius, Metrics.KILOMETERS));
        return redisTemplate.opsForGeo().radius(key, circle);
    }
}

```
	