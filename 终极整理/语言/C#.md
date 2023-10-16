## 	基础知识

#### 值类型与引用类型

```
值类型 ：普通数据类型(int long float double) bool enum struct char
引用类型： string 数组 class interface delegate  dynamic(动态类型 类型检查在运行时发生)
值类型存放在栈中 
引用类型存放在堆中
```

```
引用类型值相同时，其地址相同
string a = "123";
string b = "123";
a与b地址相同
```



```
dynamic属于引用类型 弱语言类型 运行期间进行类型检查  
var类似于语法糖 在编译期间进行类型检查并且用实际类型替代
```

```
位(bit) 字节(byte) 字     1字 = 2byte = 16bit
sbyte(8) short(16) int(32) long(64)
decimal十进制数 精度比浮点数float/double高 因此常用于财务数据
```



字节 和 字符

```
字节是一种计量单位，字符是文字或符号 
```



```
0x 十六进制   0八进制   无前缀 十进制
```

类型转换

```
1.  name.ToInt32      2 . Convert.ToInt32     3. int.Parse
```

字符串的特殊连接

```c#
//通过使用 string 构造函数
char[] letters = { 'H', 'e', 'l', 'l','o' };
string greetings = new string(letters);
Console.WriteLine("Greetings: {0}", greetings);

//方法返回字符串
string[] sarray = { "Hello", "From", "Tutorials", "Point" };
string message = String.Join(" ", sarray);
Console.WriteLine("Message: {0}", message);

//用于转化值的格式化方法
DateTime waiting = new DateTime(2012, 10, 10, 17, 58, 1);
string chat = String.Format("Message sent at {0:t} on {0:D}", 
waiting);
Console.WriteLine("Message: {0}", chat);
```

#### 装箱拆箱

```
装箱：值类型转化为引用类型
拆箱：引用类型转化为值类型
```



#### 堆栈

```
栈自动清理内存。堆需要手工控制gc

1.申请方式的不同。栈由系统自动分配，而堆是人为申请开辟;
2.申请大小的不同。栈获得的空间较小，而堆获得的空间较大;
3.申请效率的不同。栈由系统自动分配，速度较快，而堆一般速度比较慢;
4.存储内容的不同。栈在函数调用时，函数调用语句的下一条可执行语句的地址第一个进栈，然后函数的各个参数进栈，其中静态变量是不入栈的。而堆一般是在头部用一个字节存放堆的大小，堆中的具体内容是人为安排;
5.底层不同。栈是连续的空间，而堆是不连续的空间。
```



#### 访问修饰符

```
public 公共
internal 程序集内访问
private 私有 类内部访问
protected 受到保护的 类以及其子类访问
protected internal 交集  放宽访问级别

```

```
类的默认访问修饰符：internal
类成员的默认访问修饰符：private
接口: public
```



#### 关键字

abstract 抽象

```
修饰：类 方法 属性
抽象类是一个不完整的类，无法实例化。
抽象类才有抽象方法/抽象属性，抽象方法不实现，派生类必须实现。
抽象类可以有非抽象方法。
抽象类或者抽象方法无法用private修饰。
```

virtual 虚拟的

```
修饰属性或方法
虚方法被继承时需要override重写。
虚方法可以不实现。
```

static 静态的

```
静态类只能包含静态成员。静态类无法实例化。
静态方法需要用类名调用。
静态成员在 ！！类！！ 实例化时创建，属于类。
静态方法不能调用非静态成员。非静态方法可以调用静态成员。
静态构造方法用于构造静态方法，不能有参数
```

const 常量

```
编译时常量
自带static，通过类名访问
可用于修饰字段和临时变量，需要在初始化时赋值。
```

readonly 只读的

```
运行时常量
可用于修饰字段。
在初始化时赋值或者在构造函数中赋值。

static readonly定义的变量可以读写，不能重新赋值 (值类型不能修改，class不能new()，可以对内部字段修改)
```

```
const int a = b*10;                              ----->static readonly 
const int b = 10;
public static void Main()
{
	Console.WriteLine("A={0},B={1}",a,b);
}

const时   A=100,B=10     static readonly时  A=0,B=10(编译时a=0*10=0)
```



sealed 密封的

```
密封类无法被继承
用于方法或属性时必须与override一起使用？
```

override  重写

```
重写父类的abstract,virtual,override方法。  动态多态性
```

overload 重载

```
不使用关键字，概念形                                 静态多态性
同类同名方法,不同参数个数或类型
```

```
运算符重载
public static string operate+(Object o1,Object o2)
{
	return o1.name + o2.name;
}
```

new 

```
1.实例化  Dto a = new Dto();
2.泛型约束  where T:new()
3.显式隐藏从基类继承的成员
	重写父类的变量其实是重新定义了一个新的变量，变量名相同是把父类的变量隐藏了，会增加内存
```

using

```
1.引用命名空间 using system.net.http
2.命名空间重命名   
	using class1 = namespace1.myclass
	using class2 = namespace2.myclass
3.非托管资源try catch 回收   using(var cn = new sqlconnection(_constr))
```

base

