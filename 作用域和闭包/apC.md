尽管这个话题在任何细节上并没有处理this的机制，但是有一个关于ES6的主题，一个能讲述this在词法作用域上以重要的方式，这个我们可以快速的审查看看。
&nbsp;

ES6为函数的声明增加了一个特殊的语法形式，叫做“箭头”，看起来像下面的样子：

		var foo = a => {	
			console.log(a);
		};

		foo(2); //2

这个所谓的“胖箭头”就是经常被提到的作为函数中的关键字来替代冗长的function函数。
&nbsp;

但是这会有更多重要的一些问题会出现，就在使用箭头函数时候去掉其他按键的数量在你的函数声明中。
&nbsp;

简单来说，这个代码带来一个问题：

		var obj = {
			id: "awesome",
			cool:function coolFn(){
				console.log(this.id);
			}
		};
		
		var id = "not awesome";

		obj.cool();// awesome

		setTimeout(obj.cool, 100);// not awesome

这个问题是由于在cool的函数上少了this的绑定，也有很多不同的方法来处理这样的问题，但是一个经常重复的解决办法是var self = this;。
&nbsp;

那样看起来会是这样：

		var obj = {
			count: 0,
			cool: function coolFn() {
				var self = this;
				if (self.count < 1) {
					setTimeout(function timer(){
						self.count++;
						console.log("awesome?");
					}, 100);
				}
			}
		};

		obj.cool();// awesome?

这里并没有加上多余的代码，这个var self = this解决办法并不需要理解和正确使用this的绑定，也不需要回过头来处理一些事情，我们也许会更舒服的使用词法作用域。
&nbsp;

self也就成为了一个标识符来解决词法范围和闭包的问题，并且也不需要关注更多发生在this的绑定上。
&nbsp;

人们不喜欢编写冗长的东西，特别是他们重复做这件事的时候。
&nbsp;

所以，ES6就有了一个缓解这个情况的动机，在一定意义上也解决常见的词法问题，就像这个。
&nbsp;

ES6的解决办法，就是这个箭头函数，介绍了一个叫做“this 词法”的形式。

		var obj = {
			count: 0,
			cool: function coolFn() {
				if (this.count < 1) {
					setTimeout( () => {
						this.count++;
						console.log("awesome?");
					}, 100);
				}
			}
		};

		obj.cool();// awesome?

这个短的解释就是那个箭头函数没有像大多数普通的函数一样，当需要使用this的绑定来解决问题的行为。
&nbsp;

它抛弃了那些函数普通通过this绑定的规则，代替的是使用箭头函数会在闭包作用域中的this作用立即产生，不论它是什么。
&nbsp;

所以，在这段，这个箭头函数并没有拿到在一些不可预知的情况下this的未绑定，这就是在cool()函数上“继承”this的绑定。（这是正确的，如果我们调用它，就如图显示）
&nbsp;

在这段短的代码操作中，我的观点是箭头函数功能经常会在语法上被篡改，这是一个常见的错误对于开发者来说，是一个在“this 绑定”的规则和“词法作用域”的规则上的混淆。
&nbsp;

换一种方式，为什么要采用这种麻烦和冗长的编码风格，只有把它踩在脚下，并且将它和词法进行参考，这似乎是自然的方式或者其他给定的代码，而不是将它混合在同一块代码中。
&nbsp;

备注：还有一个箭头函数功能是它们都是匿名的。查看原因见第3章，匿名函数比命名函数不太可取。
&nbsp;

我的观点，一个更合适的解决办法，是正确使用这个this机制。

		var obj = {
			count: 0,
			cool: function coolFn() {
				if(this.count < 1){
					setTimeout(function timer(){
						this.count++;//`this` is safe because of `bind(...)`
						console.log("more awesome");
					}.bind(this), 100);//look, `bind()`!
				}
			}
		};

		obj.cool();//more awesome

无论你喜欢箭头函数功能新的this词法的形式，还是喜欢可靠的bind(),重要的是注意箭头函数功能不仅仅是类型上的“函数”。
&nbsp;

它们有一个故意的行为差异，我们应该学习和理解，如果我们这样选择。
&nbsp;

现在，我们完全理解了词法作用域和闭包，理解this词法就应该是容易的事了。