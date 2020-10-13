###### 可以这样想象，你有一个html页面是来自于某个服务器，如果这个html文档里任何时候代码中的vue对象发生改变了的话，整个页面中vue出现之处都会被响应式的渲染更新



###### v-on是为某一个事件添加了一个事件监听器，所以重点在 

1.这个事件是什么 

2.这个事件发生后用什么动作来处理

v-on.click = "fun1"

用.点出监听的事件，等号后面写动作函数的函数名



##### $是vue实例暴露出来的一些property和实例方法

```javascript
var app = new Vue({
    el: "#app",
    data: {},
    
})

app.$watch("a", function(para1) {
    //这是property a的监视器 a被改变时 这个函数就将被调用
    //property指的是vue响应系统中的property 每次创建vue实例的时候data对象中的property就会被加入
})
```



##### vue property中的computed属性和data属性

data属性里面放的是很一般的数据

computed属性中放有一些复杂逻辑的数据 这些数据经常是与将data中的数据做一些处理 然后return 这也符合computed 计算属性的名称意义

##### 写在methods中的方法完全可以执行到和computed属性一样的效果  但是computed属性中的数据响应式依赖是建立在data属性中的 只要是data中的数据不发生改变 那么computed属性中的数据完全就相当于用的缓存数据 而如果在模板表达式里面直接调用的是方法的话 它每次都要把方法执行一遍 与computed属性中的方法相比 computed属性中的方法只执行了一遍。

当然 如果computed属性中的数据跟data属性中的数据木有关系了 那么computed属性中的数据就只剩响应 没有依赖了  跟data属性中的数据就没啥区别了 而且还不能在方法中调用着改  好惨 因为它只在vue实例生成的时候创建的时候执行一次 而不会在每次在模板中调用的时候都执行一遍computed属性中js函数 真的很惨诶

```javascript
<div id="app">
    {{ res }}
	{{ res }}
	{{ res }}
	{{ resm() }}
	{{ resm() }}
	{{ resm() }}
	
	<script>
        var app = new Vue({
            el: "#app",
            data: {
                
            },
            computed: {
                res: function() {
                    return Math.random()
                }
            },
            methods: {
                resm: function() {
                    return Math.random()
                }
            }
        })
    </script>
</div>
```

![image-20200924203934190](C:\Users\lsdyi\AppData\Roaming\Typora\typora-user-images\image-20200924203934190.png)

上面可以看书前3行的结果是调用的computed属性中的数据res 但是3个的结果是一样的  这是因为现在的res只有响应没有和任何data中的数据依赖起来 所以res的结果只在最开始的时候计算了一次也就是调用了方法一次 因为computed属性中的数据有缓存机制 它的依赖对象如果没有发生改变或者没有任何依赖对象的时候 它就会一直去用它缓存的值 而不会说去每次在调用res的时候都去重新调用响应的方法

但是后3行的结果是不同的 这是因为它们都是调的resm()方法 每一次调都会产生一个新的结果 它没有缓存机制 所以每次用的话都要重新调用

#### 所以综上 computed属性跟methods中的方法最大区别就是computed属性中的数据有缓存机制

而且 computed属性中的数据对data属性中数据的依赖不是假依赖而是真依赖 一定是要return就是return的与data中数据有关的情况才算得上是依赖 例如下面的情况其实依赖是没有形成的

```javascript
<script>
    new Vue({
    	el: "#app",
    	data: {
            fake: 0,
        },
    	
    	//此情况下依赖是没有形成的
    	computed: {
            resm: function() {
                console.log(this.fake)
                return Math.random()
            }
        }
})
</script>


//依赖是形成了的
computed: {
    resm: function() {
        return this.fake+Math.random()
    }
}
```

##### 另外在computed属性中默认写的是一种get方法，即返回computed属性中的数据，其实还可以写一种set方法，这样可以设置computed属性中的方法，这样computed属性中的数据不仅可以有data属性中的数据决定，也可以由用户直接设置了 如下

```javascript
<script>
    var app = new Vue({
        el: "#app",
        data: {
            firstname: "Zinger",
            lastname: "Young",
        },
        
        computed: {
            lastname: {
                get: function() {
                    return this.firstname+" "+this.lastname
                },
                
                set: function(value) {
            		var temp = value.split(" ")
        			this.firstname = temp[0]
                    // 写错了 js中不是像python -1 表示最后一个元素 
                    //this.lastname = temp[-1]
        			//写成下面的形式
                    this.lastname = temp[temp.length-1]
                }
            }
            
        }
            
    })
</script>
```



