## 作用域闭包

接下来的内容需要对作用域工作原理相关的基础知识有非常深入的理解。

我们将注意力转移到这门语言中一个非常重要但又难以掌握， 近乎神话的概念上： 闭包。
如果你了解了之前关于词法作用域的讨论， 那么闭包的概念几乎是不言自明的。 魔术师的
幕布后藏着一个人， 我们将要揭开他的伪装。 我可没说这个人是 Crockford^1 ！

在继续学习之前， 如果你还是对词法作用域相关内容有疑问， 可以重新回顾一下第 2 章中
的相关内容， 现在是个好机会。

### 启示

对于那些有一点 JavaScript 使用经验但从未真正理解闭包概念的人来说， 理解闭包可以看
作是某种意义上的重生， 但是需要付出非常多的努力和牺牲才能理解这个概念。

回忆我前几年的时光， 大量使用 JavaScript 但却完全不理解闭包是什么。 总是感觉这门语
言有其隐蔽的一面， 如果能够掌握将会功力大涨， 但讽刺的是我始终无法掌握其中的门
道。 还记得我曾经大量阅读早期框架的源码， 试图能够理解闭包的工作原理。 现在还能回
忆起我的脑海中第一次浮现出关于“ 模块模式” 相关概念时的激动心情。

那时我无法理解并且倾尽数年心血来探索的， 也就是我马上要传授给你的秘诀： JavaScript
中闭包无处不在， 你只需要能够识别并拥抱它。 闭包并不是一个需要学习新的语法或模式
才能使用的工具， 它也不是一件必须接受像 Luke2 一样的原力训练才能使用和掌握的武器。
闭包是基于词法作用域书写代码时所产生的自然结果， 你甚至不需要为了利用它们而有意
识地创建闭包。 闭包的创建和使用在你的代码中随处可见。 你缺少的是根据你自己的意愿
来识别、 拥抱和影响闭包的思维环境。

最后你恍然大悟： 原来在我的代码中已经到处都是闭包了， 现在我终于能理解它们了。 理
解闭包就好像 Neo^3 第一次见到矩阵^ 4 一样。

>注 1： Douglas Crockford 是 Web 开发领域最知名的技术权威之一， ECMA JavaScript 2.0 标
>准化委员会委员，被 JavaScript 之父 Brendan Eich 称为 JavaScript 界的宗师。

### 实质问题

好了， 夸张和浮夸的电影比喻已经够多了。

下面是直接了当的定义， 你需要掌握它才能理解和识别闭包：
当函数可以记住并访问所在的词法作用域时， 就产生了闭包， 即使函数是在当前词法作用
域之外执行。

下面用一些代码来解释这个定义。
```javascript
function foo() {
	var a = 2;
	function bar() {
		console.log( a ); // 2
	} bar();
}
foo();
```
这段代码看起来和嵌套作用域中的示例代码很相似。 基于词法作用域的查找规则， 函数
bar() 可以访问外部作用域中的变量 a（ 这个例子中的是一个 RHS 引用查询）。

这是闭包吗？

技术上来讲， 也许是。 但根据前面的定义， 确切地说并不是。 我认为最准确地用来解释
bar() 对 a 的引用的方法是词法作用域的查找规则， 而这些规则只是闭包的一部分。（ 但却
是非常重要的一部分！ ）

>注 2：《 星球大战》 系列电影中的人物。
>注 3： 电影《 骇客帝国》 的主角。 
>注 4： 电影《 骇客帝国》 中拥有自我意识主宰一切的超级计算机。

从纯学术的角度说， 在上面的代码片段中， 函数 bar() 具有一个涵盖 foo() 作用域的闭包
（ 事实上， 涵盖了它能访问的所有作用域， 比如全局作用域）。 也可以认为 bar() 被封闭在
了 foo() 的作用域中。 为什么呢？ 原因简单明了， 因为 bar() 嵌套在 foo() 内部。

但是通过这种方式定义的闭包并不能直接进行观察， 也无法明白在这个代码片段中闭包是
如何工作的。 我们可以很容易地理解词法作用域， 而闭包则隐藏在代码之后的神秘阴影
里， 并不那么容易理解。

下面我们来看一段代码， 清晰地展示了闭包：
```javascript
function foo() {
	var a = 2;
	function bar() {
		console.log( a );
	}
	return bar;
}
var baz = foo();
baz(); // 2 —— 朋友， 这就是闭包的效果。
```

