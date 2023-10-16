#### 结构

```
var app = new VUE({
	el:"#app",   //元素
	data:{    //参数
	},
	computed:{   //计算属性  有缓存  基于data  data变computed自动变
	},
	methods:{    //方法
	},
	match:{      //监听
	}
})
```

#### 钩子函数(生命周期)

```
beforeCreate
created
beforeMount
mounted
beforeUpdate
updated
beforeDestory
destoryed

放置在vue定义中
var app = new Vue({
        el:"#app",
        //钩子函数created,该方法在页面显示之后,自动执行
        created() {
            console.log("created...");
        }
    });
```

#### 数据方式

##### 插值表达式

```
{{vue_data}}
```

##### 数据显示

```
<span v-text="vue_data"></span>
<span v-html="vue_data"></span>
//防止插值闪烁
```

##### 双向绑定

```
//一般用在表单中
v-model:value=""
v-model:checked="checkflag"

选择框绑定:  
单选绑定值 e为单个值
<input type="radio"  value="1" v-model="e" />1
<input type="radio"  value="2" v-model="e" />2
e与vulue值相等时选中

多选框绑定  e为数组
<input type="checkbox" id="jack1" value='a' v-model="e"/>1
<input type="checkbox" id="jack2" value='b' v-model="e"/>2
<input type="checkbox" id="jack3" value='c' v-model="e"/>3
e=['a','b']
```

##### 数据绑定

```
v-bind:标签名
:标签名
```

##### 事件绑定

```
v-on:eventname="vue_method"
@eventname="vue_method"
```

```
事件修饰符
@事件.修饰符 = "vue事件"

1.stop 阻止冒泡,只在当前元素发生事件
2.prevent  阻止默认事件
3.capture  使用事件的捕获模式
4.self   只有元素自身触发事件才执行
5.once  只执行一次
```

##### 循环

```
//遍历数组  
v-for="item in items"
v-for="(item,index) in items"  //index从0开始

//遍历对象  
v-for="value in object"
v-for="(value,key) in object"
v-for="(value,key,index) in object"
```

```
<li v-for="name in list" :key="name"></li>
key保证并发下数据遍历顺序
```

```
避免v-if和v-for在同一元素节点中使用  vue3中v-if优先级比较高，vue2中相反
```



##### 判断

```
v-if="bool_value"    
v-show="bool_value"  //元素仍然存在 只是不显示  DOM dispaly='none'

<div v-if="Type == A">
</div>
<div v-else-if="Type == B">
</div> 
<div v-else-if="Type == C">
</div>
<div v-else>
	Not A/B/C
</div>
```

##### 页面跳转

```
1.通过标签
<router-link
	:to="{name:'',params:{key:value}}"
	:class="{'flex-item-1':'flex-item-1'}"
	href="javascript:"
             >
	<span class="tabNav-txt">XXX</span> 
</router-link>
```

```
2.通过js
toIndex:function(){
	this.$router.push("/?entityId="+localStorage.getItem("entityId"));
}
```

#### 模块

##### computed 计算属性

```
可以当作一种特殊的值在{{}}中使用，自动根据值变化获取

<div id="app">
    <h1>{{birth}}</h1>
    <h1 v-text="birth"></h1>
    <h1 v-html="birth"></h1>
</div>
<script type="text/javascript">
    var app = new Vue({
        el:"#app",
        computed:{
            //定义一个birth方法,该方法就是一个计算属性,可以在插值表达式中使用
            birth(){
                let date = new Date();
                let year = date.getFullYear();
                let month = date.getMonth()+1;
                let day = date.getDay();
                return year + "-" + month + "-" + day;
            }
        }
    });
</script>
```

##### match 监听属性

