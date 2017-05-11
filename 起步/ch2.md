你不知道的JS：起步和入门
=====


第二章：进入JavaScript
---

&nbsp;

在前一章中，我介绍了编程的基本模块，比如变量，循环，条件和函数。当然，所有代码都是用JavaScript写的。但是在这一章，我们希望集中讲解JavaScript中你需要学习的，并且由此起步成为一个 JS 开发者。

&nbsp;

我们将在这章介绍一些概念并且将在本书随后的部分中充分讲解。你可以认为这章是本书的各个部分的详细介绍。

&nbsp;

尤其是，如果你刚接触JavaScript，你应该花时间多次复习本章的概念和代码示例，不积跬步，无以至千里，所以别期望一遍就能将这些内容理解清楚。

&nbsp;

学习JavaScript的旅程就从这儿开始吧。

&nbsp;

**注：** 就如我在第1章里提到的，你应该独自尝试本章提到的所有代码。请注意，本文中的一些代码介绍了最新版本的JavaScript中的一些特性（通常称为"ES"，ECMAScript的第六版--JS规范的官方名称）。如果您正好使用较旧的，不兼容ES6的浏览器，代码可能无法正常工作。建议最好使用最新的现代浏览器（如Chrome，Firefox或者IE）。

&nbsp;

### 值和类型

&nbsp;

如我们在第1章提到的，JavaScript有值类型，没有变量类型。JS提供了以下内置类型：

&nbsp;

+ `string`（字符串）
&nbsp;

+ `number`（数字）
&nbsp;

+ `boolean`（布尔值）
&nbsp;

+ `null` and `undefined`(空或未定义)
&nbsp;

+ `object`（对象）

+ `symbol`（ES6中的新类型）

&nbsp;

Javascript提供了`typeof`操作符用来检测值的类型：

&nbsp;

```js
var a;
typeof a;				// "undefined"

a = "hello world";
typeof a;				// "string"

a = 42;
typeof a;				// "number"

a = true;
typeof a;				// "boolean"

a = null;
typeof a;				// "object" -- weird, bug

a = undefined;
typeof a;				// "undefined"

a = { b: "c" };
typeof a;				// "object"

```

&nbsp;

`typeof`操作符的返回结果永远是用字符串表示的六个数据类型之一（如果是ES6，就是七个之一 -- "symbol"类型）。即，`typeof "abc"` 返回 `"string"`，而不是 `string`。

&nbsp;

注意这个代码段里面变量 `a` 是如何保存不同类型的值的，尽管出现了 `typeof a`并不是在验证 "变量 `a` 的类型"，而是 "变量 `a` 的值的类型。" 在JavaScript中，只有值有类型；变量仅是值的容器而已。

&nbsp;

然而 `typeof null` 是个有趣的例子，因为它会错误的返回 `"object"`，但其实你以为会返回 `"null"`。

&nbsp;

**提醒：** 这是JS里面长期的bug，而且可能永远不会修复。网页中的很多代码都依赖于此错误，因此修复将会导致更多的bug！

&nbsp;

你可以同样注意到 `a = undefined` 。我们明确的将 `undefined` 赋值给 `a` ，但其实这跟没赋值的变量一样没区别，就像在代码块顶部的 `var a;` 一个变量可通过不同的方式获得 “未定义”的状态，包括使用返回空值的函数和使用 `void`操作符。

&nbsp;

### 对象

&nbsp;

`object` 对象类型是一个复合值，你可以在设置属性（命名位置），保存任意类型的值。这可能是JavaScript里面最有用的值类型之一。

&nbsp;

```js
var obj = {
	a: "hello world",
	b: 42,
	c: true
};

obj.a;		// "hello world"
obj.b;		// 42
obj.c;		// true

obj["a"];	// "hello world"
obj["b"];	// 42
obj["c"];	// true
```
&nbsp;

通过以下图片来思考 `obj`对象可能很有帮助：

<img src="fig4.png">

&nbsp;

属性可以用 *点记法* （即，`obj.a`）或者 *括号记法*（即，`obj["a"]`）访问。点记法更简短而且更容易阅读，因此更推荐这种用法。

&nbsp;

如果你的属性命名内包含特殊字符，那么使用括号记法更合适，比如 `obj["hello world!"]` -- 当通过括号记法访问时，这些属性被称为 `keys` 键。`[ ]`记法内可以时变量（之后会解释）或者 `string` 字符串 *字面量*（字符串需被包裹在单引号或双引号中）。

&nbsp;

当然，如果你想访问名字被保存在其他变量中的属性／键，括号记法也很适用，例如：

&nbsp;

```js

var obj = {
	a: "hello world",
	b: 42
};

var b = "a";

obj[b];			// "hello world"
obj["b"];		// 42

```

&nbsp;

**注：** 想了解JavaScript中 `object` 的更多信息，参考本系列的 *this 和对象属性* 部分，具体见第3章。

&nbsp;

在JavaScript程序中还有一些你常会用到的其他的值的类型：*array* 和 *functions*。但是相比于内置类型，这些应该被认为是子类型 -- `object`类型的实例化版本。

&nbsp;

#### 数组

&nbsp;

一个数组是按索引位置，而非以属性／键形式保存任意值的对象。例如：

&nbsp;

```js
var arr = [
	"hello world",
	42,
	true
];

arr[0];			// "hello world"
arr[1];			// 42
arr[2];			// true
arr.length;		// 3

typeof arr;		// "object"

```

&nbsp;

**注：** 编程语言从0开始计数，如同在JS中，使用0作为数组中第一个元素的索引。

&nbsp;

通过图片帮助来思考 `arr` 数组吧：

