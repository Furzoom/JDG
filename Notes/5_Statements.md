# 语句
JavaScript中表达式是短语，语句(Statement)就是JavaScript整句或命令。JavaScript语句以分号结束。表达式计算出一个值，但语句用来执行以使某件事发生。

JavaScript程序无非就是一系列可执行语句的集合。默认情况下，JavaScript解释器依照语句的编写依次执行，还可以改变程序的默认执行顺序：
* 条件(Conditional)语句，JavaScript解释器可以根据一个表达式的值来判断是执行还是跳过这些语句，如if语句和switch语句。
* 循环(Loop)语句，可以重复执行语句，如while和for语句。
* 跳转(Jump)语句，可以让解释器跳转到程序的其他部分继续执行，如break、return和throw语句。

## 5.1 表达式语句
具有副作用的表达式是JavaScript中最简单的语句。赋值语句、递增运算符、递减运算符、delete运算符、函数调用如：

```javascript
greeting = "hello";
i *= 3;
counter++;
delete o.x;
alert(greeting);
```

调用一个没有任何副作用的函数是没有意义的，除非它是复杂表达式或者赋值语句的一部分。

## 5.2 复合语句和空语句
可以用逗号运算符将几个表达式连接在一起，形成一个表达式，同样，JavaScript中还可以将多条语句联合在一起，形成一条复合语句(Compound statement)。只须用花括号将多条语句括起来即可，称为语句块。语句块可以当作一条语句对待。

```javascript
{
	x = Math.PI;
	cx = Math.cos(x);
	console.log("cos(PI) = " + cx);
}
```

空语句(Empty statement)是允许包含0条语句的语句。如：

```javascript
;
```

JavaScript解释器执行空语句时它显然不会执行任何动作。但是当一个具有空循环体的循环时，空语句是必要的，如：

```javascript
for (i = 0; i < a.length; a[i++] = 0) ;
```

在程序时要使用空语句，最好给出明确的注释。

## 5.3 声明语句
var和function都是声明语句，它们声明或定义变量或函数。这些语句定义标识符并给其赋值，这些标识符可以在程序中做生意地方使用。声明语句本身什么也不做，但它通过创建变量和函数，可以更好地组织代码的语义。

### 5.3.1 var
var语句用来声明一个或者多个变量，关键字var之后跟随的是要声明的变量列表，列表中的每一个变量都可以带有初始化表达式，用于指定它的初始值。它的语法如下：

```javascript
var name_1 [ = value_1] [, ... , name_n [ = value_n]]
```

如果var语句出现在函数体内，那么它定义的是一个局部变量，其作用域就是这个函数。如果在顶层代码中使用var语句，它声明的是全局变量，在整个JavaScript程序中都是可见的。使用var声明的变量是无法通过delete删除的。

如果var语句中的变量没有指定初始化表达式，那么这个变量的值初始化为undefined。多次声明同一个变量是无所谓的。

### 5.3.2 function
关键字function用来定义函数。funcname是要声明的函数的名称的标识符。函数名之后的圆括号中是参数列表，参数之间使用逗号分隔。当调用函数时，这些标识符则指代传入函数的实参。函数声明语法如下：

```javascript
function funcname([arg1 [, arg2 [... , argn]]]) {
	statements
}
```

JavaScript中常用的两种定义函数的方法：

```javascript
var f = function(x) { return x+1; }
function f(x) { return x+1; }
```

上述两个种方法还有细微的差别。第一种变量f的声明会被提前到脚本或者函数的最前面，但定义依然为该代码的位置。第二种方式会将函数的定义整体提前到脚本或者函数的最前面。如：

```javascript
foo(2);					// 2
function foo(x) { 
	console.log(x); 
}
```

```javascript
foo2(2); 				// Error
var foo2 = function(x) { 
	console.log(x); 
}
```

函数声明语句通常出现在JavaScript代码的最顶层，也可以出现在函数体内，在函数体内只能出现在所嵌套函数的顶部。

## 5.4 条件语句
条件语句是通过判断指定表达式的值来决定鹎学是跳过某些语句，称为分支。

### 5.4.1 if
if语句有两种形式。第一种如下：

```javascript
if (expression)
	statements
```

在这种形式下，需要计算expression的值，如果计算结果是真值，那么就执行statements。如果为假值，就不执行statements。如：

```javascript
if (username == null)
	username = 'Furzoom';
```

JavaScript语法规定，if关键字和带圆括号的表达式之后必须跟随一条语句。可以使用语句块或者空语句。

