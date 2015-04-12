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
在Java或其他类似强类型面向对象语言中，类成员可能是这样的：
> 实例字段
>> 它们是基于实例的属性或变量，用以保存独立对象的状态。
> 实例方法
>> 它们是类的所有实例所共享的方法，由每个独立的实例调用。
> 类字段
>> 这些属性或变量是属于类的，而不是属于类的某个实例的。
> 类方法
>> 这些方法是属于类的，而不是属于类的某个实例的。

Javascript和Java的不同在于，Javascript中的函数都是以值的形式出现的，方法和字段之间并没有太大的区别。如果属性值是函数，那么这个属性就定义一个方法，否则它就是一个普通的属性。Javascript中的类牵扯三种不同的对象，三种对象的属性的行为和下面三种类成员非常相似：
> 构造函数对象
>> 构造函数(对象)为Javascript的类定义了名字。任何添加到这个构造函数对象中的属性都是类字段和类方法。
> 原型对象
>> 原型对象的属性被类的所有所有实例继承，如果原型对象的属性值是函数的话，这个函数就作为类的实例的方法来调用。
> 实例对象
>> 类的每个实例都是一个独立的对象，直接给这个实例定义的属性是不会为所有实例对象所共享的。定义在实例上的非函数属性，实际上是实例的字段。

在Javascript中定义类的步骤可以缩减为一个分三步的算法。第一步，先定义一个构造函数，并设置初始化新对象的实例属性。第二步，给构造函数的prototype对象定义实例的方法。第三步，给构造函数定义类字段和类属性。可以将这三步封装进一个简单的defineClass()函数中：

```javascript
function defineClass(constructor,			// 设置实例的属性的函数
					 methods,				// 实例的方法，复制到原型中
					 statics) {				// 类属性，复制到构造函数中
	if (methods) extend(constructor.prototype, methods);
	if (statics) extend(constructor, statics);
	return constructor;
}

// Range类的另一个实现
var SimpleRange = defineClass(function(f, t) { this.f = f; this.t = t; },
				{
					includes: function(x) { this.f <= x && x <= this.t; },
					toString: function() { return this.f + "..." + this.t; }
				},
				{ upto: function(t) { return new SimpleRange(0, t); } });
```

再如下面的复数类：

```javascript
function Complex(real, imaginary) {
	if (isNaN(real) && isNaN(imaginary))
		throw new TypeError();
	this.r = real;
	this.i = imaginary;
}

Complex.prototype.add = function(that) {
	return new Complex(this.r + that.r, this.i + that.i);
};

Complex.prototype.mul = function(that) {
	return new Complex(this.r * that.r - this.i * that.i,
					   this.r * that.i + this.i * that.r);
};

Complex.prototype.mag = function() {
	return Math.sqrt(this.r * this.r + this.i * this.i);
};

Complex.prototype.neg = function() {
	return new Complex(-this.r, -this.i);
};

Complex.prototype.toString = function() {
	return "{" + this.r + "," + this.i + "}";
};

Complex.prototype.equals = function(that) {
	return that != null &&
			that.constructor === Complex &&
			this.r === that.r &&
			this.i === that.i;
};

Complex.ZERO = new Complex(0, 0);
Complex.ONE = new Complex(1, 0);
Complex.I = new Complex(0, 1);

Complex.parse = function(s) {
	try {
		var m =  Complex._format.exec(s);
		return new Complex(parseFloat(m[1]), parseFloat(m[2]));
	} catch(x) {
		throw new TypeError("Can't parse '" + s + "' as a complex number.");
	}
};

Complex._format = /^\{([^,]+),([^,]+)\}$/;
```

使用如下：

```javascript
var c = new Complex(2, 3);
var d = new Complex(c.i, c.r);
c.add(d).toString();				// "{5,5}"
Complex.parse(c.toString()).add(c.neg()).equals(Complex.ZERO);
```

