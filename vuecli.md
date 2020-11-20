# Vue Cli

##### webpack是啥？

webpack是一个项目打包的方式，可以直接导出编译好的前端项目源码，拿到项目源码了就可以把前端页面部署到服务器啦



##### npm配置淘宝镜像

```makedown
### 用 npm config registry 设置npm的镜像
npm config registry https://registry.npm.taobao.org

### 检验是否设置成功
npm config get registry 
```



##### npm设置全局加载依赖的目录

```makedown
### 设置npm的全局下载目录
npm config set prefix "D:/nodesettings/npm-global"

### 设置npm的缓存目录 用于对远程仓库的缓存
npm config set cache "D/ndoesettings/npm-cache"
```



`在vue中：一切皆组件`

组件App是根组件，挂载跟实例中，根实例在下面

```html
<div id="app">
    
</div>
```

但是因为挂载在根实例上的Vue对象是有模板的（template）的，所以就算这个实例框框中写了东西，也都会被Vue对象的模板替换。



因此App组件中可以进行组件式的开发，把vue组件比如导航栏，页脚这些组件写好，染好导入到App这种比较大的组件

还有一个是组件<router-view>，这是vue生态中提供的路由机制，可以这样理解，它就是快递分拨中心， 它会根据浏览器的地址栏中的不同的url，给出在<router-view>中应该显示的内容，在项目的route文件目录下有一个index.js文件，就在这里面设置不同的url应该显示哪些不同的组件，打个比方，比如`localhost:8080/#/url1`我设成<router-view>这个地方显示组件a，`localhost:8080/#/url2`这个url下我设置成显示组件b，我认为vue的这个路由机制很好的一点就是它的鲁棒性，因为写url的时候是很容易出错的，只要有任何一个字母写错那么在地址栏中就会出问题，可能会404 not found， 这其实就很烦。但是vue路由中的`#`它就是保证了所有在`#`前面的它被视为是“无效的”，不是真的无效，只是为结果没有影响的意思相当于如果出现`localhost:8080/xxx#`在浏览器的地址栏中了，它和`localhost:8080/#`是等同的，它的好处如下

##### 试想你想在地址栏中请求的是`localhost:8080/xxx`

##### 但是其实在项目加上了`xxx`的url并不是一个有效的资源定位地址，所以其实你是啥都找不到

##### 找不到就会返回一个 404 Not Found的报错 （看到报错心情就会不好嘻嘻嘻）

##### 但vue的路由机制会发现找不到你请求的url，然后在你写的`localhost:8080/xxx`后面加上一个`#`，因此url地址变成了`localhost:8080/xxx#` 

##### `#`前面的一小段就相当于没写 也就是`localhost:8080/xxx#`相当于`localhost:8080/#`所以此时就回到了项目的初始url，这里默认展示的是App组件中全部内容啊，所以是有页面的

##### 这样就避免了报404 Not Found的错误



#### 路由和子路由在声明的时候不要写`/`，例子如下

```javascript
new Router({
    routes: [
        {
            path: "/user", 
        	component: User,
            children: [
                {path: "add", component: Useradd}
            ]
        }
    ]
})
```

`这样用url是***/user的时候会定位的User组件，url为***/user/add的时候会定位到Useradd组件`

