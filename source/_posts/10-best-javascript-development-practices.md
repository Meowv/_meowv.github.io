---
title: 10个最佳 javascript 开发实践
tags:
  - JavaScript
date: 2015-09-01 19:46:01
---

使用很多javascript代码的web页面会加载很慢，过多使用javascript会使网页丑陋拖沓……如何有效使用javascript是一个非常火热的话题。在此列出10个最佳javascript实践，帮助你有效地使用javascript。
<!--more-->
## 1、尽可能保持代码简洁

### 可能大家都听到过了N遍这个代码简洁问题了。作为一个开发人员你可能在你的代码开发过程中使用了很多次，但千万不要在js开发中忘记这点。

### 尽量在开发模式中添加注释和空格，这样保持代码的可读性

### 在发布到产品环境前请将空格和注释都删除，并且尽量缩写变量和方法名

### 使用第三方工具帮助你实现压缩 javascript。

## 2、思考后再修改prototypes

### 添加新的属性到对象 prototype 中是导致脚本出错的常见原因。

```
yourObject.prototype.anotherFunction = ‘Hello’;
yourObject.prototype.anotherMethod = function () { … };
```

### 在上面代码中，所有的变量都会被影响，因为他们都继承于yourObject。这样的使用会导致意想不到的行为。所以建议在使用完后删除类似的修改。

```
yourObject.prototype.anotherFunction = ‘Hello’; yourObject.prototype.anotherMethod = function () { … }; test.anotherMethod();
delete yourObject.prototype.anotherFunction = ‘Hello’;
delete yourObject.prototype.anotherMethod = function () { … };
```

## 3、Debug Javascript代码

### 即使最好的开发人员都会犯错。为了最大化减少类似错误，请在你的 debugger 中运行你的代码，确认你没有遇到任何细微的错误。

## 4、避免 Eval

### 你的JS在没有eval方法的时候也可以很好的工作。eval允许访问 javascript 编译器。如果一个字符串作为参数传递到eval，那么它的结果可以被执行。

### 这会很大的降低代码的性能。尽量避免在产品环境中使用eval。

## 5、最小化DOM访问

### DOM是最复杂的API，会使得代码执行过程变慢。有时候web页面可能没有加载或者加载不完整。最好避免DOM。

## 6、在使用js类库之前先学习javascript

### 互联网充斥着很多的javascript类库，很多程序员都往往使用js类库而不理解负面影响。强烈建议你在使用第三方类库之前学习基本的JS代码，否则，你就准备着倒霉吧。

## 7、不要用 "SetTimeOut" 和 "Setinterval" 方法来作为 "Eval" 的备选

```
setTimeOut( "document.getID('value')", 3000);
```

### 在以上代码中 document.getID('value') 在 setTimeOut 方法中被作为字符串来处理。这类似于eval方法，在每个代码执行中来执行一个字符串，因此会降低性能，因此，建议在这些方法中传递一个方法。

```
setTimeOut(yourFunction, 3000);
```

## 8、[] 比 new Array(); 更好

### 一个常犯的错误在于使用当需要数组的时候使用一个对象或者该使用对象的时候使用一个数组。但是使用原则很简单：

### “当属性名称是小的连续整数，你应该使用数组。否则，使用一个对象” - Douglas Crockford, JavaScript: Good Parts 的作者。

建议：

```
var a = ['1A','2B'];
```

避免：

```
var a = new Array();
a[0] = "1A";
a[1] = "2B";
```

## 9、尽量不要多次使用var

### 在初始每一个变量的时候，程序员都习惯使用var关键字。相反，建议你使用逗号来避免多余的关键字，并且减少代码体积。如下：

```
var variableOne = ‘string 1’,
variableTwo = ‘string 2’,
variableThree = ‘string 3’;
```

## 10、不要忽略分号 ";"

### 这往往是大家花费数个小时进行 debug 的原因之一。

### 我很确信你肯定也在其它的文章中阅读过以上相关的内容，但是大家可能往往都忽略了很多基本的规则。你是不是也曾经忽略过分号，是不是也遇到过eval关键字问题导致性能问题？