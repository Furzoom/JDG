# 数组
数组是值的有序集合。每个值叫做一个元素，而每个元素在数组中有一个位置，以数字表示，称为索引。Javascript数组是无类型的：数组元素可以是任意类型，并且同一个数组中的不同元素也可能有不同的类型。数组的元素甚至也可能是对象或其他数组，这允许创建复杂的数据结构。Javascript数组是的索引是基于零的32位数值。Javascript数组是动态的，根据需要会增长或缩减。

Javascript数组是Javascript对象的特殊形式，数组索引实际上和碰巧是整数的属性名差不多。数组的实现是经过优化的，用数字索引来访问数组元素一般来说比访问常规的对象属性要快很多。

## 7.1 创建数组
使用数组直接量是创建数组最简单的方法，在方括号中将数组元素用逗号隔开即可。如：

```javascript
var empty = [];
var primes = [2, 3, 5, 7, 11];
var misc = [1.1, true, "a", ];
```

数组直接量中的值不一定要是常量，它们可以是任意的表达式：

```javascript
var base = 1024;
var table = [base, base+1, base+2, base+3];
var b = [[1, {x:1, y:2}], 2];
```

如果省略数组元素的某个值，省略的元素将被赋值为undeinfed：

```javascript
var count = [1, , 3];		// length: 3
var undefs = [, , ,];		// length: 2
```

调用构造函数Array()是创建数组的另一个方法，有以下三种形式：

```javascript
var a = new Array();		// 调用时没有参数，空数组，等同于直接量[]
var a2 = new Array(10);		// 调用时使用1个数组参数，指定数组长度
// 显示指定两个或多个数组元素或者数组的一个非数值元素
var a3 = new Array(5, 4, 3, 2, 1, "abc");	// [5, 4, 3, 2, 1, "abc"]
```

## 7.2 数组元素的读和写
使用[]操作符来访问数组中的元素。数组的引用位于方括号的左边。方括号中是一个返回负整数值的任意表达式。该语法可以读写数组的一个元素。如：
```javascript
var a = ["furzoom"];		//
var value = a[0];			// read 
a[1] =3.14;					// write
i = 2;
a[2] = 3;					// write
a[i + 3] = ".com";			// write
```

数组是对象的特殊形式。Javascript将指定的数字索引值转换成字符串，然后将其作为属性名来使用。对常规对象也是这么处理。数组的特别之处在于，当使用小于2^32的非负整数作为属性名时数组会自动维护其length属性值。使用负整数或者非整数来索引数组，它就只能当做常规对象属性。数组索引仅仅是对象属性名的一种特殊类型。数组可以从原型中继承元素。在ECMAScript 5中，数组可以定义元素的getter和setter方法。

## 7.3 稀疏数组
稀疏数组就是包含从0开始的不连续索引的数组。通常，数组的length属性值代表数组中元素的个数。如果数组是稀疏的，length属性值大于元素的个数。可以用Array()构造函数或简单地指定数组索引值大于当前数组长度来创建稀疏数组。如：

```javascript
a = new Array(5);			// 数组没有元素，但a.length是5
a = [];						// lenght = 0
a[1000] = 0;				// a.lenght是1001
```

足够稀疏的数组通常在实现上比稠密的数组更慢，内存利用率更高，在这样的数组中查找元素的时间和常规对象属性的查找时间一样长。注意，在数组直接量中省略值时不会创建稀疏数组。省略的元素在数组中是存在的，其值为undefined。这和数组元素根本不存在是有一些微妙的区别的(在新版chrome中已无区别)，可以用in操作符检测两者之间的区别：

```javascript
var a1 = [,,,];
var a2 = new Array(3);
0 in a1;					// true
0 in a2;					// false
```

## 7.4 数组长度
每个数组都有一个length属性，就是这个数组使其区别于常规的Javascript对象。针对非稀疏数组，lenght属性值代表数组元素中元素的个数。当数组是稀疏数组时，length属性值大于元素的个数。如果为一个数组元素赋值，这经索引i大于或等于现有数组的长度时，length属性的值将设置为i+1。当设置数组的lenght属性为一个小当前长度的非负整数n时，当前数组中那些索引值大于或等于n的元素将从中删除。

```javascript
var a = [1, 2, 3];
a.length;				// 3
a.length = 2;
a[2];					// undefined
```

在ECMAScript 5中可以用Object.defineProperty()让数组的length属性变成只读的，如：

```javascript
a = [1, 2, 3];
Object.defineProperty(a, "length", {writable: false});
a.length = 0;			// a.length是3
```

此时也不能给数组添加元素了。

## 7.5 数组元素的添加和删除
给数组元素添加数组的最简单的方法就是给新索引赋值，也可以使用push()方法在数组末尾增加一个或多个元素：

```javascript
a = [];
a[0] = 1;
a.push(2, 3);		// [1, 2, 3]
```

在数组尾部压入一个元素与给数组a[a.length]赋值是一样的。

## 7.6 数组遍历

## 7.7 多维数组

## 7.8 数组方法

### 7.8.1 join()

### 7.8.2 reverse()

### 7.8.3 sort()

### 7.8.4 concat()

### 7.8.5 slice()

### 7.8.6 splice()

### 7.8.7 push() and pop()

### 7.8.8 unshift() and shift()

### 7.8.9 toString() and toLocaleString()

## 7.9 ECMAScript 5中的数组方法

### 7.9.1 forEach()

### 7.9.2 map()

### 7.9.3 filter()

### 7.9.4 every() and some()

### 7.9.5 reduce() and reduceRight()

### 7.9.6 indexOf() and lastIndexOf()

## 7.10 数组类型

## 7.11 类数组对象

## 7.12 作为数组的字符串






Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")
