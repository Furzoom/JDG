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

要ECMAScript 3中可以使用下面的方法来模拟原型继承：

```javascript
function inherit(p) {
	if (p === null) throw TypeError();
	if (Object.create)
		return Object.create(p);
	var t = typeof p;
	if (t !== "object" && t !== "function") throw TypeError();
	function f() {};
	f.prototype = p;
	return new f();
}
```

## 6.2 属性的查询和设置
可以通过点(.)或方括号([])运算符来获取属性的值。运算符左侧应当是一个表达式，它返回一个对象。对于点(.)来说，右侧必须是一个以属性名称命名的简单标识符。对于方括号来说([])，方括号内必须是一个计算结果为字符串的表达式，这个字符串就是属性的名字：

```javascript
var author = book.author;
var name = author.surname;
var title = book["main title"];
```

和查询属性值的写法一样，通过点和方括号也可以创建属性或给属性赋值：

```javascript
book.edition = 6;
book["main title"] = "ECMAScript";
```

### 6.2.1 作为关联数组的对象
上文提到，下面两个JavaScript表达式的值相同：

```javascript
object.property
object["property"]
```

第一种语法使用点运算符和一个标识符，这和C、Java中访问一个结构体或对象的静态字段非常类似。第二种语法使用方括号和一个字符串，看起来更像数组，只是这个数组元素是通过字符串索引而不是数字索引。这种数组就是所说的关联数组(associative array)，也称作散列(hash)、映射或字典(dictionary)。JavaScript对象都是关联数组。使用方括号访问属性时更加灵活。

### 6.2.2 继承
JavaScript对象具有自有属性(own property)，也有一些属性是从原型对象继承而来的。假设要查询对象o的属性x，如果o中不存在x，那么将会在继续在o的原型对象中查询属性x。如果原型对象中也没有x，但这个原型对象也有原型，那么继续在这个原型对象的原型上执行查询，直到找到x或者查找到一个原型是null的对象为止。对象的原型构成了一个链，通过这个链可以实现属性的继承。如：

```javascript
var o = {};
o.x = 1;
var p = Object.create(o);
p.y = 2;
var q = Object.create(p);
q.z = 3;
q.x + q.y;					// 3
```

如果给对象o的属性x赋值，如果o中已经有属性x(这个属性不是继承来的)，那么这人赋值操作只改变这个已有属性x的值。如果o中不存在属性x，那么赋值操作给o添加一个新属性x。如果之前o继承的属性x，那么这个继承的属性就被新创建的同名属性覆盖了。

属性赋值操作首先检查原型链，以此判定是否允许赋值操作。例如，如果o继承自一个只读属性x，那么赋值操作是不允许的。如果允许属性赋值操作，它也总是在原始对象上创建属性或对已有属性赋值，而不会去修改原型链。如：

```javascript
var unitcircle = {r: 1};
var c = Object.create(unitcircle);
c.x = 1; 
c.y = 1;
c.r = 2;
unitcircle.r;							// 1
```

### 6.2.3 属性访问错误
属性访问并不总是返回或设置一个值。查询一个不存在的属性并不会报错，如果在对象o自身的属性或继承中均未找到属性x，属性表达式o.x返回undefined。但是，如果对象不存在，那么试图查询这个不存在的对象的属性就会报错。null和undefined值都没有属性，因此查询这些值的属性会报错。如：

```javascript
book.subtitle;			// undefined
book.subtitle.length;	// TypeError
```

当然给null和undefined设置属性也会报错。给其他值设置属性也不总是成功，有一些属性是只读的，只能重新赋值，有一些对象不允许新增属性，但让人意外的是，这些设置的失败操作不会报错：

```javascript
Object.prototype = 1;	// 赋值失败，但不报错
```

这是一个bug，在ECMAScript 5的严格模式中将会报错。在如下场景下给对象o设置属性p会失败：
* o中的属性p是只读的；不能给只读属性重新赋值(可以对可配置的只读属性重新赋值)。
* o中的属性p是继承属性，且它是只读的；不能通过同名自有属性覆盖只读的继承属性。
* o中不存在自有属性p：o没有使用setter方法继承属性p，并且o的可扩展性是false。

