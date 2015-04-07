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
		o[prop] = p[prop];	// 将属性添加到o中
	}
	return o;
}

/* 
 * 把p中可枚举的属性复制到o中，并返回o
 * 如果o和p中含有同名属性，o中的属性将不受影响
 * 该函数并不处理getter和setter以及复制属性
 */
function merge(o, p) {
	for (prop in p) {		// 遍历p中的所有属性
		if (o.hasOwnProperty(prop)) continue;
		o[prop] = p[prop];
	}
	return o;
}

/* 
 * 如果o中的属性在p中没有同名属性，则从o中删除这个属性
 * 返回o
 */
function restrict(o, p) {
	for (prop in o) {
		if (!(prop in p)) delete o[prop];
	}
	return o;
}

/* 
 * 如果o中的属性在p中存在同名属性，则从o中删除这个属性
 * 返回o
 */
function substract(o, p) {
	for (prop in p) {
		if ( prop in p) delete o[prop];
	}
	return o;
}

/*
 * 返回一个新的对象，这个对象同时拥有o的属性和p的属性
 * 如果o和p中有重名属性，使用p中的属性值
 */
function union(o, p) { return extend(extend({}, o), p); }

/*
 * 返回一个新的对象，这个对象拥有o和p中的同名属性
 * 使用o中的值
 */
function intersection(o, p) { return restrict(extend({}, o), p); }

/*
 * 返回一个数组，这个数组中包含的是o中要枚举的自有属性的名字
 */
function keys(o) {
	if (typeof o !== "object") throw TypeError();
	var result = [];
	for (var prop in o) {
		if (o.hasOwnProperty(prop))
			result.push(prop);
	}
	return result;
}
 
```

除了for/in循环之外，ECMAScript 5定义了两个用以枚举属性名称的函数。第一个是Object.keys()，它返回对象中可枚举的自有属性名称组成的数组。

ECMAScript 5中第二个枚举属性的函数是Object.getOwnPropertyNames()，它返回对象中所有自有属性名称组成的数组。

## 6.6 属性getter和setter
对象的属性是由名字、值和一组特性(attribute)构成的，在ECMAScript 5中，属性值可以用一个或两个方法替代，这两个方法就是getter和setter。由setter和getter定义的属性称做存取器属性(accessor property)，它不同于数据属性(data property)，数据属性中有一个简单的值。

当程序查询存取器属性的值时，Javascript调用getter方法，这个方法的返回值就是属性存取表达式的值。当程序设置一个存取器属性的值时，Javascript调用setter方法，将赋值表达式右侧的值当做参数传入setter。

和数据属性不同，存取器属性不具有可写性(writable attribute)。如果属性同时具有getter和setter方法，那么它是一个读/写属性。如果它只有getter方法，那么它是一个只读属性。如果它只有setter方法，那么它是一个只写属性，读取只写属性总是返回undefined。

定义存取器属性的简单的方法是使用对象直接量语法的一种扩展写法：

```javascript
var o = {
	// 普通数据属性
	data_prop: value,
	// 存取器属性都是成对定义的函数
	get accessor_prop() { /*  */ }，
	set accessor_prop(value) { /*  */ }
};
```

如下程序定义了一个2D笛卡尔坐标的对象，它有两个普通的属性x和y分别表示对应点的X坐标和Y坐标，它还有两个等价的存取器属性用来表示点的极坐标：

```javascript
var p = {
	x: 1.0,
	y: 1.0,
	
	get r() { return Math.sqrt(this.x * this.x + this.y * this.y); }
	set r(newvalue) { 
		var oldvalue = Math.sqrt(this.x * this.x + this.y * this.y);
		var ratio = newvalue / oldvalue;
		this.x *= ratio;
		this.y *= ratio;
	},
	
	get theta() { return Math.atan2(this.y, this.x); }
};
```

代码中getter和setter里使用this关键字表示指向这个点的对象。

同数据属性一样，存取器属性是可以继承的，因此可以将上述代码中的对象p当做另一个点的原型，如：

```javascript
var q = Object.create(q);
q.x = 1, q.y = 1;
console.log(q.r);
console.log(q.theta);
```

另外如下代码检测属性的写入值以及在每次属性读取时返回不同值：

```javascript
var serialnum = {
	// $符号暗示这个属性是一个私有属性
	$n: 0,
	
	get next() { return this.$n++; },
	set next(n) {
		if (n >= this.$n) this.$n = n;
		else throw TypeError();
	}
};
```

## 6.7 属性的特性
除了包含名字和值之外，属性还包含一些标识它们可写、可枚举和可配置的特性。在ECMAScript 3中无法设置这些特性，所有通过ECMAScript 3的程序创建的属性都是可写的、可枚举和可配置的，且无法对这些特性做修改。但在ECMAScript 5中这些特性是十分重要的：
* 可以通过这些API给原型对象添加方法，并将它们设置成不可枚举的，这让它们看起来更像内置方法。
* 可以通过这些API给对象定义不能修改或者删除的属性，以锁定这个对象。

可以认为一个属性包含一个名字和4个特性。数据属性的4个特性分别是它的值(value)、可写性(writable)、可枚举性(enumerable)和可配置性(configurable)。存取器属性不具有值(value)特性和可写性，它们的可写性是由setter方法存在与否决定的。因此存取器属性的4个特性是读取(get)、写入(set)、可枚举性和可配置性。

为了实现属性特性的查询和设置操作，ECMAScript 5中定义了一个名为属性描述符(property descriptor)的对象，这个对象代表那4个特性。描述符对象的属性和它们所描述的属性特性是同名的。因此，数据描述符对象的属性有value、writable、enumerable、configurable。存取器属性的描述符对象则用get属性和set属性代替value、writable。其中writable、enumerable、configurable都是布尔值，get属性和set属性是函数值。

通过调用`Object.getOwnPropertyDescriptor()`可以获得某个对象特定属性的属性描述符,只能得到自有属性的描述符：

```javascript
// {value: 1, writable: true, enumerable: true, configurable: true}
Object.getOwnPropertyDescriptor({x: 1}, "x");