函数 bar() 的词法作用域能够访问 foo() 的内部作用域。 然后我们将 bar() 函数本身当作
一个值类型进行传递。 在这个例子中， 我们将 bar 所引用的函数对象本身当作返回值。
在 foo() 执行后， 其返回值（ 也就是内部的 bar() 函数） 赋值给变量 baz 并调用 baz()， 实
际上只是通过不同的标识符引用调用了内部的函数 bar()。

bar() 显然可以被正常执行。 但是在这个例子中， 它在自己定义的词法作用域以外的地方
执行。

在 foo() 执行后， 通常会期待 foo() 的整个内部作用域都被销毁， 因为我们知道引擎有垃
圾回收器用来释放不再使用的内存空间。 由于看上去 foo() 的内容不会再被使用， 所以很
自然地会考虑对其进行回收。

而闭包的“ 神奇” 之处正是可以阻止这件事情的发生。 事实上内部作用域依然存在， 因此
没有被回收。 谁在使用这个内部作用域？ 原来是 bar() 本身在使用。

拜 bar() 所声明的位置所赐， 它拥有涵盖 foo() 内部作用域的闭包， 使得该作用域能够一
直存活， 以供 bar() 在之后任何时间进行引用。

bar() 依然持有对该作用域的引用， 而这个引用就叫作闭包。

因此， 在几微秒之后变量 baz 被实际调用（ 调用内部函数 bar）， 不出意料它可以访问定义
时的词法作用域， 因此它也可以如预期般访问变量 a。

这个函数在定义时的词法作用域以外的地方被调用。 闭包使得函数可以继续访问定义时的
词法作用域。

当然， 无论使用何种方式对函数类型的值进行传递， 当函数在别处被调用时都可以观察到
闭包。

```javascript
function foo() {
	var a = 2;
	function baz() {
	console.log( a ); // 2
	} 
	bar( baz );
}
function bar(fn) {
	fn(); // 妈妈快看呀， 这就是闭包！
}
```
把内部函数 baz 传递给 bar， 当调用这个内部函数时（ 现在叫作 fn）， 它涵盖的 foo() 内部
作用域的闭包就可以观察到了， 因为它能够访问 a。
传递函数当然也可以是间接的。

```javascript
var fn;
function foo() {
	var a = 2;
	function baz() {
		console.log( a );
	} 
	fn = baz; // 将 baz 分配给全局变量
}
function bar() {
	fn(); // 妈妈快看呀， 这就是闭包！
} 
foo();
bar(); // 2
```

无论通过何种手段将内部函数传递到所在的词法作用域以外， 它都会持有对原始定义作用
域的引用， 无论在何处执行这个函数都会使用闭包。

### 现在我懂了

前面的代码片段有点死板， 并且为了解释如何使用闭包而人为地在结构上进行了修饰。 但
我保证闭包绝不仅仅是一个好玩的玩具。 你已经写过的代码中一定到处都是闭包的身影。
现在让我们来搞懂这个事实。
```javascript
function wait(message) {
	setTimeout( function timer() {
		console.log( message );
	}, 1000 );
} 
wait( "Hello, closure!" );
```
将一个内部函数（ 名为 timer） 传递给 setTimeout(..)。 timer 具有涵盖 wait(..) 作用域
的闭包， 因此还保有对变量 message 的引用。

wait(..) 执行 1000 毫秒后， 它的内部作用域并不会消失， timer 函数依然保有 wait(..)
作用域的闭包。

深入到引擎的内部原理中， 内置的工具函数 setTimeout(..) 持有对一个参数的引用， 这个
参数也许叫作 fn 或者 func， 或者其他类似的名字。 引擎会调用这个函数， 在例子中就是
内部的 timer 函数， 而词法作用域在这个过程中保持完整。

这就是闭包。
或者， 如果你很熟悉 jQuery（ 或者其他能说明这个问题的 JavaScript 框架）， 可以思考下面
的代码：
```javascript
function setupBot(name, selector) {
	$( selector ).click( function activator() {
		console.log( "Activating: " + name );
	} );
} 
setupBot( "Closure Bot 1", "#bot_1" );
setupBot( "Closure Bot 2", "#bot_2" );
```
我不知道你会写什么样的代码， 但是我写的代码负责控制由闭包机器人组成的整个全球无
人机大军， 这是完全可以实现的！

