---
title: "Redis"
date: 2022-10-16T07:07:52Z
---

## **什么是Redis？Redis主要用途是什么？**

Redis，英文全称是Remote Dictionary Server（远程字典服务），是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

## **Redis数据类型**

- **String**(字符串)
- **Hash**(哈希)
- **List**(列表)
- **Set**(集合)
- **Zset**(有序集合)

- **Geospatial**(地理位置)
- **Hyperloglog**(基数统计)
- **Bitmap**(位图)
- **Streams**(流)

    ### **常用命令**
    查找所有符合模式pattern的key。pattern可以使用通配符
    ```zsh
    keys pattern
    ```
    判断key是否存在于数据库中
    ```zsh
    exists key
    exists key key1 key2 ...    #判断key是否存在，返回存在的个数
    ```
    移动指定的key到指定的数据库实例（Redis默认有16个库），用户默认使用第0个库
    ```zsh
    move key index
    ```
    查看key的剩余生存时间
    ```zsh
    ttl key
    ```
    设置key的最大生存时间
    ```zsh
    expire key seconds
    ```
    查看指定key的数据类型
    ```zsh
    type key
    ```
    重命名指定key
    ```zsh
    rename key
    ```
    删除指定key和value
    ```zsh
    del key
    del key key1 key2 ...
    ```
    增加指定值
    ```zsh
    incrby key num
    ```
    减去指定值
    ```zsh
    decrby key num
    ```
    ### **各数据类型应用场景**
    1. **String**

        redis最基本的数据类型，二进制安全的字符串，key和字符串类型的value最大为512M，但是一般key不超过1K，节约空间，也利于检索

        - 缓存，热点数据
        - 分布式session
        - 分布式锁
        - incr计数器
        - 全局ID（int类型，incrby,利用其原子性）
        - incr限流（以访问者ip或其他信息为key,访问增加次数，超过一定次数返回false）
        - setbit位操作

        #### **常用命令**
        添加数据
        ```zsh
        set key value   #如果key已存在，之前的value将会被覆盖
        ```
        获取指定key的值
        ```zsh
        get key
        ```
        追加字符串
        ```zsh
        append key value    #返回字符串长度；如果key不存在，则存储为新的key
        ```
        获取字符串长度
        ```zsh
        strlen key
        ```
        将value数值加一
        ```zsh
        incr key    #返回计算后的值；如果该值不是数值，将报错；如果key不存在，则自动存储新的key，并初始化为0，然后加一；
        ```
        将value数值减一
        ```zsh
        decr key
        ```
        将value数值加具体值
        ```zsh
        incrby key increment
        ```
        将value数值减具体值
        ```zsh
        decrby key increment
        ```
        闭区间截取字符串中的一段
        ```zsh
        getrange key startIndex endIndex
        ```
        替换从指定下标开始的字符串
        ```zsh
        setrange key offset value
        ```
        添加数据并设置生命周期
        ```zsh
        setex key seconds value
        ``` 
        添加key值不存在的数据
        ```zsh
        setnx key value #key值不存在时添加，返回结果1；key值已存在不添加，返回结果0；
        ```
        批量添加数据
        ```zsh
        mset key1 value1 key2 value2 key3 value3 ...
        ```
        批量获取数据
        ```zsh
        mget key1 key2 key3 ...
        ```
        批量添加key值不存在的数据
        ```zsh
        msetnx key1 value1 key2 value2 ...   #所有key都不存在设置成功，只要有一个存在设置失败
        ```

    2. **Hash**

        key-value键值对形势的集合

        - 值为序列化对象时

        #### **常用命令**
        将一个或多个键值对存储到指定集合中
        ```zsh
        hset key filed value ...
        ```
        获取hash表中指定的filed值
        ```zsh
        hget key filed
        ```
        批量获取hash表中指定的filed值
        ```zsh
        hmget key filed1 filed2 ...
        ```
        获取指定hash表中的所有filed和value
        ```zsh
        hgetall key
        ```
        删除指定hash表中的一个或者多个filed
        ```zsh
        hdel key filed1 filed2 ...
        ```
        获取指定hash表中所有的filed的个数
        ```zsh
        hlen key
        ```
        判断指定hash表中指定的filed是否存在
        ```zsh
        hexists key filed
        ```
        获取指定hash表中所有filed的列表
        ```zsh
        hkeys key
        ```
        获取指定hash表中所有value的值
        ```zsh
        hvals key
        ```

    3. **List**

        保持顺序的字符串列表

        - 消息队列
        - 秒杀

        #### **常用命令**
        将一个或多个值依次插入列表的表头
        ```zsh
        lpush key value1 value2 ...
        ```
        获取列表中指定下标区间的元素
        ```zsh
        lrange key startIndex endIndex
        ```
        将一个或多个值依次插入列表的表尾
        ```zsh
        rpush key value1 value2 ...
        ```
        删除指定列表的表头元素并返回
        ```zsh
        lpop key
        ```
        删除指定列表的表尾元素并返回
        ```zsh
        rpop key
        ```
        获取指定列表中指定下标的元素并返回
        ```zsh
        lindex key index
        ```
        获取指定列表的长度
        ```zsh
        llen key
        ```
        根据count的值移除列表中的指定的某一些元素
        ```zsh
        lrem key count value    #count>0:从表头开始数前n个；count<0:从表尾开始数前n个；count=0：移除所有跟value相同的元素
        ```

    4. **Set**

        无序的字符串集合，无重复项

        - 无重复项列表

        Redis 提供 sinterstore、sunionstore、sdiffstore 命令来将集合的交集、并集、差集的结果保存， Redis 在进行上述比较时，会比较耗费时间，所以为了提高性能可以将交集、并集、差集的结果提前保存起来，这样在需要使用时，可以直接通过 smembers 命令获取

        #### **常用命令**
        将一个或多个元素添加到指定的集合中
        ```zsh
        sadd key member1 member2 ...
        ```
        获取指定集合中的所有元素
        ```zsh
        smembers key
        ```
        判断指定元素在指定集合中是否存在
        ```zsh
        sismember key member    #存在返回1，不存在返回0
        ```
        获取指定集合的长度
        ```zsh
        scard key
        ```
        移除指定集合中一个或者多个元素
        ```zsh
        srem key member1 member2 ...    #不存的元素会忽略
        ```
        随机获取指定集合中的n个元素
        ```zsh
        srandmember key [count] #count不指定，默认为1；count>0：随机获取的数不重复，count<0：随机获取的数可能重复
        ```
        从指定集合中随机移除一个或者多个元素
        ```zsh
        spop key [count]    #count不指定，默认为1
        ```
        从指定集合中移动指定一个元素到另一个集合中
        ```zsh
        smove source destination member
        ```
        返回差集
        ```zsh
        sdiff key1 key2 ...
        ```
        返回交集
        ```zsh
        sinter key1 key2 ...
        ```
        返回并集
        ```zsh
        sunion key1 key2 ...
        ```

    5. **Zset**

        已排序的字符串集合

        - 排行榜

        **常用命令**
        将一个或者多个member及score加入有序集合
        ```zsh
        zadd key score1 member1 score2 member2 ...
        ```
        根据指定集合获取指定区间的元素
        ```zsh
        zrange key startindex endindex
        ```
        根据指定分数区间获取元素
        ```zsh
        zrangebyscore key min max
        ```
        删除指定集合中一个或多个指定元素
        ```zsh
        zrem key member1 member2 ...
        ```
        获取集合中元素的个数
        ```zsh
        zcard key
        ```
        获取指定元素的排名
        ```zsh
        zrank key member    #正序
        zrevrank key member #倒序
        ```
        获取指定集合中在指定分数区间的元素个数
        ```zsh
        zcount key min max
        ```
        获取指定集合中的指定元素的分数
        ```zsh
        zscore key member
        ```

    6. **Geospatial**

        地理位置信息存储类型

        **常用命令**

    7. **Hyperloglog**

        基于概率的数据类型

        **常用命令**

    8. **Bitmap**

        更细化的操作，以Bit为单位

        **常用命令**

    9. **streams**

        **常用命令**