// 对于继承属性和不存在的属性，返回undefined
Object.getOwnPropertyDescriptor({}, "x");		// undefined
Object.getOwnPropertyDescriptor({}, "toString");// undefined
```

要想设置属性的特性，或者想让新建属性具有某种特性，则需要调用`Object.defineProperty()`，传入要修改的对象，要创建或修改的属性的名称以及属性描述符对象：

```javascript
var o = {};		// empty object
// 添加一个不可枚举的数据属性x，并赋值为1
Object.defineProperty(o, "x", {value: 1,
								writable: true,
								enumerable: false,
								configurable: true});

// 属性存在，但不可枚举								
o.x;			// 1
Object.keys(o);	// []

// 现在对属性x做出修改，让它变为只读
Object.defineProperty(o, "x", {writable: false});

// 试图改变这个属性的值
o.x = 2;		// 操作失败但不报错，在严格模式中抛出类型异常错误
o.x;			// 1

// 属性依然是可配置的，因此可以通过这种方式对它进行修改
Object.defineProperty(o, "x", {value: 2});
o.x;			// 2

// 将x从数据属性修改为存取器属性
Object.defineProperty(o, "x", {get: function() { return 0; }});
o.x;			// 0
```

传入`Object.defineProperty()`的属性描述符对象不必包含所有4个特性。对于新创建的属性来说，默认的特性值是false或undefined。对于修改的已有属性来说，默认的特性值没有做任何修改。这个方法不修改继承属性。

如果要同时修改或创建多个属性，则需要使用`Object.defineProperties()`。第一个参数是要修改的对象，第二个参数是一个映射表，它包含要新建或修改的属性的名称，以及它们的属性描述符。如：

```javascript
var p = Object.defineProperties({}, {
	x: {value: 1, writable: true, enumerable: true, configurable: true},
	y: {value: 1, writable: true, enumerable: true, configurable: true},
	r: {get: function() {return Math.sqrt(this.x * this.x + this.y * this.y); },
		enumerable: true,
		configurable: true
	}
});
```

对于那些不允许创建或修改的属性来说，如果用`Object.defineProperties()`和`Object.defineProperty()`对其操作就会抛出类型错误异常，比如，给一个不可扩展的对象新增属性就会抛出类型错误异常。任何对`Object.defineProperties()`和`Object.defineProperty()`违反规则的使用都会抛出类型错误异常：
* 如果对象是不可扩展的，则可以编辑已有的自有属性，但不能给它添加新属性。
* 如果属性是不可配置的，则不能修改它的可配置性和可枚举性。
* 如果存取器属性是不可配置的，则不能修改其getter和setter方法，也不能将它转换为数据属性。
* 如果数据属性是不可配置的，则不能将转换为存取器属性。
* 如果数据属性是不可配置的，则不能将它的可写性从false修改为true，但可以从true修改为false。
* 如果数据属性是不可配置且不可写的，则不能修改它的值。然而可配置但不可写属性的值是可以修改的(实际上是先将它标记为可写的，然后修改它的值，最后转换为不可写的).

```javascript
/* 
 * 给Object.prototype添加一个不可枚举的extend()方法
 * 这个方法继承自调用它的对象，将作为参数传入的对象的属性一一复制
 * 除了值之外，也复制属性的所有特性，除非在目标对象中存在同名的属性
 * 参数对象的所有自有对象(包括不可枚举的属性)也会一一复制
 */
