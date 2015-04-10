# 函数
Javascript中的函数，是只定义一次，但可以被执行或调用多次的代码。Javascript函数是参数化的：函数的定义包括一个称为形参(parameter)的标识符列表，这些参数在函数体中像局部变量一样工作。函数调用会为形参提供实参的值。函数使用实参的值为计算返回值，成为该函数调用表达式的值。每次调用还会拥有另一个值，this关键字，表示本次调用的上下文。

如果函数挂载在一个对象上，作为对象的一个属性，就稀烂它为对象的方法。当通过这个对象来调用函数时，该对象就j此次调用的上下文，也就是该函数的this的值。用于初始化一个新创建的对象的函数称为构造函数(constructor)。

在Javascript里，函数即对象，程序可以随意操控它们，比如，Javascript可以把函数赋值给变量，或者作为参数传递给其他函数。因为函数就是对象，所以可以给它们设置属性，甚至调用它们的方法。

Javascript的函数可以嵌套在其他函数中定义，这样它们就可以访问它们被定义时所处的作用域中的任何变量。这意味着Javascript函数构成了一个闭包(closure)，它给Javascript带来了非常强劲的编程能力。

## 8.1 函数的定义
函数使用function关键字来定义，它可以用在函数定义表达式或者函数声明语句里。在两种形式中，函数定义都从function关键字开始，其后跟随这些组成部分：
* 函数名称标识符。函数名称是函数声明语句的必需的部分。它的用途就像变量的名字，新定义的函数对象会赋值给这个变量。对函数定义表达式来说，这个名字是可选的：如果存在，该名字只存在于函数体中，并指代该函数对象本身。
* 一对圆括号，其中包含由0个或者多个用逗号隔开的标识符组成的列表。这些标识符是函数的参数名称，它们就像函数体中的局部变量一样。
* 一对花括号，其中包含0条或者多条Javascript语句。这些语句构成了函数体：一旦调用函数，就会执行这些语句。

函数定义实例如下：

```javascript
// 输出o的每个属性的名称和值，返回undefined
function printprops(o) {
	for(var p in o) {
		console.log(p + ': ' + o[p] + '\n');
	}
}

// 计算两个笛卡尔坐标(x1, y1)和(x2, y2)之间的距离
function distance(x1, y1, x2, y2) {
	var dx = x2 - x1;
	var dy = y2 - y1;
	return Math.sqrt(dx * dx + dy * dy);
}

// 计算阶乘的递归函数
function factorial(x) {
	if (x <= 1) return 1;
	return x * factorial(x - 1);
}

// 求平方
var square = function(x) { return x * x; };

// 函数表达式可以包含名称，这在递归时很有用
var f = function fact(x) { if (x <= 1) return 1; else return x * fact(x - 1); };

// 函数作为参数
data.sort(function(a, b) { return a - b; });

// 函数表达式在定义后立即使用
var tensquared = (function(x) { return x * x; }(10));
```

以表达式方式定义的函数，函数的名称是可选的。一条函数声明语句实际上声明了一个变量，并把一个函数对象赋值给它。相对而言，定义函数表达式时并没有声明一个变量。

### 嵌套函数
在Javascript里，函数可以嵌套在其他函数里，如：

```javascript
function hypotenuse(a, b) {
	function square(x) { return x * x; }
	return Math.sqrt(square(a) + square(b));
}
```

嵌套函数的可以访问嵌套它们的函数的参数和变量。例如，在上面的代码里，内部函数square()可以读写外部函数hypotenuse()定义的参数a和b。

函数声明只能出现在全局代码里，或者内嵌在其他函数中。而函数定义表达式不受此限制。

## 8.2 函数调用
构成函数主体的Javascript代码在定义时并不会执行，只有调用该函数时，它们才会执行。调用Javascript函数的方法有：
* 作为函数
* 作为方法
* 作为构造函数
* 通过它们的call()和apply()方法间接调用

### 8.2.1 函数调用
使用调用表达式可以进行普通的函数调用也可进行方法调用。一个调用表达式由多个函数表达式组成，每个函数表达式都是由一个函数对象和左圆括号、参数列表和右圆括号组成，参数列表是由逗号分隔的零个或多个参数表达式组成。如果函数表达式是一个属性访问表达式，那么它就是一个方法调用表达式。

在一个调用中，每个参数表达式都会计算出一个值，计算的结果作为参数传递给另外一个函数。这些值作为实参传递给声明函数时定义的形参。

对于普通的函数调用，函数的返回值成为调用表达式的值。如果该函数返回是因为解释器到达结尾，返回值就是undefined。如果函数返回是因为解释器执行到一条return语句，返回值就是return之后的表达式的值，如果return语句没有值，则返回undefined。

在ECMAScript 3和非严格的ECMAScript 5对函数的调用的规定，调用上下文是全局对象。然后，在严格模式下，调用上下文则是undefined。