## 9.4 类的扩充
Javascript中基于原型的继承机制是动态的：对象从其原型继承属性，如果创建对象之后原型的属性发生改变，也会影响到继承这个原型的所有实例对象。这意味着可以通过给原型对象添加新方法扩充Javascript类。如给Complex类添加计算得数的共轭算法：

```javascript
Complex.prototype.conj = function() { return new Complex(this.r, this.i); };
```

Javascript内置类的原型对象也是一样可以扩展，也就是说可以给数字、字符串、数组、函数等数据类型添加方法。如：

```javascript
if (!Function.prototype.bind) {
	Function.prototype.bind = function(o /*, args */) {
		// bind()方法的代码...
	};
}
```

其他例子如：

```javascript
Number.prototype.times = function(f, context) {
	var n = Number(this);
	for (var i = 0; i < n; i ++) { f.call(context, i); }
};

var n = 3;
n.times(function(n) { console.log(n + " hello"); });

String.prototype.trim = String.prototype.trim || function() {
	if (!this) return this;
	return this.replace(/^\s+|\s+$/g, "");
};

Function.prototype.getName = function() {
	return this.name || this.toString().match(/function\s*([^()*])\(/)[1];
};
```

可以给Object.prototype添加方法，从而使所有的对象都可以调用这些方法。但这种做法并不推荐，因为在ECMAScript 5之前，无法将这些新增的方法设置为不可枚举的，如果给Object.prototype添加属性，这些属性是可以被for/in循环遍历到的。使用Object.defineProperty()方法可以安全地扩充Object.prototype。

## 9.5 类和类型
Javascript定义了少量的数据类型：null、undeinfed、布尔值、数字、字符串、函数和对象。typeof运算符可以得出值的类型。然后更希望将类作为类型来对待，这样就可以根据对象所必的类来区分它们。Javascript语言核中的内置对象可以根据它们的class属性来区分彼此，如classof()函数。但当使用本章所提到的技术来定义类时，实例对象的class属性都是Object。

### 9.5.1 instanceof运算符
instanceof运算符左操作数是待检测其类的对象，右操作数是定义类的构造函数。如果o继承自c.prototype，则表达式`o instanceof c`值为true。这里的继承可以上是直接继承，如果o所继承的对象继承自另一个对象，后一个对象继承自c.prototype，这个表达式的运算结果也是true。

构造函数是类的公共标识，但株型是唯一的标识。尽管instanceof运算符的右操作数是构造函数，但计算过程实际上是检测 了对象的继承关系，而不是检测创建对象的构造函数。

如果想检测对象的原型链上是否存在某个特定的原型对象，有没有不使用构造函数作为中介的方法呢？可以使用isPrototypeOf()方法，如：

```javascript
range.methods.isPrototypeOf(r);	// range.methods是原型对象
```

instanceof运算符和isPrototypeOf()方法的缺点是，无法通过对象来获得类名，只能检测对象是否属于指定的类名。在客户端Javascript中还有一个比较严重的问题，在多窗口和多框架子页面的Web应用中兼容性不佳。每个窗口或框架子页面都具有单独的执行上下文，每个上下文都包含独有的全局变量和一组构造函数。在两个不同框架页面中创建的两个数组继承自两个相同但相互独立的原型对象，其中一个框架页面中的数组不是另一个框架页面的Array()构造函数的实例，instanceof运算结果是false。

### 9.5.2 constructor属性
另一个识别对象是否属于某个类的方法是使用constructor属性。因为构造函数是类的公共标识，所以最直接的方法就是使用constructor属性，如：

```javascript
function typeAndValue(x) {
	if (x == null) return "";
	switch(x.constructor) {
		case Number: return "Number: " + x;
		case String: return "String: '" + x + "'";
		case Date: return "Date: " + x;
		case RegExp: return "RegExp: " + x;
		case Complex: return "Complex: " + x;
	}
}
```

