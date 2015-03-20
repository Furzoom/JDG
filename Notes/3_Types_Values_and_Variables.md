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
