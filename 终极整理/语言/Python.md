#### 基础点

```
区分大小写
单行注释# 多行注释用三个引号

```





#### 数据类型

数字

````python
#重新指定值时 重新分配对象
a = 1
print(id(a))
a = 2
print(id(a))

#赋值方式 
a=b=c = 6
c = 7
print(a,b,c)

d,e,f = 1,2,3
print(d,e,f)
````

字符串

```python
str = 'HELLOPTHERO'
print(str[0],end=' ') #不换行
print(str[1])
```

列表

```
用[]表示
左到右0开始 右到左-1开始
```



元组

```
用()表示
内部数据不能更新 
```



字典

```
用{}表示
```

时间

```python
import time
now = time.time()
print(time.localtime())
print(time.strftime('%Y-%m-%d %H:%M:%S',time.localtime()))
```



#### 函数