在代码中关键字case后的表达式都是函数，如果改用typeof运算符或获取到对象的class属性的话，它们应当改为字符串。使用constructor属性检测对象属于某个类的技术的不足之处和instanceof一样。在多个执行上下文的场景中它是无法正常工作的。在每个框架页面各自拥有独立的构造函数集合，一个框架页面中的Array构造函数和另一个框架页面的Array构造函数不是同一个构造函数。

在Javascript中也并非所有的对象都包含constructor属性。在每个新创建的函数原型上默认会有constructor属性，但常常会忽略原型上的constructor属性。

### 9.5.3 构造函数的名称
为解决在不同执行上下文中存在构造函数的多个副本的问题，使用函数构造函数名字而不是构造函数本身作为类标识符。一个窗口的Array构造函数和另一个窗口的Array构造函数是不相等的，但它们的名字是一样的。在一些Javascript的实现中为函数对象提供了一个非标准的属性name，用来表示函数的名称。对于没有name属性的Javascript实现来说，可以将函数转换为字符串，然后从中提取出函数名。

```javascript
/**
 * 以字符串形式返回o的类型：
 * -如果o是null，返回"null"，如果o是NaN，返回"nan"
 * -如果typeof所返回的值不是"object"，则返回这个值
 * -如果o的类不是"Object"，则返回这个值
 * -如果o包含构造函数并且这个构造函数具有名称，则返回这个名称
 * -否则，一律返回"Object"
 **/
function type(o) {
	var t, c, n;			// type, class, name
	if (o === null) return "null";
	if (o !== o) return "nan";
	if ((t = typeof(o)) !== "object") return t;
	if ((c = classof(o)) !== "Object") return c;
	if (o.constructor && typeof o.constructor === "function" &&
		(n = o.constructor.getName())) return n;
	return "Object";
}

function classof(o) {
	return Object.prototype.toString.call(o).slice(8, -1);
};

Function.prototype.getName = function() {
	if ('name' in this) return this.name;
	return this.name = this.toString.match(/function\s*([^(]*)\(/)[1];
};
```

这种使用构造函数名字来识别对象的类的做法和使用constructor属性一样有一个问题，并不是所有的对象都具有constructor属性，此外，并不是所有的函数都有名字。如果使用不带名字的函数定义表达式定义一个构造函数，getName()方法则会返回空字符串：

```javascript
var Complex = function(x, y) { this.r = x; this.i = y; }
var Range = function Range(f, t) { this.from = f; this.to = t; }
```

### 9.5.4 鸭式辩型
上面的几种方式都会有些问题，至少在客户端Javascript中是如此。解决的办法就是不关注对象的类是什么，而关注对象能做什么。这种思考问题的方式在Python和Ruby中非常普遍，称为鸭式辩型：

```javascript
// 像鸭子一样走路、游泳并且嘎嘎叫的鸟就是鸭子。
```

下面的代码按鸭式辩型理念定义了quacks()函数。quacks()用以检查一个对象是否实现了剩下参数所表示的方法。对于除第一个参数外的每个参数，如果是字符串的话则直接检查是否存在以它命名的演绎法，如果是对象的话则检查第一个对象中的方法是否在这个对象中也具有同名的方法，如果参数是函数，则假定它是构造函数，函数将检查第一个对象实现的方法是否在构造函数的原型对象中也具有同名的方法。

```javascript
function quacks(o /*, ... */) {
	for (var i = 1; i < arguments.length; i ++) {
		var arg = arguments[i];
		switch (typeof arg) {
			case 'string':
				if (typeof o[arg] !== "function") return false;
				continue;
			case 'function':
				arg = arg.prototype;
			case 'object':
				for (var m in arg) {
					if (typeof arg[m] !== "function") continue;
					if (typeof o[m] !== "function") return false;
				}
		}
	}
	return true;
}
```

quacks()函数无法得知这些已经存在的属性的细节信息。

## 9.6 Javascript中的面向对象技术