第二种形式就是引用else从句，当expression的值是false时，执行else中的逻辑。语法如下：

```javascript
if (expression)
	statements 1
else
	statements 2
```

当然，if语句是可以嵌套使用的。

### 5.4.2 else if
else if语句并不是真正的JavaScript语句，它只不过是多条if/else语句连在一起时的一种惯用的写法。如：

```javascript
if (n == 1) {
	// statement 1
}
else if (n == 2) {
	// statement 2
}
else {
	// statement 3
}
```

### 5.4.3 switch
if语句在程序执行过程中创建一条分支，并且可以使用else if来处理多条分支。然后，当所有的分支都依赖于同一个表达式的值时，else if并不是最佳解决方案。重复多次计算if语句中的条件表达式是非常浪费的做法。switch语句示例如下：

```javascript
switch(n) {
	case 1:		// if n === 1
		// statement 1
		break;
	case 2:		// if n === 2
		// statement 2
		break;
	case 3:		// if n === 3
		// statement 3
		break;
	default:	// if n === 4
		// default statement
		break;
}
```

在swtich语句中，case只是指明了要执行的代码起点，但并没有指明终点，如果没有break语句，将一直执行到switch代码块结尾。因此，常配合使用break语句。case后可以跟任意表达式。switch语句首先计算switch关键字后的表达式，然后按照从上到下的顺序计算case后面的表达式，直到与switch后面的表达值相等为止，需要使用`===`进行判断。因此，匹配过程中不会有类型转换。如果没有case语句与之相等，则执行`default:`标签后的语句。如果没有`default:`语句，则跳出switch代码块。

## 5.5 循环
循环语句(Looping statement)就是程序路径的一个回路，可以让一部分代码重复执行。JavaScript中有4种循环语句：while、do/while、for、for/in。

### 5.5.1 while
while语句的语法如下：

```javascript
while (expression)
	statement
```

在执行while语句之前，Javascript解释器首先计算expression的值。如果它为假值，程序跳出循环体，转而执行程序的下一条语句。否则，执行statement语句，直到expression的值为假。

通常来说，不想让Javascript反复执行同一操作。在每次循环中都会有一个或多个变量随着循环的迭代而改变。如：

```javascript
var count = 0;
while (count < 10) {
	console.log(count);
	count++;
}
```

### 5.5.2 do/while
do/while循环和while循环非常相似，只不过它是在循环的尾部而不是顶部检测循环表达式，这意味着循环至少会执行一次。do/while循环的语法如下：

```javascript
do
	statement
while (expression);
```

do/while循环要求必须使用关键字do来标识循环的开始，用while来标识循环的结尾并进行循环条件判断。其次，do/while循环是用分号结尾的。
### 5.5.3 for
for循环语句的语法如下：

```javascript
for (initialize; test; increment)
	statement
```

initialize、test、increment三个表达式用分号间隔，分别负责初始化操作、循环条件判断、计算器变量的更新。上述结构与下面的while循环是等价的：

```javascript
initialize;
while (test) {
	statement
	increment;
}
```

### 5.5.4 for/in
for/in语句也使用for关键字，但它和常规的for循环完全不同。其语法如下：

```javascript
for (variable in object)
	statement
}
```

variable通常是一个变量名，也可以是一个可以产生左值的表达式或者一个通过var语句声明的变量，总之必须是一个适用于赋值表达式左侧的值。object是一个表达式，这个表达式的计算结果是一个对象。同样，statement是一个语句或语句块，它构成循环的主体。

如，使用for/in遍历数组元素：

```javascript
for (var p in o)
	console.log(o.[p]);
```

在执行for/in语句的过程中，Javascript解释器首先计算object表达式。如果表达式是null或者undefined，Javascript解释器会抛出一个异常。如果表达式等于一个原始值，这个原始值将会转换为与之对应的包装对象。否则，expression本身已经是对象了。Javascript会依次枚举对象的属性来执行循环。然而在每次循环之前，Javascript都会先计算variable表达式的值，并将属性名赋值给它。

事实上，for/in循环并不会遍历对象的所有属性。只有“可枚举”(enumerable)的属性才会遍历到。由Javascript语言核心所定义的内置方法就不是“可枚举”的。除了内置方法外，还有很多内置对象的属性也是不可枚举的。而代码中定义的所有属性和方法都是可枚举的。对象可以继承其他对象的属性，那些继承的自定义属性也可以使用for/in枚举出来。
## 5.6 跳转
Javascript中的跳转语句(jump statement)可以使执行从一个位置跳转到另一个位置。

