---
layout:     post
title:      "浮动-定位"
subtitle:   ""
date:       2017-03-28
author:     "Clover"
header-img: "img/education_large_2x.jpg"
catalog: true
tags:
    - Clover
    - CSS
    - HTML
    - 前端
---

# 定位与浮动

### 定位基本属性

| 值 | 属性 |
|--------|--------|
|inherit	|规定应该从父元素继承 position 属性的值|
|static|	默认值,没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）|
|relative	|生成相对定位的元素，相对于元素本身正常位置进行定位,因此，left:20px 会向元素的 left 位置添加20px|
|absolute	|生成绝对定位的元素，相对于static定位以外的第一个祖先元素（offset parent）进行定位,元素的位置通过 left, top, right 以及 bottom 属性进行规定|
|fixed	|生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过 left, top, right 以及 bottom 属性进行规定|
|sticky	|CSS3新属性，表现类似position:relative和position:fixed的合体，在目标区域在屏幕中可见时，它的行为就像position:relative; 而当页面滚动超出目标区域时，它的表现就像position:fixed，它会固定在目标位置|

### 普通流与相对定位

CSS有三种基本的定位机制：`普通流`，`相对定位`和`绝对定位`

普通流是默认定位方式，在普通流中元素框的位置由元素在html中的位置决定，元素position属性为static或继承来的static时就会按照普通流定位，这也是我们最常见的方式

相对定位比较简单，对应position属性的relative值，如果对一个元素进行相对定位，它将出现在他所在的位置上，然后可以通过设置垂直或水平位置，让这个元素相对于它自己移动，在使用相对定位时，无论元素是否移动，元素在文档流中占据原来空间，只是表现出来的位置会改变

`普通流`


```html
    <div style="border: solid 1px #0e0; width:200px;">
        <div style="height: 100px; width: 100px; background-color: Red;">
        </div>
        <div style="height: 100px; width: 100px; background-color: Green;">
        </div>
        <div style="height: 100px; width: 100px; background-color: Red;">
        </div>
	</div>
```

<div style="border: solid 1px #0e0; width:200px;">
        <div style="height: 100px; width: 100px; background-color: Red;">
        </div>
        <div style="height: 100px; width: 100px; background-color: Green;">
        </div>
        <div style="height: 100px; width: 100px; background-color: Red;">
        </div>
	</div>

`相对定位`

```html
	<div style="border: solid 1px #0e0; width:200px;">
        <div style="height: 100px; width: 100px; background-color: Red;">
        </div>
        <div style="height: 100px; width: 100px; background-color: Green; position:relative; top:20px; left:20px;">
        </div>
        <div style="height: 100px; width: 100px; background-color: Red;">
        </div>
	</div>
```

<div style="border: solid 1px #0e0; width:200px;">
    <div style="height: 100px; width: 100px; background-color: Red;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green; position:relative; top:20px; left:20px;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Red;">
    </div>
</div>

上面例子可以看出，对绿色div进行相对定位，分别右移，下移20px后第二个红色div位置并没有相应变化，而是在原位置，绿色div遮挡住了部分红色div

### 绝对定位与固定定位

相对定位可以看作特殊的普通流定位，元素位置是相对于它在普通流中位置发生变化，而绝对定位使元素的位置与文档流无关，也不占据文档流空间，普通流中的元素布局就像绝对定位元素不存在一样

绝对定位的元素的位置是相对于距离最近的非static祖先元素位置决定的。如果元素没有已定位的祖先元素，那么他的位置就相对于初始包含块儿（body或html神马的）元素。

因为绝对定位与文档流无关，所以绝对定位的元素可以覆盖页面上的其他元素，可以通过z-index属性控制叠放顺序，z-index越高，元素位置越靠上。

还是刚才的例子，稍微改动一下，让绿色div绝对定位，为了清晰显示，第二个红色div改为黄色