```c#
class Rectangle
   {
      // 成员变量
      protected double length;
      protected double width;
      public Rectangle(double l, double w)       
      {
         length = l;
         width = w;
      }
   }
   class Tabletop : Rectangle
   {
      private double cost;
      public Tabletop(double l, double w) : base(l, w)    ///-> base(l,w)传递到基类
      { }
   }
```







#### 计算相关

&&与&

```
&& 不符合条件就返回 截断
& 表达式全部执行

 & |  ^（异或  异为1）   ~（翻转）     <<（二进制左移 右补0）   >>（二进制右移 左补0）
```





#### 参数修饰符

https://blog.csdn.net/weixin_38531633/article/details/116870983

```
params:传入可变数量的参数,形参类型必须为一维数组   实参传参时可以分值传递,不需要作为数组传
ref: 传引用类型，方法中返回多个值，值在外部定义，先进再出
out: 传引用类型，方法中返回多个值，值须在内部赋值，只出不进
```



#### 抽象类,结构与接口

类型方面

```
抽象类：引用类型
结构：值类型
接口：引用类型
```

成员方面

```
抽象类：可以有类的所有成员，可以有实现的方法，可以有抽象方法
结构：不能有无参的构造函数，不能有析构函数，在有参的构造函数中必须初始化所有成员
接口: 不能有字段，不能有实现的方法，不能有构造函数和析构函数
```

继承性

```
抽象类：类之间单继承，多继承接口
结构：结构之间不能继承，不能继承类，可以多继承接口
接口：接口之间多继承
```

结构其他

```
当您使用 New 操作符创建一个结构对象时，会调用适当的构造函数来创建结构。与类不同，结构可以不使用 New 操作符即可被实例化。
如果不使用 New 操作符，只有在所有的字段都被初始化之后，字段才被赋值，对象才被使用。
```



#### 其他

```
//不会编译  ///会编译
使用关键字作为标识符，可以在关键字前面加上 @ 字符作为前缀
NULL合并运算符    ??
```

析构函数 

```
class Line()
{
	public Line(){
		//构造函数
	}
	
	~Line(){
		//析构函数 
	}
}
```





## 高级

#### 集合 Collection

```c#
数组  固定长度,同数据类型
声明一个数组不会在内存中初始化数组   实例化时需要指定数组的秩
    
一维数组
int[] a = new int[100];
char[] s = new char[] {'a','b','c' };

二维数组
int[,] b = new int[2,3];
int [,] a = new int [3,4] 
{  
     {0, 1, 2, 3} ,   /*  初始化索引号为 0 的行 */
     {4, 5, 6, 7} ,   /*  初始化索引号为 1 的行 */
     {8, 9, 10, 11}   /*  初始化索引号为 2 的行 */
};

交叉数组
int[][] c = new int{new int[]{1,2},new int[]{3,4}};
```

```
ArrayList  动态扩展长度
	ArrayList:IList
    解决数组插入困难，浪费内存和内存溢出问题
    大小动态扩充   当长度不足时，会自动扩容为原来的两倍
    方法：Add Insert RemoveAt
    属性：Capacity容量
    存Object类型 需要装箱拆箱
Queue   先进先出  
Stack   先进后出
	
HashTable 键值对 
	Hashtable ht = new Hashtable();
    ht.Add(1, 'a');
    var a =  ht.ContainsKey(1);

    foreach(DictionaryEntry item in hashtable){
        item.Key
        item.Value
    }
    
    key和value都是object类型 value可为null
Dictionary<T,T>  Hashtable的泛型版本
```

```
List<T>
	ArrayList存储的内容类型不确定 因此都是Object 类型不安全  装箱和拆箱损耗性能
Contains()  T值类型根据值判断,引用类型根据引用地址判断(值都相等dto不同 -> false)
Exists(t=>t.a==a)  
```

```
IQueryable  构建表达式树,在数据库执行   延迟加载 .ToList()时才触发查询
IEnumerable  在内存中执行
```



#### 属性 Property

```
通过{get;set;}访问器访问字段
对字段做存取的限制
属性可定义在类/接口/结构中
属性可以使用abstract抽象化
```





#### 特性 Attribute

```
AttributeUsage  自定义特性的标记
Obsolete   过时
Conditional  条件 Debug/Trace  依赖于预处理指令
	#define hong
	[Conditional("hong")]  定义了宏则调用
	
	
一般自定义特性里面只定义字段或属性   继承其他特性的重写方法
Authorization 操作Context
```



#### 索引器 Indexer

```
索引器传入类型不一定只能int 只要指定相应的返回规则就行
索引器通过实例的数组形式访问
```

```
public string this[int index]
        {
        	get{
         		string tmp;
         		if( index >= 0 && index <= size-1 ){
         			tmp = namelist[index];
         		}else{
         			tmp = "";
         		}
         		return (tmp);
         	}
         	set{
         		if( index >= 0 && index <= size-1 )
         		{
         			namelist[index] = value;
         		}
         	}
        }
        
 names[6] = "Rubic";
```



#### 委托 Delegate

```
委托：一种对具有特定参数列表和返回类型的方法的引用
public delegate return_type delegate_name(in_type);

委托能作为临时变量在函数中使用，事件不行
```