### 5.6.1 标签语句
语句是可以添加标签的，标签是由语句前的标识符和冒号组成的：

```javascript
identifier:statement
```

通过给语句定义标签，就可以在程序的任何地方通过标签名引用这条语句。只有在给语句块定义标签时它才更有用，比如循环和条件判断语句。break和continue是Javascript中唯一可以使用语句标签的语句。带有标签的语句可以有标签，标签可以嵌套。

### 5.6.2 break语句
单独使用break语句的作用是立即退出最内层循环或switch语句。语法为：

```javascript
break;
```

由于它能够使循环和switch语句退出，因此这种形式的break只有出现在这类语句中都是合法的。Javascript中同样允许在break后跟标签，如：

```javascript
break labelname;
```

当break和标签一块使用时，程序将跳转到这个标签所标识的语句块的结束，或者直接编目这个闭合语句块的执行。当没有任何闭合语句块指定了break所用的标签，这时会产生一个语法错误。如：

```javascript
var a = 5;
label: if ( a != 0 ) {
	console.log(a);
	break label;			
	console.log("break");	// not executed
}
// break jump here
```

无论是否有标签，break都无法越过函数的边界。

### 5.6.3 continue语句
continue语句和break语句非常相似，但它不是退出循环，而是转而执行下一次循环。语句如下：

```javascript
continue;
continue lobelname;
```

不管continue语句带不带标签，它只能在循环体内使用。在其他地方使用将会报语法错误。在不同的循环中，continue的行为也有所不同：
* 在while循环中，在循环开始外指定的expression会重复检测，如果检测结果为true，循环体会从头开始执行。
* 在wo/while循环中，程序的执行直接跳转到循环结尾处，这时会重新判断循环条件，之后才会继续下一次循环。
* 在for循环中，首先计算自增表达式，然后再次检测test表达式，用以判断更不执行循环体。
* 在for/in循环中，循环开始遍历下一个属性名，这个属性名赋名了指定的变量。

### 5.6.4 return语句
函数调用是一种表达式，而所有表达式都有值。函数中的return语句指定了函数表达式的值。语法如下：

```javascript
return expression;
```

return语句只能在函数体内出现，如果不是话会报语法错误。当执行到return语句的时候，函数终止执行，并返回expression的值给调用程序。如果没有return语句，则函数调用公依次执行函数体内的每一条语句直到函数结束，最后返回调用程序。这时函数表达式的结果是undefined。return可以单独使用而不带expression，这样的话函数也会向调用程序返回undefined。

### 5.6.5 throw语句
所谓异常(exception)是当发生了某种异常情况或错误时产生的一个信号。抛出异常，就是用信号通知发生了错误或异常状况。捕获异常是指处理这个信号，即采取必要的手段从异常中恢复。在Javascript中当产生运行时错误或者程序使用throw语句时就会显示地抛出异常。使用try/catch/finally语句可以捕获异常。throw语法如下：

```javascript
throw expression;
```

expression的值可以是任意类型的。可以抛出一个代表错误码的数字，或者包含可读的错误消息的字符串。Javascript解释器抛出异常时通常采用Error类型和其子类型，当然也可以使用它们。一个Error对象有一个name属性表示错误类型，一个message属性用来存放传递给构造函数的字符串。如：

```javascript
function factorial(x) {
	if (x < 0) throw new Error("x must not be negative");
	for (var f = 1; x > 1; f *= x, x--) /* empty */ ;
	return f;
}
```

当抛出异常时，Javascript解释器会立即停止当前正在执行的逻辑，并跳转到就近的异常处理程序。如果抛出异常的代码块没有一条相关联的catch语句，解释器会检查更高层次的闭合代码块，看它是否有相关联的异常处理程序。以此类推，直到找到异常处理程序为止。如果最后还没有找到，Javascript将其作为程序错误来处理，并报告给用户。

### 5.6.6 try/catch/finally语句
try/catch/finally语句是Javascript的异常处理机制。其中try从句定义了需要处理的异常所在的代码快。catch从句跟随在try从句后，当try块内警示牌发生了异常时，调用catch内的代码逻辑。catch从句后跟随finally块，后者中旋转清理代码，不管try块中是否产生异常，finally块内的逻辑部是会执行。尽管catch和finally都是可选的，但try从句需要至少二者之一与这组成完整的语句。三者语句块都需要使用花括号括起来。

```javascript
try {

}
catch (3) {

}
finally {

}
```

如：

```javascript
try {
	var n = Number(prompt("input a number", ''));
	var f = factorial(n);
	alert(n + '! = ' + f);
}
catch (e) {
	alert(e);
}
```

