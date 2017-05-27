---
title: Asp.net知识点总结(一)
tags:
  - ASP.NET
  - 技术教程
date: 2015-07-03 18:25:50
---

## Asp.net六大对象

# 1.Request--&gt;读取客户端在Web请求期间发送的值

常用方法:

### 1、Request.UrlReferrer请求的来源，可以根据这个判断从百度搜的哪个关键词、防下载盗链、防图片盗链，可以伪造(比如迅雷)。 (使用全局一般处理程序)

### 2、Request.UserHostAddress获得访问者的IP地址

### 3、Request.MapPath(virtulPath)将虚拟路径转换为磁盘上的物理路径,Request.MapPath(“./a/b.aspx”)就会得到D:\WebSites\a\b.aspx
<!--more-->
Server.MapPath里就是调用的Request.MapPath

### 4、Request.Url.GetComponents(UriComponents.HostAndPort,UriFormat.SafeUnescaped) 获取当前请求的网站的域名和端口号

### Request对象主要是让服务器取得客户端浏览器的一些数据,包括从HTML表单用Post或者GET方法传递的参数、Cookie和用户认证。因为Request对象是Page对象的成员之一，所以在程序中不需要做任何的声明即可直接使用；

# 其类名为HttpRequest 

属性很多，但方法很少，只有一个BinaryRead() 

### 1.使用Request.Form属性获取数据 

通过该属性，读取<Form></Form>之间的表单数据.注意：提交方式要设置为“Post”。 
与Get方法相比较，使用Post方法可以将大量数据发送到服务器端 

### 2.利用Request.QueryString属性获取数据 

Request对象的QuerySting属性可以获取HTTP查询字符串变量集合。

通过该属性，我们可以读取地址信息http://localhost/aaa.aspx?uid=tom&pwd=abc其中标识为红色部分的数据. 
注意：提交方式要设置为“Get” 

### 3.问题：Request.Form用于表单提交方式为Post的情况，而Request.QueryString用于表单提交方式为Get的情况，如果用错，则获取不到数据。 

解决方法：利用Request(“元素名”)来简化操作。 

### 4.Request.ServerVariables(“环境变量名称”) 类似的还有：UserHostAddress,Browser,Cookies,ContentType,IsAuthenticated Item,Params 

## 2.Response-->封装了页面执行期后返回到Http客户端的输出

# Response对象用语输出数据到客户端，包括向浏览器输出数据、重定向浏览器到另一个URL或向浏览器输出Cookie文件。 

其类名为httpResponse 

属性和方法 

Response.Write() 向客户端发送字符串信息 

Response.Buffer、Response.BufferOutput：经过Reflector反编译，发现两个属性是一样的，Buffer内部就是调用的BufferOutput。这个属性用来控制是否采用响应缓存，默认是true。 

Response.Clear()清空缓存区中的数据，这样在缓存区中的没有发送到浏览器端的数据被清空，不会被发送到浏览器。

# Redirect() 网页转向地址 

Response.ContentEncoding输出流的编码。

Response.ContentType输出流的内容类型，比如是html（text/html）还是普通文本（text/plain）还是JPEG图片（image/JPEG）。 

Response.OutputStream输出流，在输出图片、Excel文件等非文本内容的时候要使用它

Response.End() 终止响应，将之前缓存中的数据发给浏览器，End()之后的代码不会被继续执行,End方法里调用了Flush()方法。在终止一些非法请求的时候，比如盗链等可以用End()立即终止请求。

# WriteFile() 读取一个文件，并且写入客户端输出流

（实质：打开文件，并且输出到客户端。） 

### 1.Response.Write变量数据或字符串 

```
Response.Write (变量数据或字符串) 
  <%=…%> 
Response.Write(“<script language=javascript>alert(‘欢迎学习ASP.NET’)</script>”)  
Response.Write(“<script>window.open(‘WebForm2.aspx’)</script>”)  
```