### 9.6.1 一个例子：集合类
集合(set)是一种数据结构，用以表示非重复值的无序集合。集合的基础方法包括添加值、检测值是否在集合中。Javascript的对象是属性名以及与之对应的值的基本集合。如：

```javascript
function Set() {
	this.values = {};
	this.n = 0;
	this.add.apply(this, arguments);
}

Set.prototype.add = function() {
	for (var i = 0; i < arguments.length; i ++) {
		var val = arguments[i];
		var str = Set._v2s(val);
		if (!this.values.hasOwnProperty(str)) {
			this.values[str] = val;
			this.n++;
		}
	}
	return this;
};

Set.prototype.remove = function() {
	for (var i = 0; i < arguments.length; i ++) {
		var str = Set._v2s(arguments[i]);
		if (this.values.hasOwnProperty(str)) {
			delete this.values[str];
			this.n++;
		}
	}
	return this;
};

Set.prototype.contains = function(value) {
	return this.values.hasOwnProperty(Set._v2s(value));
};

Set.prototype.size = function() {
	return this.n;
};

Set.prototype.foreach = function(f, context) {
	for (var s in this.values) {
		if (this.values.hasOwnProperty(s))
			f.call(context, this.values[s]);
	}
};

Set._v2s = function(val) {
	switch(val) {
		case undefined:	return 'u';
		case null:		return 'n';
		case true:		return 't';
		case false:		return 'f';
		default: 
			switch(typeof val) {
				case 'number': return '#'+val;
				case 'string': return '"'+val;
				default: return '@'+objectId(val);
			}
	}
	function objectId(o) {
		var prop = "|**objectid**|";
		if (!o.hasOwnProperty(prop))
			o[prop] = Set._v2s.next++;
		return o[prop];
	}
};

Set._v2s.next = 100;
```

### 9.6.2 一个例子：枚举类型
枚举类型(enumerated type)是一种类型，它是值的有限集合，如果值定义为这个类型则该值是可列出的。如下单独函数enumeration()它不是构造函数，它并没有定义一个名叫enumeration的类，它是一个工厂方法，每次调用它都　会创建并返回一个新的类，如：

```javascript
var Coin = enumeration({Penny: 1, Nickel: 5, Dime: 10, Quarter: 25});
var c = Coin.Dime;
c instanceof Coin;				// true
c.constructor == Coin			// true
Coin.Quarter + 3 * Coin.Nickel;	// 40
Coin.Dime == 10;				// true
Coin.Dime > Coin.Nickel;		// true
String(Coin.Dime) + ":" + Coin.Dime;	// "Dime:10"
```

```javascript
function enumeration(namesToValues) {
	var enumeration = function() { throw "Can't Instantiate Enumerations"; };
	var proto = enumeration.prototype = {
		constructor: enumeration,
		toString: function() { return this.name; },
		valueOf: function() { return this.value; },
		toJSON: function() { return this.name; }
	};
	enumeration.values = [];
	for (name in namesToValues) {
		var e = inherit(proto);
		e.name = name;
		e.value = namesToValues[name];
		enumeration[name] = e;
		enumeration.values.push(e);
	}
	enumeration.foreach = function(f, c) {
		for (var i = 0; i < this.values.length; i ++) f.call(c, this.values[i]);
	};
	
	return enumeration;
}
```

使用上面的enumeration()函数实现一副扑克牌的类。

