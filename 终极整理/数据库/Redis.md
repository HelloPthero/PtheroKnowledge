https://blog.csdn.net/weixin_43829443/article/details/112764830

https://blog.csdn.net/weixin_43829443/article/details/112839985

https://blog.csdn.net/u011301348/article/details/105215016

#### 基础

```
key value 大小写敏感
```



通用

```
ping 
查询连接是否被正常
keys * //查询所有key
clear  //清空页面
flushall  //清空所有库中的内容
字符串有空格需要""  无空格可以不要
```

类型

```
string
	set mystr "hello redis"
	get mystr
	move mtstr 
	exists name 
	expire name 15  过期时间
	ttl strname //查询过期时间  -2 表示已经过期
	type keyname //查询类型
	指令	  (原子性)
		INCR 自增
		INCRBY 10   +10
		DECR 自减
		DECRBY  10    -10
		
	
	strlen strname //长度
    append strname value2 //字符串拼接

    incr numname  //自增1
    incrby numname n //自增n
    decr numname //自减
    decrby numname n  //自减n

    getrange strname start end //截取 0到-1表示全部
    setrange strname start value2  //替换
    setex strname seconds value  //创建并定时
    SETNX strname value //不存在则设置 存在则不设置

    MSET k1 v1 k2 v2 k3 v3 //批量设置
    MGET k1 k2 k3  //批量获取
    MSETNX //批量设置 原子性

    SET class:id:property value //规范化
        SET student:1:age 20
    GETSET key value  //先get 再set 获取get的值






lists  在底层实现上是链表而不是数组
	指令  
		LPUSH    左侧插入  
		RPUSH   右侧插入
		LRANGE    (lange mylist 0 1) 列出第0到第1的元素
		
		lpush listname v1,v2  
        rpush listname v1,v2
        LRange listname startindex endindex

        LPOP listname 左删除
        RPOP listname 右删除

        LINDEX listname index  //查询index下的值
        LLEN listname //长度
        LREM listname count value //移除count个值为value的元素
        LTRIM listname start end //截取
        RPOPLPUSH list1 list2   //取出list1最后一个元素放到list2
        LSET key value index 

        LINSET key BEFORE/AFTER value ??? //插入
        
sets  无序集合
	指令
		SADD MYSET "str" 插入
		SMEMBERS MYSET   列出集合中所有元素
		SISMEMBER MYSET "str"  判断str在不在集合中
		
		SCARD key  //数量
        SREM key value  //移除
        SRANDMEMBER key count //随机获取count个元素

        SPOP key n(默认1)  //随机删除一个元素
        SMOVE key1 key2 value  //移动

        SDIFF key1 ley2 //差集
        SINTER 交集
        SUNION 并集
		
zsets  
	指令 
		ZADD MYZSET 1 "a"  根据序号1插入"a"
		ZRANGE MYZSET 0 -1 WITH SCORES   列出所有元素同时列出序号
		
		ZADD key score1 value1 score2 value2
        ZRANGE key 0 -1 
        ZRANGEBYSCORE key -inf +inf   按从小到大
        ZREVRANGE key rank1 rank2  从大到小
        后缀可以+  WITHSCORES   
        ZREM key value
        ZCARD key 
        ZCOUNT KEY rank1 rank2 
	
hash
	HMSET USER01 NAME PTHERO PASSWORD 123456 AGE 25 
	//建名为UER01的哈希表  name=pthero password = 123456 age = 25
	HSET USER01 PASSWORD 1006
	//将USER01的PASSWORD改为1006
	HGETALL USER01  
	//获取所有内容
	
	HSET hashkey field1 value1 
    HGET hashkey field1 
    HGETALL hashkey   //获取所有 key 和 value
    HKEYS hashkey     //获取所有 key
    HVALS hashkey    //获取所有value
    HDEL hashkey field1 field2 ..
    HLEN hashkey 
    HEXISTS hashkey field1

    HINCRBY hashkey field_num num  指定+  num负数减
    HSETNX  hashkey field value  无则加 

```

作为服务启动

```
D:\Environment\Redis>redis-server --service-install redis.windows.conf
D:\Environment\Redis>redis-server --service-start
redis-server --service-stop
```

#### 持久化

```
RDB(Redis DataBase) 将Redis存储的数据生成快照并存储到磁盘
AOF(Append Only File) 记录指令 保存操作日志 恢复执行
```

#### 应用场景

```
计数器 
缓存
分布式系统中  统一存储多台应用的会话信息
消息队列
```



#### 主从同步

#### 应用

```
1.nuget   StackExchange.Redis
2.private static readonly connectionstring = '127.0.0.1:6379'
3.IConnectionMultiplexer ConnMultiplexer = ConnectionMultiplexer.Connect(ConnectionWriteString);
4._redisdb = ConnMultiplexer.GetDatabase(0);
5._redisdb.StringSet("key1", "key3value", null);
```

```
 /// <summary>
        /// 存储一个对象（该对象会被序列化保存）
        /// </summary>
        /// <param name="redisKey">名称</param>
        /// <param name="redisValue">值</param>
        /// <param name="expiry">时间</param>
        /// <returns></returns>
        public bool StringSet<T>(string redisKey, T redisValue, TimeSpan? expiry = null)
        {
            var json = Serialize(redisValue);
            return _db.StringSet(redisKey, json, expiry);
        }

        /// <summary>
        /// 获取一个对象（会进行反序列化）
        /// </summary>
        /// <param name="redisKey">名称</param>
        /// <param name="expiry">时间</param>
        /// <returns></returns>
        public T StringGet<T>(string redisKey, TimeSpan? expiry = null)
        {
            return Deserialize<T>(_db.StringGet(redisKey));
        }
```

#### 序列化和反序列化

```
static byte[] Serialize(object o)   //序列化方法
        {
            if (o == null)
            {
                return null;
            }   //二进制传化器
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream())
            {      //创建序列化的载体内存流

                binaryFormatter.Serialize(memoryStream, o);
                byte[] objectDataAsStream = memoryStream.ToArray(); //从流转化为字节数组
                return objectDataAsStream;
            }
        }

        static T Deserialize<T>(byte[] stream)   //反序列化方法
        {
            if (stream == null)
            {
                return default(T);
            }
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream(stream))
            {
                T result = (T)binaryFormatter.Deserialize(memoryStream);  //
                return result;
            }
        }
```

