##### 在jsp页面出现了实例变量 实例方法 和局部变量 

在java中实例变量和实例方法是不加static修饰的，它属于一个对象实例而不属于一个类，所以当对象被new出来实例变量和实例方法就存在了。局部变量常常是在方法块或者代码块中的变量，当方法结束或者代码块结束，局部变量的生命周期随即消失。

##### <%! %>标签内写整个jsp页面的变量和方法，里面变量和方法在整个jsp页面都是有效的。在jsp引擎把jsp生成java文件的时候，这个标签里面的变量和方法就是实例变量和实例方法。

当然了实例方法中的变量就是局部变量了，仅仅只在实例方法被调用到堆栈块的时候才有内存。所以作用域也就只在实例方法中。



jsp：

```java
<%!
	int sum = 0;
	Date date;
	
	public int getNum(int n) {
        return n;
    }
%>

<%
	int x = 7;
	getNum(x);
%>
```

在<%! %>标签中的有变量sum，和方法getNum(int n) 在由jsp引擎生成java类的时候，sum和getNum分别是类的实例变量和实例方法

<% %>标签中就是就是一些有逻辑的java代码，在jsp引擎生成java类的时候，会有一个实例方法，然后会把这些代码都放入这个实例方法中。



java：

```java
    int sum = 0;
    Date date;

    public int getNum(int n) {
        return n;
    }

	public void _jspService() {
        int x = 7;
        getNum(x);
    }
```

实际的java代码比这复杂的多，在这里只是把许多代码，参数都删除了以便于简洁地显示java类中的部分结构。

因为sum和getNum都是属于对象实例的，所以在_jspService实例方法中都可以自由的调用实例对象的变量和方法，这很简单，因为没有牵扯到static修饰的类变量和类方法。



##### 提交表单和用ajax或axios异步提交数据的区别

提交表单会直接在浏览器的地址栏向目标地址发送请求，所以发送请求和网页运行是同步的，其结果就是导致从原先的页面直接跳到新页面去了，当然这是在form得属性action有写值的情况下， 如果直接<form action=""> 的话是向原页面发送请求。注意form标签中action不写的话默认就是action="" method不写的话默认就是method="get"

而ajax或者axios是一种异步的技术，通过它向一个地址发送请求和网页的运行是在两个方向上的（对比前面的同步进行）所以这种技术的结果就是网页不发生跳转也不刷新，然后获得到的数据就可以直接再渲染到页面上，只是数据发生改变而网页的主体没有发生改变。



##### post编码问题

在request请求如果用的get方式，不会出现编码问题，但是如果用的post方式会出现乱码问题

```html
<form method="get" action="">
    <input type="text" name="text1">
    <input type="submit" value="提交">
</form>

<form method="post" actioin="">
    <input type="text" name="text2">
    <input type="submit" value="提交">
</form>
```

```java
String text1 = request.getParameter("text1");
out.print(text1);		//不会出现乱码 因为get请求的方式比较简单 中文就是按utf-8编码
					//因此和java中String的编码方式相同 不会出现中文乱码的问题



String text2 = request.getParameter("text2");

//会出现中文乱码 因为post方式过来的中文字符串是采用的iso-8859-1的格式进行编码的，
//java中的字符串用的utf-8的格式编码的 两者不匹配
outd.println(text2);	
```

##### 解决办法1:（最简单） 直接设置request对象的编码格式为utf-8

```java
request.setCharacterEncoding("utf-8");

String text2 = request.getParameter("text2");
out.println(text2);		//中文输出正常

```



##### 解决办法2: 将得到的用iso-8859-1编码格式的字符串转成utf-8格式的字符串，不能直接转，中间要先将字符串内部的字符数组调出来，当然因为是以iso-8859-1编码的，所以调字符串对象的字符数组出来的时候以此编码格式

```java
/*
这里String的构造函数有两个参数，一个是字符数组，一个是将要生成字符串所采用的编码格式
第一个参数字符数组：request.getParameter("text2").getBytes("iso-8859-1")
	request.getParameter("text2")就直接得到了request对象中text2字段的字符串了
	然后把这个字符串对象的内部的字符数组调出来 采用iso-8859-1的编码方式调出来
第二个参数编码方式："utf-8" 这就是对前面的字符数组以什么样的编码方式形成新的字符串
*/
String text2 = new String(request.getParameter("text2").getBytes("iso-8859-1"), "utf-8");

```

注意上面这块代码中要检查request.getParameter("text2")得到的字符串对象的引用是否为null，如果为null的话调用getBytes()方法是会报错的（555 血和泪的教训）所以最好的是做一个异常处理吧嘻嘻。



##### 发现一个很蛇皮的写法

经常在jsp代码块中会request对象接收到某个参数比如client

String client = request.getParameter("client");

if(client != null) {



}

因为可能在这个页面最开始的时候request对象中是没有client这个参数的，比如表单第一次打开的时候，所以在这个时候可以用一个if判断，如果client为null的话说明在request对象中没有找到client参数，if块中的代码也就不会执行了。



##### 用jsp表达式和out.print和out.println()的区别

先说结论：俺觉得就是没啥区别😂😂😂

