# 表达式和运算符

表达式(Expression)是JavaScript中的一个短语，JavaScript解释器将其计算(Evaluate)出一个结果。简单的表达式可以组成复杂表达式。

## 4.1 原始表达式

最简单的表达式是原始表达式(Primary Expression)。原始表达式是表达式的最小单位。JavaScript中的原始表达式包含常量或者直接量、关键字、变量。如：

```javascript
1.23			// literal
true			// reserved word
i				// variable
undefined		// variable
```

## 4.2 对象和数组的初始化表达式

对象和数组的初始化表达式实际上是一个新创建的对象和数组。有时称为对象直接量或者数组直接量。但它们不是原始表达式，它们所含的成员或者元素都是子表达式。

数组初始化表达式是通过一对方括号和其内由逗号隔开的列表构成，其内的元素也可以是数组。如：

```javascript
[]				// empty array
[1, 2]			// 2-elements array
[[1, 2], [2, 3]]
```

JavaScript对数组初始化表达式进行求值时，其元素的初始化表达式会先进行求值。数组直接量中的列表逗号之间的元素可以省略，省略的空值会填充为undefined。元素结尾的单个逗号不会再数组最后添加undefined值。如：

```javascript
var a = [1,,2,];		// [1, undefined, 2] length is 3
var b = [1,2,,];		// [1, 2, undefined] length is 3
```

对象初始化表达式和数组初始化表达式非常类似，只是方括号被花括号代替，且每个子表达式都包含一个属性名和一个冒号作为前缀。如：

```javascript
var p = ( x: 2.3, y:-1.2};
var q = { };
q.x = 2.3; q.y = -1.2;		// q has the same properties as p
```

## 4.3 函数定义表达式

函数定义表达式定义一个JavaScript函数。表达式的值是这个新定义的函数。也称为函数直接量。函数的典型定义方式包含关键字function，跟随其后的是一对圆括号，括号内是一个以逗号分割的列表，列表含有0个或者多个标识符(参数名)，然后再跟随一个由花括号包裹的JavaScript代码段(函数体)，如：

```javascript
var square = function(x) { return x * x; }
```

## 4.4 属性访问表达式

属性访问表达式运算得到一个对象属性或者一个数组元素的值。在JavaScript中有两种方法来访问属性：

		expression.identifier
		expression[expression]

第一种写法表达式指定对象，标识符则指定需要访问的属性的名称。第二种写法方括号内的表达式指定要访问的属性的名称或者代表要访问数组元素的索引。如：

```javascript
var o = {x: 1, y: {z: 3}};
var a = [o, 4, [5, 6]];
o.x							// 1
o.y.z						// 3
o['x']						// 1
a[1]						// 4
a[2]["1"]					// 6
a[o].x						// 1
```

在`.`和`[`之前的表达式总是会首先计算。如果结果是null或者undefined，表达式将抛出一个类型错误异常，因为这两个属性不能包含任意属性。如果结果不是对象，则JavaScript会将其转换为对象。如果对象表达式之后跟随句点和标识符，则会查找由这个标识符所指定的属性的值，并将其作为整个表达式的值返回。如果对象表达式后跟随一对方括号，则会计算方括号的表达式的值并将它转换为字符串。如果命名的属性不存在，那么整个属性访问表达式的值就是undefined。

## 4.5 调用表达式

JavaScript中调用表达式(Invocation expression)是一种调用(或者执行)函数或者方法的语法表示。它以一个函数表达式开始，这个函数表达式指定了要调用的函数。函数表达式后跟随一对圆括号，括号内是一个以逗号隔开的参数列表，参数可以有0个也可以有多个。如：

```javascript
f(0);
Math.max(x, y, z);
a.sort()
```

当对调用表达式进行求值时，首先计算函数表达式，然后计算参数表达式。如果函数表达式的值不是一个可调用的对象，则抛出一个类型错误异常。然后，实参的值被依次赋值给形参，这些形参是定义函数时指定的，接下来开始执行函数体，如果函数使用return语句给出一个返回值，那么这个值就是整个调用表达式的值。否则，调用表达式的值就是undefined。

任何一个调用表达式都包含一对圆括号和左圆括号之前的表达式。如果这个表达式是一个属性访问表达式，那么这个调用称作方法调用(Method Invocation)。方法调用中使用this指向当前对象(该方法所属对象)。如果不是方法调用，this的值为undefined。

## 4.6 对象创建表达式

对象创建表达式(Object Creation Expression)创建一个对象并调用一个函数(构造函数)初始化新对象的属性。语法如下：

```javascript
new Object();
new Object;
new Object(1, 2);
```

如果一个对象创建表达式不需要传入任何参数给构造函数，则括号可以省略。当计算一个对象创建表达式的值时，和对象初始化表达式通过{}创建对象的做法一样，JavaScript首先创建一个新的空对象，然后，JavaScript通过传入指定的参数并将这个新对象当做this的值来调用指定的函数。

## 4.7 运算符概述

JavaScript中的运算符用于算术表达式、比较表达式、逻辑表达式、赋值表达式等。如下按照运算符的优先级列出JavaScript中的运算符。

运算符 | 操作 | A | N | 类型
--- | --- | --- | --- | ---
++<br />--<br />-<br />+<br />~<br />!<br />delete<br />typeof<br />void | 前/后增量<br />前/后减量<br />求反<br />转换为数字<br />按位求反<br />逻辑非<br />删除属性<br />检测操作数类型<br />返回undefined值 | R<br />R<br />R<br />R<br />R<br />R<br />R<br />R<br />R | 1<br />1<br />1<br />1<br />1<br />1<br />1<br />1<br />1 | lval->num<br />lval->num<br />num->num<br />num->num<br />int->int<br />bool->bool<br />lval->lval<br />any->str<br />any->undef
* / % | 乘 除 求余 | L | 2 | num,num->num
+ -<br />+ | 加 减<br />字符串连接 | L<br />L | 2<br />2 | num,num->num<br />str,str->str
<<<br />>><br />>>> | 左移位<br />有符号右移位<br />无符号右移位 | L<br />L<br />L | 2<br />2<br />2 | int,int-int<br />int,int->int<br />int,int->int
< <= > >=<br />< <= > >=<br />instanceof<br />in | 比较数字顺序<br />比较在字母表中的顺序<br />测试对象类<br />测试属性是否存在 | L<br />L<br />L<br />L | 2<br />2<br />2<br />2 | num,num->bool<br />str,str->bool<br />obj,func->bool<br />str,obj->bool
==<br />!=<br />===<br />!== | 判断相等<br />判断不等<br />判断恒等<br />判断非恒等 | L<br />L<br />L<br />L | 2<br />2<br />2<br />2 | any,any->bool<br />any,any->bool<br />any,any->bool<br />any,any->bool
& | 按位与 | L | 2 | int,int->int
^ | 按位异或 | L | 2 | int,int->int
\| | 按位或 | L | 2 | int,int->int
&& | 逻辑与 | L | 2 | any,any-any
\|\| | 逻辑或 | L | 2 | any,any-any
?: | 条件运算符 | R | 3 | bool,any,any->any
=<br />*= /= %= += -= &= ^= \|= <<= >>= >>>= | 赋值<br />运算并赋值 | R<br />R | 2<br />2 | lval,any->any<br />lval,any->any
, | 逗号 | L | 2 | any,any->any


Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")