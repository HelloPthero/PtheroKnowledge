#### ADO.NET 

##### 对象

```
*Connection
*Command
*DataAdapter
*DataReader
*DataSet 
DataParameter
```

##### 应用

```c#
string connStr = ConfigurationManager.ConnectionStrings["localdb"].ConnectionString;  //.net fw

SqlConnection con = new SqlConnection(conStr);

using(SqlConnection con = new SqlConnection(conStr)){
	con.Open();
}

//数据库命令
var sql = "select * from table";
SqlCommand cmd = new SqlCommand(sql,con);

//取值
Object value = cmd.ExecuteScalar();  //获取第一行第一列数据
int rowcount = cmd.ExecuteNonQuery();  //获取受影响行数
SqlDataReader sdr = cmd.ExecuteReader(); //获取结果集

	while(sdr.Reader()){
        string name = sdr["name"];
        int age = Convert.ToInt32(sdr["age"]);
	}
	sdr.Close();

SqlDataAdapter sda = new SqlDataAdapter(sql,con);
DataTable table = new DataTable();
int count = sda.Fill(table);
foreach(DataRow dr in table.AsEnumerable()){
    string name = dr.Field<string>["name"];
    int age = dr.Field<int>["age"];
}

SqlCommand      cmd.Paramters.Add(new SqlParameter("@name",name));
SqlDataAdapter  sda.SelectCommand.Patameters.Add(new SqlParameter("@name",SqlDbType.VarChar){Value = name});、
```



#### Dapper

优点

```
1.轻量型
2.速度快
```



```
重写IDbConnection方法 

IDbConnection mysqlConnection = new MysqlConnection(_conStr);  //mysql
IDbConnection mssqlConnection = new SqlConnection(_conStr);   //sql server

connection.Excute(sql,model);
connection.Query<T>(sql,model).ToList();

public static int Insert(Person person)
{
    using (IDbConnection connection = new SqlConnection(connectionString))
    {
        return connection.Execute("insert into Person(Name,Remark) values(@Name,@Remark)", person);
    }
}
```



#### EF

##### 原理

https://blog.csdn.net/qq_36724994/article/details/81020601

##### 加载方法

预加载

```
通过Include  ThenInclude进行联级加载，直接获取下级数据
```

显式加载

```
先获取主表数据，再手动获取子表数据
context.Configration.LazyLoadingEnabled = false;
context.Entity(item).Referencez("foreign").Load();  //单个关联对象
context.Entity(item).Collection("foreign").Load();  //多个关联对象
```

懒加载

```
1.安装nuget包：Microsoft.EntityFrameworkCore.Proxies
2.使用UseLazyLoadingProxies启用该包
	//可在dbcontext中配置
	protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
        
	//或在startup里配置
	.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
3.实体类的属性通过virtual进行定义

需要时才访问数据库，在访问导航属性的数据时才进行数据库查询
EF默认有懒加载，EFCore默认没有
```



##### 跟踪查询与非跟踪查询

```
使用[Keyless]特性的实体永远不会被跟踪
```

```
跟踪加载(默认)
会把实例记录在跟踪器中，通过SaveChanges()方法时,会把更改的内容保存到数据库中

如果跟踪器中已经有当前实例，再次查询时会直接获取跟踪器中的内容(虽然还会执行sql)，使用的是跟踪器中的值。

var user =await _context.SysUsers.FindAsync(id);
user.UserName = "11111";
user = await _context.SysUsers.FindAsync(id);//此时user的name还是11111，可以使用AsNoTracking解决

```

```
非跟踪查询 AsNoTracking
获取只读数据记录,不需要进行状态变动跟踪时使用。
AsNoTracking()是定义在IQueryable中的扩展方法
使用AsNoTracking()获取的数据在变更后通过SaveChanges()方法无法更新到数据库

//或者全局级别进行设置
context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;
```

##### 拆分查询

```
AsSplitQuery要配合Include使用，而且只对一对多的关系进行查询时才生效  对查询sql进行拆分，提高查询效率
//where语句放在include前面或AsSplitQuery后面是无所谓的，不会影响sql的实际生成
_context.Roles.Include(r=>r.Users).ThenInclude(u=>u.MyChildren).AsSplitQuery().Where(r => r.Id == 1).ToList();
```





```
var q =
　　from c in db.Customers
　　select c.ContactName; 
这个语句只是一个声明或者一个描述，并没有真正把数据取出来，只有当你需要该数据的时候，它才会执行这个语句，这就是延迟加载(deferred loading)。如果，在声明的时候就返回的结果集是对象的集合。你可以使用ToList() 或ToArray()方法把查询结果先进行保存，然后再对这个集合进行查询。当然延迟加载(deferred loading)可以像拼接SQL语句那样拼接查询语法，再执行它。 
```



##### 执行原始Sql

```
//执行原始sql
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
    
//执行存储过程
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

##### 规约 Specification

```

```

##### 表达式树Express-Tree



##### Linq to EF

IQueryable和IEnumerable

```
IEnumerable  数据加载到内存中
IQueryable   需要的时候在数据库中处理

IEnumerable 数据已经从数据库获取，在内存中进行过滤和排序;Func<> 会被编译器编译成IL代码

IQueryable 延时加载  真正用到时才去数据库获取;Expression<>存储表达式树，运行期作处理
在IQueryable中进行赋值时，无法套用IList IEnumerable数组 会报错
```

```
Task() 选取
Skip() 跳过
```

```
查询语句
    from t in emumlist/query
    where 
    select t
方法语句
	var list = query.where(t=>t.name.contains()).orderby(t=>t.id).tolist();
	
分组

	 var countries = from r in Formula1.GetChampions()
                         group r by r.Country into g
                         let count = g.Count()
                         orderby count descending,g.Key
                         where count >= 2
                         select new {
                             Country = g.Key,
                             Count = count,
                             Racers = from r1 in g
                                    orderby r1.LastName
                                    select r1.FirstName + " " + r1.LastName
                         };
                      
	var countries = Formula1.GetChampions ()
                         .GroupBy (r => r.Country)
                         .Select (g => new {
                         Group = g,
                         Key = g.Key,
                         Count = g.Count ()
                        })
                        .OrderByDescending (g => g.Count)
                        .ThenBy (g => g.Key)
                        .Where (g => g.Count >= 2)
                        .Select (g => new {
                        Country = g.Key,
                        Count = g.Count,
                        Racers = g.Group.OrderBy (r => r.LastName)
                        .Select (r => r.FirstName + " " + r.LastName)
                        });

本地集合用Enumberable   数据库读取用Queryable
```

