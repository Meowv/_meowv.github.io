---
title: Asp.net知识点总结(四)
tags:
  - ASP.NET
  - 技术教程
date: 2015-07-11 22:32:58
---

## ASP.Net服务器端控件(部分)

# Repeater

### 1\.  Repeater控件使用数据源返回的一组记录呈现只读列表。与FormView控件类似，Repeater控件不指定内置布局。您可以使用模板创建Repeater控件的布局。

### 2\.  Repeater相当于一个foreach循环.

### 3\.  特征：自由定制；分页功能需要手写。
<!--more-->
### 4.Repeater

a)   头部模版<HeaderTemplate>,通常用来做表格头部信息的展示。
b)   项模版<ItemTemplate> 通常用来做数据的绑定用
c)   交替模版<AlternatingItemTemplate> 通常用来做各行变色,和项模版搭配使用.

### 5\.  数据的绑定

a)   数据的绑定通常和数据源做搭配来绑定数据(数据源会在后面文档最后做介绍)
b)   数据绑定的方式有两种:

```
1>  Eval方式： <td><%# Eval(“ID”) %></td>
2>  Bind方式:  <asp:TextBox ID=”IDTextBox” runat=”server” Text=’<%# Bind(“ID”) %>’ />
```

c)   内部服务器控件的事件的触发规则:

**1>  在服务器端的数据控件中如果包含了其他的服务器控件,那么其他的服务器控件的事件只能通过数据控件(Repeater)的ItemCommand事件来出发,在额外的服务器控件中需要给定CommandName和CommandArgument以方便后台在ItemCommand事件中处理此事件;**
2>  前台代码示例：

3>  后台代码示例：

## ListView

### 1\.  这个控件既可实现像GridView一样的效果，也可实现像DataList一样的效果（怪不得名字叫ListView），这东西应用起来也要复杂些。要点是：LayoutTemplate下面必须有一个服务器端控件，即runat=”server”，其ID必须为itemPlaceholder（除非更改ListView的ItemPlaceholderID），注意大小写，ItemTemplate模板中的内容输出时就是插入到itemPlaceholder的。

### 2\.  但ListView分页却不是那么复杂，在LayoutTemplate模板中拖入一个DataPager控件，指定好DataPager的Fields就可以了。但DataPager并不会向DataSource发送startRowIndex和maximumRows这两个参数，也就是说这种分页是取出所有的记录，只是显示部分，并不是一种高效的分页。。

### 3\.  特征：自由定制，功能强大；应用复杂；分页功能需要手写。

### 4\.  模版包括:

a)   AlternatingItemTemplate: 交替模版: 交替模版<AlternatingItemTemplate> 通常用来做各行变色,和项模版搭配使用.
b)   EditItemTemplate: 编辑模版,通常用来做行的数据编辑:可以在里面放入其他服务器控件: 按钮、 A标签等、
c)   EmptyDataTemplate空模版: 顾名思义,空模版服务器返回数据为空的时候显示的: 也可以在里面自定义其他的空页面
d)   InsertItemTemplate添加模版: 用来做添加数据用的,和普通的添加数据页面一样,只不过数据的绑定方式使用了```Bind(Text=’<%# Bind(“UserName”) %>’)``` 双向数据绑定类型.
e)   ItemTemplate: 数据项目模版,用来绑定数据:绑定方式和Repeater一样也分两种: Eval() 和  Bind();
f)   LayoutTempalate布局模版:

### 1>  使用LayoutTemplate属性可以为ListView控件的根容器定义自定义用户界面 (UI)。

### 2>  若要指定布局模板，请在ListView控件内添加一个LayoutTemplate元素。 然后可以将模板的内容添加到LayoutTemplate元素。 

### 3>  LayoutTemplate内容必须包含一个占位符控件，例如由ItemTemplate模板定义的项或由 

### 4>  GroupTemplate模板定义的组的表行 (tr) 元素。 占位符控件必须将runat特性设置为“server”，

### 5>  将ID特性设置为ItemPlaceholderID或GroupPlaceholderID属性的值（具体取决于ListView控件是否使用组）。 

### 6>  LayoutTemplate模板不是ListView控件所必需的。 您可以使用不带LayoutTemplate的ListView 

### 7>  控件，也可以使用没有占位符服务器控件但具有已知ID的控件。

g)   SelectedItemTemplate选择模版:指定当前选中的内容,可在里面添加按钮a标签或者一个复选框,以实现选择的效果

### 5、数据源控件之ObjectDataSource

# ObjectDataSource的概述