```javascript
function Card(suit, rank) {
	this.suit = suit;
	this.rank = rank;
}

Card.Suit = enumeration({Clubs: 1, Diamonds: 2, Hearts: 3, Spades: 4});
Card.Rank = enumeration({Two: 2, Three: 3, Four: 4, Five: 5, Six: 6, 
						 Seven: 7, Eight: 8, Nine: 9, Ten: 10, Jack: 11,
						 Queen: 12, King: 13, Ace: 14});
Card.prototype.toString = function() {
	return this.rank.toString() + " of " + this.suit.toString();
};

Card.prototype.compareTo = function(that) {
	if (this.rank < that.rank) return -1;
	if (this.rank > that.rank) return 1;
	return 0;
};

Card.orderByRank = function(a, b) { return a.compareTo(b); };

Card.orderBySuit = function(a, b) {
	if (a.suit < b.suit) return -1;
	if (a.suit > b.suit) return 1;
	if (a.rank < b.rank) return -1;
	if (a.rank > b.rank) return 1;
	return 0;
};

function Deck() {
	var cards = this.cards = [];
	Card.Suit.foreach(function(s) {
							Card.Rank.foreach(function(r) {
									cards.push(new Card(s, r));
							});
	});
}

Deck.prototype.shuffle = function() {
	var deck = this.cards, len = deck.length;
	for (var i = len - 1; i > 0; i --) {
		var r = Math.floor(Math.random() * (i + 1)), temp;
		temp = deck[i], deck[i] = deck[r], deck[r] = temp;
	}
	return this;
};

Deck.prototype.deal = function(n) {
	if (this.cards.length < n) throw "Out of cards";
	return this.cards.splice(this.cards.length - n, n);
};

var deck = (new Deck()).shuffle();
var hand = deck.deal(13).sort(Card.orderBySuit);
```
	
### 9.6.3 标准转换方法
对象类型转换要用到的一些方法是在需要的时候由Javascript解释器自动调用的。不需要为定义的每个类都实现这些方法，但这些方法的确非常重要，如果没有为自定义的类实现这些方法，也应当是有意为之，而不应当因为疏忽而漏掉它们。

最重要的方法首当toString()。这个作用是返回一个可以表示这个对象的字符串。在希望使用字符串的地方用对象的话，Javascript会自动调用这个方法。如果没有实现这个方法，类会默认从Object.prototype中继承toString()方法，这个方法的运算结果是"[object Object]"，这个字符串用处不大。toString()方法应当返回一个可读的字符串，这样最终用户才能将这个输出值利用起来，然而有时候并不一定非要如此，不管怎样，可以返回可读字符串的toString()方法也会让程序调试变得更加轻松。

toLocaleString()和toString()极为类似：toLocaleString()是以本地敏感性的方法来将对象转换为字符串。默认情况下，对象所继承的toLocaleString()方法只是简单地调用toString()方法。有一些内置类型包含有用的toLocaleString()方法用以实际上返回本地化相关的字符串。如果需要为对象到字符串的转换定义toString()，那么同样需要定义toLocaleString()方法用以处理本地化的对象到字符串的转换。

第三个方法是valueOf()，它用来将对象转换为原始值。如，当数学运算符(除了+运算符外)和关系运算符作用于数字文本表示的对象的，会自动调用valueOf()方法。大多数对象都没有合适的原始值来表示它们，也没有定义这个方法。

第四个方法是toJSON()，这个方法是由JSON.stringify()自动调用的。JSON格式用于序列化良好的数据结构，而且可以处理Javascript原始值、数组和纯对象。它和类无关，当对一个对象执行序列化操作时，它会忽略对象的原型和构造函数。如，将Complex对象作用参数传入JSON.stringify()，将会返回诸如{"r":1, "i":-1}之这种字符串。如果将这些字符中传入JSON.parse()，则会得到一个和Complex对象具有相同属性的对象，但这个对象不会包含从Complex继承来的方法。如果一个对象没有toJSON()方法，JSON.stringify()并不会对传入的对象做序列化操作，而会调用toJSON()来执行序列化操作。

Set类并没有定义上述方法中的任何一个。Javascript中没有哪个原始值可以表示集合，因此也没有必要定义valueOf()方法，但该类应当包含toString()、toLocaleString()和toJSON()方法。这里使用extend()来向Set.prototype来添加方法。

