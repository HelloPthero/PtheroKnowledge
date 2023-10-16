#### 基础

##### 约束

##### 增删改查

```sql
TOP/Limit

	mysql 
	select * from table limit 5
	sqlserver 
	select top 5 * from table
	select top 50 percent * from table
通配符	
	%
	_
	[charlist]   charlist中的任一字符     [^charlist] ==  [!charlist]
	SELECT * FROM TABLE WHERE CLN LIKE '[ABC]%' 
BETWEEN AND
	包含边界
```



##### 执行顺序

```
from 
join 
on 
where 
group by
sum()/average()/min()/max()
having
select
distinct
order by 
top/limit
```

#### 高级

##### 事务

```
ACID  
	原子性  事务里的操作要么都做，要么都不做  
	一致性(完整性)  事务包含的处理满足数据库提前设置的约束,如NOT NULL
	隔离性   事务之间互不干扰。不存在嵌套等情况，一个事务未提交前，其他事务里数据不会受到影响
	持久性	 事务结束后，该时间节点的数据状态会被保存，可以恢复
	
命令 
	COMMIT  提交 保存更改
	ROLLBACK  回滚   回滚到上次COMMIT
	SAVRPOINT point_name  创建节点     
		ROLLBACK TO point_name 回滚到当前节点   
		RELEASE SAVEPOINT point_name 删除当前节点
	TRANSACTION 事务命令
```

```
SQL SERVER   :  BEGIN TRANSACTION 
MYSQL        :  START TRANSACTION

BEGIN/START TRANSACTION;
	XXXX
	XXXX
COMMIT/ROLLBACK;
```



##### 函数

```sql
//不合并 排序    113
select sid ,
rank() over(
	partition by sid,
	order by score
) as rank_1
from StudentScore

//合并排序   112 
select sid,
dense_rank() over(
	partition by sid,
	order by score
) as rank_2
from StudentScore

//
TIMESTAMPDIFF(unit,begin,end);

select sname, timestampdiff(year, sage, now()) as age from student
```



##### 视图

```sql
CREATE OR REPLACE VIEW View_name AS             更新视图
SELECT ....   

定义视图不要用order by      视图和表一样,数据行是没有顺序的
```



##### 存储过程

```sql
CREATE PROC proc_name
AS
BEGIN
	XXXX
END


EXEC proc_name
```



##### 触发器

```
1.DML(Data Manipulation Language)触发器 数据操作语言触发器
	insert delete update 
2.DDL(Data Definition Language)触发器  数据定义语言触发器
	主要为  Create Drop Alter
3.登录触发器

模板 

CREATE TRIGGER Trigger_name 
ON Table_name 
AFTER|FOR|INSTEND OF  [DELETE|INSERT|UPDATE]
AS 
BEIGIN 
	SQL语句
END 
```



##### 索引

```
索引会提高查询效率 
1.增删改需要消耗时间，索引需要动态维护
2.索引是一种数据结构，需要消耗物理空间
```

```
Create index index_name on tabel_name(column_name)

索引会导致插入或更新数据的时候速度变慢 所以不要加过多索引
聚集索引   非聚集索引
聚集索引存储记录是物理上连续存在的 而非聚集索引是逻辑上连续存在的
例:字典按拼音存字，字典目录提供拼音部首两种查询方案吗，拼音查询时聚集索引，部首查询是非聚集索引
聚集索引一个表只能有一个 非聚集索引可以有多个
```

```
//创建索引
CREATE [UNIQUE][CLUSTERED|NONCLUSTERED] INDEX index_name
ON {table_name|view_name} [WITH [index_property]]

UNIQUE  唯一索引
CLUSTERED  聚集索引
NONCLUSTERED  非聚集索引  (默认)

//删除索引
DROP INDEX table_name.index_name

//查询索引
EXE SP_HELPINDEX  table_name

//覆盖索引  对非聚集索引 多关联几个字段
CREATE INDEX index_name 
ON table_name(column_name)
INCLUDE(column_name1,column_name2)


mysql
	show index/keys from table_name
	create index index_name on table_name(column_name)
	alter table table_name drop index index_name 
	explain  sql_
sqlserver 
	exec sp_helpindex table_name 
    create index index_name on table_name(column_name)
    drop index table_name.index_name
    set statistics profile on sqlserver_
```

##### 应用场景

```
经常需要搜索的列
作为主键的列
外键 常用于连接的列
常用于范围搜索的列
常排序的列
```

<img src="file://C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220419160025296.png?lastModify=1652799874" alt="image-20220419160025296" style="zoom: 50%;" />

##### 分类

```
主键索引  跟随主键一起 Primary Key
唯一索引
单列索引   一个索引绑定一列
复合索引   一个索引绑定多列
聚集索引  索引项的排序和数据项物理排序一致。根据聚集索引键顺序存储表数据。单个
非聚集索引  与物理顺序不一致，多个
```







#### 优化方案

```
1.不要用select * 
2.避免在where中使用OR   要用  Union   or会导致索引失效
3.where中不要执行计算
3.使用varchar  char不足会补空格
```