## 6.3 删除属性
delete运算符可以删除对象的属性。它的操作数应当是一个属性访问表达式。让人感到意外的是，delete只是断开属性和宿主对象的联系，而不会去操作对象中的属性。delete运算符只能删除自有属性，不能删除继承属性(只能在定义这个属性的原型对象上删除它)。当delete表达式删除成功或没有任何副作用时(比如删除不存在的属性)，它返回true。如果delete后不是一个属性访问表达式，delete同样返回true。

```javascript
o = {x: 1};
delete o.x;			// true
delete o.x;			// true
delete o.toString;	// true
delete 1;			// true
```

delete不能删除那些可配置性为false的属性。某些内置对象的属性是不可配置的，如通过变量声明和函数声明创建的全局对象的属性。在严格模式中，删除一个不可配置的属性会报一个类型错误。当在非严格模式中删除全局对象的可配置属性时，可以省略对全局对象的引用，直接在delete操作符后跟随要删除的属性名：

```javascript
this.x = 1;
delete x;			// true
```

然而在严格模式中，delete后跟随一个非法的操作数，则会报语法错误，必须显示指定对象及其属性：

```javascript
delete x;			// SyntaxError
delete this.x;		// true
```

## 6.4 检测属性
Javascript对象可以看作属性的集合，经常需要检测集合中成员的所属关系，判断某个属性是否存在于某个对象中。可以通过in运算符、hasOwnProperty()和propertyIsEnumerable()方法完成这个工作，甚至仅通过属性查询也可以做到这一点。

in运算符的左侧是属性名(字符串)，右侧是对象。如果对象的自有属性或继承属性中包含这个属性则返回true。对象的hasOwnProperty()用来检测给定的名字是否是对象的自有属性。propertyIsEnumerable()是hasOwnProperty()的增加版，只有检测到是自有属性且这个属性的可枚举性为true时，它才返回true。如：

```javascript
var o = {x: 1};
"x" in o;						// true
"y" in o;						// false
"toString" in o;				// true

o.hasOwnProperty("x");			// true
o.hasOwnProperty("y");			// false
o.hasOwnProperty("toString");	// false

var p = Object.create(o);
p.y = 2;
p.propertyIsEnumerable("x");	// false
p.propertyIsEnumerable("y");	// true
Object.prototype.propertyIsEnumerable("toString");	// false
```

更简洁的方式是使用`!==`运算符，如：

```javascript
var o = {x: 1};
o.x !== undefined;				// true
o.y !== undefined;				// false
o.toString !== undefined;		// true
```

in运算符可以区分不存在的属性和存在但值为undefined的属性。如：

```javascript
var o = {x: undefined};
o.x !== undefined;				// false
o.y !== undefined;				// false
"x" in o;						// true
"y" in o;						// false
delete o.x;						// true
"x" in o;						// false
```

## 6.5 枚举属性
除了检测对象是否存在，还经常遍历对象的属性。for/in循环可以在循环体中遍历对象中所有可枚举的属性。有许多实用工具库给`Object.prototype`添加了新的方法或属性，这些方法和属性可以被所有对象继承并使用。然而在ECMAScript 5标准之前，这些新添加的方法是不能定义为不可枚举的，因此它们都可以在for/in循环中枚举出来。为避免这种情况需要使用如下的方式：

```javascript
for (p in o) {
	if (!o.hasOwnProperty(p)) continue;
}
for (p in o) {
	if (typeof o[p] === 'function') continue;
}
```

如下定义了一些操控对象属性的工具：

```javascript
/* 
 * 把p中可枚举的属性复制到o中，并返回o
 * 如果o和p中含有同名属性，则覆盖o中的属性
 * 该函数并不处理getter和setter以及复制属性
 */
function extend(o, p) {
	for (prop in p) {		// 遍历p中的所有属性
		o[prop] = p[prop];
	}
	return o;
}



Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")
