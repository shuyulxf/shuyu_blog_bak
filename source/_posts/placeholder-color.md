title: placeholder-color
tags: [learning, html]
type: tags
date: 2016-01-13 23:31:01
---
### ** 修改placeholder的颜色 **
chrome、IE、firefox等浏览器的placeholder的默认颜色都是浅灰色，但是略有不同。

要统一个浏览器placeholder的颜色，或者修改其颜色可以用如下方法：
css修改浏览器的placeholder的颜色

::-webkit-input-placeholder { /* Safari, Chrome and Opera */
  color: orange;
}

:-moz-placeholder { /* Firefox 18- */
  color: orange;
}

::-moz-placeholder { /* Firefox 19+ */
  color: orange;
}

:-ms-input-placeholder { /* IE 10+ */
  color: orange;
}

::-ms-input-placeholder { /* Edge */
  color: orange;
}

//当前浏览器并未实现
:placeholder-shown { /* Standard one last! */
  color: orange;
}