Object.defineProperty(Object.prototype,
	"extend",
	{
		writable: true,
		enumerable: false,
		configurable: true,
		value: function(o) {
			// 得到所有的自有属性，包括不可枚举的属性
			var names = Object.getOwnPropertyNames(o);
			for (var i = 0; i < names.length; i++) {
				if (names[i] in this) continue;
				var desc = Object.getOwnPropertyDescriptor(o, names[i]);
				Object.defineProperty(this, names[i], desc);
			}
		}
	}
);

## 6.8 对象的三个属性
每一个对象都有与之相关的原型(prototype)、类(class)和可扩展性(extensible attribute)。

### 6.8.1 原型属性
对象的原型属性是用来继承属性的，常把"o的原型属性"直接称为"o原型"。

原型属性是在实例对象创建之初就设置好的。通过对象直接量创建的对象使用Object.prototype作为它们的原型。通过new创建的对象使用构造函数的prototype属性作为它们的原型。通过Object.create()创建的对象使用第一个参数作为它们的原型。

在ECMAScript 5中，将对象作为参数传入`Object.getPrototypeOf()`可以查询它的原型。在ECMAScript 3中，则没有与之等价的函数。如：

```javascript
var a = {x: 1};
var b = Object.create(a);
Object.getPrototypeOf(b);	// Object {x: 1}
```

要想检测一个对象是否是另一个对象的原型(或处于原型链中)，请使用isPrototypeOf()方法，如：

```javascript
var p = {x: 1};
var o = Object.create(p);
p.isPrototypeOf(o);			// true
Object.prototype.isPrototypeOf(o);	// true
```

isPrototypeOf()函数的实现的功能与instanceof运算符非常类似。

### 6.8.2 类属性
对象的类属性(class attribute)是一个字符串，用以表示对象的类型信息。ECMAScript 3和ECMAScript 5都未提供设置这个属性的方法，并只有一种间接的方法可以查询它。默认的toString()方法返回了如下这种格式的字符串：

```javascript
[object class]
```

因此，要想获得对象的类，可以调用对象的toString()方法，然后提取已返回字符串的第8位到倒数第二位位置之间的字符。如果对象继承的toString()方法重写了，必须间接调用Function.call()方法。如：

```javascript
function classof(o) {
	if (o === null) return "Null";
	if (o === undefined) return "Undefined";
	return Object.prototype.toString.call(o).slice(8, -1);
}
```

classof()函数可以传入任何类型的参数。数字、字符串和布尔值可以直接调用toString()方法，就和对象调用toString()方法一样，并且这个函数包含了对null和undefined的特殊处理。如：

```javascript
classof(null);			// "Null"
classof(1);				// "Number"
classof("");			// "String"
classof(false);			// "Boolean"
classof({});			// "Object"
classof([]);			// "Array"
classof(/./);			// "RegExp"
classof(new Date());	// "Date"
classof(window);		// "Window" or "global"
function f() {};
classof(new f());		// "Object"
classof(function(){});	// "Function"
```

### 6.8.3 可扩展性
对象的可扩展性表示是否可以给对象添加新属性。所有内置对象和自定义对象都是显示可扩展的，宿主对象的可扩展性是由Javascript引擎定义的。在ECMAScript 5中，所有内置对象和自定义对象都是可扩展的，除非将它们转换为不可扩展的。

ECMAScript 5定义了用来查询和设置对象可扩展性的函数。通过将对象传入`Object.esExtensible()`，来判断该对象是否是可扩展的。如果想将对象转换为不可扩展的，需要调用`Object.preventExtensions()`，将待转换的对象作为参数传进去。注意，一旦将对象转换为不可扩展的，就无法将其转换回可扩展的了。同样需要注意的是，preventExtensions()只影响到对象本身的可扩展性。如果给一个不可扩展的对象的原型添加属性，这个不可扩展的对象同样会继承这些新的属性。