玩笑开完了， 本质上无论何时何地， 如果将函数（ 访问它们各自的词法作用域） 当作第一
级的值类型并到处传递， 你就会看到闭包在这些函数中的应用。 在定时器、 事件监听器、
Ajax 请求、 跨窗口通信、 Web Workers 或者任何其他的异步（ 或者同步） 任务中， 只要使
用了回调函数， 实际上就是在使用闭包！

第 3 章介绍了 IIFE 模式。 通常认为 IIFE 是典型的闭包例子， 但根据先前对
闭包的定义， 我并不是很同意这个观点。
```javascript
var a = 2;
(function IIFE() {
	console.log( a );
})();
```
虽然这段代码可以正常工作， 但严格来讲它并不是闭包。 为什么？ 因为函数（ 示例代码中
的 IIFE） 并不是在它本身的词法作用域以外执行的。 它在定义时所在的作用域中执行（ 而
外部作用域， 也就是全局作用域也持有 a）。 a 是通过普通的词法作用域查找而非闭包被发
现的。

尽管技术上来讲， 闭包是发生在定义时的， 但并不非常明显， 就好像六祖慧能所说：“ 既
非风动， 亦非幡动， 仁者心动耳。”^ 5。

尽管 IIFE 本身并不是观察闭包的恰当例子， 但它的确创建了闭包， 并且也是最常用来创建
可以被封闭起来的闭包的工具。 因此 IIFE 的确同闭包息息相关， 即使本身并不会真的使用
闭包。

亲爱的读者， 现在把书放下， 我有一个任务要给你。 打开你最近写的 JavaScript 代码， 找
到其中的函数类型的值并指出哪里已经使用了闭包， 即使你以前可能并不知道这就是
闭包。

等你呦！现在你懂了吧！

### 循环和闭包

要说明闭包， for 循环是最常见的例子。
```javascript
for (var i=1; i<=5; i++) {
	setTimeout( function timer() {
	console.log( i );
}, i*1000 );
}
```
>注 5： 原文为 it’s a tree falling in the forest with no one around to hear it， 同六祖慧能的
>风幡之动禅喻近义， 比喻客观存在和观察认知之间的关系。 ——译者注

由于很多开发者对闭包的概念认识得并不是很清楚， 因此当循环内部包含函数定义时， 
代码格式检查器经常发出警告。 我们在这里介绍如何才能正确地使用闭包并发挥它的威力
， 但是代码格式检查器并没有那么灵敏， 它会假设你并不真正了解自己在做什么， 所以
无论如何都会发出警告。

正常情况下， 我们对这段代码行为的预期是分别输出数字 1~5， 每秒一次， 每次一个。
但实际上， 这段代码在运行时会以每秒一次的频率输出五次 6。这是为什么？

首先解释 6 是从哪里来的。 这个循环的终止条件是 i 不再 <=5。 条件首次成立时 i 的值是
6。 因此， 输出显示的是循环结束时 i 的最终值。

仔细想一下， 这好像又是显而易见的， 延迟函数的回调会在循环结束时才执行。 事实上
，当定时器运行时即使每个迭代中执行的是 setTimeout(.., 0)， 所有的回调函数依然是在
循环结束后才会被执行， 因此会每次输出一个 6 出来。

这里引伸出一个更深入的问题， 代码中到底有什么缺陷导致它的行为同语义所暗示的不
一致呢？

缺陷是我们试图假设循环中的每个迭代在运行时都会给自己“ 捕获” 一个 i 的副本。 但是
根据作用域的工作原理， 实际情况是尽管循环中的五个函数是在各个迭代中分别定义的
，但是它们都被封闭在一个共享的全局作用域中， 因此实际上只有一个 i。

这样说的话， 当然所有函数共享一个 i 的引用。 循环结构让我们误以为背后还有更复杂的
机制在起作用， 但实际上没有。 如果将延迟函数的回调重复定义五次， 完全不使用循环，
那它同这段代码是完全等价的。

下面回到正题。 缺陷是什么？ 我们需要更多的闭包作用域， 特别是在循环的过程中每个迭
代都需要一个闭包作用域。

第 3 章介绍过， IIFE 会通过声明并立即执行一个函数来创建作用域。
我们来试一下：
```javascript
for (var i=1; i<=5; i++) {
	(function() {
		setTimeout( function timer() {
			console.log( i );
		}, i*1000 );
	})();
}
```
这样能行吗？ 试试吧， 我等着你。