```
监听简单属性值变化并进行操作

var app = new Vue({
    el:"#app",
    data:{
        message:"白大锅",
        person:{"name":"heima", "age":13}
    },
    //watch监听
    watch:{
        //监听message属性值,newValue代表新值,oldValue代表旧值
        message(newValue, oldValue){
        	console.log("新值：" + newValue + "；旧值：" + oldValue);
        },
        //监控person对象的值,对象的监控只能获取新值
        person: {
            //开启深度监控；监控对象中的属性值变化
            deep: true,
            //获取到对象的最新属性数据(obj代表新对象)
            handler(obj){
                console.log("name = " + obj.name + "; age=" + obj.age);
            }
        }
    }
});
```

#### 组件

##### 组件定义

```
组件定义必须放在VUE对象创建前

const countTemp = {
	template:'<button @click=\'num++\'>你点击了{{num}}次</button>'
	data(){
		return { num:0}
	}
}

var app=new Vue({
	el:"#app",
	components:{
		counter:countTemp
	}
})
```

##### 组件传值  父->子

```js
//父组件的数据传递给子组件，子组件需要通过props接收

<div id="app">
	<childcomponent :number="pnumber" :ids="pids" :obj="pobj"></childcomponent>
</div>

<script>
	var childTemp={
		template:'<h2>{{number}}-{{ids}}-{{obj}}</h2>',
		props:{
			number:"",
			ids:[],
			obj:{}
		}
	}
	
	Vue.component("childcomponent",childTemp);
	var app = new Vue({
		el:"#app",
		data:{
			pnumber:2,
			parr:[1,2,3],
			pobj:{name:'Pthero',age:25}
		}
	})
</script>
```

##### 组件传值 子->父

```js
//子组件无法传递数据到父组件，只能调用父组件的方法

<div id="app">
    <h1>父组件中:app_num={{app_num}}</h1>
    <!-- 把父组件的add方法,绑定给子组件的aaa属性,绑定方法使用@属性名="方法名" -->
    <!-- 把父组件的rem方法,绑定给子组件的bbb属性,绑定方法使用@属性名="方法名 -->
    <!-- 把父组件的app_num变量,绑定给子组件的counter_num属性,绑定变量使用:属性名="方法名 -->
    <counter @aaa="add" @bbb="rem" :counter_num="app_num"></counter>
</div>

<script>
    //定义一个组件(模版)
    let counter = {
        template: `
             <div>
                   <h1>子组件中:counter_num={{counter_num}}</h1>
                   <input type="button" @click="fun1" value="+"/>
                   <input type="button" @click="fun2" value="-"/>
            </div>
                `,
        props:{
            //定义属性counter_num,用来接收父组件传递的数据
            counter_num:null,
            //定义aaa属性,用来绑定父组件的方法,当然,该定义也可以省略
            aaa:function(){},
            //定义bbb属性,用来绑定父组件的方法,当然,该定义也可以省略
            bbb:function(){},
        },       
        methods:{
            fun1(){
                //找到aaa属性所绑定的那个方法,执行那个方法
                return this.$emit("aaa");
            },
            fun2(){
                //找到bbb属性所绑定的那个方法,执行那个方法
                return this.$emit("bbb");
            }
        }
    }

    var app = new Vue({
        el: '#app',
        data: {
            app_num: 0
        },
        components: {
            counter
        },
        methods:{
            add(){
                this.app_num++;
            },
            rem(){
                this.app_num--;
            }
        }
    });
</script>
```

#### axios

##### get

```
axios.get('url',
{
    params:{
        ID:123
    }
})
.then(function(response){
    console.log(response)
})
.catch(function(err){
    console.log(err);
})


axios.get('url/ID=123')
.then(function(response){
    console.log(response);
})
.catch(function(err){
    console.log(err);
})
```

##### post

```
axios.post('url',
    {
        name:'Pthero',
        age:25
    }
).then(res=>{
    console.log(res);
    app.user = res.data;
})
.catch(err=>{
    console.log(err);
})
```

##### 跨域

```
跨域请求需要在服务端配置开启
```

