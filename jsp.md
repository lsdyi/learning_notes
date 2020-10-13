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

