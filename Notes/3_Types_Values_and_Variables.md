# 类型，值和变量

JavaScript中数据类型分为两类：
* 原始类型(primitive type)
* 对象类型(object type)

JavaScript是一种面向对象的语言。在JavaScript中除对象可以拥有方法外，数字、字符串和布尔值也可以拥有自己的方法。只在null和undefined没有方法。

JavaScript的类型可分为原始类型和对象类型，也可分为拥有方法的类型和不能拥有方法的类型，同样可分为可变(mutable)类型和不可变(immutable)类型。对象和数组为可变类型，数字、布尔值、字符串、null、undefined是不可变类型。

JavaScript可以自由地进行数据类型的转换。

JavaScript的变量是无类型的(untyped)，变量可以被赋予任何类型的值。

## 3.1 数字
JavaScript不区分整数值和浮点值。JavaScript所有数字均采用浮点数值表示，采用IEEE 754标准定义的64位浮点格式表示。

JavaScript中任何数字直接量前添加负号(-)可以得到它们的负值。负号是一无求反运算符，并不是数字直接量的组成部分。
### 3.1.1 整型直接量
在JavaScript中整型直接量可以是十进制、十六进制、八进制形式。如：
```javascript
0
3
1000000 	// 十进制
0xff
0xCAFEA0 	// 十六进制
076			// 八进制
```
### 3.1.2 浮点型直接量
语法格式为：

		[digits][.digits][(E|e)[(+|-)]digits]
		
如：
```javascript
3.14
2345.79
.02934023
4.3443e22
1.2374E-11
```
### 3.1.3 JavaScript中的算术运算
JavaScript程序本身支持数字的运算，如加、减、乘、除、余等。还可以通过Math对象进行复杂的运算，如：
```javascript
Math.pow(2,10)		// 1024
Math.round(.6)		// 1
Math.ceil(.6)		// 1
Math.floor(.6)		// 0
Math.abs(-3)		// 3
Math.max(1, 3, 4)	// 4
Math.min(1, 3, 4)	// 1
Math.random()		// Maybe 0.31342803593724966
Math.PI				// 3.141592653589793
Math.E				// 2.718281828459045
Math.sqrt(3)		// 1.7320508075688772
Math.sin(Math.PI/2)	// 1
Math.log(10)		// 2.302585092994046	
Math.exp(3)			// 20.085536923187668
```

JavaScript中算术运算在溢出时不提示错误，但结果会为无穷大，`Infinity`或者`-Infinity`。如除数为0的情况。

当0除以0时，或者无穷大除以无穷大、给负数开方、将数字与非数字或不能转换为数字的操作数一起操作时，结果都返回NaN(Not a Number)。

JavaScript中NaN不与任何值相等，包括其本身。也就是说不通过`x==NaN`来判断x是否是NaN，可用使用`x!=x`进行判断。或者使用函数isNaN()来判断。函数isFinite()当参数不是NaN、Infinity或-Infinity时，返回true。

JavaScript中特殊的值还有+0和-0。它们是相等的，除了将其作为除数。如
```javascript
var zero = 0;
var negz = -1;
zero === negz;		// true
1/zero === 1/negz;  // false
```
### 3.1.4 二进制浮点数和四舍五入错误
由于实数的个数是无限的，在JavaScript中通过浮点数表示的实数是有限个的，也就是说其表示的实数有时是近似值。不能精确表达数字带来一个问题。如：
```javascript
var x = 0.3 - 0.2;	// 0.09999999999999998
var y = 0.2 - 0.1;	// 0.1
x == y;				// false
x == 0.1			// false
y == 0.1			// true
```
这种问题就是由于浮点数是由近似值表示的有关，任何使用二进制浮点数的编程语言都有这个问题。
### 3.1.5 日期和时间
JavaScript语言核心包括Date()构造函数，用来创建表示日期和时间的对象，如：
```javascript
var then = new Date(2015, 2, 21);				// Mar 21, 2015
var later = new Date(2015, 2, 21, 8, 10, 30); 	// Mar 21, 2015 08:10:30
var now = new Date();							// current date and time
var elapsed = now - then;						// millisecond
later.getFullYear();							// 2015
later.getMonth();								// 2
later.getDate();								// 21
later.getDay();									// 6
later.getHours();								// 8
later.getUTCHours();							// 0
```

