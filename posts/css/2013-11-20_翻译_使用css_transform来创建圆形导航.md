## 使用css transform来创建圆形导航

---

注：本文为译文，原文地址为：[《BUILDING A CIRCULAR NAVIGATION WITH CSS TRANSFORMS》](http://tympanus.net/codrops/2013/08/09/building-a-circular-navigation-with-css-transforms/)

css transform创建圆形导航的使用指南

![顶图](http://codropspz.tympanus.netdna-cdn.com/codrops/wp-content/uploads/2013/08/CircularNavigation.png)

[VIEW DEMO](http://tympanus.net/Tutorials/CircularNavigation/)

[DOWNLOAD SOURCE](http://tympanus.net/Tutorials/CircularNavigation/CircularNavigation.zip)

在这份指南中，我会让你学会如何使用css transform来创建圆形导航。为了能够让你更深刻的理解使用到的技术，我准备一步步向你展示如何写那些css，同时也会解释背后的业务逻辑（当然很简单的）和数学知识。

我们在写css时会用到一些基本的数学知识，但是不要担心，这些知识非常简单，而且我会一点点解释清楚的。

同时，请注意，这个技术来源于[Ana Tudor](http://twitter.com/thebabydino)，我只是对它进行了改进从而实现了自己想要的结果。其实，这也是我写这份指南的最终目的：深刻理解相关技术并改造实践，从而创建出自己的风格。

闲言碎语不多讲，下面让我们马上开始实践吧。

<!--more-->


### HTML标签结构

---

我们使用一种常用的代码结构来创建导航。首先需要一个`div`来包裹所有的导航链接，同时还需要一个按钮来触发导航的打开关闭操作。在第一个示例中，还额外需要一个遮罩层，用来在导航打开的时候遮盖页面。

```html

	<button class="cn-button" id="cn-button">+</button>
	<div class="cn-wrapper" id="cn-wrapper">
  		 <ul>
      		 	<li>
      		 		<a href="#">
      		 			<span class="icon-picture"></span>
      		 		</a>
      		 	</li>
       		<li>
       			<a href="#">
       				<span class="icon-headphones"></span>
       			</a>
       		</li>
       		<li>
       			<a href="#">
       				<span class="icon-home"></span>
       			</a>
       		</li>
       		<li>
       			<a href="#">
       			       <span class="icon-facetime-video"></span>
       			</a>
       		</li>
       		<li>
       			<a href="#">
       				<span class="icon-envelope-alt"></span>
       			</a>
       		</li>
   		</ul>
	</div>
	<div id="cn-overlay" class="cn-overlay"></div>

```

这里的按钮我们使用的是[Font Awesome Icons](http://fortawesome.github.io/Font-Awesome/icons/)上的示例。

### css transform背后的数学

---

解释数学原理最好的办法是使用示意图，我也准备使用这样的思路来讲解下面用到的数学知识。通过这些知识，你会了解我们为什么这样编码以及这些css规则是如何准确实现我们想要的结果的。

首先我们来了解下什么是圆心角。我们用张示意图来简单解释下：

![圆心角](http://codropspz.tympanus.netdna-cdn.com/codrops/wp-content/uploads/2013/08/cn-central-angle1.png)

假设，你想要类似上图那样把所有的导航链接都放到半圆形区域中。如果总共有6个导航链接，那么每个的圆心角应该是：

*180 deg / 6 = 30 deg*

但是，如果你想把这些全放入一个圆形区域时，那么每个的圆心角应该是：

*360 deg / 6 = 60 deg*

以此类推，你可以计算出任何你需要的角度。那么现在开始，我们将通过这些数学知识使用css transform来实现这些角度。

我们使用css的skew()函数来让每个链接实现不同角度的倾斜：

*90 deg - x deg (x的值为相应的圆心角)*

非常简单是吧。但是要注意，每个列表元素中的内容也倾斜了，这让它们看起来有点变形，你肯定不想看到这样的效果。所以，我们需要把每个元素中的锚点“扭正”，让它们看起来更合适些。但是，请不要着急，我们稍后再实现。

我会用一个可交互的demo来一步步告诉你css transform是如何应用到导航元素上的。这样会让你对代码有更深刻的理解，知道里面究竟实现了什么效果。（注意：demo中的讲解同其他方式可能略有不同）

你可以先看看这个示例来让自己对接下来要做的事情有个更为清楚的认识。我建议你花几分钟时间去看看，同时我也在这个demo中加入了一些注释来便于你进行理解。

[View the interactive demo](http://tympanus.net/Tutorials/CircularNavigation/interactivedemo/index.html)

下面是demo中每个步骤的截图：

Initial state:

![](http://codropspz.tympanus.netdna-cdn.com/codrops/wp-content/uploads/2013/08/Step-0.png)

Step 1:

![](http://codropspz.tympanus.netdna-cdn.com/codrops/wp-content/uploads/2013/08/Step-1.png)

Step 2:

![](http://codropspz.tympanus.netdna-cdn.com/codrops/wp-content/uploads/2013/08/step-2.png)

Step 4:

![](http://codropspz.tympanus.netdna-cdn.com/codrops/wp-content/uploads/2013/08/Step-4.png)

Step 5:

![](http://codropspz.tympanus.netdna-cdn.com/codrops/wp-content/uploads/2013/08/Step-5.png)

Step 6:

![](http://codropspz.tympanus.netdna-cdn.com/codrops/wp-content/uploads/2013/08/Step-6.png)

Step 7:

![](http://codropspz.tympanus.netdna-cdn.com/codrops/wp-content/uploads/2013/08/Step-7.png)

现在让我们回顾下实现过程：

* 我们需要把每个元素都绝对定位在容器里。
* 我们将每个元素的transform-origin（变形原点）设置为右下角。
* 然后我们将每个元素向左上角偏移一定距离，以使每个元素的变形原点同容器的中心点相重合。
* 使用上文的公式来顺时针方向旋转每个元素的角度，每个对应的角度是：*i * x* （x依然是前面计算出的圆心角）
* 然后，让每个元素倾斜制定角度（使用上文中的公式）。

在demo中，总共有5个导航链接，这就需要5个圆心角。同时我们想要覆盖的区域为一个半圆，那么根据上文的分析，一个圆心角的角度应该为36deg，但是，我在示例中把角度设置为了40deg（因为它可以提供更大的可点击区域），因此总共的角度应该是*5 * 40 = 200deg*，这可比*180deg*大多了。同时，demo中还让每个元素按逆时针方向“回拨”了*(200-180)/2 deg*的角度，这样元素和元素之间都留有一定的间隙。

这时，我们已经把每个元素放到了合适的位置，但是元素的stepkew同样影响了内部的内容（a标签），导致内容看起来都变形了。因此，我们最后要做的步骤就是保证这些a标签不变形并且可访问。实现方法如下：

“扭正”这些a标签，其实就是反方向skew相同的角度，具体的角度值应该为：

*-[90 - (x/2) ]* (x为相应的圆心角)。

当圆心角为*40deg*时，我们需要每个链接选择的角度为：

*-[ 90 - (40/2)] = -70 deg*

每个链接在它们的父元素中都是绝对定位的，并且父元素的overflow属性设置为了hidden。所以这些链接被裁减了，为了让它们能够合适的显示出来，就必须要将文本内容居中。

以上就是我们编码过程中用到的所有数学知识。呵呵，我们已经悄悄完成了想要的效果。

好，让我们再次快速浏览下这些步骤：

1. 将每个li旋转一定角度：角度 *y = i * x*（其中i为第几个元素，x为圆心角的值）

1. 每个li倾斜*90deg - x*（x还是圆心角的值）

1. 如果需要在li之间留有边界，那么再将li反方向旋转一定角度（其实这一步骤可以同前面的步骤合并，你只需要在旋转的时候减去想要纠正回来的角度就ok了）

1. “扭正”li里面的a链接（同时注意把它们的文本内容居中）

当然我跳过了将li的变形原点同它们的容器中心点重合的部分（demo中提过的步骤）。以上步骤已经非常接近我们想要实现的效果了，但是要创建一个完整的导航，还有些东西需要做。剩下的步骤可以让我们的导航变的更加漂亮，那我们先从css来开始吧。

###css

---

我们首先来为第一个demo添加样式。

![](http://codropspz.tympanus.netdna-cdn.com/codrops/wp-content/uploads/2013/08/CircularNavigation_Demo1.png)

为了解决旧版本浏览器不支持css transform的问题，我们使用了[Modernzr](http://modernizr.com/)的类名来对标签进行标记。这样，当我们遇到不支持的浏览器时，就可以只显示一些简单基本的样式。

我们从wrapper的样式开始说起，它应该一直固定在页面的底部中间位置。一开始它是缩小关闭的，当点击按钮点击后，它会打开并放大显示。

```css
.csstransforms .cn-wrapper {
  font-size:1em;
  width: 26em;
  height: 26em;
  overflow: hidden;
  position: fixed;
  z-index: 10;
  bottom: -13em;
  left: 50%;
  border-radius: 50%;
  margin-left: -13em;
  transform: scale(0.1);
  transition: all .3s ease;
}
/* class applied to the container via JavaScript that will scale the navigation up */
.csstransforms .opened-nav {
  border-radius: 50%;
  transform: scale(1);
}
```

我们同样会把触发打开关闭操作的按钮放到合适的位置。

```css
.cn-button {
  border:none;
  background:none;
  color: white;
  text-align: Center;
  font-size: 1.5em;
  padding-bottom: 1em;
  height: 3.5em;
  width: 3.5em;
  background-color: #111;
  position: fixed;
  left: 50%;
  margin-left: -1.75em;
  bottom: -1.75em;
  border-radius: 50%;
  cursor: pointer;
  z-index: 11
}
.cn-button:hover,
.cn-button:active,
.cn-button:focus{
  background-color: #222;
}
```

还有就是导航打开的时候，需要把页面其他地方遮起来。下面是遮罩层的样式代码：

```css
.cn-overlay{
  width:100%
  height:100%;
  background-color: rgba(0,0,0,0.6);
  position:fixed;
  top:0;
  left:0;
  bottom:0;
  right:0;
  opacity:0;
  transition: all .3s ease;
  z-index:2;
  pointer-events:none;
}
 
/* Class added to the overlay via JavaScript to show it when navigation is open */
.cn-overlay.on-overlay{
  pointer-events:auto;
  opacity:1;
}
```

根据前面分析的数学知识，我们下面来给li和里面的a链接添加样式代码：

```css
.csstransforms .cn-wrapper li {
  position: absolute;
  font-size: 1.5em;
  width: 10em;
  height: 10em;
  transform-origin: 100% 100%;
  overflow: hidden;
  left: 50%;
  top: 50%;
  margin-top: -1.3em;
  margin-left: -10em;
  transition: border .3s ease;
}
 
.csstransforms .cn-wrapper li a {
  display: block;
  font-size: 1.18em;
  height: 14.5em;
  width: 14.5em;
  position: absolute;
  bottom: -7.25em;
  right: -7.25em;
  border-radius: 50%;
  text-decoration: none;
  color: #fff;
  padding-top: 1.8em;
  text-align: center;
  transform: skew(-50deg) rotate(-70deg) scale(1);
  transition: opacity 0.3s, color 0.3s;
}
 
.csstransforms .cn-wrapper li a span {
  font-size: 1.1em;
  opacity: 0.7;
}
/* for a central angle x, the list items must be skewed by 90-x degrees in our case x=40deg so skew angle is 50deg items should be rotated by x, minus (sum of angles - 180)2s (for this demo) */
 
.csstransforms .cn-wrapper li:first-child {
  transform: rotate(-10deg) skew(50deg);
}
 
.csstransforms .cn-wrapper li:nth-child(2) {
  transform: rotate(30deg) skew(50deg);
}
 
.csstransforms .cn-wrapper li:nth-child(3) {
  transform: rotate(70deg) skew(50deg)
}
 
.csstransforms .cn-wrapper li:nth-child(4) {
  transform: rotate(110deg) skew(50deg);
}
 
.csstransforms .cn-wrapper li:nth-child(5) {
  transform: rotate(150deg) skew(50deg);
}
 
.csstransforms .cn-wrapper li:nth-child(odd) a {
  background-color: #a11313;
  background-color: hsla(0, 88%, 63%, 1);
}
 
.csstransforms .cn-wrapper li:nth-child(even) a {
  background-color: #a61414;
  background-color: hsla(0, 88%, 65%, 1);
}
 
/* active style */
.csstransforms .cn-wrapper li.active a {
  background-color: #b31515;
  background-color: hsla(0, 88%, 70%, 1);
}

/* hover style */
.csstransforms .cn-wrapper li:not(.active) a:hover,
.csstransforms .cn-wrapper li:not(.active) a:active,
.csstransforms .cn-wrapper li:not(.active) a:focus {
  background-color: #b31515;
  background-color: hsla(0, 88%, 70%, 1);
}
```

我们同样会给那些不支持css transform的浏览器编写基本样式代码：

```css
.no-csstransforms .cn-wrapper{
  font-size:1em;
  height:5em;
  width:25.15em;
  bottom:0;
  margin-left: -12.5em;
  overflow: hidden;
  position: fixed;
  z-index: 10;
  left:50%;
  border:1px solid #ddd;
}
 
.no-csstransforms .cn-button{
  display:none;
}
 
.no-csstransforms .cn-wrapper li{
  position:static;
  float:left;
  font-size:1em;
  height:5em;
  width:5em;
  background-color: #eee;
  text-align:center;
  line-height:5em;
}
 
.no-csstransforms .cn-wrapper li a{
  display:block;
  width:100%;
  height:100%;
  text-decoration:none;
  color:inherit;
  font-size:1.3em;
  border-right: 1px solid #ddd;
}
 
.no-csstransforms .cn-wrapper li a:last-child{
  border:none;
}

.no-csstransforms .cn-wrapper li a:hover,
.no-csstransforms .cn-wrapper li a:active,
.no-csstransforms .cn-wrapper li a:focus{
  background-color: white;
}
 
.no-csstransforms .cn-wrapper li.active a {
  background-color: #6F325C;
  color: #fff;
}
```

同时，我们还想让整个导航是自适应的，这样在小屏幕上时它会变得更加合适。下面是我们的响应式样式代码：

```css
@media screen and (max-width:480px){
  .csstransforms .cn-wrapper{
    font-size:.68em;
  }
  .cn-button{
    font-size:1em;
  }
  .csstransforms .cn-wrapper li {
    font-size:1.52em;
  }
}
 
@media screen and (max-width:320px){
  .no-csstransforms .cn-wrapper{
    width:15.15px;
    margin-left: -7.5em;
  }
  .no-csstransforms .cn-wrapper li{
    height:3em;
    width:3em;
  }
}
```

哈哈，这样我们就完成了第一个demo！马上来看下一个如何实现。

![](http://codropspz.tympanus.netdna-cdn.com/codrops/wp-content/uploads/2013/08/CircularNavigation_Demo2.png)

第二个demo的圆形样式同一个不一样，但是实现具体样式所用到的数学知识和逻辑同上一个demo非常类似。区别只有三个地方，所以下面我们会着重讲讲这些不同的步骤，相同的步骤不再赘述。

我们先回顾下上文的阐述，看看是如何通过修改css来改变li形状的。

我们对a链接的背景设置了radial gradient（径向渐变），而且渐变的是背景的透明度。这样整体的效果会像下面那样（注意，这里作者讲的都是第二个绿色背景的demo，但使用的图都是紫色demo的图）：

![](http://codropspz.tympanus.netdna-cdn.com/codrops/wp-content/uploads/2013/08/cn-radial-gradient.png)

这只是第一步工作，接下来我们还要通过改变旋转角度来增加每个li之间的间隔。我们还会去掉li和container的背景色，去掉边框，同时减少a链接的上内边距来让它们在垂直方向上看起来更居中。好吧，实现的效果就如下图这样：

![](http://codropspz.tympanus.netdna-cdn.com/codrops/wp-content/uploads/2013/08/cn-demo2-step1.png)

如你所见，目前的导航已经变得不一样了，我们的工作也已经完成了一大半，但是还要一个重要的事情要去做。在目前的样式下，a锚点的可点击区域比我们期望的要大的多。我们期望的区域只是上图中有背景色的区域。从下图中，你可以看到那些我们不想要的a锚点的点击区域。

![](http://codropspz.tympanus.netdna-cdn.com/codrops/wp-content/uploads/2013/08/cn-demo2-cover.png)

当鼠标位于a链接下方的红色区域时，会激活链接的hover状态，这本来是很正常的情况，但是由于我们在这个demo中希望a链接只是图中的紫色区域，所以，当鼠标位于红色区域时，我们要想办法阻止a链接的相关事件被触发。为了达到这样的目的，我们需要在上图中的红色区域放置一个圆形的遮罩层，这样，当鼠标放上去的时候就不会触发相应的事件了。要注意，这里我们为了避免增加额外的空标签，使用了伪元素来进行实现。

至此，通过以上三步的工作，导航整体上的效果已经非常接近上文图片中的效果了。下面是用到的css代码：

```css
.csstransforms .cn-wrapper {
  position: absolute;
  top: 100%;
  left: 50%;
  z-index: 10;
  margin-top: -13em;
  margin-left: -13.5em;
  width: 27em;
  height: 27em;
  border-radius: 50%;
  background: transparent;
  opacity: 0;
  transition: all .3s ease 0.3s;
  transform: scale(0.1);
  pointer-events: none;
  overflow: hidden; 
}

/*cover to prevent extra space of anchors from being clickable*/
.csstransforms .cn-wrapper:after{
  color: transparent;
  content:".";
  display:block;
  font-size:2em;
  width:6.2em;
  height:6.2em;
  position: absolute;
  left: 50%;
  margin-left: -3.1em;
  top:50%;
  margin-top: -3.1em;
  border-radius: 50%;
  z-index:10;
}

.csstransforms .cn-wrapper li {
  position: absolute;
  top: 50%;
  left: 50%;
  overflow: hidden;
  margin-top: -1.3em;
  margin-left: -10em;
  width: 10em;
  height: 10em;
  font-size: 1.5em;
  transition: all .3s ease;
  transform: rotate(76deg) skew(60deg);
  transform-origin: 100% 100%;
  pointer-events: none;
}

.csstransforms .cn-wrapper li a {
  position: absolute;
  right: -7.25em;
  bottom: -7.25em;
  display: block;
  width: 14.5em;
  height: 14.5em;
  border-radius: 50%;
  background: #429a67;
  background: radial-gradient(transparent 35%, #429a67 35%);
  color: #fff;
  text-align: center;
  text-decoration: none;
  font-size: 1.2em;
  line-height: 2;
  transition: all .3s ease;
  transform: skew(-60deg) rotate(-75deg) scale(1);
  pointer-events: auto;
}

.csstransforms .cn-wrapper li a span {
  position: relative;
  top: 1.8em;
  display: block;
  font-size: .5em;
  font-weight: 700;
  text-transform: uppercase;
}
 
.csstransforms .cn-wrapper li a:hover,
.csstransforms .cn-wrapper li a:active,
.csstransforms .cn-wrapper li a:focus {
    background: radial-gradient(transparent 35%, #449e6a 35%);
}
```

在第二个demo中，当导航被打开的时候，我们希望li的展开能有更有趣的效果。为了达到这个目的，我们先使用`rotate(76deg) skew(60deg)`把所有li放到同一位置上。

然后，我们又使用了transition delay。这样，当导航打开时，所有的li会先升起，再向周围展开。导航关闭时，所有li会先从周围聚合到一起，然后才下降消失。很赞的效果吧？

```css
.csstransforms .opened-nav {
  border-radius: 50%;
  opacity: 1;
  transition: all .3s ease;
  transform: scale(1);
  pointer-events: auto;
}
 
.csstransforms .opened-nav li {
  transition: all .3s ease .3s;
}
 
.csstransforms .opened-nav li:first-child {
  transform: rotate(-20deg) skew(60deg);
}
 
.csstransforms .opened-nav li:nth-child(2) {
  transform: rotate(12deg) skew(60deg);
}
 
.csstransforms .opened-nav  li:nth-child(3) {
  transform: rotate(44deg) skew(60deg);
}

.csstransforms .opened-nav li:nth-child(4) {
  transform: rotate(76deg) skew(60deg);
}
 
.csstransforms .opened-nav li:nth-child(5) {
  transform: rotate(108deg) skew(60deg);
}
 
.csstransforms .opened-nav li:nth-child(6) {
  transform: rotate(140deg) skew(60deg);
}
 
.csstransforms .opened-nav li:nth-child(7) {
  transform: rotate(172deg) skew(60deg);
}
```

当然，这里我们仍然会对不支持相关技术的浏览器进行降级处理。

```css
.no-csstransforms .cn-wrapper{
  margin:10em auto;
  overflow:hidden;
  text-align:center;
  padding:1em;
}
.no-csstransforms .cn-wrapper ul{
  display:inline-block;
}
.no-csstransforms li{
  font-size:1em;
  width:5em;
  height:5em;
  float:left;
  line-height:5em;
  text-align:center;
  background-color: #fff;
}
.no-csstransforms li a{
  display:block;
  height:100%;
  width:100%;
  text-decoration: none;
  color: inherit;
}

.no-csstransforms .cn-wrapper li a:hover,
.no-csstransforms .cn-wrapper li a:active,
.no-csstransforms .cn-wrapper li a:focus{
  background-color: #f8f8f8;
}
.no-csstransforms .cn-wrapper li.active a {
  background-color: #6F325C;
  color: #fff;
}
.no-csstransforms .cn-button{
  display:none;
}
```

同时，导航依然是响应式的，这样我们在小屏幕设备上依然有良好体验。以下是相关的响应式代码：

```css
@media only screen and (max-width: 620px) {
  .no-csstransforms li{
    width:4em;
    height:4em;
    line-height:4em;
  }
}
@media only screen and (max-width: 500px) {
  .no-ccstransforms .cn-wrapper{
    padding:.5em;
  }
  .no-csstransforms .cn-wrapper li{
    font-size:.9em;
    width:4em;
    height:4em;
    line-height:4em;
  }
}
@media only screen and (max-width: 480px) {
  .csstransforms .cn-wrapper{
    font-size: .68em;
  }
  .cn-button{
    font-size:1em;
  }
}
@media only screen and (max-width:420px){
  .no-csstransforms .cn-wrapper li{
    width:100%;
    height:3em;
    line-height:3em;
  }
}
```

以上就是你需要使用的所有css代码。

###javascript

---

在这两个demo中，我们不需要使用任何javascript框架。我会使用David De Sandro的[Classie.js](https://github.com/desandro/classie)来添加或删除class（类名）。最后，对于不支持`addEventListener`和`removeEventListener`的浏览器，我会使用 Jonathan Neal的[EventListener polyfill](https://github.com/jonathantneal/EventListener)来解决问题。

我们要对demo中的按钮添加事件监听，这样当按钮被click或者hover时触发导航的打开或关闭操作。

同时，在第一个demo中，当导航打开时，点击导航的外部区域同样可以关闭导航。下面我们先来看看第一个demo的javascript代码：

```javascript
(function(){
 
  var button = document.getElementById('cn-button'),
    wrapper = document.getElementById('cn-wrapper'),
    overlay = document.getElementById('cn-overlay');
 
  //open and close menu when the button is clicked
  var open = false;
  button.addEventListener('click', handler, false);
  button.addEventListener('focus', handler, false);
  wrapper.addEventListener('click', cnhandle, false);
 
  function cnhandle(e){
    e.stopPropagation();
  }
 
  function handler(e){
    if (!e) var e = window.event;
    e.stopPropagation();//so that it doesn't trigger click event on document
 
      if(!open){
        openNav();
      }
    else{
        closeNav();
      }
  }
  function openNav(){
    open = true;
      button.innerHTML = "-";
      classie.add(overlay, 'on-overlay');
      classie.add(wrapper, 'opened-nav');
  }
  function closeNav(){
    open = false;
    button.innerHTML = "+";
    classie.remove(overlay, 'on-overlay');
    classie.remove(wrapper, 'opened-nav');
  }
  document.addEventListener('click', closeNav);
 
})();
```

第二个demo中的js代码，除了一些特殊的功能外，大部分代码是非常接近第一个demo的。

```javascript
(function(){
 
  var button = document.getElementById('cn-button'),
    wrapper = document.getElementById('cn-wrapper');
 
    //open and close menu when the button is clicked
  var open = false;
  button.addEventListener('click', handler, false);
  button.addEventListener('focus', handler, false);
 
  function handler(){
    if(!open){
      this.innerHTML = "Close";
      classie.add(wrapper, 'opened-nav');
    }
    else{
      this.innerHTML = "Menu";
    classie.remove(wrapper, 'opened-nav');
    }
    open = !open;
  }
  function closeWrapper(){
    classie.remove(wrapper, 'opened-nav');
  }
 
})();
```

好吧，以上就是所有代码。我希望你能够喜欢这个指南，并且它在工作中能够给你带来帮助。

---

[Find this project on Github](https://github.com/SaraSoueidan/Circular-Navigation-with-CSS-transforms-Codrops)

[VIEW DEMO](http://tympanus.net/Tutorials/CircularNavigation/)

[Download source](http://tympanus.net/Tutorials/CircularNavigation/CircularNavigation.zip)

---