以函数形式调用的函数通常不使用this关键字。不过，this可以用来判断当前是否是严格模式：

```javascript
var strict = (function() { return !this; }());
```

### 8.2.2 方法调用
方法无非是个保存在一个对象的属性里的Javascript函数。如果有一个函数f和一个对象o，则可以用下面的代码给o定义一个名为m()的方法：

```javascript
o.m = f;
```

给o定义了方法m()，调用它时就像这样：

```javascript
o.m();
```

上面的代码是一个调用表达式：它包括一个函数表达式o.m，以及空的实参列表，函数表达式本身就是一个属性访问表达式，这意味着该函数被当做一个方法，而不是作为一个普通函数来调用。

对方法调用的参数和返回值的处理，和普通函数调用完全一致。一个重要的区别就是调用上下文不同。方法调用中this引用该对象。如：

```javascript
var calculator = {
	operand1: 1,
	operand2: 1,
	add: function(){
		this.result = this.operand1 + this.operand2;
	}
};
calculator.add();		
calculator.result;		// 2
```

多数方法调用使用点符号来访问属性，使用方括号也可以进行属性访问操作，如：

```javascript
o["m"](x, y);
o["m"]();
```

方法和this关键字是面向对象编程范例的核心。任何函数只要作为方法调用实际上都会传入一个隐式的实参，这个实参是一个对象，方法调用的母体就是这个对象。如：

```javascript
rect.setSize(width, height);
setRectSize(rect, width, height);
```

假设这两行代码的功能完全一样，它们都作用于一个假定的对象rect。可以看出，第一行的方法调用语法非常清晰地表明这个函数执行的载体是rect对象，函数中的所有操作都将基于这个对象。需要注意的是，this是一个关键字，不是变量，也不是属性名。Javascript的语法不允许给this赋值。

和变量不同，关键字this没有作用域的限制，嵌套的函数不会从调用它的函数中继承this。如果嵌套函数作为方法调用，其this的值指向调用它的对象。如果嵌套函数作为函数调用，其this值不是全局对象(非严格模式下)就是undefined(严格模式下)。如果要访问这个外部函数的this值，需要将this的值保存在一个变量中，如:

```javascript
var o = {
	m: function() {
		var self = this;
		console.log(this === o);			// true;
		f();
		
		function f() {
			console.log(this === o);		// false
			console.log(self === o);		// true
		}
	}
};
```

### 8.2.3 构造函数调用
如果函数或者方法调用之前带有关键字new，它就构成构造函数调用。构造函数调用和普通的函数调用以及方法调用在实参处理、调用上下文和返回值方面都有不同。

如果构造函数调用在圆括号内包含一级实参列表，先计算这些实参表达式，然后传入函数内，这和函数调用和方法调用是一致的。但如果构造函数没有形参，Javascript构造函数调用的语法是允许省略实参列表和圆括号的。凡是滑形参的构造函数调用都可以省略圆括号，如：

```javascript
var o = new Object();
var o = new Object;
```

这两行代码是等价的，都创建一个新的空的对象，这个对象继承自构造函数的prototype属性。构造函数试图初始化这个新创建的对象，并将这个对象用做其调用上下文，因此构造函数可以使用this关键字来引用这个新创建的对象。注意，尽管构造函数看起来像一个方法调用，它依然会使用这个新对象作为调用上下文。也就是说，在表达式new o.m()中，调用上下文并不是o。

构造函数通常不使用return关键字，它们通常初始化新对象，当构造函数的函数体执行完毕时，它会显示返回。在这种情况下，构造函数调用表达式的计算结果就是这个新对象的值。然而如果构造函数显式使用return语句返回一个对象，那么调用表达式的值就是这个对象。如果构造函数使用return语句但没有指定返回值，或者返回一个原始值，那么这时将忽略返回值，同时使用这个新对象作为调用结果。

### 8.2.4 间接调用
Javascript中的函数也是对象，和其他Javascript对象没什么两样，函数对象也可以包含方法。其中的两个方法call()和apply()可以用来间接地调用函数。两个方法都允许显式指定调用所需的this值，也就是说，任何函数可以作为任何对象的方法来调用，哪怕这个函数不是那个对象的方法。两个方法都指定调用的实参。call()方法使用它自有的实参列表作为函数的实参，apply()方法则需要以数组的形式传入参数。

## 8.3 函数的实参和形参
Javascript中的函数定义并未指定函数形参的类型，函数调用也未对传入的实参值做任何类型检查。实际上，Javascript函数调用甚至不检查传入形参的个数。

### 8.3.1 可选形参
当调用函数的时候传入的实参比函数声明时指定的形参个数更少，剩下的形参都将设置为undefined值。因此在调用函数时形参是否可选是否可选以及是否可以省略应当保持较好的适应性。为了做到这一点，应当给省略的参数赋一个合理的默认值，如：

