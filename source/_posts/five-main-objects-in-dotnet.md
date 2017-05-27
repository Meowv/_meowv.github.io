---
title: ADO.NET中的五个主要对象？
tags:
  - ASP.NET
  - C#
date: 2016-04-07 23:34:02
---

### Connection

主要是开启程序和数据库之间的连接，没有利用连接对象将数据库打开，是无法从数据库中取得数据的。

Close和Dispose的区别，Close以后还可以Open，Dispose以后则不能再用。

### Command

主要可以用来对数据库发出一些指令，例如可以对数据库下达增、删、改、查数据等指令，以及调用存在数据库中的存储过程等。

这个对象是架构在Connection对象上，也就是Command对象是透过连接到数据源。

### DataAdapter

主要是在数据源以及DataSet之间执行数据传输的工作，它可以透过Command对象下达命令后，并将取得的数据放入DataSet对象中。

这个对象是架构在Command对象上，并提供了许多配合DataSet使用的功能。
<!--more-->
### DataSet

这个对象可以视为一个暂存区(Cache)，可以把从数据中所查询到的数据保留起来，甚至可以将整个数据库显示出来，DataSet是存在内存中。

DataSet的能力不只是可以存储多个Table而已，还可以透过DataAdapter对象取得一些例如主键等的数据表结构，并可以记录数据表间的关联。

DataSet对象可以说是ADO.NET中重量级的对象，这个对象架构在DataAdapter对象上，本身不具备和数据源沟通的能力；也就是说我们是将DataAdapter对象当做DataSet对象以及数据源传输数据的桥梁。

DataSet包含若干DataTable、DataTable包含若干DataRow。

### DataReader

当我们只需要循序的读取数据而不需要其它操作时，可以使用DataReader对象。

DataReader对象只是一次一笔向下循序的读取数据源中的数据，这些数据存在数据库服务器中，而不是一次性加载到程序的内存中，只能通过游标读取当前行的数据，而且这些数据是只读的，并不允许做其它的操作。

因为DataReader字啊读取数据的时候限制了每次只读取一笔，而且只能只读，所以使用起来不但节省资源而且效率很好。

使用DataReader对象除了效率较好之外，因为不能把数据全部传回，故可以降低网络的负载。

ADO.NET使用Connection对象来连接数据库，使用Command或DataAdapter，然后再使用取得的DataReader或DataAdapter对象操作数据结果。