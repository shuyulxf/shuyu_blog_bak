title: 函数中this的指代
tags: [learning,js]
type: tags
date: 2016-02-27 18:36:52
---
this的指代和函数调用时的上下文有关，this的取值取决于调用时的模式
共有五种调用模式：方法调用模式、函数调用模式、构造器调用模式、apply调用模式和event handler模式。
<!-- more -->
** 非匿名函数的调用 **
一般函数在执行时会自动获取两个特殊参数：this和arguments，arguments是一个类数组的对象，包含着传入函数中所有参数，this引用的是调用该函数的对象。也即函数调用的上下文。
方法调用模式：
<pre>
	var obj = {
		x：'My obj',
		getX: function(){
			console.log(this);
		}
	}
	console.log(obj.getX());//输出为Obj
</pre>
此时，this到对象的绑定发生在方法调用的时候。这个“超级”延迟绑定（very late binding）使得函数可以对this高度复用。通过this可取得它们所属对象的上下文的方法称为公共方法。<br />
函数调用模式：
<pre>
	function getX(){
		console.log(this);
	}
	console.log(getX());//输出为Window,因为相当于调用Window.getX();
</pre>
函数调用模式，不管函数定义在什么作用域内，this绑定的对象，始终为window对象。
<pre>
	var obj = {
		x: 1,
		getX: function(){
			function a(){
				console.log(this);
			}
			a();
		}
	}
	obj.getX(); //输出window
</pre>
<pre>
    var x = 'The window';
	var obj = {
	    x: 'My object',
	    getX: function() {console.log(this);
	        return function() {
	            return this.x;
	        };
	    }
	};
	console.log(obj.getX()());//输出为The Window
</pre>
为了使以函数调用方式调用的函数，共享方法对象的访问权，需要保存一个方法对象this的值。
<pre>
	var obj = {
		x: 1,
		getX: function(){
			var that = this;
			function a(){
				console.log(that);
			}
			a();
		}
	}
	obj.getX(); //输出obj
</pre>
构造器调用模式:
用new关键字来调用。此时，this指向生成的新对象。
<pre>
	varx = 2;
	function a(){
		this.x = 1;
	}
	var obj = new a();
	console.log(obj.a);//1
</pre>
apply调用模式:
apply接收两个参数，一个是this要绑定的对象，另一个是参数列表，数组表示,函数运行时赋给函数的arguments参数。this指向第一个参数
<pre>
	var sayObj = function(){
		console.log(this);
	}
	var obj = {};
	sayObj.apply(obj);   //{}
	sayObj.apply(["1"]); //["1"]
</pre>
call与apply类似，只是参数形式不同，call一个参数也是this要绑定的对象。但剩下的参数是一一列出，不是数组形式列出。<br />
bind:
<pre>
	this.x = 9; 
	var module = {
	  x: 81,
	  getX: function() { return this.x; }
	};

	module.getX(); // 81

	var retrieveX = module.getX;
	retrieveX(); // 9, because in this case, "this" refers to the global object

	// Create a new function with 'this' bound to module
	//New programmers (like myself) might confuse the global var getX with module's property getX
	var boundGetX = retrieveX.bind(module);
	boundGetX(); // 81
</pre>
apply,call,bind均为Function.prototype的属性方法。
Event handler模式：
this指代触发回调函数的对象。

** 匿名函数中this指代 **
匿名函数的上下文一般为全局的，所以this指代window对象。
<pre>
	(function(){console.log(this)})();//window
	var obj = {
		x:1,
		getX:function(){
			(function(){console.log(this);})(); //window,其实相当于函数调用模式
		}
	}
</pre>