我不卖关子了。 这样不行。 但是为什么呢？ 我们现在显然拥有更多的词法作用域了。 的确
每个延迟函数都会将 IIFE 在每次迭代中创建的作用域封闭起来。

如果作用域是空的， 那么仅仅将它们进行封闭是不够的。 仔细看一下， 我们的 IIFE 只是一
个什么都没有的空作用域。 它需要包含一点实质内容才能为我们所用。

它需要有自己的变量， 用来在每个迭代中储存 i 的值：
```javascript
for (var i=1; i<=5; i++) {
	(function() {
		var j = i;
		setTimeout( function timer() {
			console.log( j );
		}, j*1000 );
	})();
}
```
行了！ 它能正常工作了！ 。
可以对这段代码进行一些改进：
```javascript
for (var i=1; i<=5; i++) {
	(function(j) {
		setTimeout( function timer() {
			console.log( j );
		}, j*1000 );
	})( i );
}
```
当然， 这些 IIFE 也不过就是函数， 因此我们可以将 i 传递进去， 如果愿意的话可以将变量
名定为 j， 当然也可以还叫作 i。 无论如何这段代码现在可以工作了。

在迭代内使用 IIFE 会为每个迭代都生成一个新的作用域， 使得延迟函数的回调可以将新的
作用域封闭在每个迭代内部， 每个迭代中都会含有一个具有正确值的变量供我们访问。
问题解决啦！

### 重返块作用域

仔细思考我们对前面的解决方案的分析。 我们使用 IIFE 在每次迭代时都创建一个新的作用
域。 换句话说， 每次迭代我们都需要一个块作用域。 第 3 章介绍了 let 声明， 可以用来劫
持块作用域， 并且在这个块作用域中声明一个变量。

本质上这是将一个块转换成一个可以被关闭的作用域。 因此， 下面这些看起来很酷的代码
就可以正常运行了：
```javascript
for (var i=1; i<=5; i++) {
	let j = i; // 是的， 闭包的块作用域！
	setTimeout( function timer() {
		console.log( j );
	}, j*1000 );
}
```
但是， 这还不是全部！ （ 我用 Bob Barker6 的声音说道） for 循环头部的 let 声明还会有一
个特殊的行为。 这个行为指出变量在循环过程中不止被声明一次， 每次迭代都会声明。 随
后的每个迭代都会使用上一个迭代结束时的值来初始化这个变量。
```javascript
for (let i=1; i<=5; i++) {
	setTimeout( function timer() {
		console.log( i );
	}, i*1000 );
}
```
很酷是吧？ 块作用域和闭包联手便可天下无敌。 不知道你是什么情况， 反正这个功能让我
成为了一名快乐的 JavaScript 程序员。

### 模块

还有其他的代码模式利用闭包的强大威力， 但从表面上看， 它们似乎与回调无关。 下面一
起来研究其中最强大的一个： 模块。
```javascript
function foo() {
	var something = "cool";
	var another = [1, 2, 3];
	function doSomething() {
		console.log( something );
	}
	function doAnother() {
		console.log( another.join( " ! " ) );
	}
}
```
正如在这段代码中所看到的， 这里并没有明显的闭包， 只有两个私有数据变量 something
和 another， 以及 doSomething() 和 doAnother() 两个内部函数， 它们的词法作用域（ 而这
就是闭包） 也就是 foo() 的内部作用域。
接下来考虑以下代码：
```javascript
function CoolModule() {
	var something = "cool";
	var another = [1, 2, 3];
	function doSomething() {
		console.log( something );
	}
	function doAnother() {
		console.log( another.join( " ! " ) );
	}
	return {
		doSomething: doSomething,
		doAnother: doAnother
	};
}
var foo = CoolModule();
foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```
>6： Bob Barker 是美国著名的电视节目主持人。 

这个模式在 JavaScript 中被称为模块。 最常见的实现模块模式的方法通常被称为模块暴露，
这里展示的是其变体。

我们仔细研究一下这些代码。

首先， CoolModule() 只是一个函数， 必须要通过调用它来创建一个模块实例。 如果不执行
外部函数， 内部作用域和闭包都无法被创建。

其次， CoolModule() 返回一个用对象字面量语法 { key: value, ... } 来表示的对象。 这
个返回的对象中含有对内部函数而不是内部数据变量的引用。 我们保持内部数据变量是隐
藏且私有的状态。 可以将这个对象类型的返回值看作本质上是模块的公共 API。
这个对象类型的返回值最终被赋值给外部的变量 foo， 然后就可以通过它来访问 API 中的
属性方法， 比如 foo.doSomething()。