#### v-bind的值不仅可以是字符串，还可以是对象或者数组



##### 下面是值为对象的例子

只要在标签的属性上运用了v-bind命令，那么属性的值就可以用js表达式了，这样js表达式就不止用于模板语法中

```javascript
<div id="app4">
	<div :class="{active: isActive, positive: isPositive(), comp: isComputed}">
            
    </div>
        
    <script>
        var app4 = new Vue({
            el: "#app4",
            data: {
                isActive: true,
            },

            computed: {
                isComputed: function() {
                    return this.isActive && this.isPositive()
                }
            },

            methods: {
                isPositive: function() {
                    return true
                }
            }
        })
    </script>
</div>
```



![image-20200926183502010](C:\Users\lsdyi\AppData\Roaming\Typora\typora-user-images\image-20200926183502010.png)

active直接来自data中的property，positive是methods中的方法，com来自computed属性中的值与data中的数据的methods中的方法有关



将active对应的isActive改为false

![image-20200926183638725](C:\Users\lsdyi\AppData\Roaming\Typora\typora-user-images\image-20200926183638725.png)

结果

![image-20200926183723008](C:\Users\lsdyi\AppData\Roaming\Typora\typora-user-images\image-20200926183723008.png)



##### 下面是值为数组的例子

会把数组的值全部渲染到v-bind所绑定的属性中

```javascript
<div id="app5">
    <div :id="[data1, data2]">
    
    </div>

	<script>
        var app5 = new Vue({
            el: "#app5",
            data: {
                data1: "d1",
                date2: "d2",
            }
        })
    </script>
</div>
```

![image-20200926223027456](C:\Users\lsdyi\AppData\Roaming\Typora\typora-user-images\image-20200926223027456.png)

数组作为value的时候会把value用英文逗号拼接起来





#### v-if会最大效率的复用你的元素

```javascript
<template v-if="choice == 'username'">
    <label>Username</label>
	<input placeholder="Enter your username">
</template>
<template v-else>
    <label>Email</label>
	<input placeholder="Enter your email">
</template>
```

如果if不执行在else块里它也只是把input标签的placeholder换掉了 因为它检测到else块里面也有input标签 所以它不会重新渲染input标签 所以换掉了input.placeholder 而没有换掉 input.value

##### 但是 在我的实验中 v-if对于元素复用的条件是非常苛刻的，假设现在有两个v-if 一个为条件1 一个为条件2 预先设定条件1满足 则第一个元素现在显示  如果现在条件1变为不满足 条件2变为满足 则第二个元素可以复用第一个元素 这就是上面的代码的情形 但其余的情况是可能条件1变为不满足 条件2还是不满足 这样两个元素都不会显示 然后再让条件2或者说更大方一点让条件1满足的话 新出现的元素没办法再复用前面第一个元素 

```javascript
<div id="app">
    <template v-if="choice == 'username'">
    	<label>Username</label>
		<input placeholder="Enter your username">
	</template>

	<template v-if="choice == 'email'">
    	<label>Email</label>
		<input placeholder="Enter your email">
	</template>
</div>

<script>
	var app = new Vue({
        el: "#app",
        data: {
            choice: "username",
        }
    })
</script>
```

代码渲染成页面如下 

![image-20200927200621749](C:\Users\lsdyi\AppData\Roaming\Typora\typora-user-images\image-20200927200621749.png)

如果我现在在控制台中将choice的值改成email，那么vue为了效率会进行元素的复用，只将input元素的placeholder属性改做email，value属性是不变的 ，元素复用：避免了删除这个元素又再创建一个跟这个元素没啥差别的新元素啊

结果如下

![image-20200927200928800](C:\Users\lsdyi\AppData\Roaming\Typora\typora-user-images\image-20200927200928800.png)

但是如果在控制台把choice的值赋为一个不满足任何一个v-if指令的话 例如

![image-20200927201028730](C:\Users\lsdyi\AppData\Roaming\Typora\typora-user-images\image-20200927201028730.png)

则没什么好下场！！！！！！现在再把choice的值改为username或者email就没有任何元素复用存在了。

比如刚才因为我choice的值从email变成了dsadsa了，因为不满足v-if指令了，元素不显示了额， 现在我再把choice值改回来改为email，结果是

![image-20200927201310852](C:\Users\lsdyi\AppData\Roaming\Typora\typora-user-images\image-20200927201310852.png)

value的值就被清空了 说明这个元素没有复用，是重新生成的