```javascript
extend(Set.prototype, {
			toString: function() {
				var s = "{", i = 0;
				this.foreach(function(v) {s += ((i++>0) ? ", " : "") + v; });
				return s + "}";
			},
			toLocaleString: function() {
				var s = "{", i = 0;
				this.foreach(function(v) {
								if (i++ > 0) s += ", ";
								if (v == null) s += v;	// null和undefined
								else s += v.toLocaleString();
							});
				return s + "}";
			},
			toArray: function() {
				var a = [];
				this.foreach(function(v) { a.push(v); });
				return a;
			}
});
Set.prototype.toJSON = Set.prototype.toArray;
```
				
### 9.6.4 比较方法
Javascript的相等运算符比较对象时，比较的是引用而不是值。也就是说，给定两个对象引用，如果要看它们的是否指向同一个对象，不是检查这两个对象是否具有相同的属性名和相同的属性值，而是直接比较两个单独的对象是否相等，或者比较它们的顺序。如果定义一个类，并且希望比较类的实例，应该定义合适的方法来执行比较操作。

为了能让自定义类的实例具备比较的功能，定义一个名收equals()实例方法。这个方法只能接收一个实参，如果这个实参和调用此方法的对象相等的话则返回true。当然，这里所说的相等的含义是根据类的上下文来决定的。对于简单的类，可以通过简单地比较它们的constructor属性来确保两个对象是相同类型，然后比较两个对象的实例属性以保证它们的值相等。为Range类实现这个方法：

```javascript
Range.prototype.constructor = Range;
Range.prototype.equals = function(that) {
	if (that == null) return false;
	if (that.constructor !== Range) return false;
	return this.from == that.from && this.to == that.to;
};
```

给Set类定义equals()方法稍微有些复杂。不能简单地比较两个集合的values属性，还要进行更深层次的比较：

```javascript
Set.prototype.equals = function(that) {
	if (this === that) return true;
	if (!(that instanceof Set) return false;
	if (this.size() != that.size()) return false;
	try {
		this.foreach(function(v) { if (!tha.contains(v)) throw false; });
		return true;
	} catch(x) {
		if ( x === false) return false;
		throw x;
	}
};
```

对于某些类来说，往往需要比较一个实例与另一个实例的大小关系。如果将对象用于Javascript的关系运算符，Javascript会首先调用对象的valueOf()方法，如果这个方法返回一个原始值，则直接比较原始值。多数类没有valueOf()方法，可以定义一个名叫compareTo()方法。如果this对象小于参数对象，应返回小于0的值，如果this对象大于参数对象，应返回大于0的值，如果两个对象相等，应返回0。

给类定义了compareTo()方法，就可以对类的实例组成的数组进行排序了。Array.sort()方法可以接收一具可选的参数，这个参数是一个函数，用来比较两个值的大小，如果这个函数返回值的给定和compareTo()方法保持一致。

### 9.6.5 方法借用
Javascript中的方法没有特别之处：就是一些简单的函数赋值给了对象的属性，可以通过对象来调用它。一个函数可以赋值给两个属性，然后作为两个方法来调用它。如Set类中的toArray()方法和toJSON()方法。

多个类中的方法可以共用一个单独的函数。如，Array类通常定义了一些内置依法，如果定义了一个类，它的实例是类数组的对象，则可以从Array.prototype中将函数复制至所定义的类的原型对象中。

还可以自己定义泛型方法(generic method)。下面的定义了泛型方法toString()和equals()。可以被Range、Complex和Card这些简单的类使用。

```javascript
var generic = {
	toString: function() {
		var s = '[';
		if (this.constructor && this.constructor.name)
			s += this.contructor.name + ': ';
		var n = 0;
		for (var name in this) {
			if (!this.hasOwnProperty(name)) continue;
			var value = this[name];
			if (typeof value === "function") continue;
			if (n ++ ) s += ", ";
			s += name + "=" + value;
		}
		return s + ']';
	},
	equals: function (that) {
		if (that == null) return false;
		if (this.constructor !== that.constructor) return false;
		for (var name in this) {
			if (name === "|**objectid**|") continue;
			if (!this.hasOwnProperty(name)) continue;
			if (this[name] !== that[name]) return false;
		}
		return true;
	}
};
```

