#1.2、理解浏览器解析HTML的过程，理解DOM的树形结构，及相应API

浏览器==黑箱？

对前端而言，浏览器就是一整个的黑箱。我们了解里面的运行机制，到底会对我们有什么帮助呢？是不是最为迫切需要的？我们会得到哪些个方面的知识？只是单纯的编译原理？还是可以帮助我们理解跨浏览器的本质，也就是所谓浏览器设计的本质？

为什么这么多年来，所有浏览器都没有完全遵循W3C规范，而且还都在各自开发自己独有的扩展程序，从而导致发生很多的浏览器兼容问题？是因为历史遗留问题？还是与抢占市场份额有关？

> [当我在查阅每个浏览器的发展过程时，我感受到了Mozilla的倔强，IE的恐惧，Opera无可奈何下的不甘心，感受到了Safari在他的家族培养下一定会更加优秀以及Chrome这位技术先锋的美好前景。](http://www.jianshu.com/p/df97d3b866d5)

浏览器的主要组件为：

1. 用户界面 - 包括地址栏、前进/后退按钮、书签菜单等。除了浏览器主窗口显示的您请求的页面外，其他显示的各个部分都属于用户界面。

2. 浏览器引擎 - 在用户界面和呈现引擎之间传送指令。

3. 呈现引擎 - 负责显示请求的内容。如果请求的内容是 HTML，它就负责解析 HTML 和 CSS 内容，并将解析后的内容显示在屏幕上。

4. 网络 - 用于网络调用，比如 HTTP 请求。其接口与平台无关，并为所有平台提供底层实现。

5. 用户界面后端 - 用于绘制基本的窗口小部件，比如组合框和窗口。其公开了与平台无关的通用接口，而在底层使用操作系统的用户界面方法。

6. JavaScript 解释器。用于解析和执行 JavaScript 代码。

7. 数据存储。这是持久层。浏览器需要在硬盘上保存各种数据，例如 Cookie。新的 HTML 规范 (HTML5) 定义了“网络数据库”，这是一个完整（但是轻便）的浏览器内数据库。

![](src/browser_layers.png)

浏览器对HTML的解析过程，就涉及了2、3、4啦。

首先，先要有个大概的认识先：

![](src/Render-Process.jpg)

从上面这个图中，我们可以看到那么几个事：

**1）浏览器会解析三个东西，HTML（xHTML、SVG）、CSS、Javascript**：

+ HTML/SVG/XHTML，事实上，Webkit有三个C++的类对应这三类文档。解析这三种文件会产生一个DOM Tree。
+ CSS，解析CSS会产生CSS规则树。
+ Javascript，脚本，主要是通过DOM API和CSSOM API来操作DOM Tree和CSS Rule Tree。

**2）解析完成后，浏览器引擎会通过DOM Tree 和 CSS Rule Tree 来构造 Rendering Tree。**

注意：

Rendering Tree 渲染树并不等同于DOM树，因为一些像Header或display:none的东西就没必要放在渲染树中了。

CSS 的 Rule Tree主要是为了完成匹配并把CSS Rule附加上Rendering Tree上的每个Element。也就是DOM结点。也就是所谓的Frame。

然后，计算每个Frame（也就是每个Element）的位置，这又叫layout和reflow过程。

**3）最后通过调用操作系统Native GUI的API绘制。**

##HTML解析（HTML ==> DOM树）

假设有以下的HTML文件：

HTML的DOM Tree解析如下：

    <html>
    <html>
    <head>
        <title>Web page parsing</title>
    </head>
    <body>
        <div>
            <h1>Web page parsing</h1>
            <p>This is an example Web page.</p>
        </div>
    </body>
    </html>

上面这段HTML会解析成这样：

![](src/DOM-Tree-01.jpg)

如果有一个SVG标签的话：

![](src/DOM-Tree-02.jpg)

##CSS解析（CSS ==> CSS Rule Tree ==>Style Context Tree）（Firefox/Gecko玩法）

假如有以下的HTML文件：

    <doc>
    <title>A few quotes</title>
    <para class="emph">
        Franklin said that 
        <quote>
            "A penny saved is a penny earned."
        </quote>
    </para>
    <para>
          FDR said 
            <quote>
                "We have nothing to fear but <span class="emph">fear itself.</span>"
            </quote>
    </para>
    </doc>

那么变成了DOM树应该是这样的：

![](src/DOM-Tree-Example.jpg)

我们的CSS文件是这样子：

    doc { display: block; text-indent: 1em; }        /* Rule 1 */
    title { display: block; font-size: 3em; }       /* Rule 2 */
    para { display: block; }                        /* Rule 3 */
    [class="emph"] { font-style: italic; }          /* Rule 4 */

那么渲染出来的CSS规则树就是这样的：

![](src/CSS-Rule-Tree-Example.jpg)

看出了什么没有？也就是说，**CSS Rule Tree是要根据DOM树来生成的**！必须先生成DOM树，才能产生CSS Rule Tree。

注意：

**CSS匹配HTML元素是一个相当复杂和有性能问题的事情。**

所以，你就会在N多地方看到很多人都告诉你，**DOM树要小，CSS尽量用id和class，千万不要过渡层叠下去**，……

我们把这两棵树“合体”一下，就变成一个叫**Style Context Tree**的东西，也就是变成了下面这样：

![](src/CSS-Content-Tree-Example.jpg)

##超合体！**Style Context Tree + DOM Tree = Render Tree ！**

![](src/Firefox-style-context-tree.png)

Firefox基本上来说是通过CSS 解析 生成 CSS Rule Tree，然后，通过比对DOM生成Style Context Tree，然后Firefox通过把Style Context Tree和其Render Tree（Frame Tree）关联上，就完成了。

（注意：Render Tree会把一些不可见的结点去除掉。而**Firefox中所谓的Frame就是一个DOM结点，不要被其名字所迷惑了。**）

Webkit不像Firefox要用两个树来干这个，但是，Webkit也有Style对象，它直接把这个Style对象存在了相应的DOM结点上了。

##渲染（paint）

渲染的流程基本上如下（黄色的四个步骤）：

1. 计算CSS样式
2. 构建Render Tree
3. Layout – 定位坐标和大小，是否换行，各种position, overflow, z-index属性 ……
4. 正式开画

![](src/Render-Process-Skipping.jpg)

注意：

上图流程中有很多连接线，这表示了Javascript动态修改了DOM属性或是CSS属会导致重新Layout，有些改变不会，就是那些指到天上的箭头，比如，修改后的CSS rule没有被匹配到，等。

我们在画画的时候，有点小差错总是难免的，或者有些地方画得不够好，想要修改一下，这时候我们会怎么做？当然是开OT，重做啦～

浏览器也会这样子，不过分成了两种情况：Reflow和Repaint。

+ **Repaint——屏幕的一部分要重画，但是元素的几何尺寸没有变。**（比如某个CSS的背景色变了。）

+ **Reflow——意味着元件的几何尺寸变了，我们需要重新验证并计算Render Tree。**是Render Tree的一部分或全部发生了变化。这就是Reflow，或是Layout。

    （HTML使用的是flow based layout，也就是流式布局，所以，如果某元件的几何尺寸发生了变化，需要重新布局，也就叫reflow。）

    reflow 会从<html>这个root frame开始递归往下，依次计算所有的结点几何尺寸和位置，在reflow过程中，可能会增加一些frame，比如一个文本字符串必需被包装起来。

（注：HTML在初始化的时候也会做一次reflow，叫 intial reflow）

很容易看出，Reflow的成本比Repaint的成本高得多的多。

在一些高性能的电脑上也许还没什么，但是如果reflow发生在手机上，那么这个过程是非常痛苦和耗电的。

所以，下面这些动作有很大可能会是成本比较高的。

+ 当你增加、删除、修改DOM结点时，会导致Reflow或Repaint
+ 当你移动DOM的位置，或是搞个动画的时候。
+ 当你修改CSS样式的时候。
+ 当你Resize窗口的时候（移动端没有这个问题），或是滚动的时候。
+ 当你修改网页的默认字体时。

注：display:none会触发reflow，而visibility:hidden只会触发repaint，因为没有发现位置变化。

基本上来说，**reflow有如下的几个原因**：

+ **Initial**。网页初始化的时候。
+ **Incremental**。一些Javascript在操作DOM Tree时。
+ **Resize**。其些元件的尺寸变了。
+ **StyleChange**。如果CSS的属性发生变化了。
+ **Dirty**。几个Incremental的reflow发生在同一个frame的子树上。

当然，我们的浏览器是聪明的，它不会像上面那样，你每改一次样式，它就reflow或repaint一次。

一般来说，浏览器会把这样的操作积攒一批，然后做一次reflow，这又叫**异步reflow**或**增量异步reflow**。（但是有些情况浏览器是不会这么做的，比如：resize窗口，改变了页面默认的字体，等。对于这些操作，浏览器会马上进行reflow。）

但是，有时候浏览器并不会按这策略工作。

什么时候？那当然是老大下命令的时候咯！当我们的Javascript脚本需要用到下面DOM值的时候：

+ offsetTop, offsetLeft, offsetWidth, offsetHeight
+ scrollTop/Left/Width/Height
+ clientTop/Left/Width/Height
+ IE中的 getComputedStyle(), 或 currentStyle

因为，如果我们的程序需要这些值，那么浏览器需要返回最新的值，而这样一样会flush出去一些样式的改变，从而造成频繁的reflow/repaint。

##减少reflow/repaint

以下是一些Best Pratice：

**1）不要一条一条地修改DOM的样式。与其这样，还不如预先定义好css的class，然后修改DOM的className。**

**2）把DOM离线后修改。**如：

+ 使用documentFragment 对象在内存里操作DOM
+ 先把DOM给display:none(有一次reflow)，然后你想怎么改就怎么改。比如修改100次，然后再把他显示出来。
+ clone一个DOM结点到内存里，然后想怎么改就怎么改，改完后，和在线的那个的交换一下。

**3）不要把DOM结点的属性值放在一个循环里当成循环里的变量。**不然这会导致大量地读写这个结点的属性。

**4）尽可能的修改层级比较低的DOM。**当然，改变层级比较底的DOM有可能会造成大面积的reflow，但是也可能影响范围很小。

**5）为动画的HTML元件使用fixed或absoult的position**，那么修改他们的CSS是不会reflow的。（flow，flow指的就是normal flow，也就是说脱离了普通流自然不会受到reflow的啦。）

**6）千万不要使用table布局。**因为可能很小的一个小改动会造成整个table的重新布局。

> 参考：
> 
> 1. [http://taligarsiel.com/Projects/howbrowserswork1.htm](http://taligarsiel.com/Projects/howbrowserswork1.htm)
> 2. [http://ux.sohu.com/topics/50972d9ae7de3e752e0081ff](http://ux.sohu.com/topics/50972d9ae7de3e752e0081ff)
> 3. [http://coolshell.cn/articles/9666.html](http://coolshell.cn/articles/9666.html)
> 4. [http://www.webkit.org/projects/layout/index.html](http://www.webkit.org/projects/layout/index.html)