---
title: 解决方案的理念
tags:
  - 生活经验
url: 625.html
id: 625
categories:
  - 乱七八糟
date: 2013-06-14 15:22:05
---

**之前做的一个项目**，需要一个解决方案，大致上就是要无缝加载和切换页面，怎么才能最快速的加载完成页面，最好是要做到零延迟，或者是说感官上的零延迟，着实费了一番头脑来想。所以最终的解决方案是使用JS的[DOM](http://www.w3school.com.cn/htmldom/dom_intro.asp "HTML DOM")和[事件对象](http://www.w3school.com.cn/htmldom/dom_obj_event.asp "HTML DOM Event 对象")来直接替换部分需要重写的代码即可，说白一点，就是所有页面全部写在同一个文件里面，同样，页面所有的元素都只需要加载一次即可，也包括类似于比较大的图片之类的文件，也就是说第一次加载完成，后续的直接用JS调用替换就行，也就相当于在本地执行，所以算是解决了这些个方法。 

**最开始在想需求的时候**设想是需要加载很多页面的，但没想到的是，在硬件状况不理想的情况下，会有如此之大的差别,这也告诫我在以后的项目当中需要注意的事情，在预先我还没有想到的情况下，不能盲目动手；而且，在提高单个页面的加载速度的话，有一些思路可以，一种是划分，也就是分割成非常多非常多的同时加载，还有就是在一开头全部加载完成，需要一个稍微长时间的进度条，随后加载完成之后就可以实现无缝加载；显然项目需要的是后者。 

**所以具体到这个项目**的实施来看，要用到大量的JS，首先，需要一个规定成为动作（可以命名为Action）的JS文档，它包括了你需要使用的一些JS操作的基本，比如，写入，更改样式，更改文本，显示，隐藏，加载，等等一些动作；然后第二个JS文件，定义了哪些文件需要的单个操作（Dom），比如，我需要隐藏的某个DIV，或者是某个模块，还有也要把显示的内容放进去，比如基础文本，比如模块的样式；第三个文件就是属于调用了，就是大量的ID穿梭的文件把内容填充进去（Content）；这样把三个需要绕来绕去的都剥离出来，可扩展性就很强了，也算是体现了面向对象的一点点点点的小思想。 

**所以思想要咸鱼行动**，解决一件事情的时候，先别动手，想好思路在开动，要不然会在后面一定会需要大量的的Bug与Debug循环。
