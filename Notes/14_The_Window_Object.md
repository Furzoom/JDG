# Window对象
Window对象在客户端Javascript中扮演着核心角色：它是客户端Javascript程序的全局对象。

## 14.1 计时器
setTimeout()和setInterval()可以用来注册在指定的时间之后音效或重复调用的函数。因为它们都是客户端Javascript中重要的全局函数，所以定义为Window对象的方法，但作为通用函数，其实不会对窗口做什么事情。

Window对象的setTimeout()方法用来实现一个函数在指定的毫秒之后运行。setTimeout()返回一个值，这个值可以传递给clearTimeout()用于取消这个函数的执行。

setInterval()和setTimeout()一样，只不过这个函数会在指定毫秒数的间隔里重复调用：

```javascript
setInterval(updateClock, 60000);
```

和setTimeout()一样，setInterval()也返回一个值，这个值可以传递给clearInterval()，用于取消后续函数的调用。

```javascript
/*
 * 安排函数f()在未来的调用模式
 * 在等待了若干毫秒之后调用f()
 * 如果设置了interval并没有设置end参数，则对f()调用将不会停止
 * 如果没有设置interval和end，只在若干毫秒后调用f()一次
 * 只有指定了f()，才会从start=0的时刻开始
 * 注意，调用invoke(*)不会阻塞，它会立即返回
 */
function invoke(f, start, interval, end) {
	if (!start) start = 0;
	if (arguments.length <= 2)
		setTimeout(f, start);
	else {
		setTimeout(repeat, start);
		function repeat() {
			var h = setInterval(f, interval);
			if (end) setTimeout(function() { clearInterval(h); }, end);
		}
	}
}
```

由于历史原因，setTimeout()和setInterval()的第一个参数可以作为字符串传入。如果这么做，那这个字符串会在指定的超时时间或间隔之后进行求值(相当于执行eval())。除前两个参数外，HTML5规范还允许setTimeout()和setInterval()传入额外的参数，并在调用函数时把这些参数传递过境。

如果以0毫秒的超时时间来调用setTimeout()，那么指定的函数不会立刻执行，相反，会把它放到队列中，等到前面处于等待的事件处理程序全部执行完成后，再立即调用它。

## 14.2 浏览器定位和导航
Window对象的location属性引用的是Location对象，它表示该窗口中当前显示的文档URL，并定义了方法来使窗口载入新的文档。

Document对象的location属性也引用到Location对象：

```javascript
window.location === document.location // true
```

