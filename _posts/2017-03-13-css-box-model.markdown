---
layout:     post
title:      "CSS盒模型-1"
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

# CSS盒模型

![标准盒模型](/img/201503151.JPG)
![IE盒模型](/img/201503152.JPG)

==区别==：W3C标准中padding、border所占的空间不在width、height范围内，大家俗称的IE的盒模型width包括content尺寸＋padding＋border

早期IE6、IE7使用“IE盒模型”，后来更新了一次，更新后的IE6、IE7使用标准盒模型IE8及以上版本使用标准盒模型

没有DOCTYPE的情况下使用怪异模式，IE也使用“IE盒模型”

==兼容方案==：使用css3新样式box-sizing

* content-box：w3c标准盒模型
* border-box：“IE盒模型”

## `盒模型包括哪些属性`

`A:`

## `如果遇到一个属性想知道兼容性，在哪查看?`

`A:`

## `IE 盒模型和W3C盒模型有什么区别?`

`A:`

## `以下代码的作用？兼容性？`

```css
*{
  box-sizing: border-box;
}
```
## 代码
```html
<a class="btn" href="#">确定</a>
<span class="btn" >取消</span>
<div class="btn">提交</div>
<button class="btn"> 发送</button>
```
<style>
.demo{
width:100%;
height:200px;
border:1px solid #ccc;
}
<style>

<div class="demo">
</div>




