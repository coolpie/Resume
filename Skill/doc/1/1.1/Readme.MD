###1.1. H5新引进的标签

Setion类标签：

替代`<div>`标签的各种语义化标签。用以把以前被各种各样的`<div>`加各种class来划分页面区域。

整个页面的每个区域，称作一个Section，这些标签也就给section

+ `<section>`标签：表示页面中一个内容区间（也就是一个Section），比如章节、页眉、页脚等等。
+ `<article>`标签：表示页面中一块与上下文不相关的区域，如博客中的一篇文章的区域等。
+ `<aside>`标签：表示article标签内容之外，与article内容相关的的辅助信息。通常来说，用来划分Sidebar区域，也就是W3C里所说的“[a section of a page that consists of content that is tangentially related to the content around the aside element, and which could be considered separate from that content](http://www.w3.org/html/wg/drafts/html/master/sections.html#the-aside-element)”（老实说，看不懂，切线相关的article以外的标签是什么？或许[MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/aside)更好看点）
+ `<header>`标签：表示整个页面的标题，或者是页面的一个区域（Section）的标题。
+ `<footer>`标签：表示整个页面的注脚，或者页面的一个区域（Section）的注脚。

媒体化标签：

H5引进了一些新的标签，特别注意article、header、footer、aside、nav等，注意HTML的标题结构