# Web浏览器中的Javascript
Web浏览器中的Javascript通常称为客户端Javascript。前面所述的大部分例子虽然是合法的Javascript代码，但是却没有特定的上下文，也就是说它们不过是一些运行在不明环境中的代码片段。本文提供了一个可以运行Javascript的上下文。

Web浏览器中的Web页面中，呈现静态信息的页面，叫做文档(document)，相对于文档来说，其他Web页面测感觉上更像是应用。如果需要的话，这些页面可以载入新的信息，因此看起来更加图形化，而非文本化，并且它们可以进行离线操作，以及保存数据到本地，以便丙次访问时进行状态恢复。

## 13.1 客户端Javascript
Window对象是所有客户端Javascript特性和API的主要接入点。它表示Web浏览器的一个窗口或窗体，并且可以用标识符window来引用它。Window对象定义了一些属性，比如，指代Location对象的location属性，Location对象指定当前显示在窗口中的URL，并允许脚本往窗口里载入新的URL：

```javascript
// 设置location属性，从而跳转到新的Web页面
window.location = "http://furzoom.com/";
```

Window对象还定义了一些方法，如alert()，可以弹出一个对话框来显示一些信息。还有setTimeout()，可以注册一个函数，在给定的一段时间之后触发一个回调：

```javascript
// 等待2秒，然后说hello
setTimeout(function() { alert("hello"); }, 2000);
```

上面的代码并没胡显式地使用window属性。在客户端Javascript中，Window对象也是全局对象。这意味着Window对象处于作用域链的顶部，它的属性和方法实际上是全局变量和全局函数。Window对象有一个引用自身的属性，叫做window。如果需要引用窗口对象本身，可以用这个属性，但是如果只是想要引用全局窗口对象的属性，通常并不需要用到window。

Window对象还定义了很多其他重要的属性、方法和构造函数。将在后续文章中介绍。

Window对象中其中一个最重要的属性是document，它引用Document对象，后者表示显示在窗口中的文档。Document对象有一些重要方法，比如getElementById()，可以基于元素id属性的值返回单一的文档元素：

```javascript
// 查找id="timestamp"的元素
var timestamp = document.getElementById("timestamp");
```

getElementById()返回的Element对象有其他重要的属性和方法，比如允许脚本获取它的内容，设置属性值等：

```javascript
// 如果元素为空，则插入当前的日期和时间
if (timestamp.firstChild == null)
	timestamp.appendChild(document.createTextNode(new Date().toString()));
```

查询、遍历和修改文档内容的方法会在后续文章中介绍。

每个Element对象都有style和className属性，允许脚本指定文档元素的CSS样式，或修改应用到元素上的CSS类名。设置这些CSS相关的会改变文档元素的呈现：

```javascript
timestamp.style.backgroundColor = "yellow";
timestamp.className = "highlight";
```

style和className属性和其他CSS编程技术将会在后续文章中进行介绍。

Window、Document和Element对象上另一个重要的属性集合是事件处理程序相关的属性。可以在脚本中为之绑定一个函数，这个函数会在某个事件发生时以异步的方式调用。事件处理程序可以让Javascript代码修改窗口、文档和组成文档的元素的行为。事件处理程序的名是以单词"on"开始的，用法如下：

```javascript
// 点击时，更新内容
timestamp.onclick = function() { this.innerHTML = new Date().toString(); }
```

Window对象的onload处理程序是最重要的事件处理程序之一。当显示在窗口中的文档内容稳定并可以操作时会触发它。Javascript代码通常封闭在onload事件处理程序里。下面是onload处理程序的演示，并展示了客户端Javascript的实例代码，包括查询文档元素、修改CSS类和定义事件处理程序。

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
<style>
.reveal * { display: none; }
.reveal *.handle { display: block; }
</style>
<script>
	window.onload = function() {
		var elements = document.getElementsByClassName("reveal");
		for (var i = 0; i < elements.length; i ++) {
			var elt = elements[i];
			var title = elt.getElementsByClassName("handle")[0];
			addRevealHandler(title, elt);
		}
		
		function addRevealHandler(title, elt) {
			title.onclick = function() {
				if (elt.className == "reveal")
					elt.className = "revealed";
				else if (elt.className == "revealed")
					elt.className = "reveal";
			}
		}
	};
</script>
</head>
<body>
<div class="reveal">
	<h1 class="handle">Click Here to Reveal Hidden Text</h1>
	<p>This paragraph is hidden. It appears when you click on the title.</p>
</div>
</body>
</html>
```

### 13.1.1 Web文档里的Javascript
Javascript程序可以通过Document对象和它包含的Element对象遍历和管理文档内容。它可以通过操纵CSS样式和类，修改文档内容的呈现。并且可以通过注册适当的事件处理程序来定义文档元素的行为。内容、呈现和行为的组合，叫做动态DHTML或DHTML，会在后续文章进行介绍。

### 13.1.2 Web应用里的Javascript

## 13.2 在HTML里嵌入Javascript

### 13.2.1 <script>元素

### 13.2.2 外部文件中的脚本

### 13.2.3 脚本类型

### 13.2.4 HTML中的事件处理程序

### 13.2.5 URL中的Javascript

## 13.3 Javascript程序的执行

### 13.3.1 同步、异步和延迟的脚本

### 13.3.2 事件驱动的Javascript

### 13.3.3 客户端Javascript线程模型

### 13.3.4 客户端Javascript时间线

## 13.4 兼容性和互用性

### 13.4.1 处理兼容性问题的类库

### 13.4.2 分组浏览器支持

### 13.4.3 功能测试

### 13.4.4 怪异模式和标准模式

### 13.4.5 浏览器测试

### 13.4.6 Internet Explorer里的条件注释

## 13.5 可访问性

## 13.6 安全性

### 13.6.1 Javascript不能做什么

### 13.6.2 同源策略

### 13.6.3 脚本化插件和ActiveX控件

### 13.6.4 跨站脚本

### 13.6.5 拒绝服务攻击

## 13.7 客户端框架



Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")
