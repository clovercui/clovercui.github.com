---
layout:     post
title:      "浏览器兼容"
subtitle:   "css hack和条件注释"
date:       2017-04-06
author:     "Clover"
header-img: "img/education_large_2x.jpg"
catalog: true
tags:
    - Clover
    - CSS
    - HTML
    - 前端
---

# 条件注释

条件注释 (conditional comment) 是于HTML源码中被IE有条件解释的语句。条件注释可被用来向IE提供及隐藏代码。

```html
 	<!--[if IE 6]>
    <p>You are using Internet Explorer 6.</p>
    <![endif]-->
    <!--[if !IE]><!-->
    <script>alert(1);</script>
    <!--<![endif]-->
    <!--[if IE 8]>
    <link href="ie8only.css" rel="stylesheet">
    <![endif]-->
```

> 为了提高与 HTML5 的可互操作性和兼容性，Internet Explorer 10 标准模式和 Quirks 模式中删除了对条件注释的支持。 这意味着，与在其他浏览器中相同，条件注释将被视作一般注释。 使用了条件注释的页面在 Windows Internet Explorer 9 中可正常工作，但在 Internet Explorer 10 中无法正常工作。 不再支持条件注释

| 项目 | 范例 |说明 |
|--------|--------|--------|
|    !   |  [if !IE]  |   The NOT operator. This is placed immediately in front of the feature, operator, or subexpression to reverse the Boolean meaning of the expression.NOT运算符。这是摆立即在前面的功能，操作员，或子表达式扭转布尔表达式的意义。     |
|   lt    |  [if lt IE 5.5] |    The less-than operator. Returns true if the first argument is less than the second argument.小于运算符。如果第一个参数小于第二个参数，则返回true。    |
|lte      | [if lte IE 6] |   The less-than or equal operator. Returns true if the first argument is less than or equal to the second argument.小于或等于运算。如果第一个参数是小于或等于第二个参数，则返回true。     |
|  gt    | [if gt IE 5] |    The greater-than operator. Returns true if the first argument is greater than the second argument.大于运算符。如果第一个参数大于第二个参数，则返回true。    |
|  gte   |[if gte IE 7] |    The greater-than or equal operator. Returns true if the first argument is greater than or equal to the second argument.大于或等于运算。如果第一个参数是大于或等于第二个参数，则返回true。    |
|  ( )   | [if !(IE 7)] |   Subexpression operators. Used in conjunction with boolean operators to create more complex expressions.子表达式运营商。在与布尔运算符用于创建更复杂的表达式。     |
|  &    |[if (gt IE 5)&(lt IE 7)]|   The AND operator. Returns true if all subexpressions evaluate to true AND运算符。如果所有的子表达式计算结果为true，返回true     |
|  &#124; | [if (IE 6)&#124;(IE 7)] |    The OR operator. Returns true if any of the subexpressions evaluates to true. OR运算符。返回true，如果子表达式计算结果为true    |


## CSS hack

CSS hack由于不同厂商的浏览器，比如Internet Explorer,Safari,Mozilla Firefox,Chrome等，或者是同一厂商的浏览器的不同版本，如IE6和IE7，对CSS的解析认识不完全一样，因此会导致生成的页面效果不一样，得不到我们所需要的页面效果。

这个时候我们就需要针对不同的浏览器去写不同的CSS，让它能够同时兼容不同的浏览器，能在不同的浏览器中也能得到我们想要的页面效果。 简单的说，CSS hack的目的就是使你的CSS代码兼容不同的浏览器。当然，我们也可以反过来利用CSS hack为不同版本的浏览器定制编写不同的CSS效果。

### CSS hack分类

CSS Hack大致有3种表现形式，`CSS属性前缀法`、`选择器前缀法`以及`IE条件注释法`（即HTML头部引用if IE）Hack，实际项目中CSS Hack大部分是针对IE浏览器不同版本之间的表现差异而引入的。

1. 属性前缀法(即类内部Hack)：例如 IE6能识别下划线""和星号" "，IE7能识别星号" "，但不能识别下划线""，IE6~IE10都认识"\9"，但firefox前述三个都不能认识

2. 选择器前缀法(即选择器Hack)：例如 IE6能识别html .class{}，IE7能识别+html .class{}或者`*:first-child+html .class{}`

3. IE条件注释法(即HTML条件注释Hack)：针对所有IE(注：IE10+已经不再支持条件注释)： ，针对IE6及以下版本： 。这类Hack不仅对CSS生效，对写在判断语句里面的所有代码都会生效

