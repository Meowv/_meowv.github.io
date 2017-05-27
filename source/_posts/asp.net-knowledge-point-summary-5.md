---
title: Asp.net知识点总结(五)
tags:
  - ASP.NET
  - 技术教程
date: 2015-07-16 19:47:48
---

## ASP.NET高级配置

### Cache缓存机制

缓存的参数：第一个为缓存的主键；第二个人为缓存的值；第三个为缓存的依赖项；第四个为缓存移除的时间；第五个为缓存时间的间隔

### 设置一般缓存

```
//this.Cache.Insert(“w”, DateTime.Now.ToLongTimeString(), null, DateTime.Now.AddSeconds(10), TimeSpan.Zero);
```
<!--more-->
### 设置滑动缓存

```
//this.Cache.Insert(“w”,DateTime.Now.ToLocalTime(),null,DateTime.MaxValue,new TimeSpan(0,0,5));
```

### 缓存依赖为文件

```
//this.Cache.Insert(“w”, DateTime.Now.ToLocalTime(), new CacheDependency(Server.MapPath(“a.html”)));
```

### 缓存数据库

```
// this.Cache.Insert(“w”,DateTime.Now.ToLocalTime(),new SqlCacheDependency(“web.config文件中的配置数据库连接名”,”数据库缓存表名的名字”));
```

# 配置缓存数据库

### 数据库缓存依赖

S服务器名称  -E集成身份验证  -ed启动 -d数据库名称 -et指定缓冲依赖的表名 -t表名**

在vs2010的命令提示符中运行（切换到aspnet_regsql.exe所在的目录）

```
aspnet_regsql -S . -E -ed -d HKCorpData -et -t HKSJ_USERS
```
缓存依赖禁用该数据库
```
aspnet_regsql -S . -E -dd -d HKCorpData
```

### web.config配置文件如下：

connectionString为连接字符串的名称

```
/* <caching>
<sqlCacheDependency enabled=”true”>
<databases>
<add name=”数据库名” connectionStringName=”connectionString” pollTime=”500”/>
</databases>
</sqlCacheDependency>
</caching>*/
```

# session使用

```
/*stateServer数据必须是能可序列化的数据*/
```

### 1.进程内使用

```

session[”key”]=”value”;
object obj=session[”key”];

```

### 2.进程外使用

配置web.config文件：
```
<sessionState mode=”StateServer” stateConnectionString=”tcpip=localhost:42424”></sessionState>
```

### 如果是远程的stateserver配置注册表

命令：cmd → regedit.exe  
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\aspnet_state\Parameters\AllowRemoteConnection

### 3.数据库服务器上使用

```
aspnet_regsql.exe -S 127.0.0.1 -U sa -P 123 -ssadd -sstype c -d SessionTest
```

```
<sessionState mode=”SQLServer” allowCustomSqlDatabase=”true” sqlConnectionString=”Data Source=.;Initial Catalog=SessionTest;uid=sa;pwd=123”></sessionState>
```

# 全局文件的使用

### 全局文件介绍：

