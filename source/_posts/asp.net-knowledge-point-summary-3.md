---
title: Asp.net知识点总结(三)
tags:
  - ASP.NET
  - 技术教程
date: 2015-07-07 20:30:58
---

## 关于服务器响应码：（常用）

浏览器向服务器发出请求，服务器处理可能是成功、可能是失败、可能没有权限访问等原因，服务器会通过响应码来告诉浏览器处理结果。

“200” : OK

“302” : Found重定向.

“400” : Bad Request错误请求，发出错误的不符合Http协议的请求

“403” : Forbidden禁止

“404” : Not Found未找到。演示访问一个不存在的页面看报文

“500” : Internal Server Error服务器内部错误。演示页面抛出异常。

“503” : Service Unavailable。一般是访问人数过多。

200段是成功；300段需要对请求做进一步的处理；400段表示客户端请求错误；500段是服务器的错误。
<!--more-->
## 关于报文响应一些属性介绍：

Server: Cassini/3.5.0.5表示服务器的类型

Content-Type: text/html; charset=utf-8表示返回数据的类型

Content-Length: 19944表示响应报文体的字节长度，报文头只是描述，返回的具体数据（比如HTML文本、图片数据等）在两个回车之后的内容中。

# HTTP协议的主要特点可概括如下：

### 1.支持客户/服务器模式。

### 2.简单快速：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。

### 3.灵活：HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记。

### 4.无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。

### 5.无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

## get和post

Get和post是表单提交数据的两种基本方式，get请求数据通过域名后缀url传送，用户可见，不安全，post请求数据通过在请求报文正文里传输，相对比较安全。

get是通过url传递表单值，post通过url看不到表单域的值；

get传递的数据量是有限的，如果要传递大数据量不能用get，比如type=“file”上传文章、type=“password”传递密码或者< text area >发表大段文章，post则没有这个限制；

post会有浏览器提示重新提交表单的问题，get则没有(加分的回答)

对于Post的表单重新敲地址栏再刷新就不会提示重新提交了，因为重新敲地址就没有偷偷提交的数据了。Post方式的正确的地址很难直接发给别人。

## GET和POST的区别

### 1\. GET提交的数据会放在URL之后，以?分割URL和传输数据，参数之间以&相连，如EditPosts.aspx?name=test1&id=123456\.  POST方法是把提交的数据放在HTTP包的Body中.

### 2\. GET提交的数据大小有限制（因为浏览器对URL的长度有限制），而POST方法提交的数据没有限制.

### 3\. GET方式需要使用Request.QueryString来取得变量的值，而POST方式通过Request.Form来获取变量的值。

### 4\. GET方式提交数据，会带来安全问题，比如一个登录页面，通过GET方式提交数据时，用户名和密码将出现在URL上，如果页面可以被缓存或者其他人可以访问这台机器，就可以从历史记录获得该用户的账号和密码.

### GET是从服务器上获取数据，POST是向服务器传送数据。

### GET是把参数数据队列加到提交表单的ACTION属性所指的URL中，值和表单内各个字段一一对应，在URL中可以看到。POST是通过HTTP POST机制，将表单内各个字段与其内容放置在HTML HEADER内一起传送到ACTION属性所指的URL地址。用户看不到这个过程。

### 对于GET方式，服务器端用Request.QueryString获取变量的值，对于POST方式，服务器端用Request.Form获取提交的数据

### GET传送的数据量较小，不能大于2KB（这主要是因为受URL长度限制）。POST传送的数据量较大，一般被默认为不受限制。但理论上，限制取决于服务器的处理能力。

### GET安全性较低，POST安全性较高。因为GET在传输过程，数据被放在请求的URL中，而如今现有的很多服务器、代理服务器或者用户代理都会将请求URL记录到日志文件中，然后放在某个地方，这样就可能会有一些隐私的信息被第三方看到。另外，用户也可以在浏览器上直接看到提交的数据，一些系统内部消息将会一同显示在用户面前。POST的所有操作对用户来说都是不可见的。 

## 页面生命周期

### 1.当用户在浏览器地址栏输入网址址按回车之后,浏览器将请求报文进行封装,然后通过Socket套接字(三次握手)建立连接,将域名转成ip地址发送到服务器.

### 2.当服务器接收到请求时,首先到达内核模块(kernel Modal) HTTP.SYS,而它的职责就是对当前请求做最基本的解析,解析出来当前的请求是什么(html,jpg,ashx,aspx),请求地址是什么,然后访问注册表,看下当前的请求需要交给谁处理,查看了一下,要交给IIS处理。

