#### 概念

```
abp是asp.net core的一套样板工程，通过领域驱动设计对功能进行了模块化的封装
基于ASP.NET CORE,DDD领域驱动设计的样板工程。封装了开发过程中高概率用到的包，提高项目开发的效率。
```

#### 层

```
表现层 应用服务层 领域层  基础设施层
Entities Repositories Dtos(数据传输对象)  Application_Services
```

#### 特点

```
多租户，数据隔离
软删除
多语言
通过Application Service自动创建Web api
通过nswag自动创建前端的proxy代理层
日志
统一异常处理
```

#### DDD

```
DP  DTO  对传输对象进行封装，验证放在其中，降低耦合
业务区分  将不同业务方法放到不同服务中，业务之间相互调用
Domain层定义了仓储的接口 EFCore层进行实现
```