```
//网站在iis上开启使用运行的方法，并对变量进行初始化
protected void Application_Start(object sender, EventArgs e)
{
//这里的变量为全局变量，也就是静态变量，可以被整个网站共用例：
Application[”isbackground”] = “bluk”;
//需要使用的这个颜色的时候可以直接调用Application[”isbackground”]
//或者网站访问量
Application[”number”] = 0;
}
//当用户访问网站的时候执行此方法
protected void Session_Start(object sender, EventArgs e)
{
//在这里可以验证用户是否登入或者在线统计
lock (Session[”Id”])
    {
//在线人数统计
Session[”Id”] = (int)Session[”Id”] + 1;
//访问次数统计
Application[”number”] = (int)Application[”number”] + 1;
    }
}
//浏览器请求iis服务器到达管道中的第一个事件，在管道中触发
protected void Application_BeginRequest(object sender, EventArgs e)
{
//在这里可以对用户请求过滤例如：防盗链设置
if (Request.RawUrl.Contains(“images/”))
{
if (Request.UrlReferrer == null || !IsSameDomain(Request.UrlReferrer, Request.Url))
    {
Response.ContentType = “image/jpeg”;
string path = Request.MapPath(“~/daolian.jpg”);
Response.WriteFile(path);
//结束请求
Response.End();
     }
}
}
//判断两个域名是否相等
public bool IsSameDomain(Uri u1, Uri u2)
{
return Uri.Compare(u1, u2, UriComponents.HostAndPort, UriFormat.SafeUnescaped, StringComparison.CurrentCultureIgnoreCase) == 0 ? true : false;
}
//管道中的第二个事件，验证请求，开始检查用户的身份
protected void Application_AuthenticateRequest(object sender, EventArgs e)
{
//一般用来获取用户信息
//获取用户的访问证书
HttpClientCertificate hr = new HttpClientCertificate();
hr=  Context.Request.ClientCertificate;
//获取用户的访问地址
string str=  Request.UserHostAddress;
}
//网站出错时运行此方法
protected void Application_Error(object sender, EventArgs e)
{
//可以将访问的地址更改为其他不出错的地址页面上去
Response.Redirect(Server.MapPath(“a.html”));
}
//当用户下线或者不在访问该网站时执行此方法
protected void Session_End(object sender, EventArgs e)
{
lock (Session[”Id”])
    {
if ((int)Session[”Id”] > 0)
        {
Session[”Id”] = (int)Session[”Id”] - 1;
        }
    }
}
//网站停止或者重启时运行此方法
protected void Application_End(object sender, EventArgs e)
{
//可以保存当前网站的一些信息到文件中
//将网站的访问量包存到文件中
File.WriteAllText(“log.txt”, Application[”number”].ToString());
}
```

# 同时管道中的事件也可以在这里注册，下面是请求管道中的已经公开的19个事件：

### (1)BeginRequest: 开始处理请求

### (2)AuthenticateRequest授权验证请求，获取用户授权信息

### (3):PostAuthenticateRequest获取成功

### (4): AunthorizeRequest授权，一般来检查用户是否获得权限

### (5):PostAuthorizeRequest:获得授权

### (6):ResolveRequestCache:获取页面缓存结果

### (7):PostResolveRequestCache已获取缓存   当前请求映射到MvcHandler（pr）：  创建控制器工厂 ，创建控制器，调用action执行，view→responseaction   Handler : PR()

### (8):PostMapRequestHandler创建页面对象:创建 最终处理当前http请求的Handler  实例：  第一从HttpContext中获取当前的PR Handler   ，Create

### (9):PostAcquireRequestState获取Session

### (10)PostAcquireRequestState获得Session

### (11)PreRequestHandlerExecute:准备执行页面对象

执行页面对象的ProcessRequest方法

### (12)PostRequestHandlerExecute执行完页面对象了

### (13)ReleaseRequestState释放请求状态

### (14)PostReleaseRequestState已释放请求状态

### (15)UpdateRequestCache更新缓存

### (16)PostUpdateRequestCache已更新缓存

### (17)LogRequest日志记录

### (18)PostLogRequest已完成日志

### (19)EndRequest完成、

### IHttpModule接口的使用：

此接口中有两个方法Dispose和Init，在Init中也可以注册管道中的19事件

## 配置web.config文件：

### 在system.web节点下配置

```
<httpModules>
<add name=”文件名称” type=”类的全名称”/>
<!--<add name=”MyHttpModuleDemo” type=”CZBK.WebSite.Web.HttpModule.MyHttpModuleDemo”/>-->
</httpModules>
//对象释放式执行
public void Dispose()
{}
/// <summary>
/// 初始化时执行，可以对管道中的公开的事件进行注册
/// </summary>
/// <param name=”context”>上下文对像</param>
public void Init(HttpApplication context)
{
//例：注册管道中的第一事件
//注册开始处理请求事件
context.BeginRequest += new EventHandler(context_BeginRequest);
}
/// <summary>
///注册管道中的第一个事件的方法
/// </summary>
/// <param name=”sender”>请求信息</param>
/// <param name=”e”>与事件有关的基类对象</param>
private void context_BeginRequest(object sender, EventArgs e)
{
//将sender中的信息转化为应用程序的基类对像
HttpApplication application = sender as HttpApplication;
application.Context.Response.Write(“我是在继承了IHttpModle的类中注册的管道中的事件”);
}
```