因为jsp引擎其实是先把jsp文件生成为一个java文件

```html
                <%= client %>
                out.print(client);
                <%= client %>                 
```

对应java文件中的

```java
      out.write("\r\n");
      out.write("                ");
      out.print( client );
      out.write("\r\n");
      out.write("                ");

                out.print(client);
                
      out.write("\r\n");
      out.write("                ");
      out.print( client );
      out.write("\r\n");
      out.write("                ");
```

因为out其实就是对应的输出流，所以<%= client %>就是比out.print(client)多输出了几个换行还有一些空格，这些在html文档中其实是无效的 

html代码：

![image-20200919133701385](C:\Users\lsdyi\AppData\Roaming\Typora\typora-user-images\image-20200919133701385.png)

浏览器显示结果：

![image-20200919133749615](C:\Users\lsdyi\AppData\Roaming\Typora\typora-user-images\image-20200919133749615.png)

至于out.println()就是输出成html的时候比out.print()多了一个换行 这在浏览器解析html文档的时候都没啥用







# 脚本

##### 1. java代码块 

```java
<%
  //java代码  
%>
```

##### 2. 输出脚本

```java
<%= %>
```

##### 3. 全局脚本

```java
<%!
  //java对象的实例方法和实例变量 
%>
```





# 指令

<%@instruction_name key=value %>	

instruction_name有page 、include 、taglib

<%@page contentType="text/html; charset = utf-8" session="false" pageEncoding="utf-8" import="java.util.Date" %>

<%@include file="header.jsp" %>  静态包含

<%@taglib tagdir="WEB_INF/tags" prefix="fix" %>

# 动作标签

<jsp:action_name key = value />

<jsp:include page="header.jsp" />

注意动作名和前面的：之间是不能有空格的  而上面的那个指令名和@之间是可以有空格也可以无空格的



odbc: java来访问微软的软件

jdbc：

mysqlurl = “jdbc:mysql://localhost:port;databasename";





#### <% %>是普通脚本，这里面的所有代码都会放到最终生成的**_jsp.java类中的jsp_service()方法中，所以在普通脚本中定义的是局部变量，只在被调用的时候在堆栈块中有效。

当然了，小破站的教程里面说普通脚本里面不能再有普通脚本，不能有html代码这都是自然而然能联想到的，java方法里面不说别的 肯定全部都是java代码嘛嘻嘻嘻



##### <%! %>声明脚本，这里面的代码都会放到最终生成的**_jsp.java类中，代码中只能声明变量和方法，这些变量和方法最终会编程类中的实例变量和实例方法

##### <%= %>输出脚本，里面写的东西就相当于打印的代码，即在<% %>中写代码块：out.print() ，两者唯一的区别我感觉就是没有任何区别

#### 为啥会一直存在



### JSP指令：可以用来设置与整个jsp页面相关的属性

#### <%@page  %> 
##### page指令为容器提供当前页面的使用说明，一个jsp页面可以有多个page指令
属性：
contentType 设置页面内容的格式和编码格式  "type=text/html;charset=utf-8"  在服务器的response中体现 在java类中会有语句response.setContentType("text/html; charset=utf-8")

errorPage 设置该页面出现异常时的跳转页面 
isErrorPage 是否设置当前页面为一个另一个页面的错误跳转页面  "true"

import 属性用于导入包  俺觉得非常重要啊555

language属性用于设置页面的代码语言 默认为java嘻嘻嘻

session属性是关于session对象的话 如果这个属性的值为true的话是立即创建session对象，如果为false的话是使用的时候创建session对象 默认是为true的

pageEncoding用于设置jsp页面的解码格式 相当于html页面中的<meta></meta>标签 <meta charset="utf-8"></meta> 设置html页面的解码格式



#### <%@include %> 静态包含可能会有冲突问题 不建议使用

#### include指令用于包含其他文件 是一种静态包含的形式

属性：

file 设置要引入的页面，原封不动的引入，按照顺序，该写在jsp_service()中的写在写在jsp_service()中，该是jsp实例方法和实例变量的就写在实例方法和实例变量的位置。

所以在这种情况下，就得考虑一个问题，就是方法名，变量名重复的问题，要保证include进来的jsp页面普通脚本中的变量不能和母页面中的普通脚本中的变量名重复；include进来的jsp页面的声明脚本的实例方法名和实例变量名不能和母页面中的声明脚本中的实例方法名和实例变量名重复。

```java
<%include file="main.jsp" %>
```



#### <%taglib %>

#### taglib指令用于进入jsp的标准标签库

属性：

uri：外部标签库路径 

prefix：前缀



### 动作标签 

#### 语法<jsp:动作名 属性名=属性值 >



#### include动作  动态包含 非常耐斯

属性：

page 相对url地址

<jsp:include page="main.jsp" />

#### include动作标签与include指令的不同在于，用include指令静态包含，是把外部文件的代码放到了咱们的jsp_service()方法中，include动作标签是将外部文件的结果放到了咱们的jsp_service()方法中



#### useBean动作 用来加载一个将在页面中使用的javaBean

属性：

id 类的名字

class	全限定名

