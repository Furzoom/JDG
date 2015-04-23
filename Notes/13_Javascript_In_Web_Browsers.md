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

Web文档应当少量地使用Javascript，因为Javascript真正的角色是增强用户的浏览体验，使信息的获取和传递更容易。用户的体验不应依赖于Javascript，但Javascript可以增加体验，如：
* 创建动画和其他视觉效果，巧妙地引导和帮助用户进行页面导航。
* 对表格的列进行分组，让用户更容易找到所需要的。
* 隐藏某些内容，当用户深入到内容里时，再逐渐展示详细信息。

### 13.1.2 Web应用里的Javascript
在Web文档中使用的Javascript DHTML特性在Web应用中都会用到，对于Web应用来说，除了内容、呈现和操作API之外，还依赖了Web浏览器环境提供的基础服务。谨记Web浏览器是简单操作系统的概念，可以把Web应用定义为用Javascript访问更多浏览器提供的高级服务的Web页面。

## 13.2 在HTML里嵌入Javascript
在HTML文档里嵌入客户端Javascript代码有4种方法：
* 内联，放置在<script>和</script>标签对之间。
* 放置在由<script>标签的src属性指定的外部文件中。
* 放置在HTML事件处理程序中，该事件处理程序由onclick或onmouseover这样的HTML属性值指定。
* 放在一个URL里，这个URL使用特殊的"Javascript:"协议。

### 13.2.1 <script>元素
Javascript代码可以以内联的形式出现在HTML文件里的<script>和</script>标签之间：

```javascript
<script>
// 这里是你的Javascript代码
</script>
```

在XHTML中，<script>标签中的内容被当做其他内容一样对待。如果Javascript代码包含了"<"或"&"字符，那么这些字符就被解释成为XML标记。因此，如果要使用XHTML，最好把所有的Javascript代码放入到一个CDATA部分里：

```javascript
<script><![CDATA[
// 这里是你的Javascript代码
]]></script>
```

下面的例子展示了一个HTML文件，它包含简单的Javascript程序。注释解释了这个程序是做什么的，但这个例子主要演示的是Javascript代码以及CSS样式表是如何嵌入HTML文件里，注意这个例子和上面的结构类似，并同样使用onload事件处理程序。

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
<title>Digital Clock</title>
<script>
// 显示当前时间的函数
function displayTime() {
	var elt = document.getElementById("clock");
	var now = new Date();
	elt.innerHTML = now.toLocaleTimeString();
	setTimeout(displayTime, 200);
}
window.onload = displayTime;
</script>
<style>
#clock {
font: bold 24pt sans;
background: #ddf;
padding: 10px;
border: solid black 2px;
border-radius: 10px;
}
</style>
</head>
<body>
<h1>Digital Clock</h1>
<span id="clock"></span>
</body>
</html>
```

### 13.2.2 外部文件中的脚本
<script>标签支持src属性，这个属性指定包含Javascript代码的文件的URL。它的用法如：

```javascript
<script src="../../scripts/util.js"></script>
```

Javascript文件的扩展名通常是以.js结尾的。它包含纯粹的Javascript代码，其中既没有<script>标签，也没有其他HTML标签。

具有src属性的<script>标签的行为就像指定的Javascript文件的内容直接出现在标签<script>和</script>之间一样。注意，即使指定了src属性并且<script>和</script>标签之间没有Javascript代码，结束的</script>标签也是不能丢的。

使用src属性时，<script>和</script>标签之间的任何内容都会忽略。如果需要，可以在<script>标签文章添加代码的补充说明文档或版权信息。但是要注意，如果有任何非空格或Javascript注释的文本出现在<script src="">和</script>之间，HTML5检验器将会报错。

以下是src属性方式的一些优点：
* 可以把大块Javascript代码从HTML文件中删除，这有助于保持内容和行为的分离，从而简化HTML文件。
* 如果多个Web页面共用相同的Javascript代码，用src属性可以让你只管理一份代码，而不用在代码改变时编辑每个HTML文件。
* 如果一个Javascript代码文件由多个页面共享，就只需要下载它一次，通过使用它的第一个页面，随后的页面可以从浏览器缓存检索它。
* 由于src属性的值可以是任意的URL，因此来自一个Web服务器的Javascript程序或Web页面可以使用由另一个Web服务器输出的代码。很多互联网广告依赖于此。
* 从其他网站载入脚本的能力，可以更好地利用缓存。

### 13.2.3 脚本类型
Javascript是Web的原始脚本语言，而在默认情况下，假定<script>元素包含或引用Javascript代码。如果要使用不标准的脚本语言，如Microsoft的VBScript(只有IE支持)，就必须用type属性指定脚本的MIME类型：

```javascript
<script type="text/javascript">
// 这里是你的Javascript代码
</script>
```

type属性的默认值是"text/javascript"。如果需要，可以显式指定此类型，但这完全没必要。

老的浏览器在<script>标记上用language属性代替type属性，这种情况如：

<script language="javascript">
// 这里是你的Javascript代码
</script>
```

