title: layout
tags: [learning, css]
type: tags
date: 2016-03-03 22:46:55
---
### ** 浮动元素居中 **

	<style>
		.F_W,.F{
			position: relative;
			float: left;
		}
		.F_W{
			left: 50%;
		}
		.F{
			left: -50%;
		}
	</style>
	<div class="F_W">
		<div class="F">
			sfsdfdsfdddddddddddddddd
		</div>
	</div>

[例子](http://jsbin.com/fagebimige/edit?html,css,output)
<!-- more -->
### ** make floated div 100% height of its parents **
	
	<style>
		.H{
			overflow: hidden;
			border:2px solid yellow;
		}
		.A{
			margin-right: 10px;
			margin-bottom: -1000px;
			padding-bottom: 1000px;
		}
		.A,.B{
			float: left;
			background: blue;
			border:2px solid red;
		}
		.B{
			height: 400px;
			width: 200px;
		}
	</style>
	<div class="H">
		<div class="A">
			A
		</div>
		<div class="B">B</div>
	</div>

父级元素H, B的高度固定，为了使A元素的高度和父元素的高度相同，通过添加margin-bottom：-1000px,padding-bottom: 1000px;两个css属性。[例子](http://jsbin.com/tupusimanu/edit?html,css,output)

同时, 可以使用position:absolute来实现相同的排版。
元素height百分比值的计算取决于它的containing box。

    #outer {
        position:absolute; 
        height:auto; width:200px; 
        border: 1px solid red; 
    }
    #inner {
        position:absolute; 
        height:100%; 
        width:20px; 
        border: 1px solid black; 
    }
    <div id='outer'>
	    <div id='inner'>
	    </div>
    text
	</div>
[例子](http://jsbin.com/qilerumoga/edit?html,css,output)