## 3.2 文本
字符串(string)是一组由16位值组成的不可变的有序序列，字符串的长度(length)是其含有16位值的个数，字符串的索引从0开始。
### 3.2.1 字符串直接量
字符串直接是由单引号或双引号括起来的字符序列，如：
```javascript
"";	// empty string which length is 0
'testing';
'3.14';
"I'm a boy!";
```
### 3.2.2 转义字符
JavaScript字符串中有特殊含义或者控制字符需要使用转义字符(escape sequence)来表达。如：

* \0		NUL字符(\u0000)
* \b		退格符(\u0008)
* \t		水平制表符(\u0009)
* \n		换行符(\u000A)
* \v		垂直制表符(\u000B)
* \f		换页符(\u000C)
* \r		回车符(\u000D)
* \"		双引号(\u0022)
* \'		单引号(\u0027)
* \\		反斜线(\u005C)
* \xXX		2位16进制数指定的Latin-1字符
* \uXXXX	4位16进制数指定的Unicode字符

### 3.2.3 字符串的使用

JavaScript内置功能之一就是字符串的连接，使用加号(+)实现，取得字符串的长度等方法，如：

```javascript
var msg = 'Welcome to ' + 'furzoom!';	// msg = "Welcome to furzoom!" 
msg.length;								// 19
msg.charAt(0);							// W
msg.substring(0, 7);					// Welcome
msg.slice(0, 7);						// Welcome
msg.indexOf('o');						// 4
msg.lastIndexOf('o');					// 16
msg.split(' ');							// ["Welcome", "to", "furzoom!"]
msg.replace('f', 'F');					// Welcome to Furzoom!
msg.toUpperCase();						// WELCOME TO FURZOOM!
msg[0];									// W
```

### 3.2.4 模式匹配
JavaScript定义了RegExp()构造函数，用来创建表示文本匹配模式的对象。称为正则表达式(Regular Expression)，JavaScript中采用Perl中的正则表达式语法。String和RegExp对象均定义了利用正则表达式进行模式匹配和查找替换的函数。在两条斜线间地文本构成了一个正则表达式直接量，第二个斜线后可追加字符修饰匹配模式的含义，如：
```javascript
/^HTML/				// 匹配以HTML开关的字符串
/\bjavascript\b/i	// 匹配单词'javascript'，忽略大小写

var text = 'testing: 1, 2, 3";
var pattern = /\d+/g;			// 匹配一个或多个数字的实例
pattern.test(text);				// true
text.search(pattern);			// 9 首次匹配成功的位置
text.match(pattern);			// ["1", "2", "3"]
text.replace(pattern, '#');		// "testing: #, #, #"
text.split(/\D+/);				// ["", "1", "2", "3"]
```
## 3.3 布尔值
布尔值只有两个值true和false。比较表达式的结果通常都是布尔值。布尔值含有一个方法toString()，用于其值转化为字符串'true'或者'false'。

## 3.4 null和undefined
null是JavaScript语言的关键字，表示一个特殊值，“空值”。它是一个特殊的对象，表示数字、字符串和对象是无值的。

undefined是JavaScript语言全局变量，表示值的空缺，它是变量的一种取值，表明变量没有初始化。如，查询数组或对象的属性不存在会返回undefined；如果函数没有返回值，则返回undefined；没有提供实参的函数形参也是undefined。

## 3.5 全局对象
全局对象(global object)在JavaScript中有着重要的用途，全局对象有如下属性：

* 全局属性，undefined、Infinity、NaN
* 全局函数，isNaN()、parseInt()、eval()
* 构造函数，Date()、RegExp()、String()、Object()、Array()
* 全局对象，Math、JSON

## 3.6 包装对象
JavaScript对象是一种复合值，它是属性或已命名值的集合。通过“."符号来引用属性，当属性是一个函数的时候称其为方法，通过o.m()来调用对象中的方法。

字符串、数字、布尔值都有各自的方法，它们通过String()、Number()、Boolean()构造函数创建临时对象进行处理。null和undefined没有包装对象。如下代码：
```javascript
var s = 'test';
s.len = 4;
var t = s.len;
```
t的值为undefined。第2行会创建临时对象，并给其len属性赋值为4，执行完这行代码时，临时临时对象被销毁。第3行再次创建临时对象，试图读取其len属性值，由于没有属性值，故返回undefined。数字和布尔值同样如此。

