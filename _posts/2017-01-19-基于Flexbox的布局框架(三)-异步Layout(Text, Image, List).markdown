---
layout: post
title:  "基于Flexbox的布局框架(三) - 异步Layout(Text, Image, List)"
date:   2017-01-19 10:34:01 +0800
categories: android ui
---
学会了Flexbox布局之后，这篇来看一下在List中如何做异步layout来提高性能。List可以说是android开发过程中碰到的最常用的控件，一个流畅的列表会让用户有愉悦的浏览体验。在Android中做UI优化大概分几个方面:
1. 减少view层级，减少measure, layout时间
2. 处理overdraw问题
3. 在getView中重用convertView, 用ViewHolder快速查找等等。

下面就分别就上述几个方面针对Text，Image，List如何做优化:

##Layout


##减少View层级


上一篇:
[基于Flexbox的布局框架(二) - Flexbox布局][part1]
下一篇:
[基于Flexbox的布局框架(四) - 异步Layout(Text, Image, List)][part3]

[flexbox语法]:http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool
[flexbox示例]:http://www.ruanyifeng.com/blog/2015/07/flex-examples.html
[flexbox guide]:https://scotch.io/tutorials/a-visual-guide-to-css3-flexbox-properties
[guide to flexbox]:https://css-tricks.com/snippets/css/a-guide-to-flexbox/
[flexbox-layout github]:https://github.com/google/flexbox-layout/
[yoga github]:https://github.com/facebook/yoga
[yoga website]:https://facebook.github.io/yoga/
[yoga java api]:https://facebook.github.io/yoga/docs/api/java/
[part1]:https://shuijwan.github.io//android/ui/2017/01/15/基于Flexbox的布局框架(二)-Flexbox布局.html
[part3]:https://shuijwan.github.io/android/ui/2017/01/19/基于Flexbox的布局框架(三)-异步Layout(Text,Image,List).html
