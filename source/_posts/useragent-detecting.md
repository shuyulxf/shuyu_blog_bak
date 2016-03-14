title: UA sniffing
tags: [learning, 浏览器兼容]
type: tags
date: 2015-12-22 08:52:54
---
UA sniffing并不是在所有的情况下，使用UA sniffing都是最好的选择，应该尽量使用特性检测。
UA检测的目的是实现优雅降级和渐进升级设计的一种手段。可以在不同的浏览器中使用不同的WEB特性，提供不同程度的用户体验。但是UA检测并不能完美的解决以上问题。也就是说，通过识别浏览器类型和版本来判定是不是支持某种WEB特性是不准确的。有时候用户会主动禁用某些特性，但是通过UA检测我们并不能知道。所以最好的方式是使用特性检测。这些并不依赖于浏览器。