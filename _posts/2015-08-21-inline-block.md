---
layout: post
title: 从inline-block到line-height到vertical-align
---

&ensp;&ensp;&ensp;&ensp;又有东西可以写的,为何又是样式相关的,其实我很想写点其他的啊啊啊.
&ensp;&ensp;&ensp;&ensp;最近做公司招聘网站被设计给虐惨了,一方面是设计太年轻太严谨了,一方面自己做的也不好,好一些图标的垂直对齐都用像素调整,导致被设计师贼亮的眼睛发现好多个一两个像素的差距,最终导致我需要改动好多.之前以为vertical-align效果不同浏览器会有差异所以没怎么用,但现在我已追悔莫及~~让我先从inline-block讲起吧

### inline-block

&ensp;&ensp;&ensp;&ensp;一些line box什么的就不说了,直接切入问题.举一个栗子可能很多人都遇到类似情况:一个div大小为20px,里面两个inline-block元素,一个不含文本,宽度高度均20px,另一个line-height为20px,包含文本,然后我们会发现div容器是不止20px高的[看例1](#eg-link).
&ensp;&ensp;&ensp;&ensp;这是什么原因呢,其实是inline-block元素的垂直对齐引起的.inline box垂直方向上默认是与其baseline对齐的，而它的baseline又是和包裹它的line box的baseline对齐的。而当一个inline box里面没有包含文本的时候，没有了baseline，没有基准可以对齐,所以定了一个妥协的规则，就是把该box的底部对准包裹它的line box的baseline。而baseline又是在文本的底线上面的,所以整体会把容器给撑高了.
&ensp;&ensp;&ensp;&ensp;那么问题来了,linebox的baseline又是怎么定的呢,很简单,它包裹的带文本的所有inline box,高度最高的那个的baesline就是linebox的baesline,当然这个高度不是指盒子高度,而是指inline box的line-height撑高的高度.这里要注意的是,多行文本的inline box的baseline是以最后一行为基准的[看例2](#eg-link).而如果没有包含文本,就是以自身给定的line-height来决定[看例3](#eg-link).最后,如果包含文本,本身也设置了line-height,那就哪个高听哪个.

### line-height

&ensp;&ensp;&ensp;&ensp;再来看看line-height,我还是搬砖就行吧:[戳我](http://www.zhangxinxu.com/wordpress/2009/11/css%E8%A1%8C%E9%AB%98line-height%E7%9A%84%E4%B8%80%E4%BA%9B%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%E5%8F%8A%E5%BA%94%E7%94%A8/)
&ensp;&ensp;&ensp;&ensp;总结为一句:line-height撑高容器而不是文字撑高容器,line-height垂直平分线始终与文字的垂直平分线重合。

### vertical-align

&ensp;&ensp;&ensp;&ensp;再来看看vertical-align,我还也是搬砖就行吧:[戳我](http://www.zhangxinxu.com/wordpress/2010/05/%E6%88%91%E5%AF%B9css-vertical-align%E7%9A%84%E4%B8%80%E4%BA%9B%E7%90%86%E8%A7%A3%E4%B8%8E%E8%AE%A4%E8%AF%86%EF%BC%88%E4%B8%80%EF%BC%89/)
&ensp;&ensp;&ensp;&ensp;总结为两句:vertical-align只对inline或inline-block元素有效，~~是相对于父元素的line-height垂直对齐~~,实验发现是子元素竖直方向中点与父元素font-size值对应的base-line对齐。vertical-align:text-bottom是相对于父元素文本底部，vertical-align:bottom是相对于父元素line-height底部。
&ensp;&ensp;&ensp;&ensp;各个基线可以参考:
<img src="http://2.bp.blogspot.com/-sthQ7pdqw0k/UENQUh1ei-I/AAAAAAAAALk/jb1Ir8dAF6o/s400/Vertical%2BAlign_crop.png">

&ensp;&ensp;&ensp;&ensp;貌似有点虎头蛇尾了呢,但别人确实写的不错,就不重复造轮子咯.

#### <a href="http://jsfiddle.net/plusice/wg08hfmq/2/" id="eg-link">栗子</a>

***
(2015-08-31补充)
今天发现css大神张鑫旭新写的一篇博客跟我这里内容类似,不过很详细,内容更多(虽然比较啰嗦哈哈):<a href="http://www.zhangxinxu.com/wordpress/2015/08/css-deep-understand-vertical-align-and-line-height/" id="eg-link">戳我吧</a>.文中的"基本现象衍生：垂直居中"让我顿悟.