### ObjectDataSource控件对象模型类似于SqlDataSource控件。ObjectDataSource公开一个TypeName属性（而不是ConnectionString属性），该属性指定要实例化来执行数据操作的对象类型（类名）。类似于SqlDataSource的命令属性，ObjectDataSource控件支持诸如SelectMethod、UpdateMethod、InsertMethod和DeleteMethod的属性，用于指定要调用来执行这些数据操作的关联类型的方法。本节介绍一些方法，用于构建数据访问层和业务逻辑层组件并通过ObjectDataSource控件公开这些组件。 下面是该控件的声明方式：

# <asp:ObjectDataSource

```CacheDuration=”string|Infinite” CacheExpirationPolicy=”Absolute|Sliding”```

# CacheKeyDependency=”string”

```
ConflictDetection=”OverwriteChanges|CompareAllValues”
    ConvertNullToDBNull=”True|False”    DataObjectTypeName=”string”
    DeleteMethod=”string”    EnableCaching=”True|False”
    EnablePaging=”True|False”    EnableTheming=”True|False”
    EnableViewState=”True|False”    FilterExpression=\’#\’” >
    ID=”string”    InsertMethod=”string”
MaximumRowsParameterName=”string”
OldValuesParameterFormatString=”string”
OnDataBinding=”DataBinding event handler”
    OnDeleted=”Deleted event handler”    OnDeleting=”Deleting event handler”
    OnDisposed=”Disposed event handler”    OnFiltering=”Filtering event handler”
    OnInit=”Init event handler”    OnInserted=”Inserted event handler”
    OnInserting=”Inserting event handler”    OnLoad=”Load event handler”
OnObjectCreated=”ObjectCreated event handler”
OnObjectCreating=”ObjectCreating event handler”
OnObjectDisposing=”ObjectDisposing event handler”
    OnPreRender=”PreRender event handler”    OnSelected=”Selected event handler”
    OnSelecting=”Selecting event handler”    OnUnload=”Unload event handler”
    OnUpdated=”Updated event handler”    OnUpdating=”Updating event handler”
    runat=”server”    SelectCountMethod=”string”
    SelectMethod=”string”    SortParameterName=”string”
    SqlCacheDependency=”string”    StartRowIndexParameterName=”string”
    TypeName=”string”    UpdateMethod=”string”>
<DeleteParameters>
```

# <asp:ControlParameter        ControlID=”string”

```
ConvertEmptyStringToNull=”True|False”
DefaultValue=”string”
Direction=”Input|Output|InputOutput|ReturnValue”
Name=”string”
PropertyName=”string”
Size=”integer”
Type=”Empty|Object|DBNull|Boolean|Char|SByte|
Byte|Int16|UInt16|Int32|UInt32|Int64|UInt64|
Single|Double|Decimal|DateTime|String”/> 
                <asp:CookieParameter           CookieName=”string” />
                <asp:FormParameter             FormField=”string” />
                <asp:Parameter                  Name=”string” />
                <asp:ProfileParameter          PropertyName=”string” />
                <asp:QueryStringParameter     QueryStringField=”string” />
                <asp:SessionParameter          SessionField=”string”  />
</DeleteParameters>
<FilterParameters>... ...</FilterParameters>
<InsertParameters>... ...</InsertParameters>
<SelectParameters>... ...</SelectParameters>
<UpdateParameters>... ...</UpdateParameters>
</asp:ObjectDataSource>
```

## 绑定到数据访问层

### 数据访问层组件封装ADO.NET代码以通过SQL命令查询和修改数据库。它通常提炼创建ADO.NET连接和命令的详细信息，并通过可使用适当的参数调用的方法公开这些详细信息。典型的数据访问层组件可按如下方式公开： 

```
public class MyDataBllLayer {  
public DataView GetRecords();  
public int UpdateRecord(int recordID, String recordData); 
public int DeleteRecord(int recordID); 
public int InsertRecord(int recordID, String recordData);
}
```
也就是,通常是在业务逻辑访问层定义对数据库里记录的操作，上面就定义了GetRecords、UpdateRecord、DeleteRecord和InsertRecord四个方法来读取、更新、删除和插入数据库里的数据，这些方法基本上是根据SQL里的Select、Update、Delete和Insert语句而定义。
和上面方法相对应， ObjectDataSource提供了四个属性来设置该控件引用的数据处理，可以按照如下方式关联到该类型，代码如下
```
&lt;asp:ObjectDataSource TypeName=”MyDataLayer”   runat=”server”
SelectMethod=”GetRecords” 
UpdateMethod=”UpdateRecord”  
DeleteMethod=”DeleteRecord” 
```

# InsertMethod=”InsertRecord” />

