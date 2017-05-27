---
title: ASP.NET知识点面试篇
tags:
  - ASP.NET
  - 面试
date: 2016-04-16 17:13:19
---

## 列举ASP.NET页面之间传递值的几种方式？

### 1，使用QueryString，如：...Test?id=1 Redirect()..

### 2，使用Session变量

### 3，使用Server.Transfer

### 4，Cookie传值
<!--more-->
## 什么是code-Behind技术

### 就是代码隐藏，在ASP.NET中通过ASPX页面指向cs文件的方法实现显示逻辑和处理逻辑的分离，这样有助于web应用程序的创建。比如分工，美工和编程的可以各干各的，不用再像以前asp那样代码和HTML代码混在一起，难以维护。

## 请解释ASP.NET中的web页面与其隐藏类之间的关系

### 一个ASP.NET页面一般都应对一个隐藏类，一般都在ASP.NET页面的声明中指定了隐藏类例如一个页面Test.aspx的页面声明如下：

```
<%@ Page language="c#" Codebehind="Test.aspx.cs" AutoEventWireup="false" Inherits="T1.Test" %>
```

### Codebehind="Test.aspx.cs"表明经编译此页面时使用哪一个代码文件

### Inherits="T1.Test"表示运行时使用哪一个隐藏类

### aspx页面会编译生成一个类，这个类从隐藏类继承

## 通过超链接怎样传递中文参数？

### 用URL编码，通过QueryString传递，用urlencode编码，urldecode解码

## Server.Transfer和Response.Redirect的区别是什么？

### Server.Transfer仅是服务器中控制权的转向，在客户端浏览器地址栏中不会显示转向后的地址；Response.Redirect则是完全的跳转，浏览器将会得到跳转的地址，并重新发送请求链接。这样，从浏览器的地址栏中看到跳转后的链接地址。

### Server.Transfer是服务器请求资源，服务器直接访问目标地址的URL，把那个URL的相应内容读取过来，然后把这些内容再发给浏览器，浏览器根本不知道服务器发送的内容是从哪里来的，所以它的地址栏还是原来的地址。这个过程中浏览器和web服务器之间经过一次交互。

### Response.Redirect就是服务端根据逻辑，发送一个状态吗，告诉浏览器重新去请求那个地址，一般来说浏览器会用刚才请求的所有参数重新请求。这个过程中浏览器和web服务器之间经过了两次交互。

## 说出一些常用的类，接口

### 常用的类：StreamReader、WebClient、Dictionary<K,V>、StringBuilder、SqlConnection、FileStream、File、Regex、List<T>

### 常用的接口：IDisposable、IEnumerable、IDbConnection、IComparable、ICollection、IList、IDictionary

## post、get的区别

### get的参数会显示在浏览器地址栏中，而post的参数不会显示在浏览器地址栏中

### 使用post提交的页面再点击刷新按钮的时候浏览器一般会提示“是否重新提交”，而get则不会

### 用get的页面可以被搜索引擎抓取，而用post的则不可以

### 用post可以进行文件的提交，而用get则不可以

### 用post可以提交的数据量非常大，而用get可以提交的数据量则非常小(2K)，受限于网页地址的长度。

## Application、Cookie和Session两种会话有什么不同？

### Application是用来存取整个网站全局的信息，而Session是用来存取与具体某个访问者关联的信息。Cookie是保存在客户端的，机密信息不能保存在Cookie中，只能放小数据；Session是保存在服务器端的，比较安全，可以放大数据。

## http状态吗各是什么意思？

### 302：重定向

### 301：永久重定向

### 404：页面不存在

### 500：服务器内部错误

## Session有什么重大的BUG，微软提出了什么方法加以解决？

### iis中有由于有进程回收机制，系统繁忙的话Session会丢失，iis重启也会造成Session丢失。这样用户就要重新登录或者重新添加信息到Session中。可以用StateServer或SqlServer数据库的方式存储Session不过这种方式比较慢，而且无法捕获Session的end事件。但是我认为这不是bug，只能说是In-Proc方式存储Session的缺陷，缺陷和bug是不一样的，In-Proc方式存储Session会由服务器来决定什么时候释放Session，这是By Design，In-Proc方式不满足要求的话完全可以用StateServer和数据库的方式。

### StsteServer还可以解决集群Session共享的问题

### 配置StateServer的方法：略

## asp.net中<% %>、<% = %>、<% # %的区别是什么？>

### <% %>是执行<% %>中的C#代码，<% = %>是将=后的表达式的值输出到Response中，<% # %>是数据绑定，一般用来ListView、GridView、Repeater等控件的绑定中。数据绑定分为，Eval：单向绑定和Bind：双向绑定。

## asp.net中的错误机制

### 定制错误页来将显示一个友好的报错页面

### 页面中未捕捉一样会触发Page_Error，应用程序中的未捕获异常会触发

### Application_Error。通过HttpContext.Current.Server.GetLastError();拿到未捕捉异常，记录到Log4Net日志中。