### 2.Response对象的Redirect方法将客户端浏览器重定向到另外的URL上，即跳转到另一个网页。 

例如: 
```
Response.Redirect(“http://www.163.net/”) 
```

### 3\. Response.End() 终止当前页的运行 

### 4.Response.WriteFile(FileName) 

其中： FileName指代需向浏览器输出的文件的文件名 

## 3.Server-->提供对服务器上的属性和方法的访问

Server对象提供对服务器上的方法和属性进行的访问 .其类名称是HttpServerUtility. 

### Server对象的主要属性有： 

MachineName：获取服务器的计算机名称。

ScriptTimeout：获取和设置请求超时（以秒计）。 

方法名称 说明 
CreateObject创建COM对象的一个服务器实例。 

Execute执行当前服务器上的另一个aspx页，执行完该页后再返回本页继续执行 

HtmlEncode对要在浏览器中显示的字符串进行HTML编码并返回已编码的字符串。 

HtmlDecode对HTML编码的字符串进行解码，并返回已解码的字符串。 

MapPath返回与Web服务器上的指定虚拟路径相对应的物理文件路径。 

Transfer终止当前页的执行，并为当前请求开始执行新页。 

UrlEncode将代表URL的字符串进行编码，以便通过URL从Web服务器到客户端进行可靠的HTTP传输。

UrlDecode对已被编码的URL字符串进行解码，并返回已解码的字符串。 

UrlPathEncode对URL字符串的路径部分进行URL编码，并返回已编码的字符串。 

编码： Server.HtmlEncode(“HTML代码”) 

解码： Server.HtmlDecode(“已编码的HTML”) 

Server对象的MapPath方法将虚拟路径或相对于当前页的相对路径转化为Web服务器上的物理文件路径。

### 语法：Server.MapPath(“虚拟路径”) 

String FilePath 

FilePath = Server.MapPath(“/”) 

Response.Write(FilePath) 

Server.HtmlDecode()、Server.HtmlEncode() Server.UrlEncode()、 Server.UrlDecode()是对HttpUtility类中相应方法的一个代理调用。

推荐总是使用HttpUtility，因为有的地方很难拿到Server对象，而且Server的存在是为以前ASP程序员习惯而留的。

别把HtmlEncode、UrlEncode混了，UrlEncode是处理超链接中的中文问题， HtmlEncode是处理html代码的。还是推荐用HttpUtility.HtmlEncode。

Server.Transfer(path) 内部重定向请求，Server.Transfer(“JieBanRen.aspx”)将用户的请求重定向给JieBanRen.aspx处理，是服务器内部的接管（不能重定向到外部网站），浏览器是意识不到这个接管的，不是象Response.Redirect那样经历“通知浏览器‘请重新访问url这个网址’和浏览器接到命令访问新网址的过程”，是一次http请求，因此浏览器地址栏不会变化。

因为是内部接管，所以在被重定向到的页面中是可以访问到Request、Cookies等这些来源页面接受的参数的，就像这些参数是传递给他的，而Redirect则不行，因为是让浏览器去访问的。

注意Transfer是内部接管，因此不能像Redirect那样重定向到外部网站。 (常考)Response.Redirect就可以重定向到外部网站。

不能内部重定向到ashx，否则会报错“执行子请求出错”.

## 4.Application-->作用于整个运行期的状态对象

Application对象在实际网络开发中的用途就是记录整个网络的信息，如上线人数、在线名单、意见调查和网上选举等。

在给定的应用程序的多有用户之间共享信息，并在服务器运行期间持久的保存数据。

而且Application对象还有控制访问应用层数据的方法和可用于在应用程序启动和停止时触发过程的事件。 

### 1.使用Application对象保存信息  

使用Application对象保存信息

Application(“键名”) = 值 或 Application(“键名”，值) 

获取Application对象信息 

变量名 = Application(“键名”) 或：变量名 = Application.Item(“键名”) 或：变量名 = Application.Get(“键名”) 

更新Application对象的值 

