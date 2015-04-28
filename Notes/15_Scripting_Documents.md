# 脚本化文档
客户端JavaScript的存在使得静态的HTML文档变成了交互式的Web应用。脚本化Web页面内容是JavaScript的核心目标。每个Window对象有一个document属性引用了Document对象。Document对象表示窗口的内容，它是一个巨大的API中的核心对象，叫做文档对象模型(Document Object Model, DOM)，它代表和操作文档的内容。

## 15.1 DOM概览
文档对象模型(DOM)是表示和操作HTML和XML文档内容的基础API。API不是特别复杂，但是需要理解大量在架构细节。首先，应该理解HTML和XML文档的嵌套元素在DOM树对象中的表示。HTML文档的树状结构包含表示HTML标签和元素和表示文本字符串的节点，它也可能包含表示HTML注释的节点。

```html
<html>
  <head>
    <title>Sample Document</title>
  </head>
  <body>
    <h1>An HTML Documnet</h1>
	<p>This is a <i>simple</i> document.
  </body>
</html>
```

上面的代码中包含3种不同类型的节点。树形的根部是Document节点，它代表整个文档。代表HTML元素的节点是Element节点，代表文本的节点是Text节点。Document、Element和Text是Node的子类。Document和Element是两个重要的DOM类。

## 15.2 选取文档元素
大多数客户端JavaScript程序运行时总是在操作一个或多个文档元素。当这些程序启动时，可以使用全局变量document来引用Document对象。但是，为了操作文档中的元素，必须通过某种方式获得或选取这些引用文档元素的Element对象。DOM定义许多方式来选取元素，查询文档的一个或多个元素有如下方法：

* 用指定的id属性。
* 用指定的name属性。
* 用指定的标签名字。
* 用指定的CSS类。
* 匹配指定的CSS选择器。

### 15.2.1 通过ID选取元素
任何HTML元素可以有一个id属性，在文档中该值必须唯一，即同一个文档中的两个元素不能有相同的ID。可以用Document对象的getElementById()方法选取一个基于唯一ID的元素。如：

```javascript
var section1 = document.getElementById("section1");
```

如果想要操作某一组指定的文档元素，提供这些元素的id属性值，并使用ID查找这些Element对象。如果需要通过ID查找多个元素，会发现下面的getElements()函数非常有用：

```javascript
/* 
 * 函数接受任意多的字符串参数
 * 每个参数将当做元素的id传递给document.getElementById()
 * 返回一个对象，它把这些id映射到对应Element对象
 * 如任何一个id对应的元素未定义，则抛出一个Error对象
 */
function getElements(/* ids... */) {
	var elements = {};
	for (var i = 0; i < arguments.length; i++) {
		var id = arguments[i];
		var elt = document.getElementById(id);
		if (elt == null)
			throw new Error("No element with id: " + id);
		elements[id] = elt;
	}
	return elements;
}
```

在低于IE 8版本的浏览器中，getElementById()对匹配元素的ID不区分大小写，而且也返回匹配name属性的元素。

### 15.2.2 通过名字选取元素
HTML的name属性最初打算为表单元素分配名字，在表彰数据提交到服务器时使用该属性的值。类似id属性，name是给元素分配名字，但是区别于id，name属性的值不是必须唯一：多个元素可能有同样的名字，在表单中，单选和复选按钮通常是这种情况。而且，和id不一样的是name属性只在少数HTML元素中有效，包括表单、表单元素、\<iframe>和\<img>元素。

基于name属性的值选取HTML元素，可以使用Document对象的getElementsByName()方法：

```javascript
var radiobuttons = document.getElementsByName("favorite_color");
```

getElementsByName()定义在HTMLDocument类中，而不在Document类中，所以它只针对HTML文档可用，在XML文档中不可用。它返回一个NodeList对象，后者的行为类似一个包含若干Element对象的只读数组。在IE中，getElementsByName()也返回id属性匹配指定值的元素。为了兼容，应该小心谨慎，不要将同样的字符串同时用做名字和ID。

为\<form>、\<img>、\<iframe>、\<applet>、\<embed>或\<object>元素设置name属性值，即在Document对象中创建以此name属性值为名字的属性。

如果给定的名字只有一个元素，自动创建的文档属性对应的该值是元素本身。如果有多个元素，该文档属性的值是一个NodeList对象，它表现为一个包含这些元素的数组。这就意味着有些元素可以作为Document属性仅通过名字来选取：

```javascript
// 针对<form name="shipping_address">元素，得到Element对象
var form = document.shipping_address;
```

### 15.2.3 通过标签选取元素
Document对象的getElementsByTagName()方法可用来选取指定类型(标签名)的所有HTML或XML元素。如：

```javascript
var spans = document.getElementsByTagName("span");
```

类似于getElementsByName()，getElementsByTagName()返回一个NodeList对象。在NodeList中返回的元素按照在文档中的顺序排序的，所以可用如下代码选取文档中的第一个\<p>元素：

```javascript
var firstpara = document.getElementsByTagName("p")[0];
```

HTML标签是不区分大小写的，当在HTML文档中使用getElementsByTagName()时，它进行不区分大小写的标签名比较。

给getElementsByTagName()传递通配符参数"*"将获得一个代表文档中所有元素的NodeList对象。


### 15.2.4 通过CSS类选取元素

### 15.2.5 通过CSS选择器选取元素

### 15.2.6 document.all[]

## 15.3 文档结构和遍历

### 15.3.1 作为节点树的文档

### 15.3.2 作为元素树的文档

## 15.4 属性

### 15.4.1 HTML属性作为Element的属性

### 15.4.2 获取和设置非标准HTML属性

### 15.4.3 数据集属性

### 15.4.4 作为Attr节点的属性

## 15.5 元素的内容

### 15.5.1 作为HTML的元素内容

### 15.5.2 作为纯文本的元素内容

### 15.5.3 作为Text节点的元素内容

## 15.6 创建、插入和删除节点

### 15.6.1 创建节点

### 15.6.2 插入节点

### 15.6.3 删除和替换节点

### 15.6.4 使用DocumentFragment

## 15.7 例子：生成目录表

## 15.8 文档和元素的几何形状和滚动

### 15.8.1 文档坐标和视口坐标

### 15.8.2 查询元素的几何尺寸

### 15.8.3 判定元素在某点

### 15.8.4 滚动

### 15.8.5 关于元素尺寸、位置和溢出的更多信息

## 15.9 HTML表单

### 15.9.1 选取表单和表单元素

### 15.9.2 表单和元素的属性

### 15.9.3 表单和元素的事件处理程序

### 15.9.4 按钮

### 15.9.5 开关按钮

### 15.9.6 文本域

### 15.9.7 选择框和选项元素

## 15.10 其他文档特性

### 15.10.1 Document的属性

### 15.10.2 document.write()方法

### 15.10.3 查询选取的文本

### 15.10.4 可编辑的内容


Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")
