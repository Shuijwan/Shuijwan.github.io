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

### Layout
android里面View的layout过程分为onMeasure -> onLayout -> onDraw，并且都是在主线程中完成，如果View层次比较深，这些计算过程就会耗费比较多的cpu，从而影响性能。

之前提到facebook的yoga是一个独立的layout引擎，因此可以在任何线程执行，如果在List中当要显示某个ItemView时，这个ItemView里的元素都已经设置了最终的坐标与宽高，那么会大大减少在主线程中进行measure和layout的时间。

### List
我们可以监听List的onScroll回调，从而判断当前的滚动方向，来提前获取数据，给Item的各个元素设置Flexbox属性后，调用YogaoNode.calculateLayout方法来得到这个ItemView各个元素的坐标和大小，方便后续进行onMeasure和onLayout时，可以传固定的数值。

### Text
文本与其他控件的区别是，文本的大小是由字体的内容，字体大小，行间距，最大行数等最终确定的。Android里面有一个StaticLayout类就是专门用来计算文本大小的，TextView里面用的也是StaticLayout,因此我们在后台线程计算Text的大小时，可以构造StaticLayout，然后通过getWidth和getHeight得到最终的大小，另外StaticLayout有一个draw方法，因此我们在绘制Text时，可以直接调用StaticLayout.draw(Canvas)方法来直接进行绘制。

### 减少View层级, OverDraw


上一篇:
[基于Flexbox的布局框架(二) - Flexbox布局][part1]
下一篇:
[基于Flexbox的布局框架(四) - 异步Layout(Text, Image, List)][part3]


[part1]:https://shuijwan.github.io//android/ui/2017/01/15/基于Flexbox的布局框架(二)-Flexbox布局.html
[part3]:https://shuijwan.github.io/android/ui/2017/01/19/基于Flexbox的布局框架(三)-异步Layout(Text,Image,List).html