### 3.IIS接收到请求之后,首先去访问一下INetInfo.exe(IIS主进程)里面的元数据信息.INetInfo.exe主要去查询当前的请求(根据后缀名)要交给哪个扩展程序进行处理.(aspx,ashx都是交给AspNet_ISAPI.DLL)

### 4.aspnet_isapi.dll负责启动aspnet Runtime创建aspnet运行环境,将请求交给ISAPIRuntime的PR方法.

### 5.调用ISAPIRuntime.PR通过ecb句柄创建一个HttpWorkRequest对象,就是对http请求报文的简单封装.
### ecb:操作系统的句柄,指向了当前请求的内存空间地址,可以通过句柄拿到当前请求的报文.

### 6.调用HttpRuntime.PR HttpWorkRequest对象根据HttpWorkRequest对象封装一个HttpContext,它包含了所有的请求信息,HttpRequest、HttpResponse.

### 7.根据HttpApplicationFactory获得一个HttpApplication对象(获取实例的时候,先去Application池中去找是否有空闲的HttpApplication对象,如果有直接返回,没有则先编译globle文件生成一个HttpApplication的派生类,然后通过反射创建一个HttpApplication类型实力并返回).

### 8.开始处理请求,进入HttpApplication处理管道,开始执行19个事件23个步骤，只要是没ASP.Net的请求,都会经过这个管道,并且每一个请求都会经过这个处理管道.管道中所流动的是HttpContext上下文对象,在执行第7和第8个事件之间,先判断当前上下文里面有没有最终处理请求的handler的实例,如果没有则 根据请求地址创建一般处理程序或是aspx页面类型实例。

### 9.在处理管道的第9个至第11个事件中,先尝试将页面对象转换成IRequestSessionState接口对象,如果转换不成功,则不加载Session对象，转换成功则将浏览器发送过来的SessionId,根据此值到服务器的Session池中找到对应的Session对象,并将它赋值给页面对象的Session属性(Page.HttpContext.HttpSessionState).

### 10.在处理管道的第11个至第12个事件中,执行在第7-8个事件之间所创建的IHttpHandler实例的PR方法.

如果是一般处理程序:开发人员所写的,执行完成就OK了.

如果是Aspx页面那么就走页面的生面周期.

# 主要步骤:

### 1.创建页面控件树,创建整个页面控件树的结果普通的C#代码被编译到一个方法体重

### 2.判断IsPostBack,确定是否回发,通过ViewState实现若ViewState不为null,则为true

### 3.初始化:PreInit()预初始化

### 4.加载控件状态(Load Control State)

### 5.加载视图状态(Load View State)

### 6.加载回传数据(Load Post Data)

### 7.预加载(PreLoad)

### 8.加载(Load):

### 9.加载回传数据(Load Post Data)

### 10.引发回传数据修改事件(Raise Post Data Changed Event)

### 11.引发回传事件(Raise PostBack Event)

### 12.完成加载(Load Complete):

### 13.引发回调事件(Raise CallBack Event)

### 14.预呈现(PreRender)

### 15.预呈现完成(PreRender Complete)

### 16.保存控件状态(Save Control State)

### 17.存视图状态(Save View State)

### 18.保存状态完成(Save View Complete)

### 19.呈现

# 简单步骤：

在管道中管道的第十一事件到第十二事件执行页面的一般处理程序或这是页面的PR方法，开始页面的生命周期：

第一步：创建控件树BuildercontroTree内部就是将整个页面控件树的结构创建好，如果是普通的C#代码将被编译到一个方法体里。

第二步：判断是否是ISPostBack,确定当前请求是否是回发，通过Viewstate实现，如果Viewstate不为null，那么ViewState则是true。

第三步：Init方法对控件树赋初始值 包含Preinit init initcomplet

第四步：加载ViewState，加载页面的状态，解析页面上的隐藏域中的ViewState

第五步：处理回发数据，将ViewState解析的数据与提交的数据进行对比，判断改变事件和点击事件，把它们放到一个事件集合，等待后面的方法来触发

第六步：LoadPage加载页面，执行用户编写的LoadPage中的方法

第七步：ProcessPostData第二次处理回发数据

第八步：触发改变的事件和点击事件

第九步：页面加载完成

第十步：预渲染，对控件树进行最后一次处理

第十一步：保存当前页面的状态即SaveViewstate

第十二步：页面渲染

至此整个页面的生命周期结束，形成了页面，回到管道中执行管道中之后的事件。