```html
	<div style="border: solid 1px #0e0; width:200px; position:relative;">
    <div style="height: 100px; width: 100px; background-color: Red;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green; position:absolute; top:20px; left:20px;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;">
    </div>
	</div>
```

<div style="border: solid 1px #0e0; width:200px; position:relative;">
    <div style="height: 100px; width: 100px; background-color: Red;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green; position:absolute; top:20px; left:20px;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;">
    </div>
</div>


这时可以看出，绿色div是相对于父元素，也就是绿框div进行的移位，而红色和黄色div进行布局时就像绿色div不存在一样

最后要说的就是fixed属性了，应用fixed也叫固定定位，固定定位是绝对定位的一种，固定定位的元素也不包含在普通文档流中，差异是固定元素的包含块儿是视口（viewport），经常见一些页面的如人人网看在线好友那个模块总在窗口右下角，估计用的是类似技术

```html
	<div style="border: solid 1px #0e0; width:200px;">
    <div style="height: 100px; width: 100px; background-color: Red;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green; position:fixed; bottom:20px; left:20px;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;">
    </div>
	</div>
```

![](/img/201210131402313535.png)
![](/img/201210131402351710.png)

可见红色和黄色div布局没有受到绿色div影响，而无论是页面纵向滚动条在页面顶端还是底端，绿色div总是在视口左下角



### 浮动基本概念

首先介绍一些浮动模型的基本知识：浮动模型也是一种可视化格式模型，浮动的框可以左右移动（根据float属性值而定），直到它的外边缘碰到包含框或者另一个浮动元素的框的边缘。浮动元素不在文档的普通流中，文档的普通流中的元素表现的就像浮动元素不存在一样.《CSS Mastery》里作者画了几个图非常有意思，可以帮助我们理解浮动的表现，我简单的画几个

`正常`

```html
	<div style="border: solid 5px #0e0; width:300px;">
    <div style="height: 100px; width: 100px; background-color: Red;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green; ">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;">
    </div>
	</div>
```

<div style="border: solid 5px #0e0; width:300px;">
    <div style="height: 100px; width: 100px; background-color: Red;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green; ">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;">
    </div>
</div>

`红向右浮动`
```html
	<div style="border: solid 5px #0e0; width:300px;">
    <div style="height: 100px; width: 100px; background-color: Red; float:right;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green; ">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;">
    </div>
	</div>
```

<div style="border: solid 5px #0e0; width:300px;">
    <div style="height: 100px; width: 100px; background-color: Red; float:right;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green; ">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;">
    </div>
</div>

`红框左移,覆盖绿框`

```html
	<div style="border: solid 5px #0e0; width:300px;">
    <div style="height: 100px; width: 100px; background-color: Red;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;">
    </div>
	</div>
```

<div style="border: solid 5px #0e0; width:300px;">
    <div style="height: 100px; width: 100px; background-color: Red;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;">
    </div>
</div>

`都向左浮动,父元素宽度为0`

```html
	<div style="border: solid 5px #0e0; width:300px;">
    <div style="height: 100px; width: 100px; background-color: Red;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;  float:left;">
    </div>
	</div>
```    

<div style="border: solid 5px #0e0; width:300px;">
    <div style="height: 100px; width: 100px; background-color: Red;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;  float:left;">
    </div>
</div>

如果包含块儿太窄无法容纳水平排列的三个浮动元素,那么其它浮动块儿向下移动,直到有足够的空间,如果浮动元素的高度不同,那么向下移动的时候可能被卡住

`没有足够水平空间`

```html
	<div style="border: solid 5px #0e0; width:250px;">
    <div style="height: 100px; width: 100px; background-color: Red;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;  float:left;">
    </div>
	</div>
```

<div style="border: solid 5px #0e0; width:250px;">
    <div style="height: 100px; width: 100px; background-color: Red;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;  float:left;">
    </div>
</div>


`卡住了`

