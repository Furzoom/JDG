# 对象
对象是JavaScript的基本数据类型。对象是一种复合值：它将很多值聚合在一起，可以通过名字访问这些值。对象也可看作是属性的无序集合，每一个属性都是一个名/值对。属性名是字符串，因此可以将对象看作是从字符串到值的映射。然而对象不仅仅j字符串到值的映射，除了可以保持自有的属性，JavaScript对象还可以从一个称为原型的对象继承属性。对象的方法通常是继承的属性。

JavaScript对象是动态的，可以新增属性也可以删除属性，常用来模拟表态对象以及表态类型语言中的结构体，有时它们也用做字符串的集合。

除了字符串、数字、true、false、null和undefined之外，JavaScript中的值都是对象。

属性包括名字和值。属性名可以是包含空字符串在内的任意字符串。但对象中不能存在两个同名的属性。值可以是任意JavaScript值，或者可以是一个getter或setter函数，或者两者都有。除了名字和值外，每个属性还有一些与之相关的值，称为属性特性(property attribute)：
* 可写(writable attribute)，表明是否可以设置该属性的值。
* 可枚举(enumerable attribute)，表明是否可以通过for/in循环返回该属性。
* 可配置(configurable attribute)，表明是否可以删除或修改该属性。

在ECMAScript 5之前，通过代码给对象创建的所有属性都是可写的、可枚举的和可配置的。在ECMAScript 5中则可以对这些特性进行配置。

除了包含属性之外，每个对象还拥有三个相关的对象特性(object attribute)：
* 对象的原型(prototype)指向另外一个对象，本对象的属性继承自它的原型对象。
* 对象的类(class)是一个标识对象类型的字符串。
* 对象的扩展标记(extensible flag)指明了是否可以向该对象添加新属性。

如下术语用于区分三类JavaScript对象和两类属性：
* 内置对象(native object)是由ECMAScript规范定义的对象或类。如，数组、函数、日期和正则表达式都是内置对象。
* 宿主对象(host object)是由JavaScript解释器所嵌入的宿主环境定义的。
* 自定义对象(user-defined object)是由运行中rJavaScript代码创建的对象。
* 自有属性(own property)是直接在对象中定义的属性。
* 继承属性(inherited property)是在对象的原型对象中定义的属性。

## 6.1 创建对象
可以通过对象直接量、关键字new和Object.create()函数来创建对象。

### 6.1.1 对象直接量
创建对象最简单的方法就是在JavaScript代码中使用对象直接量，对象直接量是由若干名/值对组成的映射表，名/值对中间用冒号分隔，名/值对之间用分号分隔，整个映射表用花括号括起来。属性名可以是JavaScript标识符也可以是字符串直接量。属性值可以是任意类型的JavaScript表达式，表达式的值就是这个属性的值。如：

```javascript
var empty = { };							// An object with no property
var point = {x: 0, y: 0 };					// Two properties
var point2 = {x: point.x, y:point.y+1 };	// more complex values
var book = {
	"main title": "javascript",
	"sub-title": "The Definitive Guide",
	"for": "all audiences",
	author: {
		firstname: "David",
		surname: "Flanagan"
	}
};
```

对象直接量是一个表达式，这个表达式的每次运算都创建并初始化一个新的对象。每次计算对象直接量的时候，也都会计算它的每个属性的值。也就是说，如果在一个重复调用的函数中的循环体内使用了对象直接量，它将创建很多新对象，并且每次创建的对象的属性值也有可能不同。

### 6.1.2 通过new创建新对象
new运算符创建并初始化一个新对象。关键字new后跟随一个函数调用。这里的函数称作构造函数(constructor)，构造函数用来初始化一个新创建的对象，JavaScript语言核心中的原始类型都包含内置构造函数。如：

```javascript
var o = new Object();
var a = new Array();
var d = new Date();
var r = new RegExp("js");
```

### 6.1.3 原型
每一个JavaScript对象(null除外)都和另一个对象关联。这个对象就是原型对象，每一个对象都从原型继承属性。

所有通过对象直接量创建的对象都具有同一个原型对象，并可以通过JavaScript代码`Object.prototype`获得对原型对象的引用。通过关键字new和构造函数调用创建的对象的原型就是构造函数的prototype属性的值。因此，通过new Object()创建的对象也继承自`Object.prototype`。通过new Array()创建的对象原型就是`Array.prototype`。

没有原型的对象为数不多，`Object.prototype`就是其中之一。它不继承任何属性。其他原型对象都是普通对象，普通对象都具有原型。一系列的链接的原型对象就是所谓的原型链(prototype chain)。

### 6.1.4 Object.create()
EMACScript 5定义了一个名为`Object.create()`的方法，它创建一个新对象，其中第一个参数就是这个对象的原型。`Object.create()`提供的批二个参数是可选参数，用以对对象的属性进行进一步描述。

`Object.create()`是一个表态方法，而不是提供给某个对象调用的方法。使用方法如下：

```javascript
var o1 = Object.create({x: 1, y: 2});
```

可以通过传入参数null来创建一个没有原型的新对象，但通过这种方式创建的对象不会继承任何东西，甚至不包括基础方法，如toString()。如果想创建一个普通的空对象(比如通过{}或new Object()创建的对象)，需要传入`Object.prototype`。

```javascript
var o2 = Object.create(Object.prototype);
```

## 6.2 属性的查询和设置


Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")
