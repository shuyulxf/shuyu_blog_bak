title: 论css's blocks
tags: [learning, css]
type: tags
date: 2016-03-04 17:22:03
---
### ** containing block(包含块) **
包含块可以是行内元素，也可以是块级元素，为其子元素位置和大小的计算提供参考。具体定义如下：
1. 对于root element(文档结构的根节点，在(x)html文档中是html)所在的包含块叫做初始包含块。对于连续媒体，该包含块的大小和viewport(视口)一样，而且固定在画布的起点。对于分页媒体，direction(direction属性指定了块的基本书写方向，它还规定了表格列布局的方向、水平溢出的方向等)属性的值与root element一样。如body的包含块是初始包含块。

2. 对于除root Element外的元素，如果该元素的定位为"relative"或"static"，则包含块是离该元素最近的祖先节点。

3. 如果元素是"fixed"定位的，则包含块是视口，如果是分页媒体，那么包含块是页面区域(page area).

4. 如果元素是绝对定位的，包含块是离他最近的不是"static"定位的祖先节点。
   * 如果该祖先节点为行内元素，则包含块是由该元素形成的多个行内框(inline box)形成的边界框(bounding box)组成。[例子](http://jsbin.com/yimawowodi/1/edit?html,css,output)
   * 否则，包含块是祖先节点，且祖先节点的box-sizing为padding-box。[例子](http://jsbin.com/nameqoqumu/edit?html,css,output)
如果没有找到相应的祖先节点，则包含块是初始包含块。

发现浮动元素的containing block，在计算float元素的百分比高度的时候，如果最近的块级祖先节点定义了高度，则是参照该元素来定义高度的。如果最近的祖先节点并没有定义高度，则float元素的百分比高度计算不是参照该祖先节点的。[粒子](http://jsbin.com/viboxewuxe/edit?html,css,output) 对比[例子](http://jsbin.com/roturedogu/2/edit?html,css,output)
<!--more-->
### ** padding boxes **


<!-- more -->
continuous media	连续媒体
paged media		分页媒体
bounding box 边界框
padding boxes
border boxes
content boxes
inline boxes 