## **Redis持久化策略**

Redis提供持久化策略，用一些适当的手段在适当的时机将数据存在磁盘中，每次启动Redis都会自动加载磁盘的数据到内存中
- RDB(redis默认持久化策略)

    在指定时间间隔内，redis服务执行指定次数的写操作，会自动触发依次持久化操作。

        配置属性：

            save <seconds><changes>：配置持久化策略

            dbfilename：配置redis RDB持久化数据存储的文件

            dir：配置redis RDB持久化文件所在目录

- AOF

    采用操作日志来记录进行每一次写操作，每次redis服务启动时，都会重新执行一遍操作日志中的命令。效率较低，redis默认不开启。

        配置属性：

            appendonly：配置是否开启AOF

            appendfilename：配置操作日志文件

## **Redis淘汰策略**
- noeviction 内存超过配置大小直接返回错误，不进行淘汰
- allkeys-lru 内存超限时，使用lru算法淘汰最近最少使用的键
- volatile-lru 内存超限时，使用lru算法淘汰设置了过期时间并且最近最少使用的键
- allkeys-random 内存超限时，随机淘汰键
- volatile-random 内存超限时，随机淘汰设置了过期时间的键
- volatile-ttl 淘汰快到过期时间的键
- volatile-lfu 淘汰设置了过期时间并且使用频率最低的键
- allkeys-lfu 淘汰使用频率最低的键