language属性已经废弃，不应该再被使用。

当Web浏览器遇到<script>元素，并且这个<script>元素包含其值不被浏览器识别的type属性时，它会解析这个元素但不会尝试显示或执行它的内容。这意味着可以使用<script>元素来嵌入任意的文本数据到文档里，只要用type属性为数据声明一个不可执行的类型。要获取数据，可以用表示script元素的HTMLElement对象的text属性。但是，要注意这些数据购入技术只对内联脚本生效。如果同时指定src属性和一个未知的类型，那这个脚本会被忽略，并且不会从指定的URL里下载任何内容。

### 13.2.4 HTML中的事件处理程序
当脚本所在的HTML文件被转入浏览器时，这个脚本里的Javascript代码只会执行一次，为了可交互，Javascript程序必须定义事件处理程序，Web浏览器先注册Javascript函数，并在之后调用它作为事件的响应。Javascript代码可以通过把函数赋值给Element对象的属性来注册事件处理程序。

类似onclick的事件处理程序属性，用相同的名字对应到HTML属性，并且还可以通过将Javascript代码旋转在HTML属性里来定义事件处理程序。要定义用户切换表单中的复选框时调用的事件处理程序，可以作为表示复选框的HTML元素的属性指定处理程序代码：

```javascript
<input type="checkbox" name="option" value="gitfwrap"
	onchange="order.options.giftwrap = this.checked;">
```

这个属性里的Javascript代码会在用户选择或取消选择复选框时执行。

HTML中定义的事件处理程序的属性可以包含任意条Javascript语句，相互之间用逗号分隔。这些语句组成一个函数体，然后这个函数成为对应事件处理程序属性的值。但是，通常HTML事件处理程序的属性由类似上面的简单赋值或定义在其他地方的简单函数调用组成，这样可以保持大部分实际的Javascript代码在脚本里，而不用把Javascript和HTML混在一起。

### 13.2.5 URL中的Javascript
在URL后面跟一个Javascript:协议限定符，是另一种嵌入Javascript代码到客户端的方式。这种特殊的协议类型指定URL内容任意字符串，这个字符是会被Javascript解释器运行的Javascript代码。它被当做单独的一行代码对待，这意味着语句之间必须用分号隔开，而//注释必须用/* */注释代替。javascript:URL能识别的资源是转换成字符串的执行代码的返回值。如果代码返回undefined，那么这个资源是没有内容的。

javascript:URL可以用在可以使用常规URL的任意地方：比如<a>标记的href属性，<form>的action属性，甚至window.open()方法的参数。超链接里的Javascript URL可以是这样的：

```javascript
<a href="javascript:new Date().toLocaleTimeString();">
What time is it?
</a>
```

部分浏览器会执行URL里的代码，并使用返回的字符串作为待显示新文档的内容。就像单击一个http:URL链接，浏览器会擦除当前文档并显示新文档。以上代码的返回值并不包含任何的HTML标签，但是如果有，浏览器会像渲染通常载入的等价HTML文档一样渲染它们。

和HTML事件处理程序的属性一样，Javascript URL是Web早期的遗物，通常应该避免在现代的HTML里使用。但javascript:URL在HTML文档之外确实有着重要的角色。如果要测试一小段Javascript代码，可以在浏览器地址栏里直接输入javascript:URL。

#### 书签
在Web浏览器中，书签就是一个保存起来的URL。如果书签是javascript:URL，那么保存的就是一小段脚本，叫做bookmarklet。bookmarklet是一个小型程序，很容易就可以从浏览器的菜单或工具栏里启动。bookmarklet里的代码执行起来就像页面上的脚本一样，可以查询和设置文档的内容、呈现和行为。只要书签不返回值，它就可以操作当前显示的任何文档，而不把文档替换成新的内容。