```html
	<div style="border: solid 5px #0e0; width:250px;">
    <div style="height: 120px; width: 100px; background-color: Red;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;  float:left;">
    </div>
	</div>
```

<div style="border: solid 5px #0e0; width:250px;">
    <div style="height: 120px; width: 100px; background-color: Red;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;  float:left;">
    </div>
</div>

### 行框

前面指出浮动会让元素脱离文档流,不影响不浮动元素.实际上并不完全如此,如果浮动的元素后面有一个文档流中元素,那么这个元素的框会表现的像浮动元素不存在,但是框的文本内容会受到浮动元素的影响,会移动以留出空间.用术语说就是浮动元素旁边的行框被缩短,从而给浮动元素流出空间,因而行框围绕浮动框

`不浮动`

```html
	<div style="border: solid 5px #0e0; width: 250px;">
    <div style="height: 50px; width: 50px; background-color: Red;"></div>
    <div style="height: 100px; width: 100px; background-color: Green;">
       11111111111
       11111111111
    </div>
	</div>
```   

<div style="border: solid 5px #0e0; width: 250px;">
    <div style="height: 50px; width: 50px; background-color: Red;"></div>
    <div style="height: 100px; width: 100px; background-color: Green;">
       11111111111
       11111111111
    </div>
</div>

`浮动`

```html
	<div style="border: solid 5px #0e0; width: 250px;">
    <div style="height: 50px; width: 50px; background-color: Red; float:left;"></div>
    <div style="height: 100px; width: 100px; background-color: Green;">
       abc def ghi
       abc def ghi
       abc def ghi
    </div>
	</div>
```

<div style="border: solid 5px #0e0; width: 250px;">
    <div style="height: 50px; width: 50px; background-color: Red; float:left;"></div>
    <div style="height: 100px; width: 100px; background-color: Green;">
       abc def ghi
       abc def ghi
       abc def ghi
    </div>
</div>

可以看出浮动后虽然绿色div布局不受浮动影响，正常布局，但是文字部分却被挤到了红色浮动div外边。要想阻止行框围绕在浮动元素外边，可以使用clear属性，属性的left，right，both，none表示框的哪些边不挨着浮动框

```html
    <div style="border: solid 5px #0e0; width: 250px;">
        <div style="height: 50px; width: 50px; background-color: Red; float:left;"></div>
        <div style="height: 100px; width: 100px; background-color: Green; clear:both;">
           11111111111
           11111111111
        </div>
    </div>
```

<div style="border: solid 5px #0e0; width: 250px;">
    <div style="height: 50px; width: 50px; background-color: Red; float:left;"></div>
    <div style="height: 100px; width: 100px; background-color: Green; clear:both;">
       11111111111
       11111111111
    </div>
</div>

### 清理浮动

对元素清理实际上为前面的浮动元素留出了垂直空间,这样可以解决我们之前的一个问题，看前面的图片的时候我们发现div内的所有元素浮动的话就会不占据文档空间，这样父元素，高度为0，可能很多效果也不见了

`都向左浮动,父元素宽度为0`

```html
        <div style="border: solid 5px #0e0; width:300px;">
        <div style="height: 100px; width: 100px; background-color: Red;  float:left;">
        </div>
        <div style="height: 100px; width: 100px; background-color: Green;  float:left;">
        </div>
        <div style="height: 100px; width: 100px; background-color: Yellow;  float:left;">
        </div>
    </div>
```

<div style="border: solid 5px #0e0; width:300px;">
    <div style="height: 100px; width: 100px; background-color: Red;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;  float:left;">
    </div>
</div>

如果我们想让父元素在视觉上包围浮动元素可以像下面这样处理,在最后添加一个空div，对它清理

```html
	<div style="border: solid 5px #0e0; width:300px;">
    <div style="height: 100px; width: 100px; background-color: Red;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;  float:left;">
    </div>
    <div style="clear:both;"></div>
    </div>
```