<jsp:useBean id="object reference" class="complete class directory" />



#### getProperty动作 使用java bean类的私有实例变量

属性：

name java bean对象的引用变量

property 对象的实例变量



#### setProperty动作 设置java bean类的私有实例变量

属性：

name java bean对象的引用变量

property 对象的实例变量

value 实例变量的值



#### forward动作 用来将页面跳转到一个新的jsp页面 要写项目文件夹下的相对url 不能随意跳

属性：

page 页面的url

<jsp:forward page="url for a new page" />

##### forward动作标签和response对象的sendRedirect()的区别在于forward标签在跳转页面的时候根本就没有在地址栏中重新发起请求，即使搭配了<jsp:param />param动作标签也不会在浏览器的location对象中出现任何多余的与原页面有关的参数，这是forward标签的缺点，但是另一方面它的优点是request对象会直接从原页面传到新页面，而如果用response.sendRedirect()方法跳转到新页面的话浏览器对于这个新页面的request对象就是全新的了



#### param动作 和forward动作标签搭配使用 用于在不同页面中传递request的param参数

属性：

name 参数的名字

value 参数的值

<jsp:param name="para1" value="1" />

##### 当然咯 这样的参数标签 一个标签只能传递一个参数哦 因为如果在一个参数标签里写多个name value那啥不就混淆了嘛嘻嘻嘻



### 内置对象

#### 四大作用域对象

嘻嘻嘻 在学校上jsp课程的时候我一直把jsp的那几个对象弄不明白，觉得好混乱，各自有各自的应用层场景。现在才知道这几个对象对于数据的存取方式都是完全相同的，只是生命周期不同，因此也叫作作用域对象：

##### pageContext：仅在当前jsp页面有效

##### request： 一次请求有效

##### session：一次会话有效（关闭浏览器失效）

##### application：整个web应用有效（服务器重启或关闭失效）

```html
		<%
            pageContext.setAttribute("key1", "value1");
            pageContext.setAttribute("key2", "value2", pageContext.REQUEST_SCOPE);
            pageContext.setAttribute("key3", "value3", pageContext.SESSION_SCOPE);
            pageContext.setAttribute("key4", "value4", pageContext.APPLICATION_SCOPE);

        %>

        <h1>用pageContext取</h1>
        <h2><%=pageContext.getAttribute("key1") %></h2>
        <h2><%=pageContext.getAttribute("key2", pageContext.REQUEST_SCOPE) %></h2>
        <h2><%=pageContext.getAttribute("key3", pageContext.SESSION_SCOPE) %></h2>
        <h2><%=pageContext.getAttribute("key4", pageContext.APPLICATION_SCOPE) %></h2>
        
        <h1>用四大作用域取</h1>
        <h2><%=pageContext.getAttribute("key1") %></h2>
        <h2><%=request.getAttribute("key2") %></h2>
        <h2><%=session.getAttribute("key3") %></h2>
        <h2><%=application.getAttribute("key4") %></h2>

        <h1>用el表达式取法1</h1>
        <h2>${pageScope.key1}</h2>
        <h2>${requestScope.key2}</h2>
        <h2>${sessionScope.key3}</h2>
        <h2>${applicationScope.key4}</h2>
        <h1><%=pageContext.PAGECONTEXT_SCOPE %></h1>
        <h1><%=pageContext.REQUEST_SCOPE %></h1>
        <h1><%=pageContext.SESSION_SCOPE %></h1>
        <h1><%=pageContext.APPLICATION_SCOPE %></h1>
```



#### EL表达式

用于取四大内置对象中的attribute，用法在上面的代码块



用于取自建的对象

```html
<%
	User user1 = new User("Tom", "iostream");		//用户名为Tom，密码为iostream
   	request.setAttribute("user1", user1);
%>
${user1.username}
```

但这一种方法本质上还是建立在第一种也就是用四大内置对象这个方法的基础上的:

1、自建的对象需变成四大内置对象的属性

2、在${user1.username}的时候要先得到user1对象然后再取username属性。而先得到user1对象实际上完整的写法应该是requestScope.user1先通过request对象拿到名为"user1"的attribute，而且后面再取username这一属性的时候本质上也是调用了对象的共有的get方法。



 EL的内置对象

pageContext 常用的有pageContext.request.contextPath来获得应用的上下文

cookie:

cookie.username 获取取名为username的cookie对象

cookie.password 获取取名为password的cookie对象



#### JSTL标签

```
//条件判断
<c:if test="condition">
</c:if>

//用多重条件判断
<c:choose>
	<c:when test="condition1">codes</c:when>
	<c:when test="condition2">codes</c:when>
	<c:otherwise>codes</c:otherwise>
</c:choose>
```



```
//迭代遍历
<c:forEach var="it" items="collection">
	codes
</c:forEach>

// 键值对var 表示迭代器
//collection表示要迭代的集合
```

#### 注意：c:if中的test和c:forEach中的items里面应该都是要用el表达式的，因为取得事先定义好的变量，当然这些变量一般都是四大内置对象中的attribute，所以还有一步别忘了，就是把这些变量要存到内置对象中的attribute中，这样${}才是识别的出

