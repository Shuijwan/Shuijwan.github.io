---
layout: post
title:  "基于Flexbox的布局框架(二) - Flexbox布局"
date:   2017-01-15 10:34:01 +0800
categories: android ui
---
上一篇提到了，ios跟android的布局两端需要统一，这就需要一个标准的布局标准. Flexbox布局是w3c提出的用来进行css布局的一套新的方案。
定义了一套标准在横轴跟纵轴方向排列元素，结合下面的资源看一会就明白了。
网上有很多文章介绍语法，这里直接列出了网上的一些资源:

1. [Flex布局教程：语法篇][flexbox语法]
2. [Flex布局教程: 实例篇][flexbox示例]
3. [a-visual-guide-to-css3-flexbox-properties][flexbox guide]
4. [a-guide-to-flexbox][guide to flexbox]

在Android平台上，google也有一个开源的实现，[flexbox-layout][flexbox-layout github]，有5000多个star，看起来很流行的样子。
在框架实现的第一版中，我就采用了这个layout来作为布局引擎。在使用过程中也发现了挺多问题，尤其是在嵌套比较深的情况(作为list的ItemView时)，会发现性能有点差，
list滑动的时候跟用RelativeLayout实现的item比起来还是不那么流畅，虽然我自己也做了一些优化，但是考虑到其他因素(过滤器，数据绑定)，最后在2.0版本中还是抛弃了google的这个实现。当然google在开发这个layout的时候，应该也不是为了完全替代其他的layout(它自己的demo中就是跟其他layout混用的)，所以只能说flexbox-layout不适合我们这个场景。

在React-native和Weex中，底层的布局引擎用的都是facebook的[css-layout][yoga github]，这是一个跨平台布局引擎，而且最重要的是它是独立的一个引擎，跟各个平台的UI系统没有一丁点关系，这样的话，我们就可以做很多优化，比如在后台线程进行预先layout，等layout完成了再切换到主线程进行渲染，这样就大大提高了性能。最近facebook用c语言重写了css-layout,在不同平台上有对应的wrapper，改名叫yoga，不仅真正实现了不同平台一样的算法，而且性能得到更一部提升。我们这个框架在2.0版本中也进行重构，底层就是采用yoga来进行布局。

yoga中最主要的一个概念就是YogaNode,一个布局就是由各个元素对应的YogaNode组成的树，通过给每个YogaNode设置相应的属性，最终调用YogaNode.calculateLayout，就能得到各个YogaNode的坐标跟宽高，而且引擎做了各种优化来尽量减少不必要的measure。下面是一个github的一个测试用例，感兴趣的可以自己去github或者[官网][yoga website]以及[java api][yoga java api]上看。

{% highlight java %}
@Test
  public void testMeasure() {
    final YogaNode node = new YogaNode();
    node.setMeasureFunction(new YogaMeasureFunction() {
        public long measure(
            YogaNodeAPI node,
            float width,
            YogaMeasureMode widthMode,
            float height,
            YogaMeasureMode heightMode) {
          return YogaMeasureOutput.make(100, 100);
        }
    });
    node.calculateLayout();
    assertEquals(100, (int) node.getLayoutWidth());
    assertEquals(100, (int) node.getLayoutHeight());
  }
{% endhighlight %}


上一篇:
[基于Flexbox的布局框架(一)][part1]
下一篇:
[基于Flexbox的布局框架(三) - 异步Layout][part3]

[flexbox语法]:http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool
[flexbox示例]:http://www.ruanyifeng.com/blog/2015/07/flex-examples.html
[flexbox guide]:https://scotch.io/tutorials/a-visual-guide-to-css3-flexbox-properties
[guide to flexbox]:https://css-tricks.com/snippets/css/a-guide-to-flexbox/
[flexbox-layout github]:https://github.com/google/flexbox-layout/
[yoga github]:https://github.com/facebook/yoga
[yoga website]:https://facebook.github.io/yoga/
[yoga java api]:https://facebook.github.io/yoga/docs/api/java/
[part1]:https://shuijwan.github.io/android/ui/2017/01/14/基于Flexbox的布局框架(-).html
[part3]:http://jekyllrb.com/docs/home