```javascript
// 将对象o中可枚举的属性名追加到数组a中，并返回这个数组a
// 如果省略a，则创建一个新数组并返回这个数组
function getPropertyNames(o, /* optional */ a) {
	if ( a === undefined) a = [];
	for (var property in o) a.push(property);
	return a;
}

var a = getPropertyNames(o);
getPropertyNames(o, a);
```

如果在函数体的第一行不使用if语句，可以使用"||"运算符：

```javascript
a = a || [];
```

需要注意的是，当用这种可选实参来实现函数时，需要将可选实参放在实参列表的最后。函数调用时没有办法省略第一个参数，而使用第二个参数。

### 8.3.2 可变长的实参列表：实参对象
当调用函数的时候传入的实参个数超过函数定义时的形参个数时，滑办法直接获得未命名值的引用。参数对象解决了这个问题。在函数体内，标识符arguments是指向实参对象的引用，实参对象是一个类数组对象，这样可以通过数字下标就能访问传入函数的实参值，而不用非要通过名字来得到实参。

假设定义了函数f，它的实参只有一个x。如果调用这个函数时传入两个实参，第一个实参可以通过参数名x来获得，也可以通过arguments[0]来得到。第二个实参只能通过arguments[1]来得到。此外，和真正的数组一样，arguments也包含一个length属性，用心标识其所包含元素的个数。因此，如果调用函数f()传入两个参数，arguments.length的值就是2。如：

```javascript
function f(x, y, z) {
	if ( arguments.length != 3) {
		throw new Error("function f called with " + arguments.length +
		" arguments, but is expects 3 arguments.");
	}
	// 函数其他逻辑
}
```

实参对象有一个重要的，就是让函数可以操作任意数量的实参。下面的函数就可以接收任意数量的实参，并返回传入实参的最大值：

```javascript
function max(/* ... */) {
	var max = Number.NEGATIVE_INFINITY;
	for (var i = 0; i < arguments.length; i++) {
		if (arguments[i] > max) max = arguments[i];
	}
	return max;
}

var largest = max(1, 10, 100, 2, 3, 1000, 4, 5, 10000, 6);	// 10000
```

类似这种函数可以接收任意个数的实参，这种函数也称为不定实参函数(varargs function)。注意，不定实参函数的实参个数不能为零。

除了数组元素，实参对象还定义了callee和caller属性，在ECMAScript 5严格模式中，对这两个属性的读写操作都会产生一个类型错误。在非严格模式下，callee属性指代当前正在执行的函数。caller指代当前正在执行的函数的函数。如：

```javascript
var factorial = function(x) {
	if (x <= 1) return 1;
	return x * arguments.callee(x - 1);
};
```

### 8.3.3 将对象属性用做实参
当一个函数包含超过三个形参时，要记住调用函数中实参的正确顺序变得有些困难，此时，最好通过名/值对的形式来传入参数，这样参数的顺序就无关紧要了。为了实现这种风格的方法调用，定义函数的时候，传入的实参都写入一个单独的对象之中，有调用的时候传入一个对象，对象中的名/值对是真正需要的实参数据。如：

```javascript
// 将原始数组的length元素复制至目标数组
function arraycopy(/* array */ from, /* index */ from_start,
					/* array */ to, /* index */ to_start,
					/* integer */ length)
	{
		// 逻辑代码 
	}

// 顺序无关的
function easycopy(args) {
	arraycopy(args.from,
			  args.from_start || 0,
			  args.to,
			  args.to_start || 0,
			  args.length);
}

var a = [1, 2, 3, 4], b = [];
easycopy({from: a, to: b, length: 4});
```

### 8.3.4 实参类型
Javascript方法的形参并未声明类型，在开通传入函数体之前也未做任何类型检查。可以采用语义化的单词给函数实参命名，或者使用注释。Javascript在必要的时候会进行类型转换。但有时这种转换会出错，需要对类型进行必要的检查。如：

```javascript
function sum(a) {
	if (isArrayLike(a)) {
		var total = 0;
		for (var i = 0; i < a.length; i++) {
			var element = a[i];
			if (element == null) continue;
			if (isFinite(element)) total += element;
			else throw new Error("sum(): elements must be finite numbers");
		}
		return total;
	}
	else throw new Error("sum(): arguments must be array-like");
}
```

Javascript是一种灵活的弱类型语言，有时适合编写实参类型和实参个数的不确定性的函数。如：

```javascript
function flexisum(a) {
	var total = 0;
	for (var i = 0; i < arguments.length; i++) {
		var element = arguments[i], n;
		if (element == null) continue;
		if (isArray(element))
			n = flexisum.apply(this, element);
		else if (typeof element === "function")
			n = Number(element());
		else 
			n = Number(element);
		if (isNaN(n))
			throw Error("flexisum(): can't convert " + element + " to number.");
		total += n;
	}
	return total;
}
```