&nbsp;

<img src="fig5.png">

&nbsp;

因为数组是特殊的对象（如 `typeof` 所暗示的），数组同样具有属性，包括自动更新的属性 `length`。

&nbsp;

理论上你可以用自己命名的属性来将数组当成正常的对象使用，或者你也可以使用只保存数字属性（`0` ,`1`等）的类似数组的对象。然而，这通常被认为是不恰当的使用方式。

&nbps;

最好最自然的方式是使用数组保存以数字排序的值，使用`object`保存命名的属性。

&nbsp;

#### 函数

&nbsp;

你在JS程序中还会使用到的 `object` 子类型便是函数：

&nbsp;

```js
function foo() {
	return 42;
}

foo.bar = "hello world";

typeof foo;			// "function"
typeof foo();		// "number"
typeof foo.bar;		// "string"
```

&nbsp;

再次的，函数是 `对象` 的子类型 -- `typeof` 返回 `"function"`，这也暗示了 `function` 是一个主要类型 -- 因此可以具有属性，但是你也仅仅能在一定限制里使用函数对象属性（例如 `foo.bar` ）。

&nbsp;

**注：** 有关JS值和类型的更多信息，参考本系列 *类型和语法* 部分的前两章。

&nbsp;

### 内置类型方法

&nbsp;

我们刚提及的内置类型和子类型暴露出来的属性和方法是相当强大和有用的。

&nbsp;

例如：

&nbsp;

```js
var a = "hello world";
var b = 3.14159;

a.length;				// 11
a.toUpperCase();		// "HELLO WORLD"
b.toFixed(4)
```

&nbsp;

“如何”调用 `a.toUpperCase()` 背后的原理比值上面存在的方法更加复杂。

&nbsp;

简单的说，存在一个`String`（缩写为 `S`）对象包装器形式，通常称为“原始类型”，与原始 `string` 类型配对；正是这个对象包装器在它的原型上定义了 `toUpperCase`的方法。

&nbsp;

当你通过属性或方法（例如代码段里的 `a.toUpperCase()`）将原始值比如 `"hello world"` 当作 `object`对象使用时，JS会自动将值“包装”进对象包装器的对应模块中（隐藏操作）。

&nbsp;

一个 `string` 值会被 `String`对象包装，一个 `number` 值会被 `Number`对象包装，一个 `boolean` 值会被 `Boolean`对象包装。大部分情况下，你不需要担心或直接使用值的这些对象包装器形式 -- 建议在所有情况下使用原始值的形式，JavaScript会帮你处理余下的事情。

**注：** 有关JS原始类型和“包装器”，参考本系列的 *类型和语法* 部分的第3章。想更好的理解对象原型，参考本系列 *this 和 对象原型* 部分的第5章。

&nbsp;

### 值的比较

&nbsp;

在你的JS程序中你将会用到两种类型值的比较：*相等* 和 *不想等*。任何比较的结果都会输出`boolean`布尔值（`true` 或者 `false`），无论比较中值的类型是什么。

&nbsp;

#### 强制转换

我们在第1章简要的提到过强制转换，现在再进一步说明。

&nbsp;

强制转换在JavaScript中的有两个形式：*显式* 和 *隐式*。
显式强制转换是很简单的，你可以在代码中明显的知道类型转换的发生，而隐式类型转换在基于一些操作时却不是那么明显。

&nbsp;

你可能听过有些人表达过这样的情绪“强制转换就是恶魔”，这源于事实上强制转换确实会造成一些意外的结果。也许除了程序语言，也没什么东西能让开发者感到沮丧了。

&nbsp;

强制转换并不是恶魔，而且也不意外。事实上，大部分情况下，用类型强制转换来构建代码是相当明智和可理解的，甚至可以用来 *提高* 代码可读性。

&nbsp;

*显式* 强制转换的例子如下：

&nbsp;

```js
var a = "42";

var b = Number( a );

a;				// "42"
b;				// 42 -- the number!

```

&nbsp;

*隐式* 强制转换的例子如下：

&nbsp;

```js
var a = "42";

var b = a * 1;	// "42" implicitly coerced to 42 here

a;				// "42"
b;				// 42 -- the number!

```

&nbsp;

### 真或假

&nbsp;

在第1章中，我们简单的提到了值的“真”或“假”的属性：当一个不是布尔值`boolean`属性的值被强制转换成`boolean`布尔值，它分别的变成了`true`或`false`了吗？

&nbsp;

JavaScript中返回“假”`false`的具体列表如下：

&nbsp;

+ `""`（空字符串）

&nbsp;

+ `0`，`-0`，`Nan`（无效的`number`)

&nbsp;

+ `null`，`undefined`

&nbsp;

+ `false`

&nbsp;

不在这个“假”具体列表中的值便都为“真”，示例如下：

&nbsp;

+ `"hello"`

&nbsp;

+ `42`

&nbsp;

+ `true`

&nbsp;

+ `[ ]`, `[ 1, "2", 3 ]` (arrays)

&nbsp;

+ `{ }`, `{ a: 42 }` (objects)

&nbsp;

+ `function foo() { .. }` (functions)

&nbsp;

重要的是需要记住如果一个无`boolean`属性的值被实际上强制转换成`boolean`时，只遵守这种“真／假”转换机制。不过也有一种情况会令你产生疑惑，当一个值看起来被强制转换成`boolean`却不是的时候。

&nbsp;

##### 相等

&nbsp;

有四种相等运算符：`==`，`===`，`!=`和`!==`。`!`形式当然对应的是它们的“不相等”的部分；别混淆不相等和不等式。

&nbsp;