Application.Set(“键名”, 值) 

删除一个键 
Application.Remove(“键名”, 值) 

删除所有键 
Application.RemoveAll() 或Application.Clear() 

### 2.有可能存在多个用户同时存取同一个Application对象的情况。这样就有可能出现多个用户修改同一个Application命名对象，造成数据不一致的问题。  

HttpApplicationState类提供两种方法Lock和Unlock，以解决对Application对象的访问同步问题，一次只允许一个线程访问应用程序状态变量。 

关于锁定与解锁 

锁定：Application.Lock() 

访问：Application(“键名”) = 值 

解锁：Application.Unlock() 

注意：Lock方法和UnLock方法应该成对使用。  

可用于网站访问人数，聊天室等设备 

### 3\. 使用Application事件   

在ASP.NET应用程序中可以包含一个特殊的可选文件—Global.asax文件，也称作ASP.NET应用程序文件，它包含用于响应ASP.NET或HTTP模块引发的应用程序级别事件的代码。  

# Global.asax文件提供了7个事件，其中5个应用于Application对象

事件名称 说明  
Application_Start在应用程序启动时激发  

Application_BeginRequest在每个请求开始时激发  

Application_AuthenticateRequest尝试对使用者进行身份验证时激发  

Application_Error在发生错误时激发  

Application_End在应用程序结束时激发  

## 5.Session-->会话期状态保持对象，用于跟踪单一用户的会话

Session即会话，是指一个用户在一段时间内对某一个站点的一次访问。  

Session对象在.NET中对应HttpSessionState类，表示“会话状态”，可以保存与当前用户会话相关的信息。 

Session对象用于存储从一个用户开始访问某个特定的aspx的页面起，到用户离开为止，特定的用户会话所需要的信息。用户在应用程序的页面切换时，Session对象的变量不会被清除。 

对于一个Web应用程序而言，所有用户访问到的Application对象的内容是完全一样的；而不同用户会话访问到的Session对象的内容则各不相同。 

 Session可以保存变量，该变量只能供一个用户使用，也就是说，每一个网页浏览者都有自己的Session对象变量，即Session对象具有唯一性。 

### （1）将新的项添加到会话状态中 

语法格式为： 

Session (“键名”) = 值 或者 Session.Add( “键名” , 值) 

### （2）按名称获取会话状态中的值 

语法格式为： 

变量 = Session (“键名”)  或者 变量 = Session.Item(“键名”) 

### （3）删除会话状态集合中的项 

语法格式为： Session.Remove(“键名”) 

### （4）清除会话状态中的所有值 

语法格式为： 

Session.RemoveAll() 或者 Session.Clear() 

### （5）取消当前会话 

语法格式为： Session.Abandon() 

### （6）设置会话状态的超时期限，以分钟为单位。 

语法格式为： Session.TimeOut = 数值 

Global.asax文件中有2个事件应用于Session对象  

事件名称 说明 

Session_Start在会话启动时激发 

Session_End在会话结束时激发 

## 6.Cookie-->客户端保持会话信息的一种方式

Cookie就是Web服务器保存在用户硬盘上的一段文本。

Cookie允许一个Web站点在用户的电脑上保存信息并且随后再取回它。信息的片断以‘键/值’对的形式存储。  

Cookie是保存在客户机硬盘上的一个文本文件，可以存储有关特定客户端、会话或应用程序的信息，在.NET中对应HttpCookie类。  

有两种类型的Cookie：会话Cookie（Session Cookie）和持久性Cookie。

前者是临时性的，一旦会话状态结束它将不复存在；后者则具有确定的过期日期，在过期之前Cookie在用户的计算机上以文本文件的形式存储。  

在服务器上创建并向客户端输出Cookie可以利用Response对象实现。  

Response对象支持一个名为Cookies的集合，可以将Cookie对象添加到该集合中，从而向客户端输出Cookie。  

通过Request对象的Cookies集合来访问Cookie 