## 8.4 作为值的函数
函数可以定义，也可以调用，这是函数最重要的特性。函数定义和调用是Javascript的记法特性。然而Javascript中，函数不仅是一种语法，也是值，也就是说，可以将它赋值给变量，存储在对象的属性或数组的元素中，作为参数传入另外一个函数等。

```javascript
function square(x) { return x * x; }
```

这个定义创建一个新的函数对象，并将其赋值给变量square。函数的名字实际上是看不见的。它(square)仅仅是变量的名字，这个变量指代函数对象。函数还可以赋值给其他变量，并且仍可以正常工作。同样可以将函数赋值给对象的属性。当函数作为对象的属性调用时，函数就称为方法。函数有时甚至不需要带名字，如赋值给数组元素：

```javascript
var a = [function(x) { reutrn x * x; }, 20};
a[0](a[1]); 				// 400
```

更多实例如：

```javascript
function add(x, y) { return x + y; }
function subtract(x, y) { return x - y; }
function multiply(x, y) { return x * y; }
function divide(x, y) { return x / y; }

function operate(operator, operand1, operand2) {
	return operator(operand1, operand2);
}

var i = operate(add, operate(add, 2, 3), operate(multiply, 4, 5));	// 25

var operators = {
	add: function(x, y) { return x + y; },
	subtract: function(x, y) { return x - y; },
	multiply: function(x, y) { return x * y; },
	divide: function(x, y) { return x / y; },
	pow: Math.pow
};

function operate2(operation, operand1, operand2) {
	if (typeof operators[operation] === "function")
		return operators[operation](operand1, operand2);
	else throw "unknown operator";
}

var j = operate2("add", "hello", oprate2("add", " ", "world"));
var k = operate2("pow", 10, 2);
```

### 自定义函数属性
Javascript中的函数并不是原始值，而是一种特殊的对象，函数可以有属性。当函数需要一个静态变量来在调用时保持某个值不变，最方便的方式就是给函数定义属性，而不是定义全局变量，显示定义全局变量会让命名空间变得更加杂乱无章。如需要一个返回唯一整数的函数，需要跟踪每次的返回值，可以将其存放到全局变量中，但这并不是必需的，最好将这个信息保存到函数对象的一个属性中，如：

```javascript
uniqueInteger.counter = 0;

function uniqueInteger() {
	return uniqueInteger.counter++;
}
```

## 8.5 作为命名空间的函数
在函数中声明的变量在整个函数体内都是可见的，在函数的外部是不可见的，不在任何函数内声明的变量是全局变量，在整个Javascript程序中都是可见的。基于这样的原因，常常简单地定义一个函数用作临时的命名空间，在这个命名空间内定义的变量都不会污染到全局命名空间。

将代码放入一个函数内，然后调用这个函数。这样全局变量就成成了函数内的局部变量：

```javascript
function mymodule() {
	// 模块代码
	// 局部变量
}

mymodule();
```

这段代码仅仅定义了一个单独的全局变量：mymodule函数。这样还是太麻烦，可以直接定义一个匿名的函数，并在表达式中调用它：

```javascript
(function() {
	// 模块代码
}());
```

这种定义匿名函数并立即在单个表达式中调用它的写法非常常见，已经成为一种惯用法了。如下，定义了一个extend()函数的匿名函数，匿名函数中的代码检测了是否出现了一个众所周知的IE bug，如果出现了这个bug，就返回一个带补丁的函数版本。

```javascript
/* 
 * 定义一个扩展函数，用来将第二个以及后续参数复制至第一个参数
 * 在IE多数版本中，如果o的属性拥有一个不可枚举的同名属性，则for/in循环
 * 不会枚举对象o的可枚举属性
 */
var extend = (function() {
	for (var p in { toString: null }) {
		return function extend(o) {
			for (var i = 1; i < arguments.length; i++) {
				var source = arguments[i];
				for (var prop in source) o[prop] = source[prop];
			}
			return o;
		};
	}
	
	return function patched_extend(o) {
		for (var i = 1; i < arguments.length; i++) {
			var source = arguments[i];
			for (var prop in source) o[prop] = source[prop];
			
			for (var j = 0; j < protoprops.length; j++) {
				prop = protoprops[j];
				if (source.hasOwnProperty(prop)) o[prop] = source[prop];
			}
		}
		return o;
	};
	
	var protoprops = ["toString", "valueOf", "constructor", "hasOwnProperty",
					"isPrototypeOf", "propertIsEnumerable", "toLocaleString"];
}());
```