`==`之间`===`的区别通常主要在于`==`检查值是否相等，`===`检查值和类型是否同时相等。然而，这仍是不准确的。分别这两者的正确方式是，`==`检查的是值是否相等允许值强制转换，而`===`检查的值不允许强制转换；`===`经常被称为“严格相等”就是由于这个原因。

&nbsp;

以下的例子中，隐式强制转换被`==`宽容相等比较允许，不被`===`严格相等允许：

&nbsp;

```js
var a = "42";
var b = 42;

a == b;			// true
a === b;		// false

```

&nbsp;

在`a == b`的比较中，JS注意到类型不匹配，所以进行了一系列的步骤进行类型强制转换直到两个值的类型匹配，这样可以简单检查值的相等。

&nbsp;

如果你多加思考，通过类型转换后，`a == b`有两种方式得出结果为`true`。比较可以通过`42 == 42`结束，或者` "42" == "42" `，所以是哪个呢？

&nbsp;

答案是：`"42"`变成`42`,然后比较` 42 == 42 `。在这个简单的例子中，无论是以哪种方式处理都不那么重要，因为结果是一样的。不过在很多的复杂情况下，不仅要考虑的是比较的结果是什么，而且要考虑是 *怎样* 得到这个结果的。

&nbsp;

`a === b`得到的结果为`false`,因为强制转换是不被允许的。所以单一的值的比较是明显失败的。许多开发者认为`===`严格相等更加可预测，所以他们更倾向的使用这种形式而对`==`敬而远之。我认为这种观点是非常短视的。我相信`==`对于你的程序是个强大的工具，一旦你花时间了解了它的机制。

&nbsp;

