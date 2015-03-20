# 词法结构
## 2.1 字符集
JavaScript采用Unicode字符集。
### 2.1.1 区分大小写
所有标识符，如关键字、变量、函数名等都区分大小写。<br />
而在HTML中是不区分大小写的，如属性onclick可以写成onClick。
### 2.1.2 空格、换行符和格式控制符
JavaScript会忽略程序中标识(token)之间的空格。JavaScript多数情况也会忽略换行符。<br />
JavaScript可识别的空格字符包括：
* 空格符(\u0020)
* 水平制表符(\u0009)
* 垂直制表符(\u000B)
* 换页符(\u000C)
* 不中断空白(\u00A0)
* 字符序标记(\uFEFF)
* Zs类别字符
JavaScript将以下字符作为行结束符：
* 换行符(\u000A)
* 回车符(\u000D)
* 行分隔符(\u2028)
* 段分隔符(\u2029)
		回车符加换行符在一直被解析为一个单行结束符。
### 2.1.3 Unicode转义序列
使用6个ASCII字符来代表任意16位Unicode内码。`\u`开头，再加4个十六进制数表示。
### 2.1.4 标准化
Unicode允许多种方式对同一个字符进制编码，它们显示一样，但编码不同，JavaScript中不会自动将其转换为一致的编码。
## 2.2 注释
```javascript
// 单行注释
/* 多行的
 * 注释 
 */
```
## 2.3 直接量
直接量(literal)，程序中直接使用的数据值，如：
```javascript	
12 // 数字
"furzoom" // 字符串
```
## 2.4 标识符和保留字
### 2.4.1 标识符
JavaScript标识符以`字母`、`下划线(_)`、`美元符($)`开始，后续可以是`字母`、`下划线(_)`、`美元符($)`或者`数字`。如：
```javascript	
i
_dummy
$str
```
为了移植和易于书写，一般只使用ASCII字母和数字书写标识符。JavaScript支持非英语符号。

		保留字不能作为标识符。
		
### 2.4.2 保留字
JavaScript将一些标识符作为自己的关键字，因此不能再将其作为标识符。
		break					delete			function			return					typeof
		case					do 					if						switch					var
		catch					else				in						this						void
		continue			false				instanceof		throw						while
		debugger			finally			new						true						with
		default				for					null					try

JavaScript还保留一些关键字，可能会在以后版本中使用：
		class					const				enum					export					extends		
		import				super

此外，下面的关键字在变通的JavaScript代码中合法的，在严格的模式下是保留字：
		private				let					implements		public					yield
		interface			package			protected			static

在严格的模式下，下面的标识符不是保留字，但不能做变量名、函数名或者参数名使用：
		arguments	eval

JavaScript定义了一些全局变量和函数，就避免将其作为变量名和函数名：
		arguments			encodeURI		Infinity			Number					RegExp
		Array					String 			isFinite			Object					encodeURIComponent
		Boolean				Error				isNaN					parseFloat			SyntaxError
		Date					eval				JSON					parseInt				TypeError
		decodeURI			EvalError		Math					RangeError			undefined
		URIError			Function		NaN						ReferenceError	decodeURIComponent
## 2.5 可选的分号
JavaScript使用分号(;)将语句分隔开，如果语句独占一行，也可以省略末尾分号。最好**明确**使用分号标识语句结束。