## 8.6 闭包
Javascript同样采用词法作用域(lexical scoping)，也就是说，函数的执行依赖于变量作用域，这个作用域是函数定义是决定的，而不是函数调用时决定的。Javascript函数对象的内部状态不仅包含函数的代码逻辑，不必须引用当前的作用域链。函数对象可以通过作用域链相互关联起来，函数体内部的变量都可以保存在函数作用域内，这种特性在计算机科学文献中称为闭包。

从技术的角度讲，所有的Javascript函数都是闭包，它们都是对象，它们都关联到作用域链。定义大多数函数时的作用域链在调用函数时依然有效，但这并不影响闭包。当调用函数时闭包所指向的作用域链和定义函数时的作用域链不是同一个作用域链时，事情就变得非常微妙。当一个函数嵌套了另外一个函数，外部函数将嵌套的函数对象作用返回值返回的时候往往会发生这种事情。

```javascript
var scope = "global scope";
function checkscope() {
	var scope = "local scope";
	function f() { return scope; };
	return f();
}
checkscope();							// "local scope"
```

checkscope()函数声明了一个局部变量，并定义了一个函数f()，函数f()返回了这个变量的值，最后将函数f()的执行结果返回。可以很清楚的知道返回值为"local scope"。改动代码如下：

```javascript
var scope = "global scope";
function checkscope() {
	var scope = "local scope";
	function f() { return scope; };
	return f;
}
checkscope()();							// "local scope" ?
```

checkscope()现在仅仅返回函数内嵌套的一个函数对象，而不是直接返回结果。在定义函数的作用域外面，调用这个嵌套的函数，会发生什么呢？

Javascript函数的执行用到了作用域链，这个作用域链是函数定义的时候创建的。嵌套的函数f()定义在这个作用域链里，其中的变量scope一定是局部变量，不管在何时何地执行函数f()，这种绑定在执行f()时依然有效。

前面的uniqueInteger()函数使用自身的一个属性来保存第次返回的值，以便每次调用都能跟踪上次的返回值。但其值可以被外界重置为其他值，导致不一定能产生唯一的整数。使用闭包则可以实现。如：

```javascript
var uniqueInteger = (function() {
						var counter = 0;
						return function() { return counter++; };
					}());
```

这段代码定义了一个立即调用的函数，这个函数返回另一个函数，这是一个嵌套函数，它可以访问作用域内的变量，也可以访问外部函数中定义的counter变量。当外部的函数返回时，其他任何代码都无法访问counter变量。

像counter一样的私有变量不是只能用在一个单独的闭包内，在同一个外部函数内定义的多个嵌套函数也可以访问它，这多个嵌套函数都共享一个作用域链，如：

```javascript
function counter() {
	var n = 0;
	return {
		count: function() { return n ++; },
		reset: function() { n = 0; }
	};
}

var c = counter(), d = counter();
c.count();							// 0
d.count();							// 0
c.reset();
c.count();							// 0
d.count();							// 1
```

counter()函数返回一个对象，这个对象包含两个方法：count()返回下一个整数，reset()将计算器重置为内部状态。这两个方法都可以访问私有变量n。再者，每次调用counter()都会创建一个新的作用域链和一个新的私有变量。因此，如果调用counter()两次，则会得到两个计数器对象，而且彼此包含不同的私有变量，调用其中一个计数器对象的count()和reset()不会影响另一个对象。

从技术角度看，其实可以将这个闭包合并为属性存取器方法getter和setter。下面这段代码所示的counter()函数的版本是上面的变种，私有状态的实现使用了闭包：

```javascript
function counter(n) {
	return {
		get count() { return n++; },
		set count(m) {
			if (m >= n) n = m;
			else throw Error("count can only be set to a larger value");
		}
	};
}

var c = counter(1000);
c.count;				// 1000
c.count;				// 1001
c.count = 2000;			
c.count;				// 2000
c.count = 2000;			// Error!
```

注意，这个版本的counter()函数并未声明局部变量，而只是使用参数n来保存私有状态，属性存取器方法可以访问n。这样的话，调用counter()的函数就可以指定私有变量的初始值了。

```javascript
/* 
 * 这个函数给对象o增加了属性存取器方法
 * 方法名称为get<name>和set<name>，如果提供一个判定函数
 * setter方法就会用它来检测参数的合法性，然后在存储它
 * 如果判定函数返回false，setter方法抛出一个异常
 *
 * 这个函数还有一个非同寻常之处，就是getter和setter函数
 * 所操作的属性值并没有存储在对象o中
 * 相反，这个值仅仅是保存在函数中的局部变量中
 * getter和setter方法同样是局部函数，因此可以访问这个局部变量
 * 也就是说，对于两个存取器方法来说这人变量是私有的
 * 没有办法绕过存取器方法来设置或修改这个值
 */
function addPrivateProperty(o, name, predicate) {
	var value;						// 属性值
	
	o["get" + name] = function() { return value; };
	o["set" + name] = function(v) {
		if (predicate && !predicate(v))
			throw new Error("set" + name + ": invalid value " + v);
		else
			value = v;
	};
}

// test
var o = {};
addPrivateProperty(o, "Name", function(x) { return typeof x == "string"; });

o.setName("Frank");
console.log(o.getName());		// "Frank"
o.setName(0);					// Error
```

