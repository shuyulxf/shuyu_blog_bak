title: change/input/propertychange-event
tags: [learning, event]
type: tags
date: 2016-01-09 23:11:13
---
### change input propertychange事件的详细说明
** change **
change事件主要作用于表单元素，text、textarea、checkbox、radio、select
1. text和textarea元素触发的条件：
在添加和删除内容之后，失去焦点，该事件触发。
2. checkbox、radio、select元素触发条件
click该元素的选项，并且在点击之后改变了该表单元素的值。

<!-- more -->
** input **
input事件用于除ie以外的其他浏览器，并且作用于text、textarea、select元素，但对checkox和radio元素无效。
该事件实时监听以上元素的内容的改变，当内容实时发生改变的时候该事件触发。
但以下情况是不能触发的：

** propertychange **
该事件作用和input功能一样，只是它用于ie浏览器。和input不同之处在于，该事件可以用于checkbox和radio元素，而radio元素触发该事件时，每次都会连续两次调用回调函数。
不同版本的ie对该事件的实现程度不同(bug)：
ie10以上版本和ie8: 该事件的实现和input的实现程度几乎相同，可以实现在输入内容、删除内容、粘贴删除（快捷键或者右键）
ie9:基本的输入内容和粘贴都可以实现，但是任何形式的删除均有问题。

** input propertychange事件失效的情况**
1. 在脚本中改变元素的value值
2. 从浏览器默认的下拉提示菜单中选取的元素

** input propertychange事件和事件keyup/keydown/press相比优势 **
后者在实现鼠标右键粘贴删除、拖拽、长按键（按住键盘不放）的时候不够完善。