### 究其原理，就是v-if指令和v-show指令的差别，v-if指令如果不满足条件的话会把元素直接从DOM树中移除（DOM树就是html文档每个标签嵌套出来的树结构），但是vue多聪明啊，它做了一些代码优化，让你在不满足条件1但满足其余的条件的时候，能不着急着把元素从DOM树中移除，让满足条件的元素复用一下前面的元素，但是如果它发现条件1不满足，其余的条件也不满足，那么没办法啦，只能将元素从DOM树中移除了，那你后面要又满足了某个条件了，只能重新再生成这个元素了，以前的元素呢已经移除了就没办法复用了。

### v-show呢和v-if用法一样，唯一的不一样在于v-show不会把元素从DOM中移除，而是改变了元素的style样式，让它不显示 style="display: none" 因为没有把它从DOM树中移除 所以无论何时这个元素满足了条件再出现的时候 它的placeholder,value一切的一切属性和值都不会改





#### v-for的传统使用方法是遍历一个数组，并且常常这是一个对象数组，也就是数组的元素是对象

#### v-for还可以遍历对象中的属性 直接v-for="pro in object" 就可以遍历出属性的值  更鲁棒的操作是加上属性名或者是该属性的下标 这真是一个蛇皮的操作

```javascript
    <div id="app1">
        <ul>
            <li v-for="pro,keyname,index2 in object">
                {{ index2+1 }} {{ keyname }}: {{ pro }} 
            </li>
        </ul>
    </div>

    <script>
        var app1 = new Vue({
            el: "#app1",
            data: {
                object: {
                    d1:"123",
                    d2:"hello world",
                    d3:"Zinger Young"
                }
            }
        })
    </script>
```

pro,keyname,index2 顺序一定是第一个是值 第二个是键 第三个是索引（从0开始）



#### v-on







#### 每个组件就是一个Vue实例



组件开发的优点：

开发效率高

可维护性高



根实例

##### 组件实例

组件实例中的props属性中的数据就相当于data属性中的数据，都是vue实例的property，在作用于内可以直接被访问

每个组件的模板中都只能有一个根元素

把所有的要加入的props放到一个对象中 然后整体传给组件

组件间的通信 父组件只能收到子组件的触发事件 而不能收到孙子组件的触发事件



##### 全局组件和局部组件的区别在于全局组件是在Vue系统中注册 局部组件就是一个js对象， 全局组件和局部组件的注册都应该放在Vue根组件之前



给组件传值的时候无论是静态的还是动态的 都需要用v-bind指令进行绑定



##### 表单双向绑定的实现原理

用的是一个v-bind和v-on指令的结合，v-bind指令使在input标签的属性中也可以用vue的property，v-on使得在触发输入事件了能够同时改变和v-bind结合使用的属性值（vue实例的property）





组件就是一个特殊的Vue实例，区别在于一般的Vue实例中没有写template这一property，如果Vue实例中的有了template的话那么挂载Vue实例的根元素会被替换成template中的内容



Component template should contain exactly one root element

一个组件就是一个Vue实例，new Vue()出来的也其实是一个组件，只是一般的没有写它的template

js的变量一般是使用驼峰命名法，但是哦， 在html标签中的任何代码是不区分大小写的啊，所以在html代码中用短横线命名法，可以直接被解析成驼峰命名法



不加v-bind的直接把值传递给子组件中的props的话，这个值的数据类型将是string，加了v-bind的话，要传两层字符串才会被js解析成真正的字符串

#### 因此这里又深刻得体会到了v-bind指令的作用，v-bind指令不仅仅只是让Vue实例中的property能够在html标签的属性中出现，更深入一点的理解是，是为了让属性的值被当做js表达式来解析



#### slot的作用就相当于将子组件中的<slot></slot>标签全部用父组件中子组件标签中的内容替换



一定要加;



this.count++



子组件不应该修改props中的值



驼峰写法和短线写法针对是父组件中的html代码 这个html还是不包括在template中的



html组件命名规则 首字母小写 html内置的组件全部都是小写 自定义组件中间分大小写 大写的话可以用-小写来表示



js中数字串-0就等于这个串的真值



注意 不要对任何props用v-model指令



v-model实现原理，用v-bind指令让属性value的值可以是Vue实例的property，然后在<input>标签上监听input输入事件，



#### 动态组件就是用一个标签 这个标签是啥呢 就是<component :is="choice"></component> 并且这个标签有一个属性叫做is，这样它就会根据is中的js表达式，解析出应该显示的组件

##### 如果要在组件在切换的过程中仍然保活的话，用<keep-alive></keep-alive>标签把<component> 标签括起来



##### 异步组件和同步组件都是在声明组件的时候用到的