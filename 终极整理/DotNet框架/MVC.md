#### 处理流程

```
global.asax   application_start() 注册路由 
App_Start 中的RouterConfig 中配置路由
应用启动时获取RouteData以及MVCRouteHandler

http请求传入时,后台获取HttpContext  
URLRoutingModule将httpContext和RonteData封装成RequestContext
MVCRouteHandle根据RequestContext构造MVCHandler
MVCHandler继承IHttpHandler
MVCHandler创建Controller 执行方法 
```

```
应用启动时根据路由配置获取RouteData
MVCRouteHandler类继承IRouteHandler接口
UrlRoutingModule捕获http请求，将httpcontext和routedata封装成requestcontext
mvcroutehandler类根据requestcontext生成mvchandler     mvchandler继承IHttphandler接口 
MvcHandler.ProcessRequest() mvchandler以工厂模式获取控制器,执行方法
```





#### 数据验证

服务端验证

```c#
Model
[Required]
[StringLength(20,MinimumLength=4,ErrorMessage="长度必须在6-20")]
[Compare("Password",ErrorMessage="")]
[RegularExpression(@"[A-Z0-9a-z]+@[A-Za-z]+\.[a-z]{2,4}")]    //正则表达式
[Range(1,100)]
[Remote("MethodName","ControllerName")] 远程验证

Controller 
if(ModelState.IsValid)
else 操作ModelState 

View
@Html.ValidationMessageFor(model=>model.Field)
@Html.ValidationSummary(true)
```

```
stringlength
required
regularexpression
range
remote

```





客户端验证 （JQuery）

```
JQuery验证 
jquery.validate.js

通过class进行规则指定
<input class="required email" id="myemail" name="myemail"></input>

$(document).ready(function(){
 	$("form").validate();
 	
 	$("form").validate({
               rules   :{
                   name        :{required: true},
                   birhthDate  :{required: true, date: true},
                   blogAddress :{url: true},
                   emailAddress:{required: true, email: true}
               },
    
               messages: {
                   name        :{ required: "请输入姓名" },
                   birhthDate  :{required: "请输入出生日期", date: "请输入一个合法的日期"},
                   blogAddress :{ url: "请输入一个合法的URL" },
                   emailAddress:{required: "请输入Email地址", email: "请输入一个合法的Email地址"}
               }           
           });
})
```

#### 视图

##### 页面构成

![image-20220428132033324](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220428132033324.png)

```
1.分布页
	@Html.Partial(partial_view_url);
	@Html.Action
	ajax获取 
	
2.母版页  
	@{Layout = "~/Views/Shared/_Layout.cshtml";}
	@RenderBody()
	
3.Section 
	@section Header {
        <div>
            我会出现在布局页中的指定位置
        </div>
	}
	@RenderSection("Header", false)

```



##### 页面入口

```
_ViewStart.cshtml
```



#### 页面传值

对象

```
1.Cookie 存储在客户端  安全性低
2.Session 存储在服务端   有失效风险
3.Request.QueryString  跟随URL 
4.Service.Transfer 
5.Application   相当于全局变量
```



TempData/ViewBag/ViewData

```
TempData保存在Session中,获取值时从Session删除,因此只能获取一次
ViewBag和ViewData只在当前Action有效 
ViewBag基于ViewData,是ViewData的扩展，比ViewData慢.存dynamic类型数据，不需要类型转换
ViewData存key/value 需要类型转换

ViewBag.Name = 'Pthero'
ViewData['Name'] = 'Pthero'
TempData['Name'] = 'Hello'
```

Response.Redirect与Server.Transfer

```
1.Service.Transfer   参数不可见
2.Response.Redirect  数据可见，立即中断当前页面的生命周期  url输入到浏览器

Server.Transfer仅是服务器中控制权的转向，在客户端浏览器地址栏中不会显示出转向后的地址；Response.Redirect则是完全的跳转，浏览器将会得到跳转的地址，并重新发送请求链接。这样，从浏览器的地址栏中可以看到跳转后的链接地址。

Server.Transfer是服务器请求资源，服务器直接访问目标地址的URL，把那个URL的响应内容读取过来，然后把这些内容再发给浏览器，浏览器根本不知道服务器发送的内容是从哪儿来的，所以它的地址栏中还是原来的地址。 这个过程中浏览器和Web服务器之间经过了一次交互。

   Response.Redirect就是服务端根据逻辑,发送一个状态码,告诉浏览器重新去请求那个地址，一般来说浏览器会用刚才请求的所有参数重新请求。这个过程中浏览器和Web服务器之间经过了两次交互。

Server.Transfer不可以转向外部网站，而Response.Redirect可以。
```



#### 控制器

##### 返回值

```
ActionResult 基类
    ViewResult
    PartialViewResult
    EmptyResult
    RedirectResult  执行一个HTTP转向到指定的URL
    RedirectToRouteResult   执行一个HTTP转向到一个URL，这个URL由基于路由数据的路由引擎来决定
    JsonResult
    JavaScriptResult   返回一段Javascript代码，它可以在客户端执行
    ContentResult
    FileContentResult  返回一个文件到客户端
    FileStreamResult
    FilePathResult  返回一个文件到客户端
    
```



#### 路由

##### Core

![image-20220505140547794](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220505140547794.png)

![image-20220505140604265](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220505140604265.png)

#### Cors跨域

![image-20220505145836384](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220505145836384.png)



#### 返回特定http状态

```
 using system.net.http
 
 actionContext.Response = new HttpResponseMessage
 {
 	StatusCode = System.Net.HttpStatusCode.InternalServerError,   //enum  
 	Content = new StringContent(JsonConvert.SerializeObject(new APIModel.baseModel.ErrorMessage() { Message = str, Result = false }), System.Text.Encoding.UTF8, "text/plain")
 };
```

![image-20220508190142320](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220508190142320.png)