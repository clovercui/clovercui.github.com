---
layout:     post
title:      "CSS常用技巧"
subtitle:   ""
date:       2017-03-13
author:     "Clover"
header-img: "img/education_large_2x.jpg"
catalog: true
tags:
    - Clover
    - 前端
    - HTML
    - CSS

---

# CSS常用技巧

## `CSS Sprite(雪碧图|精灵图)指什么? 有什么作用`

`A:`

`雪碧图` 就是将很多小图标合并成一张图，这样在加载图片时只需要走一次网络请求，然后用`background-position`定位来显示部分图片

作用：

1. 能够减少页面的请求数、降低图片占用的字节，以此来达到提升页面访问速度的目的
2. 提高页面的加载速度；

> [参考](http://riny.net/2014/compass-sprite/)

## `img标签和CSS背景使用图片在使用场景上有何区别`

`A:`

`img`标签适用于经常改变的情况，会通过后台更新的数据,例如商品的图片，任务的照片信息；
`.css`背景适用于不会经常改变的情况，例如 图标等；

## `title 和 alt属性分别有什么作用`

`A:`

`title`用于显示额外提示文字，鼠标放在元素上面时会显示。
`alt`用于在无法加载图片时，替代图片显示,并且对SEO爬虫友好，可以提升网页的权重。

## `background: url(abc.png) 0 0 no-repeat;这句话是什么意思`

`A:`
背景图片使用css同级目录下的abc.png 位置偏移 X轴0px,Y轴0px；不重复。

## `background-size有什么作用? 兼容性如何? 常用的值是?`

`A:`

CSS 的 background-size 属性能调整背景图片的大小，从而替代了用原始大小显示图片的默认行为
常用的值有以下几类：

auto——原始图片大小；

length——根据给定长度值调整背景图片大小；

percentage——根据给定的百分比调整图片大小；

contain——按比例调整背景图片，使得其图片宽高比自适应整个元素的背景区域的宽高比，因此假如背景图片过大，而背景区域的整体宽高比不能恰好包含背景图片的话，那么其背景区域会出现空白，这个值多用于响应式页面；

cover——按比例调整背景图片，这个值的属性和contain正好相反，背景图会按照比例填充背景区域，如果背景图片过大且不能正好按照宽高比包含背景区域，那么背景图片就会被裁减显示不全；
[参考文档](http://www.webhek.com/background-size/)

![兼容性](/img/145951-71351468654e5c7b.png)

## `如何让一个div水平居中？如何让图片水平居中`

`A:`

可对div(块级元素)设置margin：0 auto

设置水平居中 对图片(行内元素)设置text-align:center

设置水平居中 或者设置display:block，按照块级元素设置；

## `如何设置元素透明? 兼容性？`

`A:`

```css
div{ 
opacity:0.6;
filter:Alpah(opacity=60)/*IE8及之前版本*/
}
```

![兼容性](/img/145951-923845199b1b1688.png)


## `opacity 和 rgba都能设置透明，有什么区别`

`A:`

rgba只能作用于颜色或背景色，并不能使设置的颜色透明化；

opacity作用于元素以及元素内部的所有元素的透明度；俩者的兼容性都需要IE8以上版本的支持

[参考](http://blog.csdn.net/q285661571/article/details/7536490)