这里的SelectMethon设置为MyDataBllLayer里的GetRecords()方法，在使用时需要注意ObjectDataSource旨在以声明的方式简化数据的开发，所以这里设置SelectMethod的值为GetRecords而不是GetRecords()。

### 同样依次类推，UpdateMethod、DeleteMethod、InsertMethod分别对应的是UpdateRecord

# DeleteRecord、InsertRecord方法。

在上面GetRecords（）的定义时，读者可以看到该方法返回的类型是DataView，由于ObjectDataSource将来需要作为绑定控件的数据来源，所以它的返回类型必须如下的返回类型之一：

### Ienumerable、DataTable、DataView、DataSet或者Object。

除此以外，ObjectDataSource还有一个重要的属性TypeName，ObjectDataSource控件使用反射技术来从来从业务逻辑程序层的类对象调用相应的方法，所以TypeName的属性值就是用来标识该控件工作时使用的类名称，下面通过Simple_ObjectDataSource.aspx来说明ObjectDataSource的基本使用。

## 建立数据业务逻辑层

为了方装业务逻辑我建立了ProductDAL.cs文件。在该文件里定义了GetProduct方法获取产品列表，UpdateProduct方法更新产品记录，DeleteProduct删除产品记录，为了便于共享，将该文件放置在App_Code目录下，完整代码如下：
```
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Configuration;
using System.Data;
using System.Data.Common;
using System.Data.SqlClient;
using System.Web;

/// <summary>
/// Summary description for ProductBLL
/// </summary>
public class ProductDAL
{
    protected int _count = -1;
    public ProductDAL()
    {   }
     string   _connectionString = ConfigurationManager.ConnectionStrings[”ConnectionString”].ConnectionString;
    public SqlDataReader GetProduct()
    {
        SqlConnection con = new SqlConnection(_connectionString);
        string selectString = “SELECT * FROM Products”;
        SqlCommand cmd = new SqlCommand(selectString, con);
        con.Open();
        SqlDataReader dtr = cmd.ExecuteReader(CommandBehavior.CloseConnection);
        return dtr;
         }
    public void UpdateProduct(int productID, string productName, int categoryID, decimal price, Int16 inStore,string description)
    {
        SqlConnection con = new SqlConnection(_connectionString);
        string updateString = “UPDATE Products set ProductName=@ProductName,CategoryID=@CategoryID,Price=@Price,InStore=@InStore,Description=@Description where ProductID=@ProductID”;
        SqlCommand cmd = new SqlCommand(updateString, con);
        cmd.Parameters.AddWithValue(“@ProductID”,productID);
        cmd.Parameters.AddWithValue(“@ProductName”,productName);
        cmd.Parameters.AddWithValue(“@CategoryID”,categoryID);
        cmd.Parameters.AddWithValue(“@Price”,price);
        cmd.Parameters.AddWithValue(“@InStore”,inStore);
        cmd.Parameters.AddWithValue(“@Description”,description);
        con.Open();
        cmd.ExecuteNonQuery();
        con.Close();
    }
    public   void DeleteProduct(int ProductId)
        {
            SqlConnection con = new SqlConnection(_connectionString);
            string deleteString = “DELETE FROM  Products  WHERE ProductID=@ProductID”
            SqlCommand cmd = new SqlCommand(deleteString, con);
            cmd.Parameters.AddWithValue(“@ProductID”, ProductId);
            con.Open();
            cmd.ExecuteNonQuery();
            con.Close();
        }
}
```

## 建立表示层

建立一个页面Simple_ObjectDataSource.aspx然后将ObjectDataSource控件托方到Web窗体创，使用默认的ID。Visual Stduio为我们建立业务逻辑提供了强大的支持。选中ObjectDataSource1，在其智能配置里选择配置数据源。
……
最后生成代码如下：
```
<asp:ObjectDataSource ID=”ObjectDataSource1” runat=”server” DeleteMethod=”DeleteProduct”
            SelectMethod=”GetProduct” TypeName=”ProductDAL” UpdateMethod=”UpdateProduct”>
            <DeleteParameters>
                <asp:Parameter Name=”ProductId” Type=”Int32” />
            </DeleteParameters>
            <UpdateParameters>
                <asp:Parameter Name=”productID” Type=”Int32” />
                <asp:Parameter Name=”productName” Type=”String” />
                <asp:Parameter Name=”categoryID” Type=”Int32” />
                <asp:Parameter Name=”price” Type=”Decimal” />
                <asp:Parameter Name=”inStore” Type=”Int16” />
                <asp:Parameter Name=”description” Type=”String” />
            </UpdateParameters>
        </asp:ObjectDataSource>
```