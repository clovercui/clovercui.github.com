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

* 透明度opacity

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



## `针对兼容、多浏览器覆盖有什么看法？渐进增强和**优雅降级**是什么意思？`

`A:`

## `reset.css和normalize.css分别是做什么的？为什么推荐使用nomalize.css?`

`A:`

## `IE盒模型和标准盒模型有什么区别? 怎样使 IE7、8使用标准盒模型?box-sizing:border-box有什么作用`






### 参考

>[条件注释 ](http://zh.wikipedia.org/wiki/%E6%9D%A1%E4%BB%B6%E6%B3%A8%E9%87%8A)

>[史上最全的CSS hack方式一览](http://blog.csdn.net/freshlover/article/details/12132801)

>[http://www.teaching-materials.org/csstools/](http://www.teaching-materials.org/csstools/)

>[知乎：怎样可以很好地保证网页的浏览器兼容性](https://www.zhihu.com/question/19736007)

>[让我们谈一谈 Normalize.css](http://jerryzou.com/posts/aboutNormalizeCss/)

>[知乎: Normalize.css 与传统的 CSS Reset 有哪些区别](https://www.zhihu.com/question/20094066)

