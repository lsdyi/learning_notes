#                             HTML
## 了解什么元素可用，以及元素的属性也就是attribute

##### web是一个由文档组成的大海，文档之间相互关联

##### 元素是指开标签，闭标签以及二者之间的任何内容

```html
	<head>元素：经常被称为页面的头部，包含页面的相关信息（此处不是页面的主体内容）
	<body>元素：经常被称为页面的主体，包含实际希望在浏览器主窗口中看到的信息
	
	<title>元素在任何网页都强制存在，它的作用有：
	1、页面存进浏览器的收藏夹时浏览器使用的名称
	2、帮助搜索引擎理解文档的内容
```

##### html中的属性（attribute）非常类似于每天生活中经历的特性。他们描述人或物的某种特征，比如一个高个子的人，一条棕颜色的狗。 😂属性被用于描述包含它们的元素😂

```html
	<a>：a代表“锚”，anchor
```

##### 没有值的属性被称之为布尔属性，意思是这个属性的值只有两种true和false，如果在一个元素的开标签中出现了这个属性，这就是表示这个属性的值为true
```html
	下面这两种写法是等价的
	<input type="text" required>
	<input type="text" required="true">
```
```html
	核心属性：id title class style
	国际化属性：dir lang
	辅助访问特性：accesskey tabindex

id属性取值的特殊规则：必须以字母卡头，在文档种必须保持唯一性

dir属性是为了设置文档的文字应该从左到右显示还是从右到左显示，因为有的语言的确是从右到左读的
lang属性是设置文档中使用的主要语言，主要作用是给搜索引擎看，给屏幕阅读器看（有的文档会自动出现翻译功能就是因为这）
dir和lang属性都应该写在<html>的开标签中
```

## 一些我不熟悉的元素
```预格式化小结<pre>
地址<address>
分组元素：<header>	<hgroup>	<nav>	<section>	<article>	
呈现性元素：<b>	<i>		<sup>	  <sub>
短语元素：<em>	<strong>	<abbr>	  <dfn>		<blockquote>	<q>		<cite>	  <code>	<kbd>	  <var>		<samp>
列表：<dl> <dt> <dd>
编辑元素：<ins>和<del>
```
##### 尽管br元素可以通过换行制造出p元素的效果，但是要记住HTML标记应该用于描述内容的结构。而前面说的行为实际上功能上越界到控制页面的显示上去了，因此只应该在块级元素内部使用br元素

```html
<!--
	使用<pre>元素预格式化文本
    任何在<pre></pre>元素中的内容在html解析的时候都会保持原有的格式
-->

```

```html
<!--
	使用<header>元素预格式化文本
	使用一个div作为块级元素，给class="header"，但是无论怎么样给元素设置id和class，其本身是没有任何语法含义的，用head元素进行标记的话，可以简化标记工作（不用写啥class或者id来标识这个块级元素了），减少标记工作的同时，实现了class和id这样给元素一些实际语义的工作
-->

<header>
	<h1>the title of the page</h1>
	<p>
		details of the paragraphy
	</p>
</header>

```
##### header元素不一定只在网页层面也就是body元素中使用，也可以在其他分组元素中使用，这表示即将显示的内容是该分组的头部内容这一实际含义

```html
<!--
	使用<hgroup>将多级标题组织起来 
-->
<header>
	<hgroup>
		<h1>一级标题</h1>
		<h2>二级标题</h2>
	</hgroup>
</header>

```
```html
<!--
	<blockquote>块级引用
	文本将以漂亮的引言的形式呈现
-->
<div>
	<p>the following is a quote from ***</p>
	<blockquote>
		the blockquote element ***
	</blockquote>
</div>
```
```html
<!--
	使用<nav>代表页面中的导航区域
    它是由一个链接列表组成
-->
<nav>
	<p><a href="recipes.html">Recipes</p>
	<p><a href="menu.html">Menu</p>
	<p><a href="opening_items.html">Opening times</p>
	<p><a href="contact.html">contact</p>
</nav>
```

#### 分组元素

### 列表元素

```html
<!-- 无序列表 -->
<ul>
	<li></li>
</ul>

<!-- 
	有序列表
	可以在ol的开标签中写start属性用来表示起始索引
	可以在ol的开标签中写reversed属性用来表示倒序排列 但这个属性是布尔属性
	可以在ol的开标签中写type属性用来表示列表的标记符号
-->
<ol>
	<li></li>
</ol>
```

### 行内元素
##### span元素对文档的视觉效果没有任何影响，但它却将相关元素组织到一起，给内容标记增加了更多语义，并且在用css为元素添加效果的时候尤其有用

```html
	<!-- em标签用于文本斜体 -->
	<em>I'm a strong man</em>
	
	<!-- 
	strong标签和em标签一样都是对文本表示强调作用
    但是strong更甚 会加粗显示
    -->
```

### 链接 
链接分为内部链接和外部链接，内部链接是跳转到你创建的工程的内部的页面的，外部链接是跳转到外部的页面
#### url是统一资源定位符 uniform resource locator，从英语的字面意思来理解的话就是资源的地址 在web上最多的资源就是html文档  搜索引擎也是web（万维网）的搜索引擎

