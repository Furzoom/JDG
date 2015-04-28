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

Element类也定义getElementsByTagName()方法，其原理和Document版本的一样，但是它只选取调用该方法的元素的后代元素。要查找文档中第一个\<p>元素里面的所有\<span>元素，代码如下：

```javascript
var firstpara = document.getElementsByTagName("p")[0];
var fitstParaSpans = firstpara.getElementsByTagName("span");
```

由于历史的原因，HTMLDocument类定义一些快捷属性来访问各种各样的节点。如images、forms和links等属性指向行为类似吟诗数组的\<img>、\<form>和\<a>元素集合。这些属性指代HTMLCollection对象，它们很像NodeList对象，但是险些之外它们还可以用元素的ID或名字来索引。

HTMLDocument也定义embeds和plugins属性，它们是同义词，都是HTMLCollection类型的<embed>元素的集合。anchors是非标准属性，它指代有一个name属性的\<a>元素而并不是一个href属性。还有两个属性，它们指代特殊的单个元素而不是元素的集合。document.body是一个HTML文档的\<body>元素，document.head是\<head>元素。这些属性总是会定义：如果文档源代码未显式地地包含\<head>和\<body>元素。浏览器将隐式地创建它们。Document类的documentElement属性指代文档的根元素。在HTML文档中，它总是指代\<html>元素。

### 15.2.4 通过CSS类选取元素
HTML元素的class属性值是一个以空格隔开的列表，可以为空或包含多个标识符。它描述一种方法来定义多组相关的文档元素：在它们的class属性中有相同标识符的任何元素属于该组的一部分，在JavaScript中class是保留字，所有客户端JavaScript使用className属性来保存HTML的class属性值。class属性通常与CSS样式表一起使用，对某组内的所有元素应用相同的样式。HTML定义了getElementsByClassName()方法，它基于其class属性值中的标识符来选取成组的文档元素。

类似getElementsByTagName()，在HTML文档和HTML元素上都可以调用getElementsByClassName()，它返回值是一个实时的NodeList对象，包含文档或元素所有匹配的后代节点。getElementsByClassName()只需要一个字符串参数，但是该字符串可以由多个空格隔开的标识符组成。只有当元素的class属性值包含所有指定的标识符时才匹配，但是标识符的顺序是无关紧要的。注意，class属性和getElementsByClassName()方法的类标识符之间都是用空格隔开的，而不是逗号。如：

```javascript
var warnings = document.getElementsByClassName("warnings");
var log = document.getElementById("log");
var fatal = log.getElementsByClassName("fatal error");
```

### 15.2.5 通过CSS选择器选取元素
CSS样式表有一种非常强大的语法，那就是选择器，它用来描述文档中的若干或多组元素。元素可以用ID、标签名或类来描述：

```html
#nav			// id="nav"的元素
div				// 所有<div>元素
.warnings		// class属性包含warning的元素
```

更一般地，元素可以基于属性值来选取：

```css
p[lang="fr"]	// 所有使用法语的段落
*[name="x]		// 所有包含name="x"属性的元素
```

这些基本的选择器可以组合使用：

```css
span.fatal.error		// 其class中包含"fatal"和"error"的所有<span>元素
span[lang="fr"].warings	// 所有使用法语且其class中包含warnings的<span>元素
```

选择器可以指定文档结构：

```css
#log span				// id="log"元素的后代元素中的所有<span>元素
#log>span				// id="log"元素的子元素中所有<span>元素
body>h1:first-child		// <body>的子元素中的第一个<h1>元素
```

选择器可以组合起来选取多个或多组元素：

```css
div, #log				// 所有<div>元素和id="log"的元素
```

CSS选择器可以使用上述所有方法选取元素：通过ID、名字、标签名、类名。与CSS3选择器的标准化一起的另一个称做选择器API的W3C标准定义了获取匹配一个给定选择器的元素的JavaScript方法。该API的关键是Document方法querySelectorAll()。它接受包含一个CSS选择器的字符串参数，返回一个表示文档中匹配选择器的所有元素的NodeList对象。与前面描述的选取元素的方法不同，querySelectorAll()返回的NodeList对象并不是实时的：它包含在调用时刻选择器所匹配的元素，但它并不更新后续文档的变化。如果没有匹配的元素，querySelectorAll()将返回一个空的NodeList对象。如果选择器字符串非法，querySelectorAll()将抛出一个异常。

Document对象还定义了querySelector()方法，它只返回第一个匹配的元素，或者没有匹配返回null。

这两个方法在Element节点也有定义。在元素上调用时，指定的选择器仍然在整个文档中进行匹配，然后过滤出结果集以便它只包含指定元素的元素。

注意，CSS定义了":first-line"和":first-letter"等伪元素。在CSS中，它们匹配文本节点的一部分而不是实际元素。如果和querySelectorAll()或querySelector()一起使用它们是不匹配的。而且，很多浏览器会拒绝返回":link"和":visited"等伪类匹配结果，因为这会泄露用户的浏览历史记录。

querySelectorAll()是终极的选取元素的方法：它是一种非常强大的技术，通过它客户端JavaScript程序能够选择它们想要操作的元素。

### 15.2.6 document.all[]
在DOM标准化之前，IE 4引入了document.all[]集合表示所有文档中的元素。

```javascript
document.all[0]					// 文档中第一个元素
document.all["navbar"]			// id或name为"navbar"的元素
document.all.tags("div")		// 文档中所有的<div>元素
```

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