Document对象也有一个URL属性，是文档首次载入后保存该文档的URL的静态字符串。如果定位到文档中的片段标识符(如#table-of-contents)，Location对象会做相应的更新，而document.URL属性却不会改变。

### 14.2.1 解析URL
Window对象的location属性引用的是Location对象，它表示该窗口中当前显示的文档的URL。Location对象的href属性是一个字符串，后者包含URL的完整文本。Location对象的toString()方法返回href属性的值，因此在会隐式调用toString()的情况下，可以使用location代替location.href。

这个对象的其他属性，protocol、host、hostname、port、pathname、search，分别表示URL的各个部分。它们称为URL分解属性，同时被Link对象支持。

Location对象的hash和search属性比较有趣。如果有的话，hash属性返回URL中的片段标识符部分。search属性也类似，它返回的是问号之后的URL，这部分通常是某种类型的查询字符串。一般来说，这部分内容是用来参数化URL并在其中嵌入参数的。虽然这些参数通常用于运行在服务器上的脚本，但在启用Javascript的页面中当然也可以使用它们。下面展示一个通用函数urlArgs()的定义，可以用这个函数将参数从URL的search属性中提取出来：

```javascript
function urlArgs() {
	var args = {};
	var query = location.search.substring(1);
	var pairs = query.split("&");
	for (var i = 0; i < pairs.length; i ++) {
		var pos = pairs[i].indexOf('=');
		if (pos == -1) continue;
		var name = pairs[i].substring(0, pos);
		var value = pairs[i].substring(pos+1);
		value = decodeURIComponent(value);
		args[name] = value;
	}
	return args;
}
```

### 14.2.2 载入新的文档
Location对象的assign()方法可以使窗口载入并显示指定的URL中的文档。replace()方法也类似，但它在载入新文档之前会从浏览历史中把当前文档删除。如果脚本无条件地载入一个新文档，replace()方法可能是比assign()方法更好的选择。否则，后退按钮会浏览器带回到原始文档，而相同的脚本则会再次载入新文档。如果检测到用户的浏览器不支持某些特性来显示功能齐全的版本，可以用location.replace()来载入静态的HTML版本。

```javascript
if (!XMLHttpRequest) location.replace("staticpage.html");
```

注意，在这个例子中传入replace()的是一个相对URL。相对URL是相对前页面所在的目录来解析的，就像将它们用于一个超链接中。

除了assign()和replace()方法，Location对象还定义了reload()方法，后者可以让浏览器重新载入当前文档。

使浏览器跳转到新页面的一种更传统的方法是直接把新的URL赋给location属性：

```javascript
location = "http://furzoom.com/";
```

还可以把相对URL赋给location，它们会相对当前URL进行解析：

```javascript
location = "page2.html";
```

纯粹的片段标识符是相对URL的一种类型，它不会让浏览器载入新文档，但只会使它滚动到文档的某个位置。#top标识符是个特殊的例子：如果文档中没有元素的ID是top，它会让浏览器跳转到文档开始处。

```javascript
location = "#top";
```

Location对象的URL分解属性是可写的，对它们重新赋值会改变URL的位置，并且导致浏览器载入一个新的文档：

```javascript
location.search = "?page" + (pagenum + 2);
```

## 14.3 浏览历史
Window对象的history属性引用的是该窗口的History对象。Histoty对象是用来把窗口的浏览历史用文档和文档状态列表的形式表示。History对象的length属性表示浏览历史列表中的元素数量，但由于安全的因素，脚本不能访问已保存的URL。

History对象的back()和forward()方法与浏览器的后退和前进按钮一样：它们使浏览器在浏览历史中前后跳转一格。第三个方法go()接受一个整数参数，可以在历史列表中向前(正参数)或向后(负参数)跳过任意多个页。

```javascript
history.go(-2);
```

如果窗口包含多个子窗口，子窗口的浏览历史会按时间顺序穿插在主窗口的历史中。这意味着在主窗口调用history.back()可能会导致其中一个子窗口往回跳转到前一个显示的文档，但主窗口保留当前状态不变。

现代Web应用可以不通过载入新文档而动态地改变自身内容。这么做可能希望用户能用后退和前进按钮在这些动态创建的应用状态之间进行跳转。HTML5将这种技术标准化。

## 14.4 浏览器和屏幕信息
脚本有时候需要获取和它们所在的Web浏览器或浏览器所在的桌面相关的信息。Window对象的navigator和screen属性。它们分别引用的是Navigator和Screen对象，而这些对象提供的信息允许脚本来根据环境定制自己的行为。

### 14.4.1 Navigator对象
Window对象的navigator属性引用的是包含浏览器厂商和版本信息的Navigator对象。Navigator对象的命名是为了纪念Netscape之后Navigator浏览器，不过所有其他的浏览器也支持它。

过去，Navigator对象通常被脚本用来确定它们是在IE中还是在Netscape中运行。这种浏览器嗅探译意风问题，因为它要求随着新浏览器和现有浏览器的新版本的引入而不断地调整。现在有更好的功能测试方法，只需要测试所需要的功能，而不是假设特定的浏览器版本及功能。

然而，浏览器嗅探有时候仍然有价值。这样的一种情况是，当需要解决存在于某个特定的浏览器的特定版本中的特殊的bug时，Navigator对象有4个属性用于提供关于运行中的浏览器的版本信息，并且可以使用这些属性进行浏览器嗅探。

> appName
>> Web浏览器的全称。在IE中，这就是"Microsoft Internet Explorer"。在Firefox中该属性就是"Netscape"。为了兼容现在的浏览器嗅探代码，其他浏览器通常也取值为"Netscape"。

> appVersion
>> 此属性通常以数字开始，并跟着包含浏览器厂商和版本信息的详细字符串。字符串前面数字通常是4.0或5.0，表示它是第4或第5代兼容的浏览器。appVersion字符串没有标准的格式，所以，没有办法直接来判断浏览器类型。

> userAgent
>> 浏览器在它的USER-AGENT HTTP头部中发送的字符串。这个属性通常包含appVersion中的所有信息，并且常常也可能包含其他的细节。和appVersion一样，它也没胡标准的格式。由于这个属性包含绝大部分信息，因此浏览器嗅探代码通常用它来嗅探。

> platform
>> 在其上运行浏览器的操作系统的字符串。

```javascript
/* 为客户端嗅探定义browser.name和browser.version，结果如下：
 * "webkit" Safari或Chrome；版本号是Webkt的版本号
 * "opera": Opera；版本号就是软件的版本号
 * "mozilla": Firefox或者其他基于gecko内核的浏览器，版本号是Gecko的版本
 * "msie": IE；版本号是软件的版本
 */
var browser = (function() {
	var s = navigator.userAgent.toLowerCase();
	var match = /(webkit)[ \/]([\w.]+)/.exec(s) ||
		/(opera)(?:.*version)?[ \/]([\w.]+)/.exec(s) ||
		/(msie) ([\w.]+)/.exec(s) ||
		!/compatible/.test(s) && /(mozilla)(?:.*? rv:([\w.]+))?/.exec(s) ||
		[];
	return { name: match[1] || "", version: match[2] || "0" };
}());
```

除了浏览器厂商和版本信息的属性之外，Navigator对象还包含一些杂项的属性和方法。以下是一些标准化的属性以及广泛 应用但未标准化的属性：
> onLine
>> navigator.onLine属性表示浏览器当前是否连接到网络。应和程序可能希望在离线状态下把状态保存在本地。

> geolocation
>> Geolocation对象定义用于确定用户地理位置信息的接口。

> javaEnabled()
>> 一个非标准的方法，当浏览器可以运行Java小程序时返回true。

> cookieEnable()
>> 非标准的方法，如果浏览器可以保存永久的cookie时，返回false。当cookie配置为视具体情况而定时可以会返回不正确的值。

### 14.4.2 Screen对象
Window对象的screen属性引用的是Screen对象。它提供有关窗口显示的大小和可用的颜色数量的信息。属性width和height指定的是以像素为单位的窗口大小。属性availWidth和availHeight指定的是实际可用的显示大小，它们排除了像桌面任务栏这样的特性所占用的空间。属性colorDepth指定的是显示的BPP(bits-per-pixel)值，典型的值有16、24、32.

window.screen属性和它引用的Screen对象都是非标准但广泛实现的。可以用Screen对象来确定Web应用是否运行在一个小屏幕的设备上。

## 14.5 对话框
Window对象提供了3个方法来向用户显示简单的对话框。alert()向用户显示一条消息并等待用户关闭对话框。confirm()也显示一条消息，要求用户单击确定或取消按钮，并返回一个布尔值。prompt()同样显示一条消息，等待用户输入字符串，并返回那个字符串，如：

```javascript
do {
	var name = prompt("What is your name?");
	var correct = confirm("You entered '" + name + "'.\n" +
		"Click Okey to proceed ro Cancel to re-enter.");
	} while(!correct)
	alert("Hello, " + name);
```

尽管alert()、confirm()和prompt()方法都很容易使用，但是良好的设计还是需要有节制地使用它们，要尽量做到这一点。这些对话框会破坏浏览体验。

方法confirm()和prompt()都会产生阻塞，也就是说，在用户关掉它们所显示的对话框之前，它们不会返回。代码会停止运行。在多数浏览器中alert()方法也会产生阻塞，并等待用户关闭对话框，但并总是这样。

除了上面三种对话框，还有个更复杂的方法showModalDialog()，显示一个包含HTML格式的模态对话框。可以给它传入参数，以及从对话框里返回值。showModalDialog()在浏览器当前窗口中显示一个模态窗口。第一个参数用以指定提供对话框HTML内容的URL。第二个参数是一个任意值，这个值在对话框里的脚本中可以通过window.dialogArguments属性的值访问。第三个参数是一个非标准的列表，包含以分号隔开的name=value对，如果提供了这个参数，可以配置对话框的或其他属性。用dialogwidth和dialogheight来设置对话框窗口的大小，用resizable=yes来允许用户改变窗口大小。

用这个方法显示的窗口是模态的，showModalDialog()这个方法直到窗口关闭之前不会返回。当窗口关闭后，window.returnValue属性的值是此方法返回的值。对话框的HTML内容往往必须包含用来设置returnValue的确认按钮，如果需要则调用window.close()。

```javascript
/* 调用方式
	var p = showModalDialog("multiprompt.html",
							["Enter 3D point coordinates", "x", "y", "z"],
							"dialogwidth:400; dialogheight: 300; resizable: yes");
*/
<form>
<fieldset id = "fields"></fieldset>
<div style = "text-align: center">
<button onclick = "okay()">Okay</button>
<button onclick = "cancel()">Cancle</button>
</div>
<script>
var args = dialogArguments;
var text = "<legend>" + args[0] = "</legend>";
for (var i = 1; i < args.length; i++)
	text += "<label>" + args[i] + ": <input id='f" + i + "'></label><br />";
document.getElementById("fields").innerHTML = text;
function cancel() { window.close(); }
function okay() {
	window.returnValue = [];
	for (var i = i; i < args.length; i ++)
		window.returnValue[i-1] = document.getElementById("f" + i).value;
	window.close();
}
</script>
</form>
```

## 14.6 错误处理
Window对象的onerror属性是一个事件处理程序，当未捕获的异常到调用栈上时就会调用它，并把错误消息输出到浏览器的Javascript控制台上。如果给这个属性赋一个函数。那么只要这个窗口中发生了Javascript错误，就会调用该函数，即它成了窗口的处理程序。

由于历史原因，Window对象的onerror事件处理函数是调用通过三个字符串参数，而不是通过通常传递的一个事件对象。window.onerror的第一个参数是描述错误的一条消息，第二个参数是一个字符串，它存入引发错误的Javascript代码所有的文档的URL，第三个参数是文档中属性错误的行数。

onerror处理程序的返回值也很重要。如果onerror处理程序返回false，它通知浏览器事件处理程序已经处理了错误，不需要其他操作。换句话说，浏览器不应该显示它自己的错误消息。但Firefox里的错误处理程序必须返回true来表示它已经处理了错误。

```javaEnabled
window.onerror = function(args, url, line) {
	if (onerror.num ++ < onerror.max) {
		alert("ERROR: " + msg + "\n" + url + ":" + line);
		return true;
	}
}
onerror.max = 3;
onerror.num = 0;
```

## 14.7 作为Window对象属性的文档元素
如果HTML文档中用id属性来为元素命名，并且如果Window对象没有此名字的属性，Window对象会赋予一个属性，它的名字是id属性的值，而它们的值指向表示文档元素的HTMLElement对象。

在HTML文档中使用的id属性会成为可以被脚本访问的全局变量。如果文档包含一个\<button id="okay" />元素，可以通过全局变量okay来引用此元素。

如果Window对象已经具有此名字的属性，这就不会发生。如id为history、location的元素，就不会以全局变量的形式出现，因为这些ID已经占用了。同样，如果HTML文档包含一个id为"x"的元素，并且还在代码中声明并赋值给全局变量x，那么显式声明的变量会隐藏隐式的元素变量。如果脚本中的变量声明出现在命名元素之前，那这个变量的存在就会阻止元素获取它的window属性。而如果脚本中的变量声明出现在命名元素之后，那么变量的显式赋值会覆盖该属性的隐式值。

假设ID并没有被Window对象使用的话，那么任何有id属性的HTML元素都会成为全局变量的值。以下HTML元素如果有name属性的话，也会这样表现：

```html
<a>  <applet>  <area>  <embed>  <form>  
<frame>  <frameset>  <iframe>  <img>  <object>
```

id元素在文档中必须是唯一的：两个元素不能有相同的id。但是，这对name属性无效。如果上面的元素有多于一个有相同的name属性(或者一个元素有name属性，而另一个元素有相同的值的id属性)，具有该名称的隐式全局变量会引用一个类数组对象，这个类数组对象的元素是所有命名的元素。

有name或id属性的\<iframe>元素是个特殊的例子，为它们隐式创建的变量不会引用表示元素自身的Element对象，而是引用表示\<iframe>元素创建的嵌套浏览器窗体的Window对象。

## 14.8 多窗口和窗体
一个Web浏览器窗口可能在桌面上包含多个标签页。每一个标签页都是独立的浏览上下文(browser context)，每一个上下都有独立的Window对象，而且相互之间互不干扰。每个标签页中运行的脚本通常并不知道其他标签页的存在，更不用说和其他标签页的Window对象进行交互操作或者其文档内容了。

但是窗口并不总是和其他窗口完全没有关系。一个窗口或标签页中的脚本可以打开新的窗口或标签页，当一个脚本这样做时，这样的多个窗口或窗口与另一个窗口的文档之间就可以互操作。

HTML文档经常使用\<iframe>来嵌套多个文档。由\<iframe>所创建的嵌套浏览上下文是用它自己的Window对象所表示的。废弃的\<frameset>和\<frame>元素同样创建了一个嵌套的浏览上下文，每一个\<frame>都由一个独立的Window对象表示。对于客户端Javascript来说，窗口、标签页、iframe和框架都是浏览上下文；对于Javascript来说，它们都是Window对象。和相互独立的标签页不同，嵌套的浏览上下文之间并不是相互独立的。在一个窗体中运行的Javascript程序总是可以看到它的祖先和子孙窗体，尽管脚本查看这些窗体中的文档受到同源策略的限制。

### 14.8.1 打开和关闭窗口
使用Window对象的open()方法可以打开一个新的浏览器窗口。Window.open()载入指定的URL到新的或已存在的窗口中，并返回代表那个窗口的Window对象。它有4个可选的参数。

open()的第一个参数是要在新窗口中显示的文档的URL。如果这个参数省略了，也可以是空字符串，那么会使用空页面的URL about:blank。

open()的第二个参数是新打开的窗口的名字。如果指定的是一个已经存在的窗口的名字(并且脚本允许跳转到那个窗口)，会直接使用已存在的窗口。否则，会打开新的窗口，并将这个指定的名字赋值给它。如果省略此参数，会使用指定的名字"_blank"打开一个新的、未命名的窗口。

脚本是无法通过简单地猜测窗口的名字来操控这个窗口中的Web应用的，只有设置了允许导航(allowed to navigate)的页面才可以这样。宽泛地讲，当且仅当窗口包含的文档来自相同的源或者是这个脚本打开了那个窗口，脚本才可以只通过名字来指定存在的窗口。如果其中一个窗口是内嵌在另一个窗口里的窗体，那么在它们的脚本之间就可以相互导航。这种情况下，可以使用保留的名字"_top"(顶级祖先窗口)和"_parent"(直接父级窗口)来获取彼此的浏览上下文。

open()的第三个可选参数是一个以逗号分隔的列表，包含大小和各种属性，用以表明新窗口是如何打蔫的。如果省略这个参数，那么新窗口就会用一个默认的大小，而且带有一整组标准的UI组件，即菜单栏、状态栏、工具栏等。在标签式浏览中，会创建一个新的标签。

另一方面，如果指定这个参数，就可以指定窗口的尺寸，以及它包含的一级属性。如，要打开允许改变大小的浏览器窗口，并且包含状态栏、工具栏和地址栏，就可以这样写：

```javascript
var w = window.open("smallwin.html", "smallwin", "width=400,height=350,status=yes,resizable=yes");
```

第三个参数是非标准的，HTML5规范也主张浏览器应该忽略它。当指定第三个参数时，所有没有显示指定的功能都会忽略。出于各种安全原因，浏览器包含对可能指定的功能的限制。如，不允许指定一个太小的或者位于屏幕之外的窗口，并且一些浏览器不允许创建一个没有状态栏的窗口。

open()的第四个参数只在第二个参数命名的是一个存在的窗口时才有用。它是一具布尔值，声明了由第一个参数指定的URL是应用替换掉窗口浏览历史的当前条目(true)，还是应该在窗口浏览历史中创建一个新的条目(false)，后者是默认设置。

open()的返回值是代表命名或新创建的窗口的Window对象。可以在自己的Javascript代码中使用这个Window对象来引用新创建的窗口，就像使用隐式的Window对象的window来引用运行代码的窗口一样：

```javascript
var w = window.open();
w.alert("About to visit http://furzoom.com");
w.location = "http://furzoom.com/";
```

在由window.open()方法创建的窗口中，opener属性引用的是打开它的脚本的Window对象，在其他窗口中，opener为null：

```javascript
w.opener !== null;		// true
w.open().opener === w; 	// true
```

通常，open()方法只有当用户手动单击按钮或者链接的时候才会调用。

### 关闭窗口

就像方法open()打开一个新窗口一样，方法close()将关闭一个窗口。如果已经创建了Window对象w，可以使用如下的代码将它关掉：

```javascript
w.close();
```

运行在那个窗口中的Javascript代码则可以使用下面的代码关闭：

```javascript
window.close();
```

注意，要显式地使用标识符window，这样可以避免混淆Window对象的close()方法和Document对象的close()方法，如果正在从事件处理程序调用close()，这很重要。

大多数浏览器只允许自动关闭由自己的Javascript代码创建的窗口。如果要关闭其他窗口，可以用一个对话框提示用户，要求他对关闭窗口的请求进行确认。在表示窗体而不是顶级窗口或标签页上的Window对象上执行close()方法不会有任何效果，它不能关闭一个窗体，但它可以从它包含的文档中删除iframe。

### 14.8.2 窗体之间的关系
Window对象的方法open()返回代表新创建的窗口的Window对象。而且这个新窗口具有opener属性，该属性可以打开它的原始窗口。这样，两个窗口就可以相互引用，彼此都可以读取对方的属性或是调用对方的方法。窗体的也是这样的。

任何窗口或窗体中的Javascript代码都可以将自己的窗口和窗体引用为window或self。窗体可以用parent属性引用包含它的窗口或窗体的Window对象：

```javascript
parent.history.back();
```

如果一个窗口是顶级窗口或标签，而不是窗体，那么其parent属性引用的就是这个窗口本身：

```javascript
parent == self;
```

如果一个窗体包含在另一个窗体中，而后者又包含在顶级窗口中，那么该窗体就可以使用parent。parent来引用顶级窗口。top属性是一个通用的快捷方式，无论一个窗体被嵌套了几层，它的top属性引用的都是指向包含它的顶级窗口。如果一个Window对象代表的是一个顶级窗口，那么它的top属性引用的就是窗口本身。对于那些顶级窗口的直接子窗体，top属性就等价于parent属性。

parent和top属性允许脚本引用它的窗体的祖先。有不止一种方法可以引用窗口或窗体的子孙窗体。窗体是通过\<iframe>元素创建的。可以用获取其他元素的方法来获取一个表示\<iframe>的元素对象。假定文档里有\<iframe id="f1">。那么，表示该iframe的元素对象就是：

```javascript
var iframeElement = document.getElementById("f1");
```

\<iframe>元素有contentWindow属性，引用该窗体的Window对象，所以此窗体的Window对象就是：

```javascript
var childFrame = document.getElementById("f1").contentWindow;
```

可以进行反向操作，从表示窗体的Window对象来获取该窗体的\<iframe>元素，用Window对象的frameElement属性。表示顶级窗口的Window对象的frameElement属性为null，窗体中的Window对象的frameElement属性不是null：

```javascript
var elt = document.getElementById("f1");
var win = elt.contentWindow;
win.frameElement === elt;		// true
window.frameElement === null;	// true
```

尽管如此，通常不需要使用getElementById()方法和contentWindow属性来获取窗口中子窗体的引用。每个Window对象都有一个frames属性，它引用自身包含的窗口或窗体的子窗体。frames属性引用的是类数组对象，并可以通常数字或窗体名进行索引。要引用窗口的第一个子窗体，可以用frames[0]。要引用第二个子窗体的第三个子窗体，可以用frames[1].frames[2]。窗体里运行的代码可以用parent.frames[1]引用兄弟窗体。frames[]数组里的元素是Window对象，而不是\<iframe>元素。

如果指定\<iframe>元素的name或id属性，那么除了用数字进行索引之外，还可以用名字来进行索引。如，frames["f1"]或frames.f1。

\<iframe>以及其他元素的name和ID都可以自动通常Window对象的属性来应用，而\<iframe>元素和其他的元素有所不同：对于窗体来说，通过Window对象的属性引用的\<iframe>是指窗体中的Window对象，而不是元素对象。也就是说，可以通常窗体的名字"f1"来代替frames.f1。实际上，HTML5规范指出frames属性是一个自引用(self-referential)的属性，就像window和self一样。而这个Window对象看起来像一个由窗体组成的数组。也就是说可以通过window[0]来获取第一个子窗体的引用，可以通过window.lenght或length查询窗体的编号。

### 14.8.2 交互窗口中的Javascript
每个窗口和窗体都是它自身的Javascript执行上下文，以Window作为全局对象。但是如果一个窗口或窗体中的代码可以应用到其他窗口或窗体，那么一个窗口或窗体中的脚本就可以和其他窗口或窗体中的脚本进行交互。

设想一个Web页面里有两个\<iframe>元素，分别叫A和B，并假设这些窗体所包含的文档来自于相同的一个服务器，并且包含交互脚本。窗体A里的脚本定义了一个变量i：

```javascript
var i = 3;
```

这个变量只是全局对象的一个属性，也是Window对象的一个属性。窗体A中的代码可以用标识符i来引用变量，或者用window对象显式地引用这个变量：

```javascript
window.i;
```

由于窗体B中的脚本可以引用窗体A的Window对象，因此它也可以引用那个Window对象的属性：

```javascript
parent.A.i = 4;
```

主义函数的关键字function可以声明一个变量，就像关键字var所做的那样。如果窗体B中的脚本声明了一个函数f，这个函数在窗体B中是全局变量，并且窗体B中的代码可以用f()调用f。但是窗体A中的代码必须将f作为窗体B的Window对象的f属性来引用：

```javascript
parent.B.f();
```

如果窗体A中的代码需要很频繁地使用这个函数，则可以将这个函数赋值给窗体A中的一个变量，这样就可以经常使用这个变量来引用窗体中的函数了：

```javascript
var f = parent.B.f;
```

当采用这种方式在窗体或窗口间共享函数时，牢记记法作用域的规则非常重要。函数在定义它的作用域中执行，而不是调用它的作用域中执行。如果函数f引用了全局变量，那么将在窗体B的属性中查找这些变量，即使函数是由窗体A调用的。

每个Window都有自己的原型对象，这意味着instanceof操作符不能跨窗口工作。如，当用instanceof来比较窗体B的一个字符串和窗体A的String()构造函数时，结果会为false。

Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")
