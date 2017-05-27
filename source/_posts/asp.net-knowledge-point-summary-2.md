---
title: Asp.net知识点总结(二)
tags:
  - ASP.NET
  - 技术教程
date: 2015-07-04 21:02:22
---

## VIEWSTATE

# 简介： 

### ASP.NET的 .aspx页面特有，页面级的；

### 就是在页面上的一个隐藏域中保存客户端单独使用的数据的一种方式；

### 服务器端控件的值都自动保存在ViewState中
<!--more-->
# 方式：

### 用户数据保存方式：

```
ViewState[”myKey”]=”MyData”;
```

### 读取数据方式：

```
string myData;
if(ViewState[”myKey”]!=null)
{  myData=(string)ViewState[”myKey”];  }
```

# 注意：

### ViewState不能存储所有的数据类型，仅支持可序列化对象。

### 非单值服务器控件的状态也自动保存在ViewState中

列入：下拉框。（文本框是单值控件，所以不会保存在ViewState中）

# 原理：

### 使用ViewState的前提：

页面上必须有一个服务器端窗体标记 &lt;form runat=“server”&gt;
服务器在接收到用户请求一个页面后，会自动在请求报文中找看是否包含__VIEWSTATE的隐藏域，如果有，则将中间的值解码后添加到页面的ViewState属性中。
服务器在输出的时候，也会自动的将ViewState中的值添加到表单里名叫__VIEWSTATE的隐藏域中

### VIEWSTATE适用于同一个页面在不关闭的情况下多次与服务器交互

# 关于

### ViewState禁用（知道）：

禁用ViewState的方法，禁用单个控件的ViewState设定enableviewstate=false，禁用ViewState以后TextBox版本不受影响，Div版本受影响，因为input的value不依靠ViewState。
禁用整个页面的，在aspx的Page指令区加上EnableViewState=”false” 。内网系统、互联网的后台可以尽情的用ViewState。
当某些控件的某些属性不属于浏览器表单的提交范围时，fw将会把这些属性添加到ViewState中保存。

### WebForm的IsPostBack依赖于__ViewState

## IsPostBack

IsPostBack是Page类有一个bool类型的属性，用来判断针对当前Form的请求是第一次还是非第一次请求。当IsPostBack＝true时表示非第一次请求，我们称为PostBack，当IsPostBack＝false时表示第一次请求。
IsPostBack是用来判断是否是回发数据的，在页面生命周期的第二步会判断，如果不是回发的，则为false，ViewState等不会加载，如果是回发的，则为true，ViewState等会在之后加载等，IsPostBack属性可以用来模拟ViewState的执行原理。

## http协议

# 什么是协议？

协议是指计算机通信网络中两台计算机之间进行通信所必须共同遵守的规定或规则。

# 什么是HTTP协议？

HTTP,即超文本传输协议是一种通信协议，是一个基于应用层的通信规范，它允许将超文本标记语言(HTML)文档从Web服务器传送到客户端的浏览器。目前主流的是Http/1.1版本

### TCP协议：通俗–两个电话机通过电话线进行数据交互的格式约定

### HTTP协议：通俗–两个人通过电话机说话的语法。

# Http协议的几个概念：

### 1.连接(Connection)：浏览器和服务器之间传输数据的通道。 一般请求完毕就关闭，http不保持连接。不保持连接会降低处理速度（因为建立连接速度很慢），保持连接的话就会降低服务器的处理的客户端请求数，而不保持连接服务器可以处理更多的请求。

### 2.请求(Request)：浏览器向服务器发送的“我要***”的消息，包含请求的类型、请求的数据、浏览器的信息（语言、浏览器版本等）。

### 3.响应(Response)：服务器对浏览器请求的返回的数据，包含是否成功、状态码等。