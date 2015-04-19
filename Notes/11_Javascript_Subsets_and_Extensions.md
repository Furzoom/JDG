# Javascript的子集和扩展
到目前为止参照ECMAScript 3和ECMAScript 5已经完整地讨论了Javascript语言。本文介绍Javascript的子集和超集。其中子集定义大部分都是出于安全考虑。如，如何更安全地执行一段由不可信的第三方提供的广告代码。超集是Javascript的扩展，不同Javascript引擎的实现提供了不同的功能

## 11.1 Javascript的子集
大多数语言都会定义它们的子集，用以更安全地执行不可信的第三方代码。

### 11.1.1 精华
在Javascript语言精粹中不提倡使用with和continue语句及eval()函数。提倡使用函数定义表达式而不是函数定义语句来定义函数。循环体和条件分支都使用花括号括起来，它不允许在循环体和条件分支中只包含一条语句时省略花括号，任何语句只要不是以花括号结束都应当使用分号做结尾。

不提倡使用逗号运算符、位运算符以及"++"和"--"。也不提倡使用"=="和"!="，因为用这两个运算符进行比较时会涉及类型的转换，这里推荐使用"==="和"!=="。

由于Javascript并不包含块级作用域，提倡var语句只能出现在函数体的顶部，并要求程序员将函数内所有的变量声明写在一条单独的var语句中，作为函数体的第一条语句。在子集中禁止使用全局变量。

### 11.1.2 子集的安全性
让Javascript代码静态地通过安全检查，必须移除一些Javascript特性：
* eval()和Function()构造函数在任何安全子集里都是禁止使用的，因为它们可以执行任意的代码，而且Javascript无法对这些代码做静态分析。
* 禁止使用this关键字，因为函数(在非严格模式中)可以通过this访问全局对象。而沙箱系统的一个重要目的就是阻止对全局对象的访问。
* 禁止使用with语句，因为with语句增加了静态代码检查的难度。
* 禁止使用某些全局变量。在客户端Javascript中，浏览器窗口对象可以当做全局对象，但也具有双重身份，因此代码中不能有对window对象的引用。同样地，客户端document对象定义了可以用来操控整个页面内容的方法。将对document的控制权交给一段不受信任的代码会有很多隐患。安全子集提供了两种不同的方法来处理类似document这类全局对象。第一种方法是，沙箱完全禁掉它们，并定义一级自定义API用以对分配给它的Web页面做有限制的访问。第二种方法，在水箱代码所运行的容器内定义一个只对外提供安全的标准DOM API的外观面板或document代码对象。
* 禁止使用某些属性和方法，以免在沙箱中的代码拥有过多的权限。这些属性和方法包括arguments对象的两个属性caller和callee、函数的call()和apply()方法，以及constructor和prototype两个属性。非标准的属性也被禁止掉了。比如__proto__。一些子集将这些不安全的属性和全局对象列进黑名单，还有一些子集提供了白名单，给出了推荐使用的安全的属性和方法。
* 静态分析可以有效地防止带有点运算符的属性存取表达式去读写特殊属性。但使用方括号[]来访问属性则与此不同，因为无法对方括号内的字符串表达式做静态分析。基于这个原因，安全子集通常禁止使用方括号，除非方括号内是一个数字或字符串直接量。安全子集将[]替换为全局函数，通过调用全局函数来查询和设置对象属性，这些函数会执行运行时检查以确保它们不会读写那些禁止访问的属性。

## 11.2 常量和局部变量
在Javascript 1.5及后续版本中可以使用const关键字来定义常量。常量可以看成不可重复赋值的变量(对常量的重新赋值会失败但不报错)，对常量的重复声明会报错。

```javascript
const pi = 3.14;
pi = 4;				// 失败
const pi = 4;		// 失败
var pi =4;			// 失败
```

关键字const和关键字var的行为类似，由于Javascript中没有块级作用域，因此常量会被提前至函数定义的顶部。

在Firefox中可以使用let声明块级变量。

## 11.3 解构赋值
在Firefox中支持混合式赋值，称为解构赋值(destructuring assignment)。如：

```javascript
[x, y] = [1, 2];
console.log([x, y]);	// [1, 2]
```

再如：

```javascript
function polar(x, y) {
	return [Math.sqrt(x*x+y*y), Math.atan2(y, x)];
}

function cartesian(r, theta) {
	return [r*Math.cos(theta), r*Math.sin(theta)];
}

var [r, theta] = polar(1.0, 1.0);
var [x, y] = cartesian(r, theta);
```

解构赋值右侧的数组所包含的元素不必和左侧的变量一一对应，左侧多余的变量的赋值为undefined，而右侧多余的值则会忽略，左侧的变量列表可以包含连续的逗号用以跳过右侧对应的值。

```javascript
let [x, y] = [1];			// x = 1, y = undefined
[x, y] = [1, 2, 3];			// x = 1, y = 2
[, x, , y] = [1, 2, 3, 4];	// x = 2, y = 4
```

## 11.4 迭代

### 11.4.1 for/each循环
for/each循环是由E4X规范(ECMAScript for XML)定义的一种新的循环语句。for/each循环和for/in循环非常类似。但for/each并不是遍历对象的属性，而是遍历属性的值：

```javascript
var o = {one: 1, two: 2, three: 3};
for (var p in o) console.log(p);		// one two three
for each (var v in o) console.log(v);	// 1 2 3
```

### 11.4.2 迭代器
略

### 11.4.3 生成器

### 11.4.4 数组推导

### 11.4.5 生成器表达式

## 11.5 函数简写

## 11.6 多catch从句

## 11.7 E4X: ECMAScript for XML


Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")