不管try语句块中的执行完成了多少，只要try语句中有一部分代码执行了，finally从句就会执行。

当由于return、continue、break语句使得解释器跳出try语句块时，解释器在执行新的目标代码之前，先执行finally块中的逻辑。

如果在try中产生了异常，而且存在一条与之相关的catch从句来处理这个异常，解释器首先执行catch中的逻辑，然后执行finally中的逻辑。如果不存在处理异常的局部catch从句，解释器首先执行finally中的逻辑，然后向上传播这个异常，直到找到能处理这个异常的catch从句。

如果finally块使用了return、continue、break或throw语句使程序发生跳转，或者通过调用抛出异常的方法改变了程序执行流程，不管这个跳使程序挂起还是继续执行，解释器都会将其忽略。

## 5.7 其他语句类型
Javascritp中剩余的三种语句为with、debugger和use strict。

### 5.7.1 with语句
with语句用于临时扩展作用域链，它的语法如下：

```javascript
with (object)
statement
```

这条语句将object添加到作用域链的头部，然后执行statement，最后把作用域链恢复到原始状态。在严格模式中是禁止使用with语句的，在非严格模式下也不推荐使用with语句。如：

```javascript
with (document.forms[0]) {
	name.value = '';
	address.value = '';
	email.value = '';
}
```

与下面的程序是等价的：

```javascript
var f = document.forms[0];
f.name.value = '';
f.address.value = '';
f.email.value = '';
```

只有在查找标识符的时候才会乃至作用域链，创建新的变量的时候不使用它，如：

```javascript
with (o) x = 1;
````

如果对象o有一个属性x，那么这行代码给这个属性赋值为1，如果o中没有定义属性x，这段代码和不使用with语句的代码`x=1`一样。

### 5.7.2 debugger语句
debugger语句通常什么也不做。然而当调试程序并运行的时候，Javascritp解释器将会以调试模式运行。实际上，这条语句用来产生一个断点(breakpoint)，Javascritp代码的执行会停止在断点的位置，这时可以使用调试器输出变量的值、检查调用栈。

### 5.7.3 "use strict"
"use strict"是ECMAScript 5引入的一条指令，指令不是语句，"use script"指令和普通的语句之间有两个重要的区别：
* 它不包含任何语言的关键字，指令仅仅是一个包含一个特殊字符串直接量的表达式(可以使用单引号或者双引号)，对于那些没有实现ECMAScript 5的Javascritp解释器来说，它只是一条没有副作用的表达式语句，它什么也不做。
* 它只能出现在脚本代码的开始或者函数体的开始、任何实体语句之前。

使用"use script"指令的目的是说明后续的代码将会解析为严格代码(strict code)。严格模式与非严格模式的主要区别如下：
* 在严格模式中禁止使用with语句。
* 在严格模式中，所有的变量都要先声明，如果给一个未声明的变量、函数、函数参数、catch从句参数或全局对象的属性赋值，将会抛出一个引用错误异常。
* 在严格模式中，调用的函数(不是方法)中的一个this值是undefined。可以用这种方式来判断Javascritp实现的否支持严格模式：
```javascript
var hasStrictMode = (function(){ "use strict"; return this === undefined; });
```
* 在严格模式中，通过call()和apply()来调用函数时，其中的this值就是通过call()和apply()传入的第一个参数。
* 在严格模式中，给只读属性赋值和给不可扩展的对象创建新成员都将抛出一个类型错误异常。
* 在严格模式中，传入eval()的代码不能在调用程序所在上下文中声明变量或定义函数。相反，变量和函数的定义是在eval()创建的新作用域中，这个作用域在eval()返回时就弃用了。
* 在严格模式中，函数里的arguments对象拥有传入参数值的副本。在非严格模式中，arguments对象具有"魔术般"的行为，arguments里的数组元素和函数参数都是指向同一个值的引用。
* 在严格模式中，当delete运算符后跟随非法的标识符时，将会抛出一个语法错误异常。
* 在严格模式中，试图删除一个不可配置的属性将抛出一个类型错误异常。
* 在严格模式中，在一个对象直接量中定义两个或者多个同名属性将产生一个语法错误。
* 在严格模式中，函数声明中存在两个或多个同名的参数将产生一个语法错误。
* 在严格模式中，不允许使用八进制整数直接量。
* 在严格模式中，标识符eval和arguments当做关键字。
* 在严格模式中，限制对调用栈的检测。arguments.caller和arguments.callee都会抛出一个类型错误异常。


Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")