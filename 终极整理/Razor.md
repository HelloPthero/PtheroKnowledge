概念

```
和MVC一起使用的 视图模板引擎  cshtml
```

作用域

```
@{


}
```

应用

```
@Href("~/")  根目录

@Html.Raw('<font color='red'>红字</font>') 输出html

@RenderBody()  母板页面填充内容

@{
    Layout = "/LayoutPage.cshtml";
    Page.Title = "测试页面哦";
}                                            内容页绑定母版

@RaderPage("/xxx.cshtml")       //页面中填充page页


@RenderSection("SubMenu")   母版页中使用section

@section SubMenu{                                              
    Hello This is a section implement in About View.
 }                                                              
```

```
@Html.ActionLink("合同台帐", "ContractAccount", "Default")        页面跳转
```