考虑下面的<a>标签里的Javascript:URL。单击链接会打开一个简单的Javascript表达式计算器，它允许在页面环境中计算表达式和执行语句：

```javascript
<a href="javascript:
var e = '', r = '';
do{
e = prompt('Expression: ' + e + '\n' + r + '\n', e);
try{ r = 'Result: ' + eval(e); }
catch(ex) { r = ex; }
} while(e);
void 0;
">Javascript Evaluator</a>
```

注意即便这个Javascript URL是写成多行的，HTML解析仍将它作为单独的一行对待，并且其中的单行//注释也是无效的。代码是双引号中的HTML属性的一部分，所有代码不可以包含任何双引号。

在开发时，把这样的链接硬编码在页面中是有用的，而把它另存为可以在任何页面上运行的书签，就更有用了。

## 13.3 Javascript程序的执行
客户端Javascript程序没有严格的定义，可以说Javascript程序是由Web页面中所包含的所有Javascript代码(内联脚本、HTML事件处理程序和javascript:URL)和通过<script>标签的src属性引用的外部Javascript代码组成。所有这些单独的代码共用同一个全局Window对象。这意味着它们都可以看到相同的Document对象，可以共享相同的全局函数和变量的集合；如果一个脚本定义了新的全局变量或函数，那么这个变量或函数会在脚本执行之后对任意Javascript代码可见。

如果Web页面包含一个嵌入的窗体(通常使用<iframe>元素)，嵌入文档中的Javascript代码和被嵌入文档里的Javascript代码会有不同的全局对象，它可以当做一个单独的Javascript程序。但是，没有严格的关于Javascript程序范围的定义。如果外面和里面的文档来自于同一个服务器，那么两个文档中的代码就可以进行交互，并且如果愿意，就可以把它们当做是同一个程序的两个相互作用的部分。

Javascript程序的执行有两个阶段，在第一阶段，载入文档内容，并执行<script>元素里的代码(包括内联脚本和外部脚本)。脚本通常会按它们在文档里的出现顺序执行。所有脚本里的Javascript代码是从上往下，按照它在条件、循环以及其他控制语句中的出现顺序执行。

当文档载入完成，并且所有脚本执行完成后，Javascript执行就进入它的第二阶段。这个阶段是异步的，而且由事件驱动的。在事件驱动阶段，Web浏览器调用事件处理程序函数，来响应异步发生的事件。调用事件处理程序通常是响应用户输入。但是，还可以由网络驱动、运行时间或者Javascript代码中的错误来触发。嵌入在Web页面里的Javascript:URL也可以被当做是一种事件处理程序，因为直到用户通过单击链接或提交表彰来激活之后它们才会有效果。

事件驱动阶段里发生的第一个事件是load事件，指示文档已经完全载入，并可以操作。Javascript程序经常用这个事件来触发或发送消息。经常看到一些定义函数的脚本程序，除了定义一个onload事件自带程序函数处不做其他操作，这个函数会在脚本事件驱动阶段开始时被load事件触发。正是这个onload事件会对文档进行操作，并做程序想做的任何事。Javascript程序的载入阶段是相对短暂的，通常只持续1~2秒。在文档载入完成之后，只要Web浏览器显示文档，事件驱动阶段就会一直持续下去。因为这个阶段是异步的和事件驱动的，所以可能有长时间处于不活动状态，没有Javascript被执行，被用户或网络事件触发的活动打断。

核心Javascript和客户端Javascript都有一个单线程执行模型。脚本和事件处理程序在同一个时间只能执行一个，没有并发性。这保持了Javascript编程的简单性。

### 13.3.1 同步、异步和延迟的脚本
Javascript第一次添加到Web浏览器时，还没有API可以用来遍历和操作文档的结构和内容。当文档还在载入时，Javascript影响文档内容的唯一方法是快速生成内容。它使用document.write()方法完成上述任务。

```javascript
<h1>Table of Factorials</h1>
<script>
function factorial(n) {
	if (n <= 1) return n;
	else return n * factorial(n - 1);
}
document.write("<table>");
document.write("<tr><th>n</th><th>n!</th></tr>");
for (var i = 1; i <= 10; i ++) {
	document.write("<tr><td>" + i + "</td><td>" + factorial(i) + "</td></tr>");
}
document.write("</table>");
document.write("Generated at " + new Date());
</script>
```

