# vue指令
'''javascript	

```javascript
<div id="app">
	<input type="button" value="v-on指令" v-on:click="doIt">
	<input type="button" value="v-on简写" @click="doIt">
	<input type="button" value="双击事件" @dblclick="doIt">
	<h1 @click="changeFood">{{ food }}</h1>
	<script>
		var app = new Vue({
			el: "#app",
			data: {
				food: "西兰花炒蛋",
			}
			methods: {
				doIt: function() {
					alert("做IT")
				},
				changeFood: function() {
					food += "好好吃哦加油2333"
				}
			}
		})
	</script>
</div>
```

'''
### 1、button中的value表示按钮显示的值 表单也是如此 value属性表示表单中的预设值
### 2、简写v-on指令 直接用@  但是@后面是没有：的
### 3、js中就是函数的调用需要一个事件的触发 所有要给元素的某一个时间绑定函数 如为button的click时间绑定doIt函数
### 4、双击事件是dblclick
### 5、vue的构造函数中的形参就是一个字典 然后这个字典的键el不是字典，键data和键methods都是字典
### 6、vue的理念是数据驱动页面 所以在编程的时候要思考的是如何改变数据 也就是data中的键值对 然后在methods中的函数中使用this关键字点出data中的数据





# v-show指令
'''javascript

```javascript
<div id="showandhide">
	<img src="./Figure_1.png" v-show="true">
	
	<script>
		var showandhide = new Vue({
			el: "#showandhide",
			data: {
				isShow: true,
			},
			methods: {
				
			}
		
		})
	</script>
</div>
```

'''

### 1、写url的时候一个文件后面就不能跟/ 因为/这是使表示文件夹的啦哈哈哈哈哈
### 2、没有声明vue的引用的时候任何vue格式的表示都是空谈哦 可以想象成一个vue对象在上面操纵着挂载的页面元素惹
### 3、v-show属性中的值会被解析成Boolean值 然后在vue对象的大操纵下 vue指令后面的""中可以写所有在vue对象中出现的值 比如data中的数据 或者methods中的函数
### 4、v-show和v-if的区别在于 v-if直接让该元素在DOM树中移除了 而v-show使用的是css的样式

<br><br>
# v-bind指令
'''javascript
	
```javascript
<div id="elementset">
	<img v-bind:src="imgSrc" v-bind:title="details">
	<br>
	<img v-bind:src="imgSrc" v-bind:title="details" :class="isActtive?'active':''" @click="changeisActive">
	<br>
	<img v-bind:src="imgSrc" v-bind:title="details" :class="{active:isActive}" @click="changeisActive">
	<style>
		.active: {
			boarder: 1px solid red,
		}
	</style>
	<script>
		var elementset = new Vue({
			el: "#elementset",
			data: {
				imgSrc: "./Figure_1.png",
				details: "有关时间复杂度的图片",
				isActive: false,
			},
			methods: {
				changeisActive: function() {
					this.isActive = !this.isActive
				}
			}
			
		})
	</script>
</div>
```
'''

### 1、style标签不能跟想要修改样式的元素平级
### 2、利用v-bind标签是纯字符串 这个与v-show和v-if不同后面两个是将字符串解析成布尔值
### 3、这样利用v-bind指令 元素就可以在元素的属性值中的运用vue对象中的data中的值 很方便
### 4、注意在属性值中单引号和双引号的交互使用
### 5、注意方法名一定在v-on指令中不能写错 这样的话不仅事件发生后无方法响应 其余的方法也会木有反应 要死
<br><br>
# v-for指令
'''
```javascript
<div id="vfor">
	<ul>
		<li v-for="(item, index) in arr">{{ index+1 }}地址:{{ item }}</li>
	</ul>
	
	<h1 v-for="item in food">{{ item }}</h1>
	<script>
		var vfor = new Vue({
			el: "#vfor",
			data: {
				arr: [
					"北京",
					"上海",
					"广州",
					"深圳",
				],
				food: [
					{name: "西兰花炒蛋"},
					{name: "蛋炒西兰花"},
					{nickname: "西红柿炒鸡蛋",
				]
			}
		})
	</script>
</div>
```

'''
### 1、根据数据生成列表结构 并且是响应式的
### 2、数组中的迭代器是自己命名的新变量 就会归结到vue对象中了  在其余的地方都可以正常使用了
### 3、使用二元组写成的迭代器 后面一项是索引
### 4、注意数组要写成[]而不能写成{}
### 5、变量名的命名要符合规则 且不能重复 因为vue代码一旦出错网页也就会出错
### 6、对于数组有一些方法 pop和push都是熟悉的方法 就是一个remove就是pop(0)
### 7、一个对象写成一个键值对的形式 也就是写成只有一个键值对元素的字典






