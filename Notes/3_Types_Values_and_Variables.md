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
```
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