当脚本把文本传递给document.write()时，这个文本被添加到文档输入流中，HTML解析器会在当前位置创建一个文本节点，将文本插入文本节点后面。如今并不推荐使用document.write()，但在某些场景下它有着重要的用途。当HTML解析器遇到<script>元素时，它默认必须先执行脚本，然后再恢复文档的解析和渲染。这对于内联脚本没什么问题，但如果脚本源代码是一个由src属性指定的外部文件，这意味着脚本后面的文档部分在下载和执行脚本之前，都不会出现在浏览器中。

脚本的执行只在默认情况下是同步和阻塞的。<script>标签可以有defer和async属性，这可以改变脚本执行方式。这些都是布尔属性，没有值；只需要出现在<script>标签里即可。HTML5说这些属性只在和src属性联合使用时才有效，但有些浏览器还支持延迟的内联脚本：

```javascript
<script defer src="deferred.js"></script>
<script async src="async.js"></script>
```

defer和async属性都像在告诉浏览器链接进来的不会使用document.wirte()，也不会生成文档内容。因此浏览器可以在下载脚本时继续解析和渲染文档。defer属性使得浏览器延迟脚本的执行。直到文档的载入和解析完成，并可以操作。async属性使得浏览器可以尽快地执行脚本。而不用在下载脚本时阻塞文档解析。如果<script>标签同时有两个属性，同时支持两者的浏览吕会遵从async属性并忽略defer属性。

注意，延迟的脚本会按它们在文档里的出现顺序执行。而异步脚本在它们载入后执行，这意味着它们可能会无序执行。

```javascript
// 异步载入并执行一个指定的URL中的脚本
function loadasync(url) {
	var head = document.getElementsByTagName("head")[0];	// 得到<head>元素
	var s = document.createElement("script");				// 创建一个<script>元素
	s.src = url;
	head.appendChild(s);
}
```

注意这个loadasync()函数会动态地载入脚本，脚本载入到文档中，成为正在执行的Javascript程序的一部分，既不是通过Web页面内联包含，也不是来自Web页面的静态引用。

### 13.3.2 事件驱动的Javascript
上面展示的古老的Javascript程序是同步载入的程序：在页面载入时开始执行，生成一些输出，然后结束。这种类型的程序在今天已经不常见了。反之，通过注册事件处理程序函数来写程序。在注册的程序发生时异步调用这些函数。想要为常用操作启用键盘快捷键的Web应用会为键盘事件注册事件处理程序。甚至非交互的程序也使用事件。如要写一个分析文档结构并自动生成文档内容的表格程序。程序不需要用户输入事件的事件处理程序，但它还是会注册onload事件处理程序，这样就可以知道文档在什么时候载入完成并可以生成内容表格了。

事件都有名字，如click、change、load、mouseover、keypress或readystatechange，指示发生的事件的通用类型。事件还有目标，它是一个对象，并且事件就是在它上面发生的。事件必须同时指定事件类型和目标。

如果要程序响应一个事件，写一个函数，叫做事件处理程序。然后注册这个函数，这样它就会在事件发生时调用它。这可以通过HTML属性来完成，但是不鼓励将Javascript代码和HTML内容混淆在一起。反之，注册事件处理程序最简单的方法是把Javascript函数赋值给目标对象的属性，如：

```javascript
window.onload = function() { ... };
document.getElementById("button1").onclick = function() { ... };
function handleResponse() { ... }
request.onreadystatechange = handleResponse;
```

按照约定，事件处理程序的属性的名字是以on开始，后面跟着事件的名字。还在注意上面的代码中没有函数调用：只是把函数本身赋值给这些属性。浏览器会在事件发生时执行调用。用事件进行异步编程会经常涉及嵌套函数，也经常要在函数的函数里定义函数。

对于大部分浏览器中的大部分事件来说，会把一个对象传递给事件处理程序作为参数，那个对象的属性提供了事件的详细信息。如，传递给单击事件的对象，会有一个属性说明鼠标的哪个按钮被单击。事件处理程序的返回值有时用来指示函数是否充分处理了事件，以及阻止浏览器执行它默认会进行的各种操作。

有些事件的目标是文档元素，它们会经常往上传递给文档树，这个过程叫做冒泡。如，如果用户在<button>元素上单击鼠标，单击事件就会在按钮上触发。如果注册在按钮上的函数没有处理该事件，事件会冒泡到按钮嵌套的容器元素，这样，任何注册在容器元素上的单击事件都会调用。