从模块中返回一个实际的对象并不是必须的， 也可以直接返回一个内部函
数。 jQuery 就是一个很好的例子。 jQuery 和 $ 标识符就是 jQuery 模块的公
共 API， 但它们本身都是函数（ 由于函数也是对象， 它们本身也可以拥有属
性）。

doSomething() 和 doAnother() 函数具有涵盖模块实例内部作用域的闭包（ 通过调用
CoolModule() 实现）。 当通过返回一个含有属性引用的对象的方式来将函数传递到词法作
用域外部时， 我们已经创造了可以观察和实践闭包的条件。

如果要更简单的描述， 模块模式需要具备两个必要条件。

1. 必须有外部的封闭函数， 该函数必须至少被调用一次（ 每次调用都会创建一个新的模块
实例）。
2. 封闭函数必须返回至少一个内部函数， 这样内部函数才能在私有作用域中形成闭包， 并
且可以访问或者修改私有的状态。
一个具有函数属性的对象本身并不是真正的模块。 从方便观察的角度看， 一个从函数调用
所返回的， 只有数据属性而没有闭包函数的对象并不是真正的模块。
上一个示例代码中有一个叫作 CoolModule() 的独立的模块创建器， 可以被调用任意多次，
每次调用都会创建一个新的模块实例。 当只需要一个实例时， 可以对这个模式进行简单的
改进来实现单例模式：

```javascript
var foo = (function CoolModule() {
var something = "cool";
var another = [1, 2, 3];
function doSomething() {
	console.log( something );
	}
	function doAnother() {
		console.log( another.join( " ! " ) );
	}
	return {
		doSomething: doSomething,
		doAnother: doAnother
	};
})();
foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

我们将模块函数转换成了 IIFE（ 参见第 3 章）， 立即调用这个函数并将返回值直接赋值给
单例的模块实例标识符 foo。

模块也是普通的函数， 因此可以接受参数：
```javascript
function CoolModule(id) {
	function identify() {
		console.log( id );
	}
	return {
		identify: identify
	};
}
var foo1 = CoolModule( "foo 1" );
var foo2 = CoolModule( "foo 2" );
foo1.identify(); // "foo 1"
foo2.identify(); // "foo 2"
```

模块模式另一个简单但强大的变化用法是， 命名将要作为公共 API 返回的对象：
```javascript
var foo = (function CoolModule(id) {
function change() {
	// 修改公共 API
	publicAPI.identify = identify2;
	}
	function identify1() {
		console.log( id );
	}
	function identify2() {
		console.log( id.toUpperCase() );
	}
	var publicAPI = {
		change: change,
		identify: identify1
	};
	return publicAPI;
})( "foo module" );
foo.identify(); // foo module
foo.change();
foo.identify(); // FOO MODULE
```
通过在模块实例的内部保留对公共 API 对象的内部引用， 可以从内部对模块实例进行修
改， 包括添加或删除方法和属性， 以及修改它们的值。


### 现代的模块机制

大多数模块依赖加载器 / 管理器本质上都是将这种模块定义封装进一个友好的 API。 这里
并不会研究某个具体的库， 为了宏观了解我会简单地介绍一些核心概念：
```javascript
var MyModules = (function Manager() {
var modules = {};
function define(name, deps, impl) {
	for (var i=0; i<deps.length; i++) {
		deps[i] = modules[deps[i]];
	} 
		modules[name] = impl.apply( impl, deps );
	}
	function get(name) {
		return modules[name];
	}
	return {
		define: define,
		get: get
	};
})();
```

这段代码的核心是 modules[name] = impl.apply(impl, deps)。 为了模块的定义引入了包装
函数（ 可以传入任何依赖）， 并且将返回值， 也就是模块的 API， 储存在一个根据名字来管
理的模块列表中。

下面展示了如何使用它来定义模块：
```javascript
	MyModules.define( "bar", [], function() {
		function hello(who) {
			return "Let me introduce: " + who;
		}
		return {
			hello: hello
		};
	} );
	MyModules.define( "foo", ["bar"], function(bar) {
		var hungry = "hippo";
		function awesome() {
		console.log( bar.hello( hungry ).toUpperCase() );
		}
		return {
		awesome: awesome
		};
	} );
	var bar = MyModules.get( "bar" );
	var foo = MyModules.get( "foo" );
	console.log(
		bar.hello( "hippo" )
	); // Let me introduce: hippo
	foo.awesome(); // LET ME INTRODUCE: HIPPO