### 9.6.6 私有状态
在经典的面向对象编程中，经常需要将对象的某个状态封装或隐藏在对象内，只能通过对象的方法才能些状态，对外只暴露一些重要的状态变量可以直接读写。在Javascript中可以将变量或者参数闭包在一个构造函数内来模拟实现私有实例字段，调用构造函数会创建一个实例。需要在构造函数内部定义一个函数(因此这人函数可以访问构造函数内部的参数和变量)，并将这个函数赋值给新创建对象的属性。下面的例子对Range类的另一各封装：

```javascript
function Range(from, to) {
	this.from = function() { return from; };
	this.to = function() { return to; };
}

Range.prototype = {
	constructor: Range,
	includes: function(x) { return this.from() < x && x <= this.to(); },
	foreach: function(f) {
		for (var x = Math.ceil(this.from()), max = this.to(); x <= max; x ++) f(x);
	},
	toString: function() { return "(" + this.from() + "..." + this.to() + ")"; }
};
```

### 9.6.7 构造函数的重载和工厂方法
有时候，需要对象的初始化有多种方法。如，通过半径和角度来初始化一个Complex对象等。可以通过重载(overload)这个构造函数让它根据传入参数的不同来执行不同的初始化方法。如：

```javascript
function Set() {
	this.values = {};
	this.n = 0;
	if (argumnets.length == 1 && isArrayLike(arguments[0]))
		this.add.apply(this, arguments[0]);
	else if (arguments.length > 0)
		this.add.apply(this, arguments);
}
```

这段代码定义的Set()构造函数可以将一组元素作为参数列表传入，也可以传入元素组成的数组。但是这个构造函数有多义性，如果集合的某个成员是一个数组就无法通过这个构造函数来创建这个集合了。但可以先创建一个空集合，然后显式调用add()方法：

```javascript
Set.fromArray = function(a) {
	s = new Set();
	s.add.apply(s, a);
	return s;
};
```

可以给工厂方法定义任意的名字，不同名字的工厂方法用以执行不同的初始化。但由于构造函数是类的公有标识，因此每个类只能有一个构造函数。但这并不是一个必须遵守的规则。在Javascript中是可以定义多个构造函数继承自一个原型对象的，如果这样做的话，由这些构造函数的任意一个所创建的对象都属于同一类型。并不推荐这种技术，但下面的示例代码使用这种技术定义了该类型的一个辅助构造函数：

```javascript
function SetFromArray(a) {
	Set.apply(this, a);
}

SetFromArray.prototype = Set.prototype;

var s = new SetFromArray([1, 2, 3]);
s instanceof Set;		// true;
```

## 9.7 子类
在面向对象编程中，类B可以继承自另外一个类A。A称为父类(superclass)，B称为子类(subclass)。B的实例从A继承了所有的实例方法。类B可以定义自己的实例方法，有些方法可以重载A中的同名方法，B中的重载方法可能会调用A中的重载方法，这种做法称为方法链(method chaining)。子类的构造函数B()有时需要调用父类的构造函数A()，这种做法称为构造函数链(constructor chaining)。子类还可以有子类，当涉及类的层次结构时，往往需要定义抽象类(abstract class)。抽象类中定义的方法没有实现。抽象类中的抽象方法是在抽象类的具体子类中实现的。

在Javascript中创建子类的关键之处在于，采用合适的方法对原型对象进行初始化。如果类B继承自类A，B.prototype必须是A.prototype的后嗣。B的实例继承自B.prototype，后者同样也继承自A.prototype。

### 9.7.1 定义子类
Javascript的对象可以从类的原型对象中继承属性。如果O是类B的实例，B是A的子类，那么O也一定从A中继承了属性。为此，首先要确保B的原型对象继承自A的原型对象。通过inherit()函数，可以这样来实现：

```javascript
B.prototype = inherit(A.prototype);
B.prop.constructor = B;
```