在同一个作用域链中定义两个闭包，这两个闭包共享同样的私有变量或变量。但要特别小心那些不希望共享的变量往往不经意间共享给了其他的闭包，如：

```javascript
function constfunc(v) { return function() { return v; }; }
var funcs = [];
for (var i = 0; i < 10; i ++) funcs[i] = constfunc(i);
funcs[5]();					// 5
```

这段代码利用循环创建了很多个闭包，当写类似这种代码的时候往往会犯一个错误：那就j试图将循环代码移入定义这个闭包的函数之内，如：

```javascript
function constfuncs() {
	var funcs = [];
	for (var i = 0; i < 10; i ++) 
		funcs[i] = function() { return i; };
	return funcs;
}

var funcs = constfuncs();
funcs[5]();					// 10
```

上面的代码创建了10个闭包，并将它们存储到一个数组中。这些闭包都是在同一个函数调用中定义的，因此它们共享变量i。当constfuncs()返回时，变量i的值是10，因此，数组中的函数的返回值都是同一个值。关联到闭包的作用域链都是活动的，记住这一点非常重要。嵌大戏的函数不会将作用域内的私有成员复制一份，也不会对所绑定的变量生成表态快照。

## 8.7 函数属性、方法和构造函数
Javascript程序中，函数是值。对函数执行typeof运算会返回字符串"function"，但是函数是Javascript中特殊的对象。因为函数也是对象，它们也可以拥有属性和方法，就像普通的对象可以拥有属性和方法一样。

### 8.7.1 length属性
在函数体里，arguments.length表示传入函数的实参的个数。而函数本身的length属性则有着不同含义。表示函数定义时给出的个数，通常也是在函数调用时期望传入函数的实参的个数。

下面的函数check()，从另外一个函数给它传入arguments数组，它比较arguments.length(实际传入的实参个数)和arguments.callee.length(期望传入的实参个数)来判断所传入的实参个数是否正确。如：

```javascript
function check(args) {
	var actual = args.length;
	var expected = args.callee.length;
	if (actual !== expected)
		throw Error("Expected " + expected + " args; got " + actual);
}

function f(x, y, z) {	
	check(arguments);
	return x + y + z;
}
```

### 8.7.2 prototype属性
每一个函数都包含一个prototype属性，这个属性是指向一个对象的引用，这个对象称做原型对象。每一个函数都包含不同的原型对象。当将函数用做构造函数的时候，新创建的对象会从原型对象上继承属性。

### 8.7.3 call()方法和apply()方法
可以将call()和apply()看做是某个对象的方法，通过调用方法的形式来间接调用函数。call()和apply()的第一个实参是要调用函数的母对象，它是调用上下文，在函数体内通过this来获得对它的引用。要想以对象o的方法来调用函数f()，可以这样使用，如：

```javascript
f.call(o);
f.apply(o);
```

每行代码和下面的代码的功能类似：

```javascript
o.m = f;
o.m();
delete o.m;
```

有ECMAScript 5的严格模式中，call()和apply()的第一个实参都会变为this的值，哪怕传入的实参是原始值甚至是null或undefined。其他模式下，传入null和undefined都会被全局对象代替，其他原始值会被包装对象替代。

对于call()来说，第一个调用上下文实参之后的所有实参就是要传入待调用函数的值。apply()方法和call()类似，但传入实参都放入一个数组中，如：

```javascript
f.call(o, 1, 2);
f.apply(o, [1, 2]);
```

给apply()传入的参数数组可以是任意长度的，传入apply()的参数数组可以是类数组对象也可以是真实数组。

### 8.7.4 bind()方法
bind()是在ECMAScript 5中新增的方法，但在ECMAScript 3中可以轻易模拟bind()。主要作用是将函数绑定至某个对象。当在函数f()上调用bind()方法并传入一个对象o作为参数，这个方法将返回一个新的函数。调用新的函数将会把原始的函数f()当做o的方法来调用。传入新函数的任何实参都将传入原始函数，如：

```javascript
function f(y) { return this.x + y; }
var o = {x: 1 };
var g = f.bind(o);
g(2);				// 3
```

可以通过如下代码实现这种绑定：

```javascript
function bind(f, o) {
	if (f.bind) return f.bind(o);
	else return function() {
		return f.apply(o, arguments);
	};
}
```

bind()方法不仅仅是将函数绑定至一个对象，它还附带一些其他应用：除第一个实参外，其他实参也会绑定到this。这是一种常见的函数式编程技术，称为柯里化(currying)。如：