存取字符串、数字和布尔值的属性时创建的临时对象称做包装对象。由于字符串、数字、布尔值都是只读对象，不能给其定义新的属性。但是可以通过String()、Number()、Boolean()显式地创建对象，`==`将原始值和其包装对象视为相等，`===`将原始值和其包装对象视为不相等。

## 3.7 不可变的原始值和可变的对象引用
JavaScript中原始值与对象有着根本的区别。原始值为是可更改的：任何方法都无法更改一个原始值。对数字和布尔值容易理解，字符串同样如此。如：

```javascript
var s = 'hello';
s.toUpperCase();	// "HELLO"
s					// "hello"
````

原始值的比较是值的比较，只能它们的值相等时它们能相等。对字符串来讲，只有它们长度相等且每个索引位置的字符都相等时才相等。

对象是可变的，它们的值是可以修改的：

```javascript
var o = {x:1};
o.x = 2;
o.y = 3;			// {x: 2, y: 3}

var a = [1, 2, 3];
a[0] = 4;			// [4, 2, 3]
```

对象的比较并非值的比较：即使两个对象包含同样的属性及相同的值，它们也是不相等的。各个索引元素完全相等的两个数组也不相等。如：

```javascript
var o = {x: 1};
var p = {x: 1};
o === p;			// false

var a = [];
var b = [];
a === b;			// false
```

通常称对象为引用类型(reference type)，区别于JavaScript的原始类型。因此对象的值都是引用(reference)，对象的比较均是引用的比较：当且仅当它们引用同一个基对象时，它们才相等。

```javascript
var a = [];
var b = a;
b[0] = 1;
a[0];				// 1
a === b;			// true
```

如果想得到一个对象的副本，就必须显式地复制对象的每个属性每个元素，如：

```javascript
var a = [1, 2, 3];
var b = [];
for (var i  = 0; i < a.length; i++) {
	b[i] = a[i];
}
```

如果要比较两个单独的对象，就必须比较它的每一个属性。

##　3.8 类型转换
JavaScript中的取值是非常灵活的。当JavaScript期望使用一个布尔值时，可以提供任意类型，JavaScript会根据需要自动将其转换为布尔值。对于字符串和数字同样如此。如：

```javascript
10 + " objects";		// "10 objects"
"7" * "4";				// 28
var n = 1 - "x";		// NaN
n + " objects";			// "NaN objects"
```

下表说明了类型转换的方式。

值 | 字符串 | 数字 | 布尔值 | 对象
--- | --- | --- | --- | ---
undefined | "undefined" | NaN | false | throws TypeError
null | "null" | 0 | false | throws TypeError
--- | --- | --- | --- | ---
true | "true" | 1 |   | new Boolean(true)
false | "false" | 0 |   | new Boolean(false)
--- | --- | --- | --- | ---
"" |   | 0 | false | new String("")
"1.2" |   | 1.2 | true | new String("1.2")
"one" |   | NaN | true | new String("one")
--- | --- | --- | --- | ---
0 | "0" |   | false | new Number(0)
-0 | "0" |   | false | new Number(-0)
NaN | "NaN" |   | false | new Number(NaN)
Infinity | "Infinity" |   | true | new Number(Infinity)
-Infinity | "-Infinity" |   | true | new Number(-Infinity)
1 | "1" |   | true | new Number(1)
--- | --- | --- | --- | ---
{} | Ref 3.8.3 | Ref 3.8.3 | true |  
[] | "" | 0 | true |  
[9] | "9" | 9 | true |  
['a'] | join() | NaN | true |  
function(){} | Ref 3.8.3 | NaN | true |  

### 3.8.1 转换和相等性
由于JavaScript可以做灵活的转换，运算符`==`也是灵活多变的。如：
```javascript
null == undefined; 		// true
"0" == 0;				// true	 to number
0 == false;				// true  to number
"0" == false;			// true  to number
````

一个值转换为另一个值并不意味着两个值相等。如，在期望用布尔值的地方使用了undefined，它将会转换为false，但这并不意味着`undefined == false`。if语句将undefined转换为false，但是`==`从不试图将其操作数转换为布尔值。

### 3.8.2 显式类型转换
尽管JavaScript可以做许多类型转换，显式转换还是需要的，或者为了使代码变得清晰易读。做显式转换的简单方法就是使用Boolean()、Number()、String()、Object()函数，当不使用new操作符调用这些函数时，它们会作为类型转换函数被调用。除null和undefined类型外，都有toString()方法，它与String()方法的返回结果一致。

