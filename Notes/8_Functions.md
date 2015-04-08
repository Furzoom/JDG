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

## 8.5 作为命名空间的函数

## 8.6 闭包

## 8.7 函数属性、方法和构造函数

### 8.7.1 length属性

### 8.7.2 prototype属性

### 8.7.3 call()方法和apply()方法和apply

### 8.7.4 bind()方法

### 8.7.5 toString()方法

### 8.7.6 Function()构造函数

### 8.7.7 可调用的对象

## 8.8 函数式编程

### 8.8.1 使用函数处理数组

### 8.8.2 高阶函数

### 8.8.3 不完全函数

### 8.8.4 记忆






Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")