### 错误页配置：

在web.config文件中的system.web配置：
```
<customErrors mode=”错误页配置模式：On开启, Off关闭，RemoteOnly远程开启  “ defaultRedirect=”默认错误发生时跳转的页面”>
<error statusCode=”http状态错误代码” redirect=”指定跳转的页面”/>
</customErrors>
```
例：
```
<customErrors mode=”RemoteOnly” defaultRedirect=”myErrorpage.aspx”>
<error statusCode=”404” redirect=”error.html”/>
<error statusCode=”403” redirect=”error.html”/>
</customErrors>

```

## url重写原理

```

void Application_BeginRequest(object sender, EventArgs e)
    {
//url重写
HttpApplication app = sender as HttpApplication;
string url = app.Request.RawUrl;
/根据请求的地址解析出实际地址来
Regex r = new Regex(“/(\\d+)/details\\.htm",RegexOptions.IgnoreCase);
Match m = r.Match(url);
if (m.Success)
        {
string id = m.Groups[1].Value;
app.Context.RewritePath(“~/PhotoDetails.aspx?id=” + id);
        }
    }

```

# urlRewriter

### 1、在<configSections>节点加入

```

<section name=”RewriterConfig”type=”URLRewriter.Config.RewriterConfigSerializerSectionHandler, URLRewriter” />

```

### 2、在</configSections>之后加入   

```

<RewriterConfig>
<Rules>
<RewriterRule>
<LookFor>~/(\d{4})/(\d{2})/Default\.aspx</LookFor>
<SendTo>~/Default.aspx?ID=$1</SendTo>
</RewriterRule>
</Rules>
</RewriterConfig>

```

### 3、<httpHandlers>中加入

```

<add verb=”*” path=”*.aspx” type=”URLRewriter.RewriterFactoryHandler, URLRewriter” />

```

## 数据绑定：

### 1.<%#Eval()%>Eval单向绑定,只是计算表达式的值输出

### 2.<%: %>将绑定的数据进行格式化后再显示在页面上

### 3.<%= %>将绑定的数据直接输出来不做什么处理

### 4.<%#Bind()%>双向绑定,不仅可以计算表达式的值输出，还可以将用户填入的值更新到数据中

### 5.第二种方法的方法重载:

```

<a href=’<%# Eval(“userId”,”Default.aspx?id={0}”)%>’><%# Eval(“userName”) %></a>

```

### 6.eval同时绑定两个值

```

<a href=’<%# string.Format(“Default.aspx?id={0}&role={1}”, Eval(“userId”),Eval(“userRole”))%>’><%# Eval(“userName”) %></a>

```

注意%和后面的#不能有空格或其他发符号

### WebRequest类使用:

```

PageUrl = “http://xj8c.cc “; //需要获取源代码的网页
WebRequest request = WebRequest.Create(PageUrl); //WebRequest.Create方法，返回WebRequest的子类HttpWebRequest
WebResponse response = request.GetResponse(); //WebRequest.GetResponse方法，返回对Internet请求的响应
Stream resStream = response.GetResponseStream(); //WebResponse.GetResponseStream方法，从Internet资源返回数据流。
Encoding enc = Encoding.GetEncoding(“GB2312”); // 如果是乱码就改成utf-8 / GB2312
StreamReader sr = new StreamReader(resStream, enc); //命名空间:System.IO。 StreamReader类实现一个TextReader (TextReader类，表示可读取连续字符系列的读取器)，使其以一种特定的编码从字节流中读取字符。
ContentHtml.Text = sr.ReadToEnd(); //输出(HTML代码)，ContentHtml为Multiline模式的TextBox控件
resStream.Close();
sr.Close();

```