我们在这不会过度的强调`==`中类型转换的所有细节。其中的一些机制非常的明智，但也要小心其中一些重要的个别情况。你可以阅读ES5规范的11.9.3([http://www.ecma-international.org/ecma-262/5.1/](http://www.ecma-international.org/ecma-262/5.1/))去了解具体的规则，相比于所有关于它的负面评论，你会惊讶于这些机制是多么简单。

&nbsp;

为了使你明白如何在不同情况下选择`==`或者`===`,我将大部分细节性的东西总结成一些简单的规则，如下所示：

&nbsp;

+ 如果比较中等号两侧的值是`true`或者`false`，使用`===`，避免使用`==`。

&nbsp;

+ 如果比较中的值是这几个特殊值（`0`,`""`,或者`[]` -- 空数组），使用`===`,避免使用`==`。

&nbsp;

+ 在其他 *所有* 情况下，你都可以使用`==`。不仅是因为它可靠，还因为在大部分情况下，可以在一定程度上精简你的代码并且提高代码可读性。

&nbsp;

这些规则的应用要求你可以客观的检查自己的代码，并且知道用于比较的变量中的值是属于什么类型的。如果你确定值的类型，`==`是可靠的，大胆使用它！但是如果你不确定值的类型，还是用`===`就好，就是这样。

&nbsp;

`!=`不等式与`==`配对，`!==`不等式与`===`配对，所有我们在刚才讨论的规则同样也适用于这些不相等的比较。

&nbsp;

你应该特别注意`==`和`===`的比较规则，如果你在比较两个非原始类型值，比如`object`对象（包括`function`函数和`array`数组），因为这些值实际上是被引用的，`==`和`===`只是简单比较引用的类型是否匹配，而不是里面的值。

&nbsp;

例如，`array`数组只是通过在里面每个值之间加入逗号（`,`)，被默认转换成`string`字符串。你可能会认为两个有着完全相同内容的数组会相等，但是并不是这样：

&nbsp;

```js
var a = [1,2,3];
var b = [1,2,3];
var c = "1,2,3";

a == c;		// true
b == c;		// true
a == b;		// false
```

&nbsp;

**注：** 有关`==`相等比较规则的更多信息，参考ES5规范（section 11.9.3），还可参考本系列的 *类型和语法* 部分的第4章内容；参考第2章查看更多关于值和引用的信息。

&nbsp;

#### 不等式

`<`，`>`，`<=`和`>=`操作符被用于不等式，规范的叫法为“ 关系比较 ”。通常的，它们将被用于常见值的比较比如`number`数字。`3 < 4` 是很容易理解的。

&nbsp;

但是JavaScript中的`string`值也可以用不等式比较,用常见的字母规则（`"bar" < "foo" `）。

&nbsp;

那不等式里面的强制类型转换是怎样的呢？类似于`==`比较的规则（虽然不是完全一样）也同样适用于不等式运算符。值得注意的是，没有不允许强制转换的“严格不相等”运算，就像`===`“严格相等”做的一样。

&nbsp;

比如：

&nbsp;

```js
var a = 41;
var b = "42";
var c = "43";

a < b;		// true
b < c;		// true
```

&nbsp;

这里发生了什么呢？在ES5规范的11.8.5点部分，里面提到了如果`<`两侧的值都为`string`字符串，就像`b < c`,比较是按照字母顺序进行（即 字母字典顺序）。但是如果一侧不是`string`字符串，就像`a < b`,那两侧的值便会被强制转换成`number`数字，然后常见的数字比较就发生了。

&nbsp;

你可能遇到的最大的误区可能是潜在的不同类型值之间的比较 -- 记住，不存在“严格不相等”形式 -- 当其中的一个值不会转换成一个有效的数字，例如：

&nbsp;

```js

ar a = 41;
var b = "42";
var c = "43";

a < b; // true
b < c; // true


```

&nbsp;

等等，为什么这三组值的比较都返回`false`？因为`b`的值在`<`与`>`的比较中被强制转换成了“无效的值”`NaN`，而且规范中提到`NaN`既不别的值大，也不比别的值小。

&nbsp;

`==`相等比较不成立是由于其他原因。`a == b`，会因为 `42 == NaN` 或 `"42" == "foo"`被阻止，我们之前有解释原因，之前的就是例子。

&nbsp;

**注：**  有关更多关于不等式比较规则的信息，请参考ES5规范的11.8.5部分，同样可以参考本系列 *类型和语法* 部分的第4章。

&nbsp;

### 变量

在JavaScript中，变量的名字（包括函数的名字）必须为有效的标识，不过当你考虑使用非传统的字符例如Unicode的时候，标识中的有效字符的严格和完整规则可能会给你造成一定困扰。如果只考虑典型的ASCII字母数字型字符，那么规则就简单一些。

&nbsp;

标识必须以`a-z`，`A-Z`，`$`，或者`_`起头。并且只能包含这些字符，加上数字`0-9`。

&nbsp;

通常，变量标识的规则同样适用于属性命名。然而，有些确切的单词不能用于变量命名，但可以用于属性命名，这些单词被称为“保留字”，包括JS关键字（`for`，`in`，`if`等），以及`null`，`true`，和`false`。

**注：** 更多有关保留字的信息，请参考本系列 *类型和语法* 部分的附录A。

&nbsp;

### 函数作用域

&nbsp;

你使用`var`关键词声明一个变量，它会属于当前函数的作用域，或者全局作用域如果它位于所有函数之外的最外层。

&nbsp;

#### 提升

当一个`var`出现在作用域中，声明的变量将属于整个作用域并且可以随处访问。

&nbsp;

打个比喻，这种行为称为 *提升*，当一个`var`声明在概念上提升到所在的闭合作用域的顶层。从技术上讲，这个过程更加准确的解释了代码是如何被编译的，但是我们暂时先跳过这些细节。

&nbsp;

例子：

&nbsp;

```js

ar a = 2;

foo();					// works because `foo()`
						// declaration is "hoisted"

function foo() {
	a = 3;

	console.log( a );	// 3

	var a;				// declaration is "hoisted"
						// to the top of `foo()`
}

console.log( a );	// 2

```

&nbsp;

**提示：** 依赖变量 *提升* 来提前使用变量而不是`var`声明之后再使用，这种方式是不常见的，而且不是一个好主意，可能会造成困扰。更常见和易于接受的是使用函数声明 *提升*,就像我们正式声明声明之前先调用`foo()`函数。

&nbsp;

#### 嵌套作用域

&nbsp;

当你声明一个变量，在作用域内的任何地方都能访问到，包括任何更内层的作用域，例如：

&nbsp;

```js
function foo() {
	var a = 1;

	function bar() {
		var b = 2;

		function baz() {
			var c = 3;

			console.log( a, b, c );	// 1 2 3
		}

		baz();
		console.log( a, b );		// 1 2
	}

	bar();
	console.log( a );				// 1
}

foo();

```

&nbsp;

注意，`c`不能在`bar()`的作用域中被访问到，因为它只在`baz()`的作用域中被声明，`b`不能在`foo()`中被访问到也是因为同样的原因。

&nbsp;

如果你想在一个访问不了某个变量的作用域中访问它，你将得到一个`ReferenceError`（引用错误）。如果你想设置一个未声明的变量，在全局作用域的顶部将创建这个变量（不恰当的做法），或者这会导致错误，这都依赖于“严格模式”，例如：

&nbsp;

```js
function foo() {
	a = 1;	// `a` not formally declared
}

foo();
a;			// 1 -- oops, auto global variable :(

```

&nbsp;

这是非常不好的尝试，别这么写！记住永远要先声明你的变量。

&nbsp;

除了在函数层级创建一个函数声明，ES6允许你在闭合代码块（`{..}`）中声明变量，使用`let`关键字。除了一些细微的差别，作用域的规则跟函数的基本一致。

&nbsp;

```js
function foo() {
	var a = 1;

	if (a >= 1) {
		let b = 2;

		while (b < 5) {
			let c = b * 2;
			b++;

			console.log( a + c );
		}
	}
}

foo();
// 5 7 9

```

因为使用 `let` 而不是 `var`，变量`b` 将只属于 `if` 语句，而不是`foo()`函数作用域。相似的，`c`只属于`while`循环。代码块作用域对于更加细粒度范围内的变量作用域管理是很有用的，它能使得你的代码更容易被维护。

&nbsp;

**注：** 更多关于作用域的信息，请参考本系列的 *作用域和闭包* 部分，更多关于`let` 代码块作用域的信息，可以参考 *ES6和未来* 部分。

&nbsp;

### 条件

&nbsp;

除了在第一章中提到的`if`，JavaScript还提供了其他一些我们需要注意的条件机制。

&nbsp;

有时候你会发现你写了一系列这样的代码：

&nbsp;

```js

if (a == 2) {
	// do something
}
else if (a == 10) {
	// do another thing
}
else if (a == 42) {
	// do yet another thing
}
else {
	// fallback to here
}

```

&nbsp;

这种结构是有效的，但是过于冗长，因为你需要为每个情况指定`a`。有其他的形式，`switch`语句：

&nbsp;

```js
switch (a) {
	case 2:
		// do something
		break;
	case 10:
		// do another thing
		break;
	case 42:
		// do yet another thing
		break;
	default:
		// fallback to here
}
```

&nbsp;

`break`是重要的，如果你只希望在一种情况下运行语句，如果你在`case`里面忽略了`break`，但是那个`case`是匹配或者运行的，程序会继续执行下一个`case`语句，不管上个`case`是否匹配。这种所谓的“通过”有时候也是有用的：

&nbsp;

```js
switch (a) {
	case 2:
	case 10:
		// some cool stuff
		break;
	case 42:
		// other stuff
		break;
	default:
		// fallback
}

```

&nbsp;

这里，如果`a`是`2`或是`10`,它会执行“some cool stuff”这部分代码语句。

&nbsp;

JavaScript中其他形式的条件语句是“条件操作符”，或者称为“三元操作符”。这像一个更简洁的`if..else`语句，例如：

&nbsp;

```js
var a = 42;

var b = (a > 41) ? "hello" : "world";

// similar to:

// if (a > 41) {
//    b = "hello";
// }
// else {
//    b = "world";
// }

```

&nbsp;

如果测试表达式（`a > 41`）判断结果为`true`，那结果就为第一项（`"hello"`），反之，就是第二项(`"world"`)，无论结果是哪个最终都会赋值给`b`。

&nbsp;

条件操作符不是必须在声明里面使用，但是这绝对是最普遍的用法。

&nbsp;

**注：** 有关更多测试条件句的信息和`switch`和`?:`的其他模式，参考此系列的 *类型和语法*。

&nbsp;

### 严格模式

ES5在语言中加入了“严格模式”，为具体的行为束缚了规则。通常，这些限制被视为将代码保持为更安全和更合适的准则集合。同样的，严格模式使得你的代码可以被引擎更加优化的执行。严格模式对于代码大有益处，你应该将其用于所有程序。

&nbsp;

你可以在单独的函数中选择使用严格模式，或者整个文件中，这得看你将严格模式编译符放在哪儿：

&nbsp;

```js
function foo() {
	"use strict";

	// this code is strict mode

	function bar() {
		// this code is strict mode
	}
}

// this code is not strict mode
```

&nbsp;

同这个比较：

&nbsp;

```js
"use strict";

function foo() {
	// this code is strict mode

	function bar() {
		// this code is strict mode
	}
}

// this code is strict mode

```

&nbsp;

严格模式的一个关键区别在于（提高！）不允许通过忽略`var`隐式声明全局变量：

&nbsp;

```js
function foo() {
	"use strict";	// turn on strict mode
	a = 1;			// `var` missing, ReferenceError
}

foo();
```

&nbsp;

如果你在代码中开启严格模式，接着报错了，或者代码运行起来有bug，你可能就想避免使用严格模式。但这可能是个坏主意。如果严格模式在你的程序里导致了问题，几乎肯定这个一个提示，你的程序中的有些东西是需要修复的。

&nbsp;

严格模式不仅让你的代码更安全，或者能优化你的代码，而且也代表了语言的未来趋势。对于你来说，比起不使用它，现在习惯严格模式更简单 - 越晚转换使用越困难。

&nbsp;

**注：** 有关更多严格模式的信息，参考本系列*类型和语法*部分的第5章。

&nbsp;

### 作为值的函数

&nbsp;

至今为止，我们已经讨论了函数作为JavaScript中的`scope`(作用域)的主要机制是如何的。典型的调用`function`声明句式如下：

&nbsp;

```js
function foo() {
	// ..
}
```

&nbsp;

尽管在句式中看起来不明显，`foo`基本上只是一个外部封闭作用域的变量，与被声明的`function`的相关联。即，`function`本身就是一个值，就像`42`或者`[1,2,3]`。

&nbsp;

这可能在一开始的时候是个陌生的概念，所以你得花点时间思考它。你不仅能向函数*传递*一个值（参数），而且也可以传入被赋给变量的*函数本身*，或者从别的函数中传入和返回。

&nbsp;

因此，一个函数值应该被认为是表达式，与别的值或者表达式的一样。

&nbsp;

思考：

&nbsp;

```js
var foo = function() {
	// ..
};

var x = function bar(){
	// ..
};

```

&nbsp;

第一个被赋值给变量`foo`的函数表达式被称为*匿名函数*，因为它没`name`名字。

&nbsp;

第二个函数表达式是被命名的（`bar`），即使它的引用被赋值给变量`x`。*命名函数* 通常更好，即使 *匿名函数* 仍然很常见。

&nbsp;

有关更多信息，参考本系列的 *作用域和闭包* 部分。

&nbsp;

## 立即执行函数表达式

&nbsp;

在前面的代码片段中，每个函数都没有执行 -- 我们可以使用如果包含`foo()`或者`x()`。

&nbsp;

执行函数有另外一种方式，通常被引用为一个 *立即执行函数表达式* （IIFE）：

&nbsp;

```js
(function IIFE(){
	console.log( "Hello!" );
})();
// "Hello!"
```

&nbsp;

包裹在 `(function IIFE(){ .. })` 外面的 `(..)`只是JS语法中的一点细微差别来阻止被当作正常的函数声明。

&nbsp;

表达式末尾的`()` -- `})()`这一行 -- 实际上是执行在它之前立即引用的函数表达式。

&nbsp;

这看起来很陌生，但是乍看之下也不是那么陌生。思考下
`foo`和`IIFE`两者相似处：

&nbsp;

```js
function foo() { .. }

// `foo` function reference expression,
// then `()` executes it
foo();

// `IIFE` function expression,
// then `()` executes it
(function IIFE(){ .. })();

```

&nbsp;

你可以看到，在执行括号`()`前列出`(function IIFE(){ .. })`与将`foo`放在执行括号`()`前是一样的；在两种情况下，函数引用都被后面的`()`立即执行。

&nbsp;

因为一个IIFE只是个函数，函数会创建变量*作用域*,以这种使用IIFE的方式经常被用来声明不会影响到IIFE外代码的变量：

&nbsp;

```js

var a = 42;

(function IIFE(){
	var a = 10;
	console.log( a );	// 10
})();

console.log( a );		// 42

```

&nbsp;

IIFEs也可以有返回的值：

&nbsp;

```js
var x = (function IIFE(){
	return 42;
})();

x;	// 42
```

&nbsp;

`42`在`IIFE`命名函数执行的时候被返回，然后被赋值给`x`；

&nbsp;

## 闭包

*闭包* 是JavaScript里，最重要并且经常难被理解的概念之一。在这里我不会进行详述，相反的你可以参考本系列的 *作用域和闭包*部分。但是我想稍微介绍一些以便你能了解常见的概念。这会是你的JS技能中最重要的技术之一。

&nbsp;

你可以将闭包理解为“记住”和持续访问函数的作用域（变量），甚至函数已经完成运行。

&nbsp;

思考：

&nbsp;

```js

function makeAdder(x) {
	// parameter `x` is an inner variable

	// inner function `add()` uses `x`, so
	// it has a "closure" over it
	function add(y) {
		return y + x;
	};

	return add;
}

```

&nbsp;

外部函数`makeAdder(..)`每次调用都会返回的内部函数`add(..)`的引用会记住传入`makeAdder(..)`中的`x`值。现在，让我们使用`makeAdder(..)`：

&nbsp;

```js
// `plusOne` gets a reference to the inner `add(..)`
// function with closure over the `x` parameter of
// the outer `makeAdder(..)`
var plusOne = makeAdder( 1 );

// `plusTen` gets a reference to the inner `add(..)`
// function with closure over the `x` parameter of
// the outer `makeAdder(..)`
var plusTen = makeAdder( 10 );

plusOne( 3 );		// 4  <-- 1 + 3
plusOne( 41 );		// 42 <-- 1 + 41

plusTen( 13 );		// 23 <-- 10 + 13
```

&nbsp;

这段代码是如下运行的：

&nbsp;

1. 当我们调用`makeAdder(1)`，我们得到了内部返回的函数`add(..)`,并且将`x`标记为`1`。我们称这个函数为`plusOne(..)`的引用。

&nbsp;

2. 当我们调用`makeAdder(10)`，我们得到了另一个内部返回的函数`add(..)`,并且将`x`标记为`10`。我们称这个函数为`plusTen(..)`的引用。

&nbsp;

3. 当我们调用`plusOne(3)`，它将`3`（内部的`y`）和`1`（被`x`保存）相加，然后我们得到结果为`4`。

&nbsp;

4. 当我们调用`plusOne(13)`，它将`13`（内部的`y`）和`10`（被`x`保存）相加，然后我们得到结果为`23`。

&nbsp;

不要担心这个在一开始看起来就陌生和困惑 -- 你行的！只要花足够的时间练习去了解它。

&nbsp;

但是相信我，一旦你了解了，它会是编程中最有用和强大的技术之一。花些精力来研究闭包是绝对值得的。在下一个部分，我们将有更多关于闭包的练习。

&nbsp;

## 模块

&nbsp;

JavaScript中闭包最常见的用法就是模块模式。模块可以让你定义私有实现的细节（变量，函数），并且对外层不可见，同时还有从外层可以访问的公有API。

&nbsp;

思考：

```js

function User(){
	var username, password;

	function doLogin(user,pw) {
		username = user;
		password = pw;

		// do the rest of the login work
	}

	var publicAPI = {
		login: doLogin
	};

	return publicAPI;
}

// create a `User` module instance
var fred = User();

fred.login( "fred", "12Battery34!" );

```

&nbsp;

`User()`函数作为外层作用域保存变量`username`和`password`，以及内部的`doLogin()`函数；这些都是`User`模块的所有内部细节，并且不能被外层访问。

&nbsp;

**提示：** 我们在这儿不会调用`new User()`，尽管这种情况对于大多数读者来说更常见。`User()`只是一个函数，并不是可以实例化的类，所以是被正常调用的。使用`new`是不当的，并且实际上浪费资源。

&nbsp;

执行`User()`创建了一个`User`模块的*实例* -- 一个新的作用域被创建，并且复制了内部全部的变量和函数。我们将这个实例声明给`fred`。如果我们再次执行`User()`，我们将得到一个新的实例，并且它与`fred`完全独立。

&nbsp;

内部的`doLogin()`函数有一个包含`username`和`password`的闭包，这表示它会保留对它们的访问就算是在`User()`函数完成执行之后。

&nbsp;

`publicAPI`是包含一个属性或方法的对象，`login`，是对内部`doLogin()`函数的引用。当我们从`User()`返回`publicAPI()`，它变成了我们称为`fred`的实例。

&nbsp;

此时，外层的`User()`函数已经完成执行。正常的想，你会认为内部的变量`username`和`password`已经不见了。但其实并没有，因为`login()`内部的闭包使得它们一直存在。

&nbsp;

这就是我们为什么调用`fred.login()` -- 和调用内部的`doLogin()`一样 -- 而且它还能访问内部的`username`和`password`。

&nbsp;

对于闭包和模块模式有个简要的理解是一个好机会，虽然有些概念还是会造成困惑，但是没关系，熟能生巧。

&nbsp;

如果想更多的探索相关概念，建议阅读本系列的 *作用域和闭包* 部分。

### `this`关键字

&nbsp;

JavaScript中另一个非常容易混淆的概念就是`this`关键字。再次，*this和对象* 部分中的一些章节有讨论到它，在这里我们只是简单的介绍一下这个概念。

&nbsp;

`this`经常与“面向对象模式”相联系，在JS中，`this`是不同的机制。

&nbsp;

如果一个函数内有个`this`的引用，那`this`引用通常被指向为一个`object`，但是指向哪个`object`是依赖于函数是如何被调用的。

&nbsp;

重要的是要意识到`this`*不会指向*到函数本身，这是最常见的误解。

&nbsp;

快速浏览：

```js

function foo() {
	console.log( this.bar );
}

var bar = "global";

var obj1 = {
	bar: "obj1",
	foo: foo
};

var obj2 = {
	bar: "obj2"
};

// --------

foo();				// "global"
obj1.foo();			// "obj1"
foo.call( obj2 );	// "obj2"
new foo();			// undefined

```

&nbsp;

`this`被以上四种形式设置，然后在代码段的最后四行展示结果：

&nbsp;

1. `foo()`在非严格模式下将`this`设置成全局对象 -- 如果在严格模式下，`this`会变成`undefined`,并且当你在访问`bar`属性的时候会得到报错 -- 所以`"global"`是`this.bar`找到的值。

&nbsp;

2. `obj1.foo()`将`this`设置为`obj1`对象。

&nbsp;

3. `foo.call(obj2)`将`this`设置为`obj2`对象。

&nbsp;

4. `new foo()`将`this`设置为一个全新的空对象。

&nbsp;

结论：为了了解`this`的指向，你必须检查问题里的函数是如何被调用的，它通常会这四种方式之一来展示结果。

**注：** 有关更多关于`this`的信息，参考*this和对象原型* 的第一章和第二章。

&nbsp;

### 原型

&nbsp;

JavaScript中的原型机制比较复杂。我们在这只是简单了解一下。你将需要花费大量时间重温*this和对象原型*部分的第4-6章节的所有细节。

&nbsp;

当你在一个对象中引用一个属性，如果那个属性不存在，JavaScript将使用那个对象的内部原型引用去寻找另一个属性所在的对象。你可以认为这是备选如果属性没有找到的话。

&nbsp;

从一个对象链接到另一个备选的内部原型引用在对象创建后便会生效。用一个内建使用的方法`object.create(..)`来说明它是最简单的。

&nbsp;

思考：

&nbsp;

```js
var foo = {
	a: 42
};

// create `bar` and link it to `foo`
var bar = Object.create( foo );

bar.b = "hello world";

bar.b;		// "hello world"
bar.a;		// 42 <-- delegated to `foo`

```

&nbsp;

这个可能帮助你更直观的观察`foo`和`bar`对象和它们的关系：

&nbsp;

![alt text](fig6.png)

&nbsp;

`a`属性并不实际存在于`bar`对象中，但是因为`bar`是`foo`的原型链，JavaScript自动的在`foo`对象中查找`a`.

&nbsp;

这种链接的方式在程序语言中可能看起来是陌生的特征。这种特征最常见的使用方式 -- 我不推崇的 -- 是通过“继承”模拟“class”机制。

&nbsp;

但是更常见的使用原型的方式是被称为“事件代理”的模式，当你有意设计你的链接对象为可以*代理*另一个对象中部分需要的行为。

**注：** 更多关于原型和事件代理的信息，参考*this和对象原型*的第4-6章节。

&nbsp;

### 旧和新

&nbsp;

我们已经提及的JS的一些特性，以及剩余将在本系列中提到的，是最新的并且不会在旧版浏览器中生效。事实上，一些规范中的新特性深知不兼容任何稳定的浏览器版本。

&nbsp;

所以，你将怎么看待这些新特性呢？只是等上一段时间知道旧版浏览器消失吗？

&nbsp;

这便是许多开发者如何看待现状的，但这对JS来说，并不是一个良好的途径。

&nbsp;

你可以使用两项主要的技术来在旧版浏览器上应用新特性：polyfilling和transpiling。

&nbsp;

#### Polyfilling

&nbsp;

"polyfill"是一个被发明的术语[（by Remy Sharp）](https://remysharp.com/2010/10/08/what-is-a-polyfill)，被用来指代新特性的定义并且制造可以执行相同行为都代码，同时也可以在旧版浏览器上运行。

&nbsp;

例如，ES6定义了一个方法称为`Number.isNaN(..)`来提供快速准确的检测用于`NaN`值，废弃了原来的`isNaN()`方法。但是可以用polyfill来在你代码中使用它，不管是不是ES6浏览器。

&nbsp;

思考：

&nbsp;

```js
if (!Number.isNaN) {
	Number.isNaN = function isNaN(x) {
		return x !== x;
	};
}

```

&nbsp;

`if`条件句防止在ES6浏览器应用pollyfill定义已经存在的特性，如果没有，我们将定义`Number.isNaN(..)`。

&nbsp;

**注：** 我们在这儿做的检测利用了`NaN`值的灵活之处，即它们在整个语言中，是唯一不等于自身的值。所以`NaN`是唯一一个可以领`x !== x`为`true`的值。

&nbsp;

不是所有的新特性都能应用polyfill。有时候大部分的事件可以被polyfill，但还是会有些不同。你应该非常非常小心使用polyfill，来确保你尽可能严格的应用规范。

&nbsp;

或者更好的是，使用你能确信的一套polyfill工具，像ES5-Shim[https://github.com/es-shims/es5-shim](https://github.com/es-shims/es5-shim)和ES6-Shim[https://github.com/es-shims/es6-shim](https://github.com/es-shims/es6-shim)

&nbsp;

#### Transpiling

&nbsp;

还没有方法polyfill语言中新增的语法。新的语法将在旧的JS引擎中抛出错误，比如无法识别／无效.

&nbsp;

所以更好的选项是使用工具将你的新版本代码转换成同等作用的旧版本代码。这个过程通常被称为“transpiling“，一个表示转换和编译的名词。

&nbsp;

实质上，你的源代码是以新版本语法形式写的，但是运行在浏览器上的是转换和编译后的，旧版本语法的代码。一般是在构建过程中加入transpiler，类似于代码检测或者压缩。

&nbsp;

你可能会疑问为什么要这么麻烦去写新语法的代码然后再转化成旧版本的代码 -- 直接写成旧版本代码不好吗？

&nbsp;

以下是几点你应该关注transpiling的原因：

&nbsp;

* 语言中新增的语法是设计成令你的代码可读性更强，更加容易维护。同样旧版本语法的代码经常更令人费解。建议你用更新和更简洁的语法写代码，这不仅是为了你自己，而且是为了开发团队中的其他成员。

&nbsp;

* 如果你只是为了旧版本浏览器转换编译代码，但是在最新的浏览器上应用新语法，你需要利用浏览器使用新语法时的性能优化。这也将使得浏览器开发者有更多的真实代码去测试它们的实现和性能。

&nbsp;

* 及早的使用新语法可以使得它在实际环境的测试中，变得更稳定，这也将给JavaScript委员会提供及早的反馈。如果问题被及时发现，在语言设计的错误变成长期遗留前，它们可以被更改或修复。

&nbsp;

以下是tanspiling的一个例子，ES6添加了一个称为“默认参数值”的特性，它看起来像这样：

&nbsp;

```js
function foo(a = 2) {
	console.log( a );
}

foo();		// 2
foo( 42 );	// 42

```

&nbsp;

很简单对吧？也很有帮助！但这个语法在ES6之前版本的引擎上是无效的／所以transpiler是如何使得它们在旧环境上运行的呢？

&nbsp;

```js
function foo() {
	var a = arguments[0] !== (void 0) ? arguments[0] : 2;
	console.log( a );
}
```

&nbsp;

如你所见，它首先检测`arguments[0]`是否为`void 0`（即`undefined`),如果是这样便提供默认值`2`,相反，传入的值是什么就会声明什么。

&nbsp;

除了现在使用更好的语法，即使是在旧版浏览器中，查看转换编译后的代码实际上更清楚的解释了预期事件。

&nbsp;

如果只是看ES6版本，你可能不会发现`undefined`是唯一不能传递给默认值参数的值，但是转换编译后的代码看起来更清晰。

&nbsp;

最后一个该强调的关于transpiler的重要细节是它们现在应该被认为是JS开发生态和进程的标准部分了。JS将继续发展，并且比之前更快，所以每过几个月新的语法和特性就会被添加。

&nbsp;

如果你默认就使用transpiler，无论何时只要你发现它有用，你就能将其转换成较新的语法，而不是等很久直到现在的浏览器被淘汰。

&nbsp;

有几个不错的transpiler供你选择，在本书写作的同时有以下几个推荐：

&nbsp;

* Babel [https://babeljs.io](https://babeljs.io)Transpiles E6+ into ES5

&nbsp;

* Traceur [https://github.com/google/traceur-compiler](https://github.com/google/traceur-compiler)
Transpiles ES6, ES7, and beyond into ES5

### 非JavaScript

&nbsp;

至今，我们在JS语言中仅仅涉及到的是其本身。事实上大部分JS是运行和交互在环境中的，比如浏览器。严格来说，你在代码中编写的一小部分东西不是直接由JavaScript控制的。 这听起来有点奇怪。

&nbsp;

最常见的非JavaScript的JavaScript你将遇到的是 DOM API，例如：

&nbsp;

```js
var el = document.getElementById( "foo" );
```

&nbsp;

当你的代码在浏览器中运行的时候，变量`document`就作为全局变量存在了。它不是JS引擎提供的，也不是被JavaScript规范控制。它的形式看起来差不多是一个JS`object`,但是实际上也不是。它是一个特殊的`object`，通常被称为“宿主对象”。

&nbsp;

此外，`document`中的`getElementById(..)`方法看起来就像一个正常的JS函数，但是它只是一个由浏览器提供的，暴露在交互界面的内置方法。在一些新版本的浏览器中，这个层也在JS中，但是传统的DOM和事件是运行在C/C++环境中的。

&nbsp;

另外一些例子就是关于输入和输出 (I/O)

&nbsp;

开发者都喜欢使用的`alert()`会在用户浏览器窗口弹出一个消息框。`alert(..)`是浏览器提供给你的JS程序的，而不是JS引擎提供的。你发起的调用给浏览器内部发送消息，然后浏览器处理图形界面，显示消息框。

&nbsp;

`console.log(..)`也是如此；你的浏览器提供这些机制，并且将它们放置在开发者工具中。

&nbsp;

这本书，和整个系列，都集中在探讨JavaScript语言。这就是为什么你没有看到实质性的提及这些非Javascript的JavaScirpt机制。即使如此，你还是需要了解它们，因为它们会出现在你写的每个JS程序中。

&nbsp;

### 复习

&nbsp;

学习JS编程风格的第一步就是对它的核心机制有个基本了解，例如值，类型，函数闭包，`this`，和原型。

&nbsp;

当然，这些主题中的每一个所涉及的地方比你在这儿了解到的都要多，这就是为什么它们们在本系列的其余部分都有专门的章节和书籍来进行探讨。在你习惯本章中提及多概念和代码示例后，你就可以开始深入本系列的其余部分，并且更加深入的了解JS语言。

&nbsp;

本书的最后一章将简要总结系列中的其他所有部分以及他们涵盖的其他概念，除去我们已经探讨过的内容。