```javascript
x + '';		// String(x)
+x;			// Number(x)
!!x;		// Boolean(x)
```

JavaScript中还提供了专门的函数和方法用来做更加精确的数字到字符串(number to string)和字符串到数字(string to number)的转换。如：

```javascript
var n = 17;
var binary_string = n.toString(2);	// "10001"
var octal_string = n.toString(8);	// "21"
var hex_string = n.toString(16);	// "11"

var m = 123456.789;
n.toFixed(0);				// "123457"
n.toFixed(2);				// "123456.79"
n.toFixed(5);				// "123456.78900"
n.toExponential(1);			// "1.2e+5"
n.toExponential(3);			// "1.235e+5"
n.toPrecision(4);			// "1.235e+5"
n.toPrecision(7);			// "123456.8"
n.toPrecision(10);			// "123456.7890"
```

Number类的toFixed()函数固定小数点后的位数将数字转换为字符串。toExponential()指定小数点后的位数，以指数形式返回将数字转换为字符串。toPrecision()函数指定有效数字的位数，将数字转换为字符串。

通过Number()转换函数传入一个字符串，试图将其转换为一个整数或者浮点数直接量，只能基于十进制进行转换，并且不能出现非法的字符。parseInt()和parseFloat()函数更加灵活，前者解析整数，后者解析整数和浮点值。如果字符串前缀是0x或者0X，parseInt()将其解释为十六进制数。二者试图解释更多的数值字符，并忽略后面的内容。如：

```javascript
var n1 = new Number("12.1");
n1.valueOf();				// 12.1
var n2 = new Number("12abc");
n2.valueOf();				// NaN

parseInt("3 blind mice");	// 3
parseFloat(" 3.14 meters");	// 3.14
parseInt("-12.34");			// -12
parseInt("0xff");			// 255
parseInt("0XFF");			// 255
parseInt("-0xFF");			// -255
parseFloat(".1");			// 0.1
parseInt("0.1");			// 0
parseInt(".1");				// NaN
parseInt("$73.23");			// NaN

parseInt("11", 2);			// 3
parseInt("ff", 16);			// 255
parseInt("zz", 36);			// 1295
parseInt("077", 8);			// 63
parseInt("077", 10);		// 77
```

### 3.8.3 对象转换为原始值
对象到布尔值的转换非常简单，所有对象都转换为true。

对象到字符串(object to string)或者对象到数字(object to number)的转换是通过调用待转换对象的方法来实现的。但JavaScript中有两种不同的方法来执行转换。第一个方法就是toString()方法，它的作用是返回一个反映这个对象的字符串。然后默认的toString()方法不会返回一个想要的值。

```javascript
({x: 1}).toString();		// "[object Object]"
([1, 2]).toString();		// "1,2"
(function(x) { f(x); }).toString();	// "function (x) { f(x); }"
/\d+/g.toString();			// "/\\d+/g"
new Date().toString();		// "Sat Mar 21 2015 22:23:35 GMT+0800 (中国标准时间)"
```

另一个转换对象的函数是valueOf()。如果存在任意原始值，它就默认将对象转换为表示它的原始值。对象是复合值，而且大多数对象无法真正表示为一个原始值，因此默认的valueOf()方法简单地返回对象本身，而不是返回一个原始值。如：
```javascript
({x: 1, y: 2}).valueOf();	// Object {x: 1, y: 2}
([1, 2]).valueOf();			// [1, 2]
(function(x) { f(x); }).valueOf();	// function (x) { f(x); }
/\d+/g.valueOf();			// /\\d+/g
new Date().valueOf();		// 1426948242785
```

JavaScript中对象到字符串的转换经过了如下步骤：
* 如果对象具有toString()方法，则调用这个方法。如果它返回一个原始值，JavaScript将这个值转换为字符串(如果本身不是字符串的话)，并返回这个字符串结果。
* 如果对象没有toString()方法，或者这个方法并不返回一个原始值，那么JavaScript会调用valueOf()方法。如果存在这个方法，则JavaScript调用它。如果返回值是原始值，JavaScript将这个值转换为字符串(如果本身不是字符串的话)，并返回这个字符串结果。
* 否则，JavaScript无法从toString()或valueOf()方法获得一个原始值，因此会抛出错误异常。