```
"foo" 和 "bar" 模块都是通过一个返回公共 API 的函数来定义的。 "foo" 甚至接受 "bar" 的
示例作为依赖参数， 并能相应地使用它。

为我们自己着想， 应该多花一点时间来研究这些示例代码并完全理解闭包的作用吧。 最重
要的是要理解模块管理器没有任何特殊的“ 魔力”。 它们符合前面列出的模块模式的两个
特点： 为函数定义引入包装函数， 并保证它的返回值和模块的 API 保持一致。

换句话说， 模块就是模块， 即使在它们外层加上一个友好的包装工具也不会发生任何变化。

### 未来的模块机制

ES6 中为模块增加了一级语法支持。 但通过模块系统进行加载时， ES6 会将文件当作独立
的模块来处理。 每个模块都可以导入其他模块或特定的 API 成员， 同样也可以导出自己的
API 成员。

基于函数的模块并不是一个能被稳定识别的模式（ 编译器无法识别）， 它们
的 API 语义只有在运行时才会被考虑进来。 因此可以在运行时修改一个模块
的 API（ 参考前面关于公共 API 的讨论）。

相比之下， ES6 模块 API 更加稳定（ API 不会在运行时改变）。 由于编辑器知
道这一点， 因此可以在（ 的确也这样做了） 编译期检查对导入模块的 API 成
员的引用是否真实存在。 如果 API 引用并不存在， 编译器会在运行时抛出一
个或多个“ 早期” 错误， 而不会像往常一样在运行期采用动态的解决方案。

ES6 的模块没有“ 行内” 格式， 必须被定义在独立的文件中（ 一个文件一个模块）。 浏览
器或引擎有一个默认的“ 模块加载器”（ 可以被重载， 但这远超出了我们的讨论范围） 可
以在导入模块时异步地加载模块文件。
考虑以下代码：
```javascript
bar.js

function hello(who) {
	return "Let me introduce: " + who;
}
export hello;


foo.js
// 仅从 "bar" 模块导入 hello()
import hello from "bar";
var hungry = "hippo";
function awesome() {
	console.log(
		hello( hungry ).toUpperCase()
	);
}
export awesome;


baz.js
// 导入完整的 "foo" 和 "bar" 模块
module foo from "foo";
module bar from "bar";
console.log(
	bar.hello( "rhino" )
); // Let me introduce: rhino
foo.awesome(); // LET ME INTRODUCE: HIPPO
```
需要用前面两个代码片段中的内容分别创建文件 foo.js 和 bar.js。 然后如第三
个代码片段中展示的那样， bar.js 中的程序会加载或导入这两个模块并使用
它们。

import 可以将一个模块中的一个或多个 API 导入到当前作用域中， 并分别绑定在一个变量
上（ 在我们的例子里是 hello）。 module 会将整个模块的 API 导入并绑定到一个变量上（ 在
我们的例子里是 foo 和 bar）。 export 会将当前模块的一个标识符（ 变量、 函数） 导出为公
共 API。 这些操作可以在模块定义中根据需要使用任意多次。

模块文件中的内容会被当作好像包含在作用域闭包中一样来处理， 就和前面介绍的函数闭
包模块一样

### 小结

闭包就好像从 JavaScript 中分离出来的一个充满神秘色彩的未开化世界， 只有最勇敢的人
才能够到达那里。 但实际上它只是一个标准， 显然就是关于如何在函数作为值按需传递的
词法环境中书写代码的。

当函数可以记住并访问所在的词法作用域， 即使函数是在当前词法作用域之外执行， 这时
就产生了闭包。

如果没能认出闭包， 也不了解它的工作原理， 在使用它的过程中就很容易犯错， 比如在循
环中。 但同时闭包也是一个非常强大的工具， 可以用多种形式来实现模块等模式。

模块有两个主要特征：（ 1） 为创建内部作用域而调用了一个包装函数；（ 2） 包装函数的返回
值必须至少包括一个对内部函数的引用， 这样就会创建涵盖整个包装函数内部作用域的闭
包。

现在我们会发现代码中到处都有闭包存在， 并且我们能够识别闭包然后用它来做一些有用
的事！