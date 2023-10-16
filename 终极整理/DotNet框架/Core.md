#### Core和Framework的区别

```
1.跨平台
2.开源
3.自带kestrel Web服务器
	在跨平台的web服务器中运行时，无需使用不同的启动配置
4.配置文件 json
5.fw全家桶，core动态配置   通过中间件
```



#### 程序运行

```
program.cs  
1.main()
2.CreateWebHostBuilder()  获取Web主机构造器
3.Builder().Build().Run()  构造Web主机并运行

startup.cs

1.ConfigueServices(IServiceCollection service){}
	配置服务与依赖注入
2.Configue(IApplicationBuilder app){}
	配置中间件

```

#### Service

##### 常用的服务

```
AddControllersWithViews
```

##### 服务的生命周期

```
1.singleton  单例 只获取一个服务实例  ILogger日志  [ˈsɪŋɡltən]
2.scoped   作用域    [skəʊpt]
3.transient  每次调用重新生成一个服务  [trænziənt]
```

##### 依赖注入DI

```
//概念
通过控制反转(IoC)容器，将服务添加到容器中，容器将其注入到被依赖项。被依赖项无需考虑服务的实例化，降低耦合度。
```

```
//依赖注入方式
1.构造器注入
2.属性注入
3.全局注入
```

##### 服务定位器





#### Middleware

##### 概念

```
用于处理HttpRequest和HttpResponse的组件，以同心圆的形式构成管道，请求在中间件之间传递处理。是程序的处理根本。
中间件可以控制请求是否传递到下一个中间件。可以在调用下一个中间件前后进行处理。
最内默认返回404
```

##### 常用的中间件

<img src="C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220517220915993.png" alt="image-20220517220915993" style="zoom:50%;" />



##### 实现方式

```
public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            // Do work that doesn't write to the Response.
            await next.Invoke();
            // Do logging or other work that doesn't write to the Response.
        });

        app.Run(async context =>
        {
            await context.Response.WriteAsync("Hello from 2nd delegate.");
        });
    }
```

```

public class RequestUserCheckMiddleware   
 {            
    private readonly RequestDelegate _next;            
    public RequestUserCheckMiddleware(RequestDelegate next)           
     {                    
	      _next = next;           
     }           
     public async Task InvokeAsync(HttpContext context)           
     {                    
         string userName = context.Request.Query["UserName"];                    
         if(!string.IsNullOrEmpty(userName)&&userName.ToLower().Contains("admin"))                     
        {                             
            await context.Response.WriteAsync("用户名：" + userName);                            
            await _next(context);                     
        }                     
        else                     
        {                             
            await context.Response.WriteAsync("非法用户");                     
        }             
    }    

---------------------------------

public static class RequestUserCheckExtensions    
{                   
  
 public static IApplicationBuilder UseRequestUserCheck(this IApplicationBuilder builder)    {                      
	return builder.UseMiddleware<RequestUserCheckMiddleware>();             
 }     

------------------------------------

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)   
{              
app.UseRequestUserCheck();//用户检查     
app.Run(async (context) =>             
{                   
await context.Response.WriteAsync($"Hello {CultureInfo.CurrentCulture.DisplayName}");             
});    

```

#### AOP

##### 概念

```
面向切面编程
在面向对象的开发中,有一些不可避免的通用流程，比如验证，日志等，将这些过程提取出来作为切面，降低程序耦合。
```

##### 静态代理

```
通过装饰器模式或代理模式，给实现类外部套一层装饰器/代理类，在装饰器，代理类中实现切面程序。
```

```
IService接口声明方法
Service实现方法
装饰器模式：Proxy作为中间类继承接口,构造器中传入IService:Service,实现中完成AOP内容，再实现Service的方法
代理模式：构造函数不传参，内部写死Service,外部无法得知代理的内容
Main中实例化Service,再实例化Proxy,通过Proxy实现Service中的方法
```

##### 动态代理

```
通过代码织入  PostSharp
通过反射 MVC过滤器 Autofac等
https://www.jb51.net/article/241358.htm
```

##### Filter

###### 默认的过滤器

```
IAuthorizationFilter 验证过滤器
IResourceFilter 资源过滤器
IActionFilter
IResultFilter 
IExceptionFilter 

顺序 IAuthorizationFilter->IResourceFilter->IExceptionFilter->IActionFilter->IResultFilter
```

###### 实现方式

```
写一个过滤器，继承ActionFilterAttribute,重写方法

1.在MVC的中间件中addFilter(sizeof(myfilter))
2.在控制器或方法上添加特性
```



```
IAuthentication 认证  IDentity          有没有登录
IAuthorization  授权  IPrincipal        授予什么权限     授权 无账户信息则跳转到登录页面
```

```
作用：
	·用户验证：是否登录，是否有权限
	·防盗链(获取其他服务商的功能为己用)，防爬虫
	·语言版本的切换,本地化和国际换化
	·权限管理中的动态Action
	·决策输出缓存
```









#### AutoFac

使用方案

https://blog.csdn.net/WuLex/article/details/115556834


1.nuget安装autofac
2.programs中增加方法代替默认ioc

```
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
    .UseServiceProviderFactory(new AutofacServiceProviderFactory())  //用Autofac代替默认ioc
    .ConfigureWebHostDefaults(webBuilder =>
    {
    webBuilder.UseStartup<Startup>();
});
```

3.startup中增加方法 由programs中新增的方法自动调用

```
public void ConfigureContainer(ContainerBuilder containerBuilder)
{
    //单个注册 有问题？

    //containerBuilder.RegisterType<AutofacService>()
    //	.As<IAutofacService>().InstancePerLifetimeScope().AsImplementedInterfaces();
    //var container = containerBuilder.Build();
    //IAutofacService autofacService = container.Resolve<IAutofacService>();
    //autofacService.SayHello();


    //通过反射批量
    //Assembly service = Assembly.Load("Service");  //获取Service类库中的内容
    //containerBuilder.RegisterAssemblyTypes(service)
    //    //.Where(t=>t.Name.EndsWith("Service"))  //当一个接口有多个调用时，用于过滤
    //    .AsImplementedInterfaces()
    //    .InstancePerDependency();


    //通过一个module将当前代码抽离
    containerBuilder.RegisterModule<AutofacModule>();
}
```

4.startup中的注册逻辑可以放到一个module中

```
public class AutofacModule:Autofac.Module  //继承
{
    //重写load方法
    protected override void Load(ContainerBuilder builder)
    {
    //通过反射
    Assembly service = Assembly.Load("Service");  //获取Service类库中的内容


    builder.RegisterAssemblyTypes(service)
    .Where(t => t.Name.EndsWith("Service"))  //当一个接口有多个调用时，用于过滤
    .AsImplementedInterfaces()
    .InstancePerDependency();
    }
}
```

5.批量操作时反射的Service放在单独的类库中，当前的项目引用类库

6.Controller中使用构造器注入依赖



#### IoC

##### 注入方式

```
构造函数注入
属性注入
方法注入
.net core 内置的IServiceCollection只支持构造函数注入
```

##### 生命周期

```
service.AddSingleton(IService,Service);   //单例
service.AddTransient(IService,Service);   //瞬时 
service.AddScoped(IService,Service);      //作用域
```



