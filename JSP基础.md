##脚本程序
脚本程序可以包含任意的java语句，变量，方法或表达式，只要它们在脚本语言中是有效的。

* **语法格式**

```shell
<% 代码片段%>
```

##JSP声明
一个声明语句可以声明一个或多个变量、方法，供后面的Java代码使用。在JSP文件中，您必须先声明这些变量和方法然后才能使用它们。

* **语法格式**

```shell
<%！ int a = 1; %>
```

##JSP注释
JSP注释主要有两个作用：为代码作注释以及将某段代码注释掉。

* **语法格式**

```shell
<%-- 注释--%>   //JSP注释，注释内容不会被发送至浏览器甚至不会被编译
<!-- 注释-->    //HTML注释，通过浏览器查看网页源代码时可以看见注释内容
<\%             //代表静态 <%常量
%\> 	        //代表静态 %> 常量
\'              //在属性中使用的单引号
\" 	            //在属性中使用的双引号

```

##JSP指令
JSP指令用来设置与整个JSP页面相关的属性。 

* **语法格式**

```shell
<%@ page ... %>     //定义页面的依赖属性，比如脚本语言、error页面、缓存需求等等
<%@ include ... %> 	//包含其他文件
<%@ taglib ... %> 	//引入标签库的定义，可以是自定义标签

```

##JSP行为
JSP行为标签使用XML语法结构来控制servlet引擎。它能够动态插入一个文件，重用JavaBean组件，引导用户去另一个页面，为Java插件产生相关的HTML等等。

* **语法格式**

```shell
jsp:include     //用于在当前页面中包含静态或动态资源
jsp:useBean 	//寻找和初始化一个JavaBean组件
jsp:setProperty 	//设置 JavaBean组件的值
jsp:getProperty 	//将 JavaBean组件的值插入到 output中
jsp:forward 	//从一个JSP文件向另一个文件传递一个包含用户请求的request对象
jsp:plugin 	//用于在生成的HTML页面中包含Applet和JavaBean对象
jsp:element 	//动态创建一个XML元素
jsp:attribute 	//定义动态创建的XML元素的属性
jsp:body 	//定义动态创建的XML元素的主体
jsp:text 	//用于封装模板数据
```

##JSP隐含对象
JSP支持九个自动定义的变量，也就是说不用定义就能使用的变量

* **隐含对象**

```shell
request     //HttpServletRequest类的实例
response 	//HttpServletResponse类的实例
out 	//PrintWriter类的实例，用于把结果输出至网页上
session 	//HttpSession类的实例
application 	//ServletContext类的实例，与应用上下文有关
config 	//ServletConfig类的实例
pageContext 	//PageContext类的实例，提供对JSP页面所有对象以及命名空间的访问
page 	//类似于Java类中的this关键字
Exception 	//Exception类的对象，代表发生错误的JSP页面中对应的异常对象
```

##控制流语句
JSP提供对Java语言的全面支持。您可以在JSP程序中使用Java API甚至建立Java代码块，包括判断语句和循环语句等等。
###判断语句
**if...else**
```html
<%! int day = 3; %> 
<html> 
<head><title>IF...ELSE Example</title></head> 
<body>
<% if (day == 1 | day == 7) { %>
      <p> Today is weekend</p>
<% } else { %>
      <p> Today is not weekend</p>
<% } %>
</body> 
</html> 
```
运行结果:
```html
Today is not weekend
```

**swithc...case**

整个都装在脚本程序中的标签中，使用out.println()来进行输出
```html
<%! int day = 3; %> 
<html> 
<head><title>SWITCH...CASE Example</title></head> 
<body>
<% 
switch(day) {
case 0:
   out.println("It\'s Sunday.");
   break;
case 1:
   out.println("It\'s Monday.");
   break;
case 2:
   out.println("It\'s Tuesday.");
   break;
case 3:
   out.println("It\'s Wednesday.");
   break;
default:
   out.println("It's Saturday.");
}
%>
</body> 
</html> 

```
运行结果:

```html
It's Wednesday.
```
###循环语句
在JSP程序中可以使用Java的三个基本循环类型：for，while，和 do…while。

**for**
```html
<%! int fontSize; %> 
<html> 
<head><title>FOR LOOP Example</title></head> 
<body>
<%for ( fontSize = 1; fontSize <= 3; fontSize++){ %>
   <font color="green" size="<%= fontSize %>">
    JSP Tutorial
   </font><br />
<%}%>
</body> 
</html> 
```

**while**
```html
<%! int fontSize; %> 
<html> 
<head><title>WHILE LOOP Example</title></head> 
<body>
<%while ( fontSize <= 3){ %>
   <font color="green" size="<%= fontSize %>">
    JSP Tutorial
   </font><br />
<%fontSize++;%>
<%}%>
</body> 
</html> 
```