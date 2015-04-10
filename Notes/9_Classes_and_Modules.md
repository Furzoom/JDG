# 类和模块
前面介绍的每个Javascript对象都是一个属性集合，相互之间没有任何联系。在Javascript中也可以定义对象的类，让每个对象都共享某些属性。类的成员或实例都包含一些属性，用以存入或定义它们的状态，其中有些属性定义了它们的行为，这些行为通常由类定义的，而且为所有实例所共享。

在Javascript中，类的实现是基于其原型继承机制的。如果两个实例都从同一个原型对象上继承了属性，则它们就是同一个类的实例。如果两个对象继承自同一个原型，往往意味着(但不是绝对)它们是由同一个构造函数创建并初始化的。

Javascript中类的一个重要特性是动态可继承(dynamically extendable)。

## 9.1 类和原型
在Javascript中，类的所有实例对象都从同一个原型对象上继承属性。因此，原型对象是类的核心。前面定义了inherit()函数，这个函数返回一个新创建的对象，后者继承自某个原型对象，如果定义一个原型对象，然后通过inherit()函数创建一个继承自它的对象，这样就定义了一个Javascript类。通常，类的实例还需要进行一步的初始化，通常是通过定义一个函数来创建并初始化这个对象，如：

```javascript
// range.js 实现一个能表示值的范围的类
// 下面的工厂方法返回一个新的 范围对象
function range(from, to) {
	var r = inherit(range.methods);
	r.from = from;
	r.to = to;
	return r;
}

range.methods = {
	includes: function(x) {
		return this.from <= x && x <= this.to; 
	},
	foreach: function(f) {
		for (var x = Math.ceil(this.from); x <= this.to; x++) f(x);
	},
	toString: function() { return "(" + this.from + "..." + this.to + ")"; }
};

var r = range(1, 3);	
r.includes(2);				// true
r.foreach(console.log);		// 1 2 3	???
console.log(r);				// (1...3)  ???
```

这段代码定义了一个工厂方法range()，range()函数定义了一个属性range.methods，用以存放定义类的原型对象。range()函数给每个范围对象都定义了from和to属性，用以定义范围的起始位置和结束位置，这两个属性是非共享的。

## 9.2 类和构造函数
上面展示了在Javascript中定义类的一种方法，但这种方法并不常用，毕竟它没有定义构造函数，构造函数是用来初始化新创建的对象的。调用构造函数的一个重要特征是，构造函数的prototype属性被用做新对象的原型。这意味着通过同一个构造函数创建的所有对象都继承自一个相同的对象，因此它们都是同一个类的成员：

```javascript
// range2.js 实现一个能表示值的范围的类
function Range(from, to) {
	this.from = from;
	this.to = to;
}

Range.prototype = {
	includes: function(x) { return this.from <= x && x <= this.to; },
	foreach: function(f) { for (var x = Math.ceil(this.from); x <= this.to; x++) f(x); },
	toString: function() { return "(" + this.from + "..." + this.to + ")"; }
};

var r = new Range(1, 3);
r.includes(2);				// true
r.foreach(console.log);		// 1 2 3
console.log(r);				// (1...3)
```

这段代码与上节中比较，工厂方法函数range()转化为构造函数并重命名为Range()。构造对象时，使用new关键字。使用new关键字调用时，会自动使用Range.prototype作为新Range对象的原型。

### 9.2.1 构造函数和类的标识
上文提到原型对象是类的唯一标识：当且仅当两个对象继承自同一个原型对象时，它们才是属于同一个类的实例。而初始化对象的状态的构造函数则不能作为类的标识，两个构造函数的prototype属性可能指向同一个原型对象，那么这两个构造函数创建的实例是属于同一个类的。

构造函数的名字通常用作类名。然而，更根本地讲，当使用instanceof运算符来检测对象是否属于某个类时会用到构造函数。假设这里有一个对象r，要知道r是否是Range对象，这样写：

```javascript
r instanceof Range;			// 如果r继承自Range.prototype，则返回true
```

实际上instanceof运算符并不会检查r是否是由Range()构造函数初始化而来，而会检查r是否继承自Range.prototype。

### 9.2.2 constructor属性
将Range.prototype定义为一个新的对象，这个对象包含类所需要的方法。其实没有必需新创建一个对象，用单个对象直接量的属性就可以方便地定义原型上的方法。任何Javascript函数都可以用做构造函数，并且调用构造函数是需要用到一个prototype属性的。每个Javascript函数(除了Function.bind()方法)都自动拥有一个prototype属性。这个属性的值是一个对象，这个对象包含唯一一个不可枚举属性constructor。constructor属性的值是一个函数对象：

```javascript
var F = function() {};
var p = F.prototype;
var c = p.constructor;
c === F;				// true 对于任意函数F.prototype.constructor === F
```

构造函数的原型中存在预先定义好的constructor属性，这意味着对象通常继承的constructor均指代它们的构造函数。由于构造函数是类的"公共标识"，因此这个constructor属性为对象提供了类。

```javascript
var o = new F();
o.constructor === F;	// true
```

在上面的Range类使用了它自身的一个新对象重写预定义的Range.prototype对象。这人新定义的原型对象不含有constructor属性。因此Range类的实例也不含有constructor属性。可以显式给原型添加一个构造函数：

```javascript
Range.prototype = {
	constructor: Range,
	includes: function(x) { return this.from <= x && x <= this.to; },
	foreach: function(f) { for (var x = Math.ceil(this.from); x <= this.to; x++) f(x); },
	toString: function() { return "(" + this.from + "..." + this.to + ")"; }
};
```

另一种方式就是使用预定义的原型对象，预定义的原型对象包含constructor属性，然后依次给原型对象添加方法：

```javascript
Range.prototype.includes = function(x) { return this.from <= x && x <= this.to; };
Range.prototype.foreach = function(f) { for (var x = Math.ceil(this.from); x < this.to; x ++) f(x); };
Range.prototype.toString = function() { return "(" + this.from + "..." + this.to + ")"; };
```

## 9.3 Javascript中Java式的类继承


## 9.4 类的扩充

## 9.5 类和类型

### 9.5.1 instanceof运算符

### 9.5.2 constructor属性

### 9.5.3 构造函数的名称

### 9.5.4 鸭式辩型

## 9.6 Javascript中的面向对象技术

### 9.6.1 一个例子：集合类

### 9.6.2 一个例子：枚举类型

### 9.6.3 标准转换方法

### 9.6.4 比较方法

### 9.6.5 方法借用

### 9.6.6 私有状态

### 9.6.7 构造函数的重载和工厂方法

## 9.7 子类

### 9.7.1 定义子类

### 9.7.2 构造函数和方法链

### 9.7.3 组合VS子类

### 9.7.4 类的层次结构和抽象类

## 9.8 ECMAScript 5中的类

### 9.8.1 让属性不可枚举

### 9.8.2 定义不可变的类

### 9.8.3 封装对象状态

### 9.8.4 防止类的扩展

### 9.8.5 子类和ECMAScript 5

### 9.8.6 属性描述符

## 9.9 模块

### 9.9.1 用做命名空间的对象

### 9.9.2 作为私有命名空间的函数








Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")
