title: CSS常见错误
date: 2016-03-19 11:35:19
tags:
  - CSS坑
  - CSS+DIV布局
categories:
  - 我踩过的坑
  - 日常总结
---
<Excerpt in index | 首页摘要>
CSS+DIV是网站标准（或称“WEB标准”）中常用的术语之一，通常为了说明与HTML网页设计语言中的表格（table）定位方式的区别，因为XHTML网站设计标准中，不再使用表格定位技术，而是采用css div的方式实现各种定位。应用应用DIV+CSS编码时很轻易犯一些错误。本文列举了一些常见的错误。
<!-- more -->
<The rest of contents | 余下全文>
### 检查HTML元素标签是否有拼写错误、是否忘记结束标记
即使是老手也经常会弄错div的嵌套关系。可以用dreamweaver的验证功能检查一下有无错误。

###  检查CSS是否正确
检查一下有无拼写错误、是否忘记结尾的 } 等。可以利用CleanCSS来检查 CSS的拼写错误。CleanCSS本是为CSS减肥的工具（css压缩），但也能检查出拼写错误。

### 确定错误发生的位置
假如错误影响了整体布局，则可以逐个删除div块，直到删除某个div块后显示恢复正常，即可确定错误发生的位置。

### 利用border属性确定出错元素的布局特性
使用float属性布局一不小心就会出错。这时为元素添加border属性确定元素边界，错误原因即水落石出（也叫浮动产生，了解如何清除css浮动）。

### float元素的父元素不能指定clear属性
MacIE下假如对float的元素的父元素使用clear属性，四周的float元素布局就会混乱。这是MacIE的闻名的bug，倘若不知道就会走弯路。

### float元素务必指定width属性
很多浏览器在显示未指定width的float元素时会有bug。所以不管float元素的内容如何，一定要为其指定width属性。

### float元素不能指定margin和padding等属性
IE在显示指定了margin和padding的float元素时有bug。因此不要对float元素指定margin和padding属性(可以在float元素内部嵌套一个div来设置margin和padding)。也可以使用hack方法为IE指定非凡的值。

### float元素的宽度之和要小于100%
假如float元素的宽度之和正好是100%，某些古老的浏览器将不能正常显示。因此请保证宽度之和小于99%。

### 是否重设了默认的样式?
某些属性如margin、padding等，不同浏览器会有不同的解释。因此最好在开发前首先将全体的margin、padding设置为0、css列表样式设置为none等。

### CSS常见问题
> * 选择器问题
> * 继承问题
> * 覆盖问题
> * 缺少依赖样式（改天重点总结下）