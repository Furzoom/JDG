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

## 5.7 其他语句类型
## 5.8 小结

Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")