这两行代码是在Javascript中创建子类的关键。

```javascript
function defineSubClass(superclass,		// 父类构造函数
						constructor,	// 新的子类构造函数
						methods,		// 实例方法：复制到原型中
						statics)		// 类属性：复制到构造函数中
{
	constructor.prototype = inherit(superclass.prototype);
	constructor.prototype.constructor = constructor;
	if (methods) extend(constructor.prototype, methods);
	if (statics) extend(constructor, statics);
	return constructor;
}

Function.prototype.extend = function(constructor, methods, statics) {
	return defineSubClass(this, constructor, methods, statics);
};
```

下面是不使用defineSubClass()函数来手动实现子类：

```javascript
function SingletonSet(member) {
	this.member = member;
}
SingletonSet.prototype = inherit(Set.prototype);
extend(SingletonSet.prototype, {
			constructor: SingletonSet,
			add: function() { throw "read-only set"; },
			remove: function() { throw "read-only set"; },
			size: function() { return 1; },
			foreach: function(f, context) { f.call(context, this.member); },
			contains: function(x) { return x === this.member; }
		});
```

这里的SingletonSet类是一个比较简单的实现。

### 9.7.2 构造函数和方法链
上一节的SingletonSet类定义了全新的集合实现，而且将它继承自其父类的核心方法全部替换。然而定义子类时，往往希望对父类的行为进行修改或扩充，而不是完全替换掉它们。为此，构造函数和子类的方法需要调用或链接到父类构造函数和父类方法。

```javascript
function NonNullSet() {
	Set.apply(this, arguments);
}

NonNullSet.prototype = inherit(Set.prototype);
NonNullSet.prototype.constructor = NonNullSet;

NonNullSet.prototype.add = function() {
	for (var i = 0; i < arguments.length; i ++) {
		if (argumnets[i] == null)
			throw new Error("Can't add null or undefined to a NonNullSet");
	}
	return Set.prototype.add.apple(this, arguments);
};
```

这个非null集合的概念推而广之，称为过滤后的集合，这个集合中的成员必须首先传入一个过滤函数再执行添加操作。为此，定义一个类工厂函数，传入一个过滤函数，返回一个新的Set子类。还可以定义接收两个参数的类工厂：子类和用于add()方法的过滤函数，这个工厂方法称为filteredSetSubclass()，如：

```javascript
var StringSet = filteredSetSubclass(Set, function(x) { return typeof x === "string" ;});
var MySet = filteredSetSubclass(NonNullSet, function(x) { return typeof x !== "function"; });
```

```javascript
function filteredSetSubclass(superclass, filter) {
	var constructor = function() {
		superclass.apply(this, arguments);
	};
	var proto = constructor.prototype = inherit(superclass.prototype);
	proto.constructor = constructor;
	proto.add = function() {
		for (var i = 0; i < arguments.length; i ++) {
			var v = arguments[i];
			if (!filter(v)) throw ("value " + v + " rejected by filter");
		}
		superclass.prototype.add.apply(this, arguments);
	};
	return constructor;
}
```

上面用一个函数将创建子类的代码包装起来，这样就可以在构造函数和方法链中使用父类的参数，而不是通过写死某个父类的名字来使用它的参数。如果想修改父类，只须修改一处代码即可，而不必对每个用到父类类名的地方都做修改。

```javascript
var NonNullSetj = (function() {
	var superclass = Set;
	return superclass.extend(
		function() { superclass.apply(this, arguments); },
		{
			add: function() {
				for (var i = 0; i < arguments.length; i ++)
					if (arguments[i] == null)
						throw new Error("Can't add null or undefined");
				return subclass.prototype.add.apple(this, arguments);
			}
		});
}());
```

### 9.7.3 组合VS子类
可以利用组合的原理定义一个新的集合实现，它包装了另外一个集合对象，在将受限制的成员过滤掉之后会用到这个集合对象。

```javascript

```

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
