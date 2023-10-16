#### 局域网

```
通过交换机和路由器将计算机连在一起,交换机工作在链路层,用来传输本网络上的数据帧,路由器工作在网络层，连接不同网络
```

#### 网络结构

##### 协议

![image-20220414154939825](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220414154939825.png)

##### IP与MAC

![image-20220414160854551](file://C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220414160854551.png?lastModify=1653998386)

```
IP地址   32位 
MAC地址   48位
端口号用来标识 进程
```

#### TCP/UDP

##### 差别

```
tcp和udp都是计算机网络传输层的协议
1.TCP连接式  UDP非连接式
2.TCP面向字节流,安全完整；UDP面向报文，不一定完整但速度快
3.TCP经历三次握手,UDP直接传递
4.TCP点到点，UDP可以一对多，多对多通信
5.TCP首部开销20字节，UDP8字节
```

![image-20220414094612934](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220414094612934.png)

##### 三次握手

```
SYN  同步序号
ACK  应答域有效
SEQ 第一个数据字节在数据流中的序号
```

![image-20220414201441912](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220414201441912.png)

![image-20220414201453436](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220414201453436.png)

#### HTTP

##### Http请求

```
1.请求行
	method:post get put delete
	request-URL:
	http-version: HTTP/1.1
2.请求头
	Accept:可以接受的MIME文件格式
	User-Agent:客户浏览器名称
	Accept-Language:可接受的语言
	connection:是否可以维持固定的连接 keep-alive(HTTP/1.1)  http无连接
	Cookie:
	Referer:产生请求的网页地址
	Accept-Encoding:可接受的编码格式
	
	Content-Type:请求的报文体的格式
	Content-Length:post的数据长度 
		
3.请求体
	
```

##### 请求的方法

![image-20220414161707066](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220414161707066.png)

##### Http响应

```
1.状态行 
	HTTP/1.1 200 OK
2.响应头
	Server:服务器信息
	Date:日期
	Content-Type:内容类型  text/html
	Content-Length:112
3.响应正文 
	返回的页面或数据内容
```

##### 响应状态码

```
1XX  请求接受,处理中
2XX  成功 
3XX  重定向
4XX  客户端错误      400语法错误  403认证失败   404资源未找到
5XX  服务器错误      500 服务器内部错误
```

![image-20220414161723236](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220414161723236.png)

##### POST和GET

```
本质都是TCP链接
Get请求 header data一起发   Post  先发header  接受后再发data


GET 
	承载的内容少 2K
	内容通过URL显示在外部 不安全
	一般用于获取数据
	第三次握手传递数据 返回200
POST
	承载的内容多  IIS 4 80K IIS 5 100K    64k
	内容存放在请求体中 安全
	一般用于提交数据
	第三次握手传递请求头 返回100 再发送请求体 返回200
```

##### ajax

```
1、不需要插件
2、不需要刷新页面就可以更新数据   
3、减轻服务器和带宽负担
```

```
const xhr = new XMLHTTPRequest();
xhr.open("post/get","url",是否异步);   
xhr.send();
	1.get更快,post没有数据限制,post更稳定可靠
	2.传参 
		get通过url拼接传参     url= "./text.php?a=10&b=20"
		post通过send()传参     xhr.send('a=10&b=20');
		
	3.默认true表示异步
	4.状态
        readyState === 0 : 表示未初始化完成，也就是 open 方法还没有执行 
        readyState === 1 : 表示配置信息已经完成，也就是执行完 open 之后 
        readyState === 2 : 表示 send 方法已经执行完成
        readyState === 3 : 表示正在解析响应内容
        readyState === 4 : 表示响应内容已经解析完毕，可以在客户端使用了
        status =>   200~299    http状态码
        
        if(xhr.readyState == 4){
        	if(xhr.status == 200){
        		if(success){
        			success(xhr.responseText);
        		}
        	}else{
        		if(error){
        			error("Error：" + xhr.status);
        		}
        	}
        }


let request = new XMLHttpRequest();
    request.open('GET', cssUrl, true);
    request.send(null);
    request.onreadystatechange = () => {
      if (request.readyState === 4 && request.status === 200) {
        let type = request.getResponseHeader('Content-Type');
        if (type.indexOf('text') !== 1) {
          this.listModule = this.createModule(tpl, request.responseText);
        }
      }

    };
```



#### HTTPS

##### 构成

```
HTTPS=HTTP+TLS/SSL

HTTP构成：
	request：统一资源定位符URL,协议版本号，MIME信息(请求修饰符+客户机信息+许可内容)
	response:协议版本号，MIME信息（服务器信息+实体信息）
TLS:传输层安全性协议
SSL:安全套接层 加密
```

##### http和https的区别

![image-20220417214414690](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220417214414690.png)

#### JWT(Json Web Token)

```
用于用户身份验证
Header 标头
PlayLoad 有效荷载
Signature 签名

base64加密
```

![](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220417145224251.png)

#### 加密

##### 对称加密

```
验证服务器和接口存同一个key   客户端通过验证服务器获得加密Token，提交请求给接口时，key解密 
```

##### 非对称加密

```
验证服务器和接口存不同key ，两个key为同组，（私钥公钥），无法相互解析，私钥加密，公钥解密
RSA算法
```

##### 散列函数加密

```
MD5 SHA256
```

#### Socket编程

```
socket是一种连接模式，是一个调用接口，对http/ip进行封装

socket  车站
ip  公路
tcp/udp  车
http 货物
```