```html
<!-- 链接到电子邮件 并会把邮箱填到收件人的一栏 -->
	<a href="mailto:yangjinzhehao123@outlook.com"> </a>
```

## web（万维网）是Internet（互联网）上提供的一种应用，其主要特点就是搭载在浏览器上，以浏览器网页的方式，对Internet上的资源进行访问

```html
<!-- 
	base元素是用来设置页面的url的，这样页面再要用到相对url的时候会和base中设置的url
-->

	<base href="http://localhost:5500/kfc/"></base>
```
##### 注意这href里面的最后一个/不能少，因为它的意思是kfc是一个目录。如果不写的话会被当做是一个文件 因此base url的设置就不会生效

### 图片 音频 视频 

### 表格
##### 在创建表格的时候，很多人实际上并不在乎使用th元素，而是对任何单元（包括表头）都是用td元素。但是无论何时当你有一个表头是，你都应该尽量使用th元素。特别在使用scope属性的时候，因为它只对th元素有效

```html
	<table>
		<caption>表格的标题</caption>
		<tr>
			<td>数据11</td>
			<td>数据12</td>
			<td>数据13</td>
		</tr>
		<tr>
			<td>21</td>
			<td>22</td>
			<td>23</td>
			
		</tr>
	</table>
```

### 表单 
表单的required属性 是一个布尔属性 如果写上了说明这个表单控件必填
表单的autocomplete属性不是一个布尔属性 但是可以取两个值 一个off一个on 表示浏览器是否可以缓存表单内容以保证下一次的预填写 但是type为password的输入框表单控件的autocomplete属性是直接设成off了嘞的
value属性是每个表单控件都有的，其最主要的作用是用于提交表单的时候传参，还有另外一个功能是用于在一些文本输入类的表单控件，如单行输入框之类的表单控件，用来作为这一类表单控件的显示，但是非文本输入类的表单控件如checkbox，value属性就只是一个传参的功能而没有任何作为文本显示的功能，必须写成类似于如下形式checkbox表单控件才能看到文本
```html
<input type="checkbox" name="chkskill" value="css">css<br>
<input type="checkbox" name="chkskill" value="html">html<br>
<input type="checkbox" name="chkskill" value="javascript">javascript

<!--
	除此之外checkbox控件还有一个布尔属性叫做checked表示该控件最开始的时候是否被选中
-->
```

radio单选框控件和selected下拉框控件在本质上是一样的，它要求用户单选，但是它会比radio占用更少的空间
```html
<!-- 如果option中没有selected属性的话 会默认显示第一个option 我觉得这样就可以了额嘻嘻嘻-->
	<select name="selColor">
		<option selected="selected" value="">Select color</option>
		<option value="red">Red</option>
		<option value="green">Green</option>
		<option value="blue">Blue</option>
	</select>
	
<!-- 
	在下拉框中实现多选 只需要在select标签中用multiple属性 
	点击的时候按住ctrl键就可以达到效果
-->
	<select name="selColor" multiple>
		<option value="red">Red</option>
		<option value="green">Green</option>
		<option value="blue">Blue</option>
	</select>
	
	
<!-- 在下拉的option中实现分组的效果 用optgroup元素 元素中要带label属性 -->
	<select name="selColor">
		<optgroup label="hardware">
			<option value="desktop">Desktop computers</option>
			<option value="laptop">Laptop computers</option>
		</optgroup>
		<optgroup label="software">
			<option value="os">oprating system software</option>
			<option value="app">application software</option>
		</optgroup>
	</select>
```

##### 下列情况中不应该使用http get方法 因为参数会作为url的一部分而暴露
处理敏感信息 如密码或者信用卡细节
更新数据源，如数据库或者电子表格（因为有人可能伪造能够修改数据源的url）
表单包含了文件上传空间（因为上传文件无法再url中传递）

通常为文档的主体指定白色的背景颜色，以防用户自己定义了浏览器的背景颜色导致网页的背景不是白色的额

操作字体的几个css属性
font-family
font-size
font-weight	可以加粗字体
font-style	可以做意大利斜体
font-variant	可以让大写字母和小写字母的大小搞成一样的

操作文本的几个css属性
color 
text-align 控制文本在被包含元素的对齐方式（就是第一个字也就是开头怎么写）
vertical-align 用于设置内容的相对于父元素的垂直居中样式

通用选择器和对body运用选择器是不同的，通用选择器是对所有的元素进行样式的应用，对body运用选择器的话只是body中所有的元素继承了来自body的样式，虽然还不知道这看起来效果一样的两种到底有啥子区别额

在选择器中直接用到元素类型会使页面变慢，这是因为浏览器必须解析所有该类型的元素，并检查其是否具有匹配的id特性，而不是简单瞄准由选择器指定的唯一元素（如用id选择器）

border：盒子的边框
content：盒子的内容
padding（填充物）：盒子边框和盒子内容之间的部分
margin（边缘）：盒子边框和其他的盒子边框之间的部分 是可以继承的



居中方式的出发点应该是相对于父级元素居中 而不是说控制盒子模型 修改padding让content在相对于border来说实现左居中