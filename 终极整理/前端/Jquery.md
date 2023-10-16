#### DOM转换

```
jquery对象和js DOM对象的差别
jquery对象存的是数组，数组中存js DOM对象

相互转化  
var jq = $(js);

var js = jq[0];  var js = jq.get(1);
```

#### 入口

```
$(document).ready(function(){

});

$(function(){

});
```



#### 选择器

##### 基本选择器

```
$("*")  全部
$("div")  标签选择器
$("#id")  id选择器
$(".class")  class选择器
$("A,B")  交集&&选择   满足其一即可
$("AB")   并集||选择   都要满足  
```

##### 层级选择器

```
$("A>B")  子代
$("A B")  后代
```

##### 兄弟选择器

```
$("A+B")  下一个兄弟
$("A~B")  下面所有兄弟
```

##### 属性选择器

```
$("[attr]")   包含属性attr的
$("[attr='attrvalue']")  属性Attrname = attrvalue的
$("[attr*='value']")  包含
$("[attr&='value']")  以value结尾
$("[attr^='value']")  以value开头
$("[attr!='value']")  不等于value 
```

##### 过滤选择器

```
$("A:first")  第一个
$("A:last")  最后一个
$("A:even")  偶数
$("A:odd")  奇数
$("A:eq(index)") 索引等于
$("A:lt(index)") 索引小于
$("A:gt(index)") 索引大于
$("A:not(B)")
$(":header") 标题
```

##### 后代选择器

```
selector:nth-child(index)  index从1开始
selector:first-child
selector:last-child
selector:only-child 独生子
selector:first-of-type  每类选第一个子元素
selector:last-of-type  每类选最后一个子元素
```

##### 表单过滤选择器

```
$(":input")  可输入标签
$(":text")     input type='text'
$(":checkbox") 
$(":button")
```



#### API

##### 插入操作

```
A.append(B)  B插入到A的内部结尾
A.appendTo(B)  A插入的B的内部结尾
A.prepend(B) B插入到A的内部开头
A.prepend(B)  A插入到B的内部开头
A.after(B)  B插入到A之后
A.insertAfter(B)  A插入到B之后
A.before(B)   B插入到A之前
A.insertBefore   A插入到B之前
```

##### 删除操作

```
A.remove();
A.detach();
A.detach(B);
A.empty();
```

##### 属性操作

```
A.attr(attrname)  获取属性
A.attr(attrname,attrvalue)  修改属性
A.prop() 同上
A.removeAttr(attrname) 删除
```

##### 样式操作

```
A.css(cssname) 获取样式
A.css(cssname,cssvalue)  修改样式

$().css("属性":"属性值","属性2":"属性值2");
```

##### 类操作

```
A.hasClass(classname)
addClass(classname)
removeClass(classname)
```

##### 其他

```
克隆
A.clone();
A.swap(B);每个A都包裹B
A.swapAll(B);一起包裹
```



#### Ajax

```
$.ajax({
        url: '/MXDBI/GetInvestAndProduce2',
        type: 'post',
        async: false,//使用同步的方式,true为异步方式
        data: { 'FSDate': $('#FSDate').val(), 'FEDate': $('#FEDate').val(), 'FYear': Years.substring(0, 4)/*, 'FEmpName': name  */ },//这里使用json对象
        beforeSend: function () {
          // $("#loadingModal").modal('show');
          layer.msg("数据加载中。。。", {
            icon: 16, //代表加载的图标
            time: 3,  //0秒关闭（如果不配置，默认是3秒）
            shade: 0.5
          });
        },
        success: function (data) {
          Listdata = $.parseJSON(escape2Html(data));
          GetSum("");
        },
        complete: function () {
          // $('#loadingModal').modal('hide');
        },
        fail: function () {
        }
      });
```