```javascript
var sum = function(x, y) { return x + y; };
var succ = sum.bind(null, 1);
succ(2);			// 3 x:1, y:2

function f(y, z) { return this.x + y + z; };
var g = f.bind({x: 1}, 2);
g(3);				// 6 this.x:1, y:2, z:3
```

```javascript
if (!Function.prototype.bind) {
	Function.prototype.bind = function(o){
		var self = this, boundArgs = arguments;
		return function() {
			var args = [], i;
			for (i = 1; i < boundArgs.length; i ++) args.push(boundArgs[i]);
			for (i = o; i < arguments.length; i ++) args.push(arguments[i]);
			return self.apply(o, args);
		};
	};
}
```

### 8.7.5 toString()方法
函数也有toString()方法，其返回一个字符串，函数的toString()方法都返回函数的完整源码。内置函数往往返回一个类似"[native code]"的字符串作为函数体。

### 8.7.6 Function()构造函数
不管是通过函数定义语句还是函数直接量表达式，函数的定义都要使用function关键字。但函数还可以通过Function()构造函数来定义，如：

```javascript
var f = new Function("x", "y", "return x * y;");
```

其等价于：

```javascript
var f = function(x, y) { return x * y; }
```

Function()构造函数可以传入任意数量的字符串实参，最后一个实参表示的文本就是函数体，它可以包含任意的Javascript语句，每两条语句之间用分号分隔。传入构造函数的其他所有的实参字符串是指定函数的形参名字的字符串。如果定义的函数不包含任何参数，只需给构造函数简单传入一个字符串即可。Function()构造函数创建一个匿名函数。

需要注意以下几点：
* Function()构造函数允许Javascript在运行时动态地创建并编译函数。
* 每次调用Function()构造函数都会解析函数体，并创建新的函数对象。如果是在一个循环或者多次调用的函数中执行这个构造函数，执行效率会受影响。相比之下，循环中嵌套函数和函数定义表达式则不会每次执行时都重新编译。
* 最后一点，也是关于Function()构造函数非常重要的一点，就是它所创建的函数并不是使用词法作用域，相反，函数体代码的编译总是会在顶层函数执行，如：

```javascript
var scope = "global";
function constructFunction() {
	var scope = "local";
	return new Function("return scope;");
}

constructFunction()();		// "global"
```

可以将Function()构造函数认为是在全局作用域中执行的eval()，eval()可以在自己的私有作用域内定义新的变量和函数，Function()构造函数在实际编程过程中很少会用到。

### 8.7.7 可调用的对象
可调用的对象(callable object)是一个对象，可以在函数调用表达式中调用这个对象。所有的函数都是可调用的，但并非所有的可调用对象都是函数。

客户端方法(如Window.alert())使用了可调用的宿主对象，而不是内置函数对象，它们本质上不是Function对象。

另一个可调用对象是RegExp对象，可以直接调用RegExp对象，这比调用它的exec()方法更快捷一些。

检测对象是否是真正的对象：

```javascript
function isFunction(x) {
	return Object.prototype.toString.call(x) === "[object Function]";
}
```

## 8.8 函数式编程
Javascript并非函数式编程语言，但在Javascript中可以像操控对象一样操控函数，也就是说可以在Javascript中应用函数式编程技术。

### 8.8.1 使用函数处理数组
假设有一个数组，数组元素都是数字，想要计算这些元素的平均值和标准差。若使用非函数式编程风格，代码如下：

```javascript
var data = [1, 1, 3, 5, 5];
var total = 0;

for (var i = 0; i < data.length; i ++) total += data[i];
var mean = total / data.length;

total = 0;
for (var i = 0; i < data.length; i ++) {
	var deviation = data[i] - mean;
	total += deviation * deviation;
}

var stddev = Math.sqrt(total / (data.length - 1));
```

可以使用数组方法map()和reduce()来实现同样的计算，这各实现极其简洁：

```javascript
var sum = function(x, y) { return x + y; };
var square = function(x) { return x * x; };

var data = [1, 1, 3, 5, 5];
var mean = data.reduce(sum) / data.length;
var deviations = data.map(function(x) { return x - mean; });
var stddev = Math.sqrt(deviations.map(square).reduce(sum) / (data.length - 1));
```

如果在ECMAScript 3中可以自定义map()和reduce()函数：