在对象转换为数字的过程中，JavaScript做了同样的事情，不过它先尝试valueOf()方法而已。

JavaScript中的`+`运算符可以进行数学加法和字符串连接操作。如果它的一个操作数是对象，则JavaScript将使用特殊的方法将对象转换为原始值，而不是使用其他算术运算符的方法执行对象到数字的转换。`==`运算符类似。

`+`和`==`应用的对象到原始值的转换包含日期对象的一种特殊情形。日期类定义了有意义的向字符串和数字类型的转换。对于所有非日期的对象来说，对象到原始值的转换基本上是对象到数字的转换，日期则使用对象到字符串的转换模式。然而，日期类的valueOf()和toString()返回的原始值将被直接使用，而不会被强制转换为数字或字符串。

关系运算符也要求做对象到原始值的转换，除日期类对象外，首先尝试调用valueOf()，然后调用toString()。不管得到的原始值是否直接使用，不会进一步被转换为数字或者字符串。

其他运算符对特定类型的转换都明确，而且对日期类型也没有特殊情况，如：
```javascript
var now = new Date();
typeof(now + 1);			// string
typeof(now - 1);			// number
now == now.toString();		// true
now > (now - 1);			// true
```

## 3.9 变量声明
在JavaScript中，使用一个变量之前就先声明。变量声明使用var关键字。如：

```javascript
var i;
var sum;
var name, address;
var message = "furzoom";
```

如果在声明变量时没有指定初始值，那么它的值就是undefined。

还可以这样：

```javascript
for (var i = 0; i < 10; i++) { console.log(i); }
```

使用var语句重复声明变量是合法且无害的。如果使用没有声明的变量将会报错。

## 3.10 变量作用域
一个变量的作用域(scope)是程序源代码中定义这个变量的区域。全局变量拥有全局作用域，在JavaScript代码中的任何地方都是有意义的。然而在函数内声明的变量只在函数体内有定义。它们是局部变量。在函数体内，局部变量的优先级高于同名的全局变量。如果函数内声明的一个局部变量或者函数参数中带有的变量和全局变量重名，那全局变量就被局部变量所遮盖。

```javascript
var scope = "global";
function checkscope() {
	var scope = "local";
	return scope;
}
checkscope();				// "local"
```
全局的变量声明可以不使用var关键字，局部变量必须使用var进行变量的声明。

### 3.10.1 函数作用域和声明提前
JavaScript中没有块级作用域，JavaScript中使用函数作用域(function scope)。变量在声明它们的函数体以及这个函数体嵌套的任意函数体内都是有定义的。

在函数体内，变量在声明之前可以使用它。JavaScript的这个特性被非正式的称为声明提前(hoisting)，即JavaScript函数里声明的所有变量都被提前至函数体的顶部。如：

```javascript
var scope = "global";
function f() {
	console.log(scope);		// undefined
	var scope = "local";
	console.log(scope);		// local
}
```

上述代码与下面的代码是等价的。

```javascript
var scope = "global";
function f() {
	var scope;
	console.log(scope);		// undefined
	scope = "local";
	console.log(scope);		// local
}
```

### 3.10.2 作为属性的变量
当声明JavaScript的全局变量时，实际上是定义了全局对象的一个属性。使用var声明的全局变量是不能通过delete删除的。JavaScript可以允许使用this关键字来引用对象。如：

```javascript
var truevar = 1;
fakevar = 2;
this.fakevar2 = 3;
delete truevar; 		// false
delete fakevar;			// true
delete this.fakevar2;	// true

var foo = function() { 
	var truevar = 2;
	console.log(truevar);		// 2
	console.log(this.truevar); 	// 1
}
```

### 3.10.3 作用域链
JavaScript是基于词法作用域的语言，全局变量在程序中始终是有定义的。局部变更在声明它的函数体内以及其嵌套的函数内始终是有定义的。每一段JavaScript代码都有一个与之凑聚的作用域链(scope chain)。这个作用域链是一个对象列表或者链表，这组对象定义了这段代码作用域中的变量。

在JavaScrip的最顶层代码中，作用域链由一个全局对象组成。在不包含嵌套的函数体内，作用域链上有两个对象，第一个是定义函数参数和局部变量的对象，第二个是全局对象。