###1.1. H5新增的标签

结构类标签：

替代`<div>`标签的各种语义化标签。用以把以前被各种各样的`<div>`加各种class来划分页面区域。

整个页面的每个区域，称作一个Section，这些标签也就给每个section赋予语义，也就是所谓的语义化。

+ `<section>`标签：表示页面中一个内容区间（也就是一个Section），比如章节、页眉、页脚等等。

+ `<article>`标签：表示页面中一块与上下文不相关的区域，如博客中的一篇文章的区域等。

+ `<aside>`标签：表示article标签内容之外，与article内容相关的的辅助信息。通常来说，用来划分Sidebar区域，也就是W3C里所说的“[a section of a page that consists of content that is tangentially related to the content around the aside element, and which could be considered separate from that content](http://www.w3.org/html/wg/drafts/html/master/sections.html#the-aside-element)”（老实说，看不懂，切线相关的article以外的标签是什么？或许[MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/aside)更好看点）

+ `<header>`标签：表示整个页面的标题，或者是页面的一个区域（Section）的标题。

+ `<footer>`标签：表示整个页面的注脚，或者页面的一个区域（Section）的注脚。

+ `<nav>`标签：表示页面中导航的区域。在W3C上也有不少[奇怪的东西](http://www.w3.org/html/wg/drafts/html/master/sections.html#the-nav-element)。

+ `<hgroup>`标签：对整个页面或页面中的一个内容区域里面的标题进行组合。

+ `<figure>`标签：表示一段独立的流内容，一般表示文档主体流内容中的一个独立单元。使用`<figcaption>`标签为`<figure>`标签组添加标题。

媒体化标签：

+ `<video>`标签：定义视频流。

+ `<music>`标签：定义音频流。

+ `<embed>`标签：插入各种多媒体，比如midi、wav、aiff、au、mp3等。大致等于以前的`<object>`标签吧。

+ `<source>`标签：为媒体标签（比如music、video等标签，具体有哪些还需查实）定义媒体资源。

+ `<canvas>`标签：表示图形，比如图标和其他图像。这个标签没有行为，仅提供一个画布，但它把一个绘图API展示给客户端Javascript，以使脚本可以在这块画布上绘制想绘制的图形。

其他标签：

+ `<progress>`标签：表示一个进度条。

+ `<time>`标签：表示时间和日期。

+ `<ruby>`标签、`<rt>`标签、`<rp>`标签：表示ruby注释（中文注音或字符等）。

+ `<command>`标签：表示命令按钮，比如单选按钮、复选按钮、按钮等。

+ `<datalist>`标签：表示用户要求得到并且可以得到的细节。它可以与summary标签配合使用。summary标签提供标题或图标。summary标签应该是detail标签的第一个子标签。

与`<input>`标签有关的：

+ `<datalist>`标签：表示可选数据的列表，与input标签配合使用，可以制作出输入值的下拉列表。

+ `<datagrid>`标签：表示可选数据的列表，它以树形列表的形式来显示。

与`<form>`标签有关的：

+ `<menu>`标签：表示菜单列表，希望列出表单控件时使用该标签。

H5引进了一些新的标签，特别注意article、header、footer、aside、nav等，注意HTML的标题结构
 
> 参考：
> 
> 1. http://www.cnblogs.com/jasonsoop/p/4032721.html