如果需要为一个事件注册多个事件处理程序函数，或者如果想要写一个可以安全注册事件处理程序的代码模块，就算另一个模块已经为相同的目标上的相同的事件注册了一个处理程序，也需要用到另一种事件处理程序注册技术。大部分可以成为事件目标的对象都有一个叫做addEventListener()的方法，允许注册多个监听器：

```javascript
window.addEventListener("load", function() { ... }, false};
request.addEventListener("readystatechange", function() { ... }, false};
```

在IE9及以上版本中实现了addEventListener()，在之前的版本中需要使用attachEvent()：

```javascript
window.attachEvent("onload", function() { ... });
```

客户端Javascript程序还使用异步通知类型，这些类型往往不是事件。如果设置Window对象的onerror属性为一个函数，会在发生Javascript错误时调用函数。还有，setTimeout()和setInterval()函数会在指定的一段时间之后触发指定函数的调用。传递给setTimeout()的函数和真实事件处理程序的注册不同，它们通常叫做回调逻辑而不是处理程序，但它们和事件处理程序一样，也是异步的。

```javascript
function onLoad(f) {
	if (onLoad.loaded)
		window.setTimeout(f, 0);
	else if (window.addEventListener)
		window.addEventListener("load", f, false);
	else if (window.attachEvent)
		window.attachEvent("onload", f); // before IE8
}

onLoad.loaded = false;
onLoad(function() { onLoad.loaded = true;});
```

### 13.3.3 客户端Javascript线程模型
Javascript语言核心并不包含任何线程机制，并且客户端Javascript传统上也没有定义任何线程机制。HTML5定义了一种作为后台线程的"WebWorker"，但是客户端Javascript还像严格的单线程一样工作。甚至当可能并发执行的时候，客户端Javascript也不会知晓是否真的有并行逻辑的执行。

单线程执行是为了让编程更加简单。编写代码时可以确保两个事件处理程序不会同一时刻运行，操作文档内容时也不必担心会有其他纯种试图同时修改文档，并且永远不需要在写Javascript代码的时候担心锁、死锁和竟态条件。

单线程执行意味着浏览器必须在脚本和事件语句处理程序执行的时候停止响应用户输入。这为Javascript程序员带来了负担，它意味着Javascript脚本和事件处理程序不能运行太长时间。如果一个脚本执行计算密集的任务，它将会给文档载入带来延迟，而用户无法在脚本完成前看到文档内容。如果事件自带程序执行计算密集的任务，浏览器可能变得无法响应，可能会导致用户认为浏览器崩溃了。

如果应用程序不得不执行太多的计算而导致明显的延迟，应该允许文档在执行这个计算之前完全载入，并确保能够告知用户计算正在进行并且浏览器没有挂起。如果可能将计算分解为离散的子任务，可以使用setTimeout()和setInterval()方法在后台运行子任务，同时更新一个进行指示器向用户显示反馈。

### 13.3.4 客户端Javascript时间线
Javascript程序执行的时间线：
* Web浏览器创建Document对象，并且开始解析Web页面，解析HTML元素和它们的文本内容后添加Element对象和Test节点到文档中。在这个阶段document.readyState属性的值是loading。
* 当HTML解析器遇到没有async和defer属性的<script>元素时，它把这些元素添加到文档中，然后执行行内或外部脚本。这些脚本会同步执行，并且在脚本下载和执行时解析器会暂停。这样脚本就可以用document.write()来把文本插入到输入流中。解析器恢复时这些文本会成为文档的一部分。同步脚本经常简单定义函数和注册后面使用的注册事件处理程序，但它们可以遍历和操作文档树，因为在它们执行时已经存在了。这样，同步脚本可以看到它自己的<script>元素和它们之前的文档内容。
* 当解析器遇到设置了async属性的<script>元素时，它开始下载脚本文本，并继续解析文档。脚本会在它下载写成后尽快执行。但是解析器没有停下来等它下载。异步脚本禁止使用document.write()方法。它们可以看到自己的<script>元素和它之前的所有文档元素，并且可能或干脆不可能访问其他的文档内容。
* 当文档完成解析，document.readyState属性变成interactive。
* 所有有defer属性的脚本，会按它们在文档里的出现顺序执行。异步脚本可能也会在这个时间执行。延迟脚本能访问完整的文档树，禁止使用document.write()方法。
* 浏览器在Document对象上触发DOMContentLoaded事件。这标志着程序执行从同步脚本执行阶段转换到了异步事件驱动阶段。但要注意，这时可能还有异步脚本没有执行完成。
* 这时，文档已经完全解析完成，但是浏览器可能还在等待其他内容载入，如图片。当所有这些内容完成转入时，并且所有异步脚本完成载入和执行，document.readyState属性改变为complete，Web浏览器触发Window对象的上的load事件。
* 从此刻起，会调用异步事件，以异步响应用户输入事件、网络事件、计时器过期等。

