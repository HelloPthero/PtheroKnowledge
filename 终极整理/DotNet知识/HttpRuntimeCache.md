#### HttpRuntime.Cache

##### 方法

###### 插入缓存

```
HttpRuntime.Cache.Add( 
       KeyName,//缓存名 
       KeyValue,//要缓存的对象 
       Dependencies,//依赖项 
       AbsoluteExpiration,//绝对过期时间 
       SlidingExpiration,//相对过期时间 
       Priority,//优先级
       CacheItemRemovedCallback//缓存过期引发事件
);

//insert参数相同
```

```
HttpRuntime.Cache.Add();    //存入相同的键会异常，返回原来的
HttpRuntime.Cache.Insert(); //存入相同的键会覆盖内容，无返回值
```

```
绝对过期时间:到了指定时间便会失效       不设置:System.Web.Caching.Cache.NoAbsoluteExpiration
相对过期时间:在指定时间内无访问会失效   不设置:System.Web.Caching.Cache.NoSlidingExpiration
```

```
优先级：enum CacheItemPriority
Low/BelowNormal/Normal/Default(Normal)/AboveNormal/High/NotRemovable
```

###### 获取

```
获取到的是Object类型的 需要做类型转换
HttpRuntime.Cache[key] as List<T>  
```

###### 移除单个

```
HttpRuntime.Cache.Remove(key); //移除单个
HttpRuntime.Cache.Remove(HttpRuntime.Cache.GetEnumerator().Key.ToString());
```

###### 移除全部

```
var cache = HttpRuntime.Cache;
IDictionaryEnumerator cacheEnum = cache.GetEnumerator();
while(cacheEnum.MoveNext()){
	cache.Remove(cacheEnum.Key.ToString());
}
```

##### 缓存依赖项

```
创建缓存k1,k2 让k2依赖于k1,k1发生变化时,k2失效
```

```
HttpRuntime.Cache.Insert("k1","value1");
CacheDependency dep = new CacheDependency(null,new string[]{"k1"});
HttpRuntime.Cache.Insert("k2","value2",dep,Cache.NoAbsoluteExpiration,Cache.NoSlidingExpiration);
HttpRuntime.Cache.Insert("k1","newvalue");   //k1值改变后，k2失效
```



#### HttpContext.Cache

```
HttpContext.Current.Cache 当前web
对HttpRuntime.Cache的封装,只能在知道HttpContext时使用,因此只能用于Web应用。
```

