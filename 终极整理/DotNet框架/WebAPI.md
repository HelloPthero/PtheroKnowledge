#### RESTful

```
Restful风格

POST:   新增
GET:    获取列表 获取对象
PUT:    更新全部
DELETE: 删除


基本规则：
1、可以根据请求类型判断业务类型，不需要在方法中使用update/getlist等关键字，方法中使用名词
2、使用请求状态表示业务类型
3、使用复数名词
4、使用子资源表达关系    cars/1/drivers/3
5、使用Http头声明序列化格式
	Content-Type 定义请求格式
	Accept 定义可接受的响应格式
6、为集合提供过滤，排序，选择，分页功能
	Filter:  GET /cars?color=red
	Sort:    GET /cars?sort=model
	Select:  GET /cars?fields=id,model,color
	Paging:  GET /cars?offset=10&limit=5
7、注明版本  api/v2/cars
8、Http错误包装 根据状态
	200 – OK – 一切正常
    201 – OK – 新的资源已经成功创建
    204 – OK – 资源已经成功擅长
    304 – Not Modified – 客户端使用缓存数据
    400 – Bad Request – 请求无效，需要附加细节解释如 "JSON无效"
    401 – Unauthorized – 请求需要用户验证
    403 – Forbidden – 服务器已经理解了请求，但是拒绝服务或这种请求的访问是不允许的。
    404 – Not found – 没有发现该资源
    422 – Unprocessable Entity – 只有服务器不能处理实体时使用，比如图像不能被格式化，或者重要字段丢失。
    500 – Internal Server Error – API开发者应该避免这种错误。

VS项目建立流程
	core web项目 api   
	安装EFCore包
	建model  建context  依赖注入+配置sql地址  controller基架  post-nameof  测试

postman要点
	post要配置context-type
		body中传row - > json
	put url中传id body中传model json

为api配置过滤排序等
	GETItems([FromQuery] FilterModel filter){}
	参数值通过url传  ?isfilter=true
```

#### 安全性

##### 基于Session认证

```
用户登录后在Session/Cookies中存用户信息,请求时通过验证过滤器进行查询，是否有Session数据
```

##### Token

```
用户登录时根据用户ID生成Token,Token包含用户Id和随机码,Token保存在缓存中。请求时，请求头添加token数据。
```

##### JWT

###### 构成

```
Header头部：
申明类型：jwt
申明加密算法：HMAC SHA256

Playload 有效荷载：

Signature 签名:
base64(header)+base64(playload)+256(secret)
```

![image-20220605141910707](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220605141910707.png)

![image-20220605141933250](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220605141933250.png)



##### Token+签名验证

```
1.服务器端写一个认证服务，用户登录时根据用户ID生成token并返回给客户端,服务器端保存在缓存中,访问api前先访问认证服务
2.客户端在header添加关键数据以及签名
3.签名由请求的数据+时间戳+随机数+token+用户id构成    通过MD5签名算法进行加密

4.服务器端获取关键数据,计算签名
5.判断token,签名是否相等,时间是否过期 
6.执行

https://www.cnblogs.com/MR-YY/p/5972380.html
```



#### 跨域请求

```
Cors 跨域资源共享
```

```
configure中添加中间件 AddCors();
configureService中添加服务 
```

```
services.AddCors(
	//添加策略
                options => options.AddPolicy(
                    _defaultCorsPolicyName,                                                //localhost
                    builder => builder
                    //配置源
                        .WithOrigins(
                            _appConfiguration["App:CorsOrigins"]                          //appsetting配置
                                .Split(",", StringSplitOptions.RemoveEmptyEntries)
                                .Select(o => o.RemovePostFix("/"))
                                .ToArray()
                        )
                        .AllowAnyHeader()
                        .AllowAnyMethod()
                        .AllowCredentials()
                )
            );
```

```
app.UseCors(DefaultCorsPolicyName); //Enable CORS!
```