## 13.4 兼容性和互用性
Web浏览器是Web应用的操作系统，但是Web是一个存在各种差异性的环境，Web文档和应用会在不同操作系统的不同开发商的不同时代的浏览器上查看和运行。写一个健壮的客户端Javascript程序并能正确地运行在这么多类型的平台上，的确是一种挑战。

客户端Javascript兼容必和交互性的问题可以归纳为以下三类：
> 演化
>> Web平台一直在演变和发展当中。一个标准规范会倡导一个新的特性或API。
> 未实现
>> 同时实现一个功能在不同的浏览器之间有很大的差别。
> bug
>> 每个浏览器都有bug，并且没有按照规范准确地实现所有的客户端Javascript API。

### 13.4.1 处理兼容性问题的类库
处理不兼容问题其中一种最简单的方法是使用类库。如JQuery。

### 13.4.2 分组浏览器支持
分级浏览器是由Yahoo提出的一种测试技术。从某种维度对浏览器厂商/版本/操作系统谈何进行分级。A级要通过所有功能测试用例。C级只需在HTML完整情况下可用即可，其他浏览器称为X级。

### 13.4.3 功能测试
功能测试(capability testing)是解决不兼容性问题的一种强大技术。如果你想试用某个功能，但又不清楚这个功能是否在所有的浏览器中有比较好的兼容性，则需要在脚本中添加相应的代码来检测是否在浏览器中支持该功能。如：

```javascript
if (element.addEventListener) {
	element.addEventListener("keydown", handler, false);
	element.addEventListener("keypress", handler, false);
}
else if (element.attachEvent) {
	element.attachEvent("onkeydown", handler);
	element.attachEvent("onkeypress", handler);
}
else {
	element.onkeydown = element.onkeypress = handler;
}
```

### 13.4.4 怪异模式和标准模式
Microsoft在发布IE6的时候，增加了IE5里没有的很多CSS标准特性。但为了确保与已有Web内容的后向兼容性，它定义了两种不同的渲染模式。在标准模式和CSS兼容模式中浏览器要遵循CSS标准，在怪异模式中，浏览器表现的和IE4和IE5中的怪异非标准模式一样。渲染模式的选择依赖于HTML文件顶部的DOCTYPE声明。

### 13.4.5 浏览器测试
功能测试非常适用于检测大型功能领域的支持，比如可以使用这种方法来确定浏览器是否支持W3C事件处理模型还是IE的事件处理模型。另外，有时候可能会需要在某种浏览器中解决个别的bug或难题，但却没有太好的方法来检测bug的存在性。这时需要针对某个平台建立解决方案。

在客户端Javascript中检测浏览器类型和版本的方法就是使用Navigator对象，确定当前浏览器的厂商和版本的代码通常叫做浏览器嗅控器或者客户端嗅探器。

客户端嗅探也可以在服务器端完成，Web服务器根据User-Agent头部可以有选择地返回特定的Javascript代码给客户端。

### 13.4.6 Internet Explorer里的条件注释
实际上，客户端的Javascript编程中的很多不兼容性都是针对IE的。也就是说，必须按照某个方式为IE编写代码，而按照另环保方式为其他的浏览器编写代码。IE支持条件注释，如：

```javascript
<!--[if IE 6]>
This content is actually inside an HTML comment.
It will only be displayed in IE 6.
<![endif]-->

<!--[if lte IE 7]>
This content will only be displayed by IE 5, 6, 7 and earlier.
lte stands for "less than or equal". You can also use "lt", "gt" and "gte".
<![endif]-->

<!--[if !IE]><-->
This is normal HTML content, but IE will not display it
because of the comment above and the comment below.
<!--><![endif]-->

This is normal content, displayed by all browsers.
```

## 13.5 可访问性

## 13.6 安全性

### 13.6.1 Javascript不能做什么

### 13.6.2 同源策略

### 13.6.3 脚本化插件和ActiveX控件

### 13.6.4 跨站脚本

### 13.6.5 拒绝服务攻击

## 13.7 客户端框架



Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")