> [CSS hack查询站->browserhacks](http://browserhacks.com/)

## `如何调试 IE 浏览器`

`A:`

* `使用高版本IE控制台（对于IE7以上）`
* `border: 1px solid red;` （对于IE6以下没有开发者工具，可以使用此方法。看边界来进行调试，这也是调试的重要手段。border不行就再加上background。）
* `outline: 1px solid red;` （IE6不支持。）
* `javascript: alert (document.get ...) ` 在IE里面执行JS，在JS里面写样式进行调试。）
* `利用远程服务器，连接到有对应版本的机器上进行调试`
* `安装多个虚拟机，每个虚拟机安装不同版本的IE浏览器进行测试`
* `利用一些可以对不同版本进行切换的工具，比如IEtester、SuperPreview、Xenocode Browser Sandbox （可以实现真正的在线测试，但是有的要收费。）`


## `什么是CSS hack？在 CSS 和 HTML里如何写 hack？在 CSS 中 IE 7、IE 8的 hack 方式？`

`A:`

* CSS hack由于不同的浏览器，比如Internet Explorer,Safari,Mozilla Firefox,Chrome等，或者是同一厂商的浏览器的不同版本，如IE6和IE7，对CSS的解析认识不完全一样，因此会导致生成的页面效果不一样，得不到我们所需要的页面效果。这个时候我们就需要针对不同的浏览器去写不同的CSS，让它能够同时兼容不同的浏览器，能在不同的浏览器中也能得到我们想要的页面效果。 简单的说，CSS hack的目的就是使你的CSS代码兼容不同的浏览器。当然，我们也可以反过来利用CSS hack为不同版本的浏览器定制编写不同的CSS效果。

* CSS hack分类

	CSS Hack大致有3种表现形式，`CSS属性前缀法`、`选择器前缀法`以及`IE条件注释法`（即HTML头部引用if IE）Hack，实际项目中CSS Hack大部分是针对IE浏览器不同版本之间的表现差异而引入的。
	
    * `属性前缀法`(即类内部Hack)：比如，IE6能识别下划线和星号，IE7能识别星号，但不能识别下划线，IE6~IE10都认识"\9"，但firefox前述三个都不认识。
    * `选择器前缀法`(即选择器Hack)：例如 IE6能识别`*html .class{}`，IE7能识别`*+html .class{}`或者`*:first-child+html .class{}`。
    * `IE条件注释法`(即HTML条件注释Hack)：针对所有IE(`IE10+已经不再支持条件注释`) 
    	
        只在IE下生效
        ```html
            <!--[if IE]> 
            <link rel="stylesheet" type="text/css" href="ie.css">
            <![endif]-->
        ```
        只在IE6下生效
        ```html
            <!--[if IE 6]> 
            <link rel="stylesheet" type="text/css" href="ie6.css">
            <![endif]-->
        ```
        只在IE6以上版本生效
        ```html
            <!--[if gte IE 6]>
            这段文字只在IE6以上(包括)版本IE浏览器显示
            <![endif]-->
        ```
        非IE浏览器生效
        ```html
            <!--[if !IE]>
            这段文字只在非IE浏览器显示
            <![endif]-->
        ```

CSS hack现在用的越来越少，能不用尽量不用，可以用最少的hack去实现多浏览器兼容的页面。所以不需要花很大精力去记，有这个概念，知道一些常见的就行。i知道星号是做ie67的hack，下划线是ie6的hack就行。浏览器越老，bug越多，但是越来越不关注这个了。

## `列举几种 浏览器兼容问题`

`A:`

* `透明度opacity`

```css
opacity: 0.4;
```
![透明度兼容](/img/2244513-ccecb85ada3aeca5.png)

可以看到在IE8是部分支持。

![](/img/2244513-e2ff4d45cfd92135.png)

解决方案：
```css
	filter:alpha(opacity=50);
	zoom: 1; /* IE7需要加上这句，来触发hasLayout，不然没有效果。*/
```
![解决之后](/img/2244513-fbc504cd4abb7efa.png)

* `display: inline-block`

inline元素的display属性设置为inline-block时，所有的浏览器都支持；

block元素的display属性设置为inline-block时，IE6/IE7浏览器是不支持的。

![未解决之前](/img/2244513-2b589ed87f2a6472.png)

`解决方案1：`

```css
*display: inline; /* 将块级元素设置为内联对象呈递。*/
*zoom: 1;  /* 触发haslayout */
```
![](/img/2244513-3d4a06731527638d.png)

`解决方案2：`

先使用`display: inline-block;`属性触发块级元素，然后再定义`display:inline;，`让块元素呈递为内联对象。需要注意的是，两个display 要先后放在两个CSS声明中才有效果，顺序也不能反了。

```css
div{
display: inline-block;
}
div{
*display: inline;
}
```

![](/img/2244513-8f70598ef70099c5.png)

* `块级元素的外边距margin无效`

块级元素设置外边距margin无效，行内元素有效果（当然行内元素只有左右的外边距会有效果）。

![块级元素设置外边距](/img/2244513-44d02caa3ba72bd3.png)
![行内元素设置外边距](/img/2244513-4c463f063dddfd62.png)

在IE中，一个元素要么自己对自身的内容进行计算大小和组织，要么依赖于父元素来计算尺寸，所以子元素的margin失效。解决方法的思想主要是触发haslayout。

`解决方案1：`

给父元素加overflow: hidden;或者overflow: auto;。

![](/img/2244513-bbecdae6f7e3d0a0.png)

`解决方案2：`

![](/img/2244513-d99ed7f89474b7b5.png)

`解决方案3：`

```css
height: 1%; /* 父元素上面 */
```

![](/img/2244513-d1aeb75981d470c9.png)

* `margin加倍`

给ie6的浮动元素添加margin样式的时候，实际的渲染效果是本身设置的外边距的两倍。解决方案是在这个元素里面加上`display: inline`;。

这里需要注意的一点是，清除浮动的时候一定要在父元素上加上`zoom: 1;`，否则没有效果。

![ie6margin加倍](/img/2244513-a168afa8a49cc869.png)

不过按道理来说，这个margin加倍的情况应该是左右方向上都有的，不知道为什么这里只有左边方向上有。

![解决之后](/img/2244513-868cd9bbc698824f.png)
![ie7margin不加倍](/img/2244513-09f00d49cbaa6b21.png)
![ie8margin不加倍](/img/2244513-3708b37931ad74d2.png)

* `文本框不能对齐`

当给input输入框设置了高度之后，在IE9以下会出现文本和文本框不能对齐的现象。

![](/img/2244513-7c8484f8a0103125.png)

`解决方案：`

在input里面增加`vertical-align: middle;。`

![](/img/2244513-a84fcd6c7d902e7b.png)



## `针对兼容、多浏览器覆盖有什么看法？渐进增强和优雅降级是什么意思？`

`A:`

* `针对兼容问题`，首先做个统计，看看使用自己的产品的用户。如果使用IE的比例多（大于5%），可能就要考虑兼容问题。

针对自己的代码，如果效果没有渲染出来，首先看某个元素是否有问题。怎么看呢，在Can I use上面查一下兼容性（当然也可以自己一步步调试代码，一个个删元素，找到问题所在）。如果不兼容，再搜一下解决方案,因为这些版本都比较老了，遇到的问题一般都已经遇到了，一般可以找到解决方案。

* `渐进增强与优雅降级`
	* 渐进增强
	
    开发时先去兼顾，针对某一版本浏览器做开发。开发之后，再对高级浏览器增加一些特别的、额外的一些效果，使得看起来更好看一些，增强用户体验。
	* 优雅降级
	
    开发过程中，不用考虑低版本，等开发完成之后，慢慢去作一个适应。只要页面正常，不乱，就可以了，不用追求特别高的还原度。
    
    * 两者的区别

	`渐进增强`是从一个比较基础的，从能够起作用的版本开始，然后再对不断扩充，来达到要求。`优雅降级`则是从比较复杂的现状开始，把设计对象限定为当前主流的浏览器版本。


## `reset.css和normalize.css分别是做什么的？为什么推荐使用nomalize.css?`

`A:`

* `reset.css`和`normalize.css`

在html当中，一些标签会有默认的样式，比如body、ul等会有外边距。`reset.css`和`normalize.css`就是去掉默认样式的。当然它们也有区别，`normalize.css`是一种`reset.css`的替代方案。

`normalize.css`的作用有：保护有用的浏览器的一些默认样式，而不是完全重置；修复浏览器自身的一些bug；优化css可用性；解释代码。

* 推荐用`nomalize.css`的原因有以下几点：

	* `reset.css`比较暴力，它会对所有标签的默认样式重置为一样的效果；而`normalize.css`比较温和，注重通用的方案，重置掉该重置的样式，保留有用的 user agent 样式
	* `normalize.css`对一些浏览器的 bug 进行修复；而`reset.css`是无法做到的。
	* `normalize.css`不会让调试工具变得杂乱；而`reset.css`在浏览器调试工具中有大段的继承链，显得比较杂乱，难看。



## `IE盒模型和标准盒模型有什么区别? 怎样使 IE7、8使用标准盒模型?box-sizing:border-box有什么作用`

`A:`

* IE盒模型设置的width和height不仅仅是指content内容的大小，还包括border边框和padding内边距；而标准盒模型设置的width和height就是content内容的大小。

* 不添加doctype，也就是IE678怪异模式，使用的是IE盒模型；添加doctype，chrome， ie9+, ie678，使用的是标准盒模型。

* box-sizing是定义元素盒尺寸大小的方式。它的属性值可以为content-box、padding-box、border-box、inherit。

`box-sizing: border-box;`计算方法为width/height=content+padding+border，表示指定的宽度和高度包含边框、内边距和内容区域。这就是上面提到的`IE盒模型`。

## `在 ie 6, 7, 8中展示 盒模型、inline-block、max-width的区别`

`A:`

* `盒模型`
	
    * `IE6`
    ![添加doctype，标准盒模型](/img/2244513-da793b0c1341c630.png)
    ![不添加doctype，IE盒模型](/img/2244513-86c6b5fa373a73eb.png)
	
	* `IE7`
    ![添加doctype，标准盒模型](/img/2244513-e860d8b428d27941.png)
    ![不添加doctype，IE盒模型](/img/2244513-866f2115cc5006b6.png)
    
    * `IE8`
    ![添加doctype，标准盒模型](/img/2244513-19062e0ac59bdec9.png)
    ![不添加doctype，IE盒模型](/img/2244513-e980c2ecdaf04035.png)


在上面的截图中，可以看到当IE为IE盒模型的时候，它变成了长方形，高度变高了。因为在盒模型中，撑起高度的就是line-height，所以这里的高度应该就是字体（这里为空白字符）的line-height造成的。经过测试，发现IE678以及11都有这种情况，这应该是IE的一个bug。
![IE8截图](/img/2244513-91d8536925399037.png)
解决方案就是把字体设为0px就好啦。
![](/img/2244513-8e8982e9b29a9c26.png)

* `inline-block`

	* 块级元素
	
		* IE6
		![ie6不识别inline-block](/img/2244513-7e36315fa850604c.png)
		
		* IE7
		![ie7不识别inline-block](/img/2244513-eb2b6cfcfb9bdac1.png)
		
		* IE8
		![ie8识别inline-block](/img/2244513-4b92e208fb7027ab.png)
    
    * 行内元素
    
    	* IE6
		![ie6中行内元素支持inline-block](/img/2244513-0c89256b1fc6ada3.png)
        可以看到在ie6中，行内元素可以设置宽高。所以在ie6中，行内元素支持inline-block。
		* IE7
		![ie7中行内元素支持inline-block](/img/2244513-98a537249b710d28.png)
        同理，行内元素在ie7中也支持inline-block。
		* IE8
		![ie8中行内元素支持inline-block](/img/2244513-839f16c391b15d07.png)
        
        在ie8中，行内元素也支持inline-block
        
        在Can I use中我们也可以看到，IE67是部分支持。
        
        ![inline-block的兼容性](/img/2244513-91770c02857fa098.png)
       
* `max-width`

 	* `IE6`
 	
    ![ie6不支持max-width](/img/2244513-912384a834d8db4a.png)
	
	* `IE7`
    ![ie7支持max-width](/img/2244513-67b9cae986281d65.png)
    
    * `IE8`
    ![ie8支持max-width](/img/2244513-191b8f553393f15f.png)



## 参考

>[条件注释 ](http://zh.wikipedia.org/wiki/%E6%9D%A1%E4%BB%B6%E6%B3%A8%E9%87%8A)

>[史上最全的CSS hack方式一览](http://blog.csdn.net/freshlover/article/details/12132801)

>[http://www.teaching-materials.org/csstools/](http://www.teaching-materials.org/csstools/)

>[知乎：怎样可以很好地保证网页的浏览器兼容性](https://www.zhihu.com/question/19736007)

>[让我们谈一谈 Normalize.css](http://jerryzou.com/posts/aboutNormalizeCss/)

>[知乎: Normalize.css 与传统的 CSS Reset 有哪些区别](https://www.zhihu.com/question/20094066)