<div style="border: solid 5px #0e0; width:300px;">
    <div style="height: 100px; width: 100px; background-color: Red;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Green;  float:left;">
    </div>
    <div style="height: 100px; width: 100px; background-color: Yellow;  float:left;">
    </div>
    <div style="clear:both;"></div>
</div>

当然这样做有很多缺点，有些javascript也可以做出类似效果，这里不细说，值得注意的是应用值为hidden或auto的overflow属性会有一个副作用：自动清理包含的任何浮动元素，所以说当页面出现相关问题时，可以看看是不是这个属性搞的鬼


### BFC清理浮动

BFC的全称是 [Block Format Content](http://www.w3.org/TR/CSS21/visuren.html#block-formatting)，有三个特性

* BFC会阻止垂直外边距（margin-top、margin-bottom）折叠

> 按照BFC的定义，只有同属于一个BFC时，两个元素才有可能发生垂直Margin的重叠，这个包括相邻元素，嵌套元素，只要他们之间没有阻挡(例如边框，非空内容，padding等)就会发生margin重叠。 因此要解决margin重叠问题，只要让它们不在同一个BFC就行了，但是对于两个相邻元素来说，意义不大，没有必要给它们加个外壳，但是对于嵌套元素来说就很有必要了，只要把父元素设为BFC就可以了。这样子元素的margin就不会和父元素的margin发生重叠

* BFC不会重叠浮动元素

* BFC可以包含浮动

我们可以利用BFC的第三条特性来“清浮动”，这里其实说清浮动已经不再合适，应该说包含浮动。也就是说只要父容器形成BFC就可以，简单看看如何形成BFC

* float为 left|right

* overflow为 hidden|auto|scroll

* display为 table-cell|table-caption|inline-block

* position为 absolute|fixed

我们可以对父容器添加这些属性来形成BFC达到“清浮动”效果

### 局限性

上面提到使用BFC使用float的时候会使父容器长度缩短，而且还有个重要缺陷——父容器float解决了其塌陷问题，那么父容器的父容器怎么办？难道要全部使用folat吗（确实有这种布局方式倒是）。BFC的几种方式都有各自的问题，overflow属性会影响滚动条和绝对定位的元素；position会改变元素的定位方式，这是我们不希望的，display这几种方式依然没有解决低版本IE问题。。。

#### hasLayout

我们知道在IE6、7内有个hasLayout的概念，很多bug正是由hasLayout导致的

* 当元素的hasLayout属性值为false的时候，元素的尺寸和位置由最近拥有布局的祖先元素控制
* 当元素的hasLayout属性值为true的时候会达到和BFC类似的效果，元素负责本身及其子元素的尺寸设置和定位

我们可以利用这点儿在IE6、7下完成清浮动，先看看怎么使元素hasLayout为true

* position: absolute

* float: left|right

* display: inline-block

* width: 除 “auto” 外的任意值
* height: 除 “auto” 外的任意值
* zoom: 除 “normal” 外的任意值
* writing-mode: tb-rl

* 在IE7中使用overflow: hidden|scroll|auto 也可以使hasLayout为true

### 通用的清理浮动法案

```css
    .clearfix{
        *zoom:1;
    }
    .clearfix:after{
        content:".";
        display:block;
        height:0;
        visibility:hidden;
        clear:left;
    }
```

```css
	.clearfix{
    	*zoom:1;
    }
    .clearfix:after{
        content:"";
        display:table;
        clear:both;
    }
```

### 两种方案

虽然我们得出了一种浏览器兼容的靠谱解决方案，但这并不代表我们一定得用这种方式，很多时候我们的父容器本身需要position：absolute等形成了BFC的时候我们可以直接利用这些属性了，大家要掌握原理，活学活用。总而言之清理浮动两种方式

1. 利用 clear属性，清除浮动
2. 使父容器形成BFC





## ``

`A:`


## 代码