```
//默认的泛型委托
Action<T>
Func<T,R> 

委托可以通过+=实现多播

委托内容的执行方式
1.delegate_name();
2.delegate_name.Invoke();
```

```
闭包  
	委托中存的临时变量的值是其声明周期的最终值
```

```
协变逆变

装箱 -  子到父  -  值到引用  - 协变（和谐的） -- out  
里氏替换原则  父类装子类


in out 用于修饰 委托 
out对应的泛型修饰符只能用作返回值
只能泛型接口和泛型委托中使用

delegate T FunOut<out T>();
delegate void FunIn<in T>(T t);
```



#### 事件 Event

概念

```
事件：类或对象通过事件对另外的类或对象进行通知。引发事件的为发布者，接收处理事件的为订阅者
public event de_name event_name;
委托是对方法的引用,事件用于多个类或对象之间的相互通知。
```

```
事件是对委托的安全包裹
事件只能放在class中，通过事件执行委托
```

发布订阅模式

```
using System;

 
namespace ConsoleApp1
{
    public class PublishEvent
    {
        public delegate void NoticeHandler(string message);
 
 
        public event NoticeHandler NoticeEvent;
 
 
        public void Works()
        {
            //触发事件
            OnNoticed();
        }
 
 
        protected virtual void OnNoticed()
        {
            if (NoticeEvent != null)
            {
                //传递事件及参数
                NoticeEvent("Notice发布的报警信息！");
            }
        }
    }
    
    public class SubscribEvent
    {
        public SubscribEvent(PublishEvent pub)
        {
            //订阅事件
            pub.NoticeEvent += PrintResult;
        }
        
        /// <summary>
        /// 订阅事件后的响应函数
        /// </summary>
        /// <param name="message"></param>
        void PrintResult(string message)
        {
            Console.WriteLine(string.Format("已收到{0}！采取措施！",message));
        }
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            PublishEvent publish = new PublishEvent();
            SubscribEvent subscrib = new SubscribEvent(publish);
            //触发事件
            publish.Works();
            Console.Read();
        }
    }
}



发布者中包含委托以及封装委托的事件定义
订阅者中包含发布者,并对发布者的委托进行方法传递。
触发发布者的发布方法,调用执行订阅者中的实际方法。
```





#### 泛型 Generic

```
主要约束 : 引用类型,class,struct
次要约束:实现的接口

public class GClass<T> where T:struct,IComparable	{}

//多个泛型 分别用 where
public class MyGenericClass<T，U> 
where T:IComparable 
where U:struct
{ }
```



```
动态化定义类型 为了减少类型转换与装箱拆箱，不指定特定参数或返回值的类型，实例化时再确定
通过 where 对泛型进行约束 
class 
struct  
new() 实现公共无参构造函数 放最后
```



#### 反射 Reflection

优缺点 

```
优点：灵活方便低耦合
缺点：速度慢 逻辑不清晰  可读性低 不利于维护
```

获取type

```c#
Type ct = typeof(Class);   //Class为类名
Type ct = class.GetType();   //class为实例
Type ct = Type.GetType("System.String");
```

获取内容

```c#
object[] attributes = ct.GetCustomAttributes(true);  //特性
MethodInfo[] mothods = ct.GetMethods();  //公共方法
PropertyInfo[] propertyInfos = ct.GetProperties();  //公共属性
FieldInfo[] fields = ct.GetFields(); //公共字段
MethodInfo mothod = ct.GetMethod("methodname"); //获取单个方法
```

执行

```c#
Object instance = System.Activator.GetInstance(ct);  //获取实例
mothod.Invoke(instance,null);                  //执行方法
```

程序集

```c#
Assembly assembly = Assembly.GetExecutingAssembly()  //当前程序集
Assembly service = Assembly.Load("Service");  //Service为类库名
Type[] types = assembly.GetTypes();
```



#### 多线程 Thread

```
为了实现异步处理，通过Thread建立新的线程，分别处理不同事务。
订阅监听模式
```

```
线程状态
1.未启动  没有调用Start()方法
2.就绪   等待线程池
3.不可运行  阻塞  sleep wait 
4.死亡
```

```
Thread th = new Thread(()=>{})
th.Start();

th.Join();  等当前的线程执行完，再执行主线程
```

#### 异步编程（async/await)

```
异步编程强调等待,多线程强调并行;
应用await关键字后，它将挂起调用方法，并将控制权返还给调用方，直到等待的任务完成。

Thread方法每次的Thread Id都是不同的，而Task方法的Thread Id是重复出现的。我们知道线程的创建和销毁是一个开销比较大的操作，Task每次执行将不会立即创建一个新线程，而是到CLR线程池查看是 否有空闲的线程，有的话就取一个线程处理这个请求，处理完请求后再把线程放回线程池，这个线程也不会立即撤销，而是设置为空闲状态，可供线程池再次调度， 从而减少开销。
```





#### IO

```
主要对象 

Stream   流
Directory   目录 
File        文件
Reader  (BinaryReader/StreamReader/StringReader/TextReader)
Writer

Drive 驱动
```

