title: doctype
tags: [learning, html]
type: tags
date: 2016-02-21 21:19:08
---
浏览器的渲染模式可以说有三种：标准模式、准标准模式（含较多怪异模式的废弃）、怪异模式。各浏览器为了兼容一些老的站点，均会提供两种模式。但现在的浏览器都不能通过操作浏览器来改变文档的渲染模式。为了得到浏览器文档模式，可以用 document.compatMode来获取。这个属性共有两个值 BackCompat 和CSS1Compat。前者为怪异模式，后者为标准模式。 怪异模式与标准模式的差异主要体现在 盒模型（box model）、表格单元格高度的处理等。 例如IE的怪异模式中，元素的width包含了padding和border，而标准模式中padding和border 并不属于宽度的一部分。
ie6以下的浏览器均运行在怪异模式。而ie7以上的浏览器可以运行怪异模式和标准模式，包括ie7标准模式，和ie5怪异模式。
ie8用4种模式，ie5.5怪异模式，ie7标准模式，ie8标准模式，ie8准标准模式
ie9有7中模式，ie5.5怪异模式，ie7标准模式，ie8标准模式，ie8准标准模式，ie9标准模式，ie9怪异模式，xml模式
触发标准模式的方法：

	1. 添加<!DOCTYPE html>
	2. X-UA-Compatible（ie8以后）来设置 （添加meta标签，http header中设置，但页面中的meta标签优先级要高于http header）

Firefox、Safari、Chrome和Opera中，HTTP头设置了MIME-type(Content-Type为 application/xhtml+xml时)，会触发XML模式。在XML模式中，浏览器会严格地以XML解析XHTML文档

为了解决低版本ie浏览器的兼容问题（当然最简单的方法是升级浏览器，但有些企业浏览器版本受电脑操作系统的限制），所以，必须要用其他的解决方法。在技术不能解决浏览器兼容问题的时候，可以采用检测浏览器版本，如果太低，跳转到浏览器下载页面。还可以使用google chrome frame来解决。在meta标签中使用 IE=Edge,chrome=1表示使用ie浏览器的最高版本的内核来渲染页面，如何支持google chrome frame则用它来渲染。
firefox、chrome、opera、ie等浏览器工作的模式可以用document.compatMode来判定。这个属性两种可能的返回值：BackCompat和CSS1Compat，前者为怪异模式，后者为标准模式。
参考文章：
http://www.cnblogs.com/yuzhongwusan/archive/2012/02/17/2355695.html http://blog.csdn.net/freshlover/article/details/11616563 http://stackoverflow.com/questions/627097/how-to-tell-if-a-browser-is-in-quirks-mode