Object.seal()和Object.preventExtensions()类似，除了能够将对象设置为不可扩展的，还可以将对象的所有自有属性都设置为不可配置的。也就是说，不能给个对象添加属性，而且它已有的属性不能删除或配置，不过它已有的可写属性依然可以设置。对于那些已经封闭(sealed)起来的对象是不能解封的。可以使用Object.isSealed()来检测对象的是否封闭。

Object.freeze()将更严格地锁定对象。除了将对象设置为不可扩展的和将其属性设置为不可配置的之外，还可以将它自有的所有数据属性设置为只读(如果对象的存取器属性具有setter方法，存取器属性将不受影响)。使用Object.isFrozen()来检测对象是否冻结。

Object.preventExtensions()、Object.seal()、Object.freeze()都返回传入的对象，可以进行如下使用：

```javascript
// 创建一个封闭的对象，包括一个冻结的原型和一个可枚举的属性
var o = Object.seal(Object.create(Object.freeze({x: 1}), {y: {value: 2, writable: true}}));
```

## 6.9 序列化对象
对象序列化(serialization)是指将对象的状态转换为字符串，也可将字符串还原为对象。ECMAScript 5提供了内置函数JSON.stringify()和JSON.parse()用来序列化和还原Javascript对象。这些方法都使用JSON作为数据交换格式，JSON的全称是Javascript Object Notation，Javascript对象表示法，它的语法和Javascript对象与数组直接量的语法非常相近：

```javascript
o = {x: 1, y: {z: [false, null, ""]}};		// 定义一个测试对象
s = JSON.stringify(o);						// '{"x":1,"y":{"z":[false,null,""]}}'
p = JSON.parse(s);							// p是o的深拷贝
```

JSON的语法是Javascript语法的子集，它并不能表示Javascript里的所有值。支持对象、数组、字符串、无穷大数字、true、false和null，并且它们可以序列化和还原。NaN、Infinity和-Infinity序列化的结果是null，日期对象序列化的是ISO格式的日期字符串，但JSON.parse()依然保留它们的字符串形态，而不会将它们还原为原始日期对象。函数、RegExp、Errror对象和undefined值不能序列化和还原。JSON.stringify()只能序列化对象可枚举的自有属性。对于一个不能序列化的属性来说，在序列化后的输出字符串中会将这个属性省略掉。JSON.stringify()和JSON.parse()都可以接收第二个可选参数，通过传入需要序列化或还原的属性列表来定制自定义的序列化或还原操作。

## 6.10 对象方法
除了那些不通过原型显示创建的对象，所有的Javascript对象都从Object.prototype继承属性。这些继承属性主要是方法，因为Javascript程序员普遍对继承方法更感兴趣。本节主要对定义在Object.prototype里的对象方法展开讲解。

### 6.10.1 toString()方法
toString()方法没有参数，它将返回一个表示调用这个方法的对象值的字符串。在需要将对象转换为字符串的时候，Javascript都会调用这个方法。

默认的toString()依法的返回值带有的信息量很少，因此很多类都带有自定义的toString()方法。

### 6.10.2 toLocaleString()方法
除了基本的toString()方法之外，对象都包含toLocaleString()方法，这个方法返回一个表示这个对象的本地化字符串。Object中默认的toLocaleString()方法并不做任何本地化自身的操作，它仅调用toString()方法并返回对应值。Date和Number类对toLocaleString()方法做了定制，可以用它对数字、日期、时间做本地化的转换。Array类的toLocaleString()方法和toString()方法很像，唯一的不同是每个数组元素会调用toLocaleString()方法转换为字符串，而不是调用各自的toString()方法。

### 6.10.3 toJSON()方法
Object.prototype实际上没有定义toJSON()方法，但对于需要执行序列化的对象来说，JSON.stringify()方法会调用toJSON()方法。

### 6.10.4 valueOf()方法
valueOf()方法和toString()方法非常类似，但往往当Javascript需要将对象转换为某种原始值而非字符串的时候才会调用它，尤其是转换为数字的时候。如果在需要使用原始值的上下文中使用了对象，Javascript就会自动调用这个方法。


Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")