```javascript
var map = Array.prototype.map
	? function(a, f) { return a.map(f); }
	: function(a, f) {
		var results = [];
		for (var i = 0, len = a.length; i < len; i ++) {
			if (i in a) results[i] = f.call(null, a[i], i, a);
		}
		return results;
	};

var reduce = Array.prototype.reduce
	? function(a, f, initial) {
		if (arguments.length > 2)
			return a.reduce(f, initial);
		else
			return a.reduce(f);
	}
	: function(a, f, initial) {
		var i = 0, len = a.length, accumulator;
		if (arguments.length > 2) accumulator = initial;
		else {
			if (len == 0) throw TypeError();
			while (i < len) {
				if (i in a) {
					accumulator = a[i++];
					break;
				}
				else i ++;
			}
			if (i == len) throw TypeError();
		}
		while (i < len) {
			if (i in a)
				accumulator = f.call(undefined, accumulator, a[i], i, a);
			i++;
		}
		return accumulator;
	};
```

使用如下方法调用这两个函数：

```javascript
var data = [1, 1, 3, 5, 5];
var sum = function(x, y) { return x + y; };
var square = function(x) { return x * x; };
var mean = reduce(data, sum) / data.length;
var deviations = map(data, function(x) { return x - mean; });
var stddev = Math.sqrt(reduce(map(deviations, square), sum) / (data.length - 1));
```

### 8.8.2 高阶函数
所谓高阶函数就是操作函数的函数，它接收一个或多个函数作为参数，并返回一个新函数，如：

```javascript
function not(f) {
	return function() {
		var result = f.apply(this, arguments);
		return !result;
	};
}

var even = function(x) {
	return x % 2 === 0;
};

var odd = not(even);
[1, 1, 3, 5, 5].every(odd);		// true
```

上面的not()函数就是一个高阶函数，因为它接收一个函数作为参数，并返回一个新函数。下面的例子：

```javascript
function mapper(f) {
	return function(a) { return map(a, f); };
}

var increment = function(x) { return x + 1; };
var incrementer = mapper(increment);
incrementer([1, 2, 3]);				// [2, 3, 4]
```

下面是一个更常见的例子，它接收两个函数f()和g()，并返回一个新的函数用以计算f(g())：

```javascript
function compose(f, g) {
	return function() {
		return f.call(this, g.apply(this, arguments));
	};
}

var square = function(x) { return x * x; };
var sum = function(x, y) { return x + y; };
var squareofsum = compose(square, sum);
squareofsum(2, 3);							// 25
```

### 8.8.3 不完全函数
函数f()的bind()方法返回一个新函数，给新函数传入特定的上下文和一组指定的参数，然后调用函数f()。bind()方法只是将实参放在左侧，传入bind()的实参都是放在传入原始函数的实参列表开始的位置，但有时期望将传入bind()的实参放在右侧：

```javascript
function array(a, n) { return Array.prototype.slice.call(a, n || 0); }
function partialLeft(f /* , ... */) {
	var args = arguments;
	return function() {
		var a = array(args, 1);
		a = a.concat(array(arguments));
		return f.apply(this, a);
	};
}

function partialRight(f /*, ... */) {
	var args = arguments;
	return function() {
		var a = array(arguments);
		a = a.concat(array(args, 1));
		return f.apply(this, a);
	};
}

function partial(f /*, ... */) {
	var args = arguments;
	return function() {
		var a = array(args, 1);
		var i = 0, j = 0;
		for (; i < a.length; i++)
			if (a[i] === undefined) a[i] = arguments[j++];
		a = a.concat(array(arguments, j));
		return f.apply(this, a);
	};
}

var f = function(x, y, z) { return x * (y - z); };
partialLeft(f, 2)(3, 4);			// -2 = 2 * (3 - 4)绑定第一个参数
partialRight(f, 2)(3, 4);			// 6 = 3 * (4 - 2)绑定最后一个参数
partial(f, undefined, 2)(3, 4);		// -6 = 3 * (2 - 4)绑定中间的实参
```

利用这种不完全函数的编程技巧，可以编写一些有意思的代码，如：

```javascript
var increment = partialLeft(sum, 1);
var cuberoot = partialRight(Math.pow, 1/3);
String.prototype.first = partial(String.prototype.charAt, 0);
String.prototype.last = partial(String.prototype.substr, -1, 1);

var not = partialLeft(compose, function(x) { return !x; });
var even = function(x) { return x % 2 === 0; };
var odd = not(even);
var isNamber = not(isNaN);
```

### 8.8.4 记忆
在前面的8.4.1中定义了一个阶乘函数，这可以将上次的计算结果缓存起来，在函数编程中，称为为记忆(memorization)。下面的memorize()接收一个函数作为实参，并返回带有记忆能力的函数。

```javascript
function memorize(f) {
	var cache = [];
	return function() {
		var key = arguments.length + Array.prototype.join.call(arguments, ",");
		if (key in cache) return cache[key];
		else return cache[key] = f.apply(this, arguments);
	};
}

function gcd(a, b) {
	var t;
	if (a < b) t = b, b = a, a = t;
	while(b != 0) t = b, b = a % b, a = t;
	return a;
}

var gcdmemo = memorize(gcd);
gcdmemo(85, 187);				// 17
```

Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")
