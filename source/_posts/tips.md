title: tips
tags: [learning,js]
type: tags
date: 2016-02-29 22:03:00
---
### ** 函数 **
1. 返回值
(1) 没有返回数值   返回undefined
(2) 有返回值       返回return语句后边的值
(3) 与new前缀一起调用  如果没有return语句或者return语句后边返回的不是对象，则返回this(即返回刚构造的对象)

2. 变量提升


	var foo = 1;
	function x(){
	    alert(foo);
	    var foo = 2; 
	    alert(foo);
	}


相当于

	var foo = 1;
	function x(){
		var foo;
	    alert(foo);
	    foo = 2; 
	    alert(foo);
	}
<!--more-->

### ** 错误处理 **
throw {} 抛出一个异常,是一个Exception对象，如果没有被try语句捕获，则会被浏览器抛出

	try(){
		add();
	} catch(e){
		console.log(e);
	}

try语句异常处理，可以捕获语句内部自定义抛出的异常，也可以捕获因为语法错误等自动抛出的异常。只要捕获到一个异常便会跳转到catch语句执行。如果捕获到的是自动抛出的异常，则会有固定格式的提示信息。

### ** 对象直接量的处理 **
程序运行时每次遇到对象直接量都会创建新对象。如{},[]等。
但RegExp直接量的处理与此有出入。ECMAscript3规定一段程序中返回同一个实例，但ECMAscript5则规定返回不同的实例。低版本的firefox(4以下)遵循ECMAscript3，而高版本都开始遵循ECMAscript5。ie低版本一直没有遵循ECMAscript3，在遵循ECMAscript5。

### ** 数组 **
1. 清空数组
(1) length赋值为0
	var a = [1,2]; a.length = 0; console.log(a); //输出[]
(2) 赋值一个新的空对象
	var a = [1,2]; a = []; console.log(a); //输出[]

相比第一种方式，第二种方式的效率更高一些。而且第一种方式其实并不会被垃圾回收。

	Exp:
	var ary = [1,2,3,4];
	var ary2 = ary;
	ary = []; 
	console.log(ary2); //输出[1,2,3,4]





