## CSS(3)部分

### 1.水平居中的方法

1.元素为行内元素，设置父元素 text-align: center;

2.如果元素宽度固定，可以设置左右 margin 为 auto;

3.如果元素为决定定位，设置父元素 position 为 relative，元素设 left:0; right: 0; margin: auto;

4.使用 flex-box 布局，指定 justify-content 属性为 center;

5.display 设置为 tabel-ceil;

### 2.垂直居中的方法

1.将显示方式设置为表格，display: table-cell,同时设置 vertial-align: middle;

2.使用 flex 布局，设置为 align-item: center;

3.绝对定位中设置 bottom: 0; top: 0;并设置 margin: auto;

4.绝对定位中固定高度设置 top: 50%, margin-top 值为高度一半的负值;

5.文本垂直居中设置 line-height 为 height 值

参考：

1. [CSS 实现垂直居中的 5 种方法](https://www.qianduan.net/css-to-achieve-the-vertical-center-of-the-five-kinds-of-methods/)

2. [16 种方法实现水平居中垂直居中](https://juejin.im/post/58f818bbb123db006233ab2a)

3. [Centering in CSS: A Complete Guide](https://css-tricks.com/centering-css-complete-guide/)

### 3.如何实现一个自适应的正方形

利用 padding 设置为百分比时是相对于父级元素的，因此同时设置 width 与 padidng-top(padding-bottom)为同一个百分数，并且设置 height: 0;即可实现一个正方形

### 4.简要介绍一下 flex 布局

flex 布局可以帮我们快速布局一些区块，实现你想要的效果，不用再去 float，position 之类的。我们在布局网页的时候很多时候都是一些特殊布局，flex 就能帮我快速去布局，不需要去定位。

任何一个盒子都可以指定为 flex 布局，但是要注意，设为 Flex 布局以后，子元素的 float、clear 和 vertical-align 属性将失效。

### 5.如何实现两列布局

1.将元素的 display 设置为 inline-block

2.两个元素全部使用浮动

3.一个元素左浮动，第二个元素不便，同时设置一个 margin-left 值

4.使用 flex-box 布局

### 6.简要介绍一下 CSS3 的新特性

可以对 CSS3 的新特性做一个简单的分类，在布局方面新增了 flex 布局，在选择器方面新增里例如：first-of-type，nth-child 等选择器，在盒模型方面添加了 box-sizing 来改变盒模型，在动画方面增加了 animation、2d 变换、3d 变换等。在颜色方面添加透明、rgba 等，在字体方面允许潜入字体和设置字体阴影，同时当然也有盒子的阴影，最后还有关键的媒体查询。

### 7.如何使用 实现硬件加速

硬件加速是指通过创建独立的复合图层，让 GPU 来渲染这个图层，从来提高性能，一般触发硬件加速的 CSS 属性有 transform、opacity、filter，为了避免 2d 动画在开始和结束的时候的 repaint 操作，一般使用 transform: translateZ(0)

参考：

[CSS 动画原理及硬件加速](http://www.cnblogs.com/shytong/p/5419565.html)

[CSS 动画之硬件加速](https://www.w3cplus.com/css3/introduction-to-hardware-acceleration-css-animations.html)

[CSS3 硬件加速也有坑！！！](https://div.io/topic/1348)

### 8.重绘和回流（重排）是什么，如何避免？

DOM 的变化影响到了元素的几何属性（宽高），浏览器重新计算元素的几何属性，其他元素的几何属性和位置也会受到影响，浏览器需要重新构造渲染树，这个过程成为重排，浏览器将受到影响的部分重新绘制到屏幕上的过程称为重绘。<br />
引起重排的原因有：<br /> 1.添加或删除可见的 DOM 元素<br /> 2.元素位置、尺寸、内容改变<br /> 3.浏览器页面初始化<br /> 4.浏览器窗口尺寸改变，重排一定重绘，重绘不一定重排。<br />
减少重绘和重排的方法：<br /> 1.不在布局信息改变时做 DOM 查询<br /> 2.使用 cssText 或者 className 一次性改变属性<br /> 3.使用 fragment<br /> 4.对于多次重排的元素，如动画，使用绝对定位脱离文档流，让它的改变不影响到其他元素<br />
参考：

1.[高性能 JavaScript 重排与重绘](http://www.cnblogs.com/zichi/p/4720000.html)

2.[网页性能管理详解](http://www.ruanyifeng.com/blog/2015/09/web-page-performance-in-depth.html)

3.[重排重绘，看这一篇就够了](https://juejin.im/entry/582f16fca22b9d006b7afd89)

### 9.说一说你了解的圣杯布局和双飞翼布局？

圣杯布局和双飞翼布局是前端需要日常掌握的重要布局方式。两者的功能相同，都是为了实现一个两侧宽度固定，中间宽度自适应的三栏布局。

圣杯布局来源于文章[In Search of the Holy Grail](https://alistapart.com/article/holygrail)，而双飞翼布局来源于淘宝 UED。虽然两者的实现方法略有差异，不过都遵循了以下要点：

- 两侧宽度固定，中间宽度自适应
- 中间部分在 DOM 结构上优先，以便先行渲染
- 允许三列中的任意一列成为最高列
- 只需要使用一个额外的<div>标签

### 10.说一说 css3 的 animation

css3 的 animation 是 css3 新增的动画属性，这个 css3 动画的每一帧是通过@keyframes 来声明的，keyframes 声明了动画的名称，通过 from、to 或者是百分比来定义
每一帧动画元素的状态，通过 animation-name 来引用这个动画，同时 css3 动画也可以定义动画运行的时长、动画开始时间、动画播放方向、动画循环次数、动画播放的方式，
这些相关的动画子属性有：animation-name 定义动画名、animation-duration 定义动画播放的时长、animation-delay 定义动画延迟播放的时间、animation-direction 定义
动画的播放方向、animation-iteration-count 定义播放次数、animation-fill-mode 定义动画播放之后的状态、animation-play-state 定义播放状态，如暂停运行等、animation-timing-function
定义播放的方式，如恒速播放、艰涩播放等。

### 11.绝对定位和相对定位的区别？

绝对定位是相对于最近的已经定位的祖先元素，没有则相对于 body，绝对定位脱离文档流，而相对定位是相对于元素在文档中的初始位置，并且
相对定位的元素仍然占据原有的空间。

参考：

[MDN Positioning](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Positioning)

### 12.说一下你了解的CSS选择器？

参考：

[CSS 选择器参考手册](http://www.w3school.com.cn/cssref/css_selectors.asp)


### 13.BFC是什么？介绍一下，如何触发BFC？
BFC也就是常说的块格式化上下文，这是一个独立的渲染区域，规定了内部如何布局，并且这个区域的子元素不会影响到外面的元素。其中比较重要的布局规则有内部box垂直放置、计算BFC的高度的时候，浮动元素也参与计算。
触发BFC的规则有根元素、浮动元素、position为absolute或flxed的元素、display属性为inline-block、table-cell、table-caption、flex、inline-flex、overflow不为visible的元素。

参考：

[前端精选文摘：BFC 神奇背后的原理](http://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html)

[块格式化上下文](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)

### 14.CSS3动画如何实现暂停？
css3动画可以通过设置animation-play-state属性为paused来设置这个动画暂停。

### 15.说一说你知道哪些伪类选择器？

参考：
[CSS选择器详解（伪类）](https://blog.csdn.net/Panda_m/article/details/50084699?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)

### 16.简要介绍一下一种css预处理器？

参考：
[详说css与预处理器（以及less、sass、stylus的区别）](https://blog.csdn.net/ly2983068126/article/details/77737292)