# v-model指令
'''
```javascript
<div id="inp">
	<input v-model="message" v-on:keyup.enter="change('杨金哲')">
	
	<script>
		var inp = new Vue({
			el: "#inp",
			data: {
				message: "我要加油的",
			},
			methods: {
				change: function(p) {
					console.log(this.message+p)
				}
			}
		})
	</script>
</div>

<div id="notebook">
	<input type="text" @keyup.enter="add" v-model="message">
	<h1 v-for="(item, index) in items">{{ index+1 }}. {{ item }}
		<input type="button" value="清除" @click="remove">
	</h1>
```


​		
```javascript
	<script>
		var notebook = new Vue({
			el: "#notebook",
			data: {
				items: [],
				message: "请输入任务",
			},
			methods: {
				add: function() {
					this.items.push(this.message)
				},
				remove: function() {
					
				}
			}
		})
	</script>
</div>
```

'''
### 1、v-model可以获取表单元素中的值 也叫作双向绑定 因为v-bind是单向绑定的 标签的值的改变并不会影响vue对象中data中的值
### 2、{{  }}中可以直接调用数组的属性 比如length
### 3、清空数组的方式 this.items.splice(0, this.items.length)或者一种简单直接的我的天哪 this.items = []

<br><br>
# axios
'''
```javascript
<div>
	<input type="button" value="post请求" class="post">
	<input type="button" value="get请求" class="get">
	<script>
		document.querySelector(".post").onclick = function() {
			axios.post("https://autumnfish.cn/api/user/reg",{username:'杨金哲'})
			.then(function (response) {
				console.log(response)
			}, function(error){
				console.log(error)
			})
		}
		
		document.querySelector(".get").onclick = function () {
			axios.get("https://autumnfish.cn/api/joke/list?num=3")
			.then(function (response) {
				console.log(response)
			}, function (error) {
				console.log(error)
			})
		}
	</script>
</div>
```

'''
### 1、跟任何一个库一样 比如先导包 再用包 
### 2、生成一个全局axios对象 使用其post或get方法发送请求
### 3、then方法中的回调函数会在请求成功或者失败的时候被触发 就是写两个函数 写在then方法的括号里

## Vue实例的构造方法需要穿进去一个对象引用 对象引用中有很多对象引用比如el，data， methods 然后在Vue实例被创建时data引用的对象的数据才会实时地呗更新渲染






# 计算属性
在computed中写函数
与在methods中写函数的区别在于 计算属性是基于它们的响应式依赖进行缓存的 因此只要vue实例中的data中的对象不发生变化  那么这个computed中的函数也不会被触发
'''

```javascript
<div id="app">
	<p>{{ reversedBuffer }}</p>
	<p>{{ reversedBufferMethod() }}</p>
</div>

<script>
	var app = new Vue({
		el:"#app",
		data: {
			buffer: "hello world",
		},
		methods: {
			reversedBufferMethod: function() {
				return this.buffer.split(" ").reverse().join(" ")
			}
		},
		computed: {
			reversedBuffer: function() {
				return this.buffer.split(" ").reverse().join(" ")
			}
		}
	})
</script>
```
'''





# 计算属性

计算属性是一个跟data属性同等地位的属性，它也以对象的形式出现，对象里面有各种键值对，键自己取名，值就是一个个函数，在这一点上跟data属性是有区别的额

#### 所以和data属性中的键值对一样， computed属性中的键值对也可以在作用域中直接使用呢 

#### 虽然使用的时候是直接写键，但是在computed属性声明的时候，值要写成匿名函数的形式

####  和data和computed属性不同，methods中的方法在vue对象作用域调用的方法是使用方法名()

但两者的声明方法确实非常像的，两者渲染出的页面结果也是完全相同的，因为每一次vue对象发生了改变整个vue作用域都会重新渲染啊

```javascript
var vm = new Vue({
    el: "#vm",
    data: {
        message: "hello world",
    },
    computed: {
        reversedMessage: function() {
            return this.message.split("").reverse().join("")
        }
    },
    methods: {
    	reverse: function() {
            return this.message.split("").reverse().join("")
        }
    }
})
```

```html
<div id="vm">
    {{ message }}
    <br>
    {{ reversedMessage }}
    <br>
    {{ reverse() }}
</div>
```

计算属性是会直接跟data属性中的值形成一个缓存依赖的。



# 侦听属性

# vue-cli
vue init webpack vueproject 创建vue-cli工程
vue create projectname 创建工程

# django
settings.py中ALLOWED_HOSTS = ["*"] 
csrf给注释掉
LANGUAGE_CODE = 'zh-hans'

新建了app 要安排在settings.py中 这样在/admin中才会显示出这张数据表的入口
进行数据表迁移之后数据表的入口就可以用