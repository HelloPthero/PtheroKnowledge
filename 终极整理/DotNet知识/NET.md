#### 概念内容

##### CLR(Common Language Runtime) 公共语言运行时

```
核心功能:内存管理,程序集加载,安全性,异步处理和线程同步
```

##### CTS(Common Type System) 通用类型系统

```
值类型(System.ValueType)和引用类型(System.Object) 
System.ValueType 也是一种引用类型
```

##### CLS(Common Language Specification) 公共语言规范

```
对.net平台上使用的不同语言进行规范
```

#### 六大对象

```
1.Response  HTTP响应
2.Request   HTTP请求
3.Service   服务
4.Session
5.Cookie
6.Application   全局集合对象 存值取值
```

#### 获取配置文件

```
Core
IConfiguration configuration = new ConfigurationBuilder().SetBasePath(Environment.CurrentDirectory).AddJsonFile("appsettings.json", true, true).AddInMemoryCollection().Build();
            _conStr = configuration["ConnectionStrings:mysqldb"];
 
 Framework
 _conStr = ConfigurationManager.ConnectionStrings["mysqldb"].ConnectionString;
```



#### GC

##### 概念

```
（Garbage Collector垃圾收集器）
以应用程序为基础,遍历堆中的对象,如果没有被引用,则被当作垃圾(通过标记清除算法),需要被回收。
GC不能释放非托管资源。
非托管资源可以通过继承IDispose接口,实现Dispose()方法完成显示释放。
```

```
GC通过调用Object.Finalize()方法释放资源，编译器通过析构函数自动生成此方法
对于非托管资源,可以将释放资源的代码写在析构函数中。
非托管资源包括：数据库连接,StreamWriter,Timer,Tooltip,Image Socket,文件句柄
```

##### 函数

```
GC.SuppressFinalize();  //请求公共语言运行时不要调用指定对象的终结器
GC.Collect();   	//强行对所有代进行资源回收
GC.GetTotalMemory(false);    //检索当前要分配的字符数
```

##### 应用

![image-20220428144915597](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220428144915597.png)



#### System.IO

##### 流

