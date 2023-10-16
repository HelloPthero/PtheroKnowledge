#### Angular2.x

##### angularjs和angular2.x的区别

```
angualrjs mvc js
angular2.x 
	模块化组件化模式(module+component) ts
	依赖注入简单
```

##### 事件发射器 EventEmitter [ɪ'mɪtə]

```
@output() somethingChanged = new EventEmitter();

this.test.emit({
mess:’message’;
});

<child (test)=“demo($event)”></child>
demo(event){
console.log(event)
                                              ———－－－－－－－－emit发射的数据
}
```

##### 生命周期 hooks

```
ngOnInit();
ngOnDestroy();
ngAfterViewInit();
```

##### 路由

```
this.router.navigate([item.fComponentUrl]);

//路由器 Router
//通过Router.forChild()给Module添加路由
const routes: Routes = [
    {
        path: '',
        component: MenuUnitsComponent,
        data: { useCache: true, key: 'mn_menu-management' }
    },
    {
        path: 'createOrEditMenu/:menuId',
        component: CreateOrEditMenuModalComponent,
        data: { useCache: true, key: 'mn_createOrEditMenu' }
    },
];

@NgModule({
    imports: [RouterModule.forChild(routes)],
    exports: [RouterModule],
})

//路由出口
<router-outlet></router-outlet>

//激活的路由树
ActivatedRoute 可通过rooter访问路由状态
```

![image-20220508213524060](C:\Users\13550\AppData\Roaming\Typora\typora-user-images\image-20220508213524060.png)

