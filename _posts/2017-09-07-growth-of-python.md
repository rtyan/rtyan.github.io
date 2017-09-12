---
layout: post
title:  疯狂发展的Python
date:   2017-09-11 22:19:02
categories: 翻译
---
#### 原文信息

>原文地址： [https://stackoverflow.blog/2017/09/06/incredible-growth-python/](https://stackoverflow.blog/2017/09/06/incredible-growth-python/)

>原文作者：by David Robinson

>原文发布时间：September 6, 2017

>翻译时间：2017-09-08 ~ 2017年09月12 晚上、中午抽空翻译

我们最近在探索发达国家（可以理解为高收入）相对其他国家如何倾向于不同的技术。我们
发现最大的不同在于编程语言Python。当我们聚焦于发达国家的时候，Python的增长甚至
比一些像`Stack Overflow Trends`这样的工具更多，或者其他全球软件开发的排名。

这篇文章中，我们将探索python这种编程语言在过去5年的非凡成长，通过对Stack Overflow
中发达国家流量的统计。“最快增长“这个概念我们很难精确定义，但我们可以通过例子来证明
Python确实将要成为快速增长的主流语言。

本文所提到的所有数据都是对发达国家而言的。他们通常代表美国、英国、德国、加拿大以及
其他类似国家的趋势，这结合使用了Stack Overflow大约64%的流量。许多像印度、巴西、瑞
士以及中国也对全球软件生态系统做出了巨大的贡献，这篇文章很少讨论他们的经济，尽管我
们也可以看见Python在这些地方增长。

值得一提的是，一种语言使用者的数量不能预估语言的好坏：我们讨论开发者使用的语言，而
不是定义任何事情（彩蛋：我曾经主要使用Python，尽管已经完全转到了R）。

> 这段后半段描述不出作者想要说的东西

#### Python在发达国家的增长
你可以在Stack Overflow Trends中看到Python在过去几年里快速增长。而这篇文章中我们将
聚焦于发达国家，并且只考虑问题浏览，而不是问题提问数（这种趋势会得到类似的结果，
但随着时间的流逝有轻微的误差，尤其对小标签）。

我们有Stack Overflow上面可以追溯到2011年早期的问题浏览数据，我们可以考虑在这个时间
周期中Python相对于其他5中主流语言的增长（注意，因此相对于追溯到2008年的趋势工具，这
儿是相对短的时间范围）。这儿有Stack Overflow中发达国家最高访问标签10个中的6个，其他
4个没包含的是CSS, HTML, Android, and JQuery。

![主流语言的增长]({{ site.url }}/assets/images/posts/growth-of-major-language.png)

2017年6月，Stack Overflow中的Python标签首次成为了发达国家访问最多的标签。在英国、美国
成为了最受欢迎的标签，其他发达国家成为了top2的标签（第一可能是java或者JavaScript）。
这真的让人感慨，在2012年，相对于其他5种语言，他是访问量最少的，现在相对于那个啥时候已经增
长了2.5倍。

另外一部分原因是java周期性的流量变化。自从他成为了本科教学中的重点，java流量趋向于在秋
春季上升，夏季下降，他会在年终的时候重新赶上Python吗？我们可以通过一个叫[STL](http://
otexts.org/fpp2/sec-6-stl.html)的模型尝试预测接下来两年的增长情况，他会结合周期性
的增长情况对未来的值作出预测。

![主流语言未来流量预测]({{ site.url }}/assets/images/posts/projections-feature-values.png)

根据这个模型的预测结果，Python也许会保持领先，或者被Java在秋季赶上（大致根据模型的预测范围）,
但可以清晰的看到在2018年他将正式成为最受欢迎的标签。STL模型也指出java和JavaScript在发达
国家将维持和他们过去2年基本相同的流量。

#### 哪些标签是增长最快的？
上面我们只看到了6种访问最多的编程语言，在其他值得注意的技术中，哪个目前是发达国家增长最快的？

我们用2017年和2016年的比率作为增长率。我们决定在这个分析中，只考虑编程语言（像java和python）
以及平台（像 iOS, Android, Windows和Linux），不考虑像Angular这样的框架或者TensorFlow这
样的类库（尽管他们增长得也很快，也许会在未来的谋篇文章中关注他们）。

如[这篇漫画](https://xkcd.com/1102/)中描述的定义“快速增长”是很有挑战的，所以我们用平均差图
中的整体平均值来类比增长。

> 这篇漫画讲了A和B讨论增宗教人数长率的问题，A有38000人，增长率是85%，B只有一个人，为了超过A
> 他现场叫了C来加入自己的宗教，马上增长率达到了100%。。。。

![主流语言未来流量预测]({{ site.url }}/assets/images/posts/tag-growth-scatter.png)

通过27%的年增长率，Python在占有量大、增速快的位置上独领风骚；类似增长速度的第二大标签是R。
我们可以看到发达国家中其他大标签基本保持稳定，也可以看到Android, iOS和PHP有轻微的下滑。
我们之前调查过一些像Objective-C, Perl和Ruby这些下滑的语言，像[这篇文章](https://stackoverflow.blog/2017/08/01/flash-dead-technologies-might-next/?utm_source=so-owned&utm_medium=blog&utm_campaign=gen-blog&utm_content=blog-link&utm_term=incredible-growth-python)。
我们也可以注意到，函数式编程语言中，Scala是占有量最大并且正在增长的，然而占有量最小的F#和
Clojure正在下滑，在中间的Haskell基本保持稳定占有量。

上面的图中遗漏了一个重要的信息：去年TypeScript相关问题的流量的增长率达到了惊人的142%，
我们去除它为了避免影响其他的显示比例。你也可以看到一些占有量小的语言，有类似、甚至超过
Python的增长速度（比如R, Go和Rust）。还有很多像Swift和Scala这样的标签，他们都有惊人
的增长率，再过一段时间和Python相比他们的流量又如何呢？

![主流语言未来流量预测]({{ site.url }}/assets/images/posts/growth-smaller-tags.png)

像R和Swift这些语言的增长速度确实惊人，TypeScript在短时间内展示了很高的增长速度。许多
占有量比较小的语言几乎是从没有问题流量开始到格外显著的存在于软件生态系统中。但就像图表中
所示，在tag开始相对较小的时候很容易出现高速增长。

注意，我们不是说这些语言和Python占有量具有“竞争”关系。相反，我们再解释为什么分类别对待他们的增长；
这些是较低流量标签的开始。Python是一个不寻常的例子，既是Stack Overflow上访问量最大的标签，也是
增长最快的标签之一（顺便说一句，他还在加速增长！从2013年开始他的年增长率一直在增加）。

#### 其他国家
截至目前，这篇文章都在讨论发达国家的趋势。Python在发展中国家也会有类似的增长速度吗，在类似
印度, 巴西, 瑞士和中国这样的国家？

确实是这样。

![主流语言未来流量预测]({{ site.url }}/assets/images/posts/non-high-income-graph.png)

发展中国家Python仍然是主流语言中增长最快的；他从低占有率开始，两年前开始增长（2014年，不是2012）。
事实上，Python的年增长率在发展中国家比发达国家还要稍稍高一点儿。我们这里不再调查他的原因，但对于R
，[这个语言的使用是和GDP成正相关](https://stackoverflow.blog/2017/08/29/tale-two-industries-programming-languages-differ-wealthy-developing-countries/?utm_source=so-owned&utm_medium=blog&utm_campaign=gen-blog&utm_content=blog-link&utm_term=incredible-growth-python)
的，也会在发展中国家增长。

这篇文章中的关于发达国家的标签增长下降的大多数结论（除绝对排名外）对其他国家也适用，两者的增长率的
相关性达到了0.979。有时候，你可以看见类似与出现在Python上的“落后”现象，发达国家1、2年前已经广泛
采用的技术，在发展中国家才慢慢传播开(这是一个有趣的现象，也许会成为我未来谋篇博客的主题)。
