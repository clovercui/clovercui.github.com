---
layout:     post
title:      "CSS选择器"
subtitle:   ""
date:       2017-03-09
author:     "Clover"
header-img: "img/education_large_2x.jpg"
catalog: true
tags:
    - Clover
    - 前端
    - HTML
    - CSS

---

# CSS选择器

## 选择器类型

* 基础选择器

|选择器|	含义|
|--------|--------|
|*	|通用元素选择器，匹配页面任何元素（这也就决定了我们很少使用）|
|#id	|id选择器，匹配特定id的元素|
|.class	|类选择器，匹配class包含(不是等于)特定类的元素|
|element|	标签选择器|

* 组合选择器

|选择器|	含义|
|---|---|
|E,F	|多元素选择器，用,分隔，同时匹配元素E或元素F|
|E F	|后代选择器，用空格分隔，匹配E元素所有的后代（不只是子元素、子元素向下递归）元素F|
|E>F	|子元素选择器，用>分隔，匹配E元素的所有直接子元素|
|E+F	|直接相邻选择器，匹配E元素之后的相邻的同级元素F|
|E~F	|普通相邻选择器（弟弟选择器），匹配E元素之后的同级元素F（无论直接相邻与否）|
|.class1.class2	|id和class选择器和选择器连写的时候中间没有分隔符，. 和 # 本身充当分隔符的元素|
|element#id	|id和class选择器和选择器连写的时候中间没有分隔符，. 和 # 本身充当分隔符的元素|

* 属性选择器

|选择器	|含义|
|---|---|
|E[attr]	|匹配所有具有属性attr的元素，div[id]就能取到所有有id属性的div|
|E[attr = value]	|匹配属性attr值为value的元素，div[id=test],匹配id=test的div|
|E[attr ~= value]	|匹配所有属性attr具有多个空格分隔、其中一个值等于value的元素|
|E[attr ^= value]	|匹配属性attr的值以value开头的元素|
|E[attr $= value]	|匹配属性attr的值以value结尾的元素|
|E[attr *= value]	|匹配属性attr的值包含value的元素|

* 伪类选择器

往下看

* 伪元素选择器

|选择器	|含义|
|---|---|
|E::first-line	|匹配E元素内容的第一行|
|E::first-letter	|匹配E元素内容的第一个字母|
|E::before	|在E元素之前插入生成的内容|
|E::after	|在E元素之后插入生成的内容|


## CSS优先级

从高到低分别是

在属性后面使用 !important 会覆盖页面内任何位置定义的元素样式

作为style属性写在元素标签上的内联样式

1. id选择器

2. 类选择器

3. 伪类选择器

4. 属性选择器

5. 标签选择器

6. 通配符选择器

7. 浏览器自定义

这样就可以看出来上面例子肯定使用id选择器的定义了，还有一种复杂的情况，CSS规则由多个选择器组成
```
#test p.class1 {...}
#test .class1.class2 {...}
```
这种场景该使用那条规则呢？

这时候我们可以做个简单的加法运算，id选择器的权值为1000，class选择器为100，标签选择器为10，做一下运算（当然只是为了形象这么说，一万个class选择器加起来也不如一个id选择器优先级高）

这样我们就能得知第二条的规则优先级高一些，但是还有一种情况
```
#parent p.class1
div #child.class1

<div id="parent">
    <p id="child" class="class1">
        Text
    </p>
</div>
```
按照我们刚才的方法计算，两个的权值时相同的，这时候应该用那条规则呢？简单来说一句话谁更具体用谁，也就是权值高的选择器作用的越具体优先级越高

我们可以看到两个选择器作用的元素都是p标签，id选择器权值最高，第一条规则作用在了父元素上，第二条规则直接作用在了p标签本身上，所以我们认为第二条选择器的优先级高

如果两个选择器规权值就是一样，我们改怎么判断呢？ 后面的覆盖前面的！
```
div {color: #333;}
....
div {color: #666;}
```
这样`div`文案的颜色明显会是`#666`

总而言之判断CSS选择器规则优先级很简单，每个选择器本身有优先级，越具体优先级越高


## `列出你知道的伪类选择器`

`A:`

| 选择器 | 含义 |
|--------|--------|
|E:first-child	|匹配元素E的第一个子元素|
|E:link	|匹配所有未被点击的链接|
|E:visited	|匹配所有已被点击的链接|
|E:active	|匹配鼠标已经其上按下、还没有释放的E元素|
|E:hover	|匹配鼠标悬停其上的E元素|
|E:focus	|匹配获得当前焦点的E元素|
|E:lang(c)	|匹配lang属性等于c的E元素|
|E:enabled	|匹配表单中可用的元素|
|E:disabled	|匹配表单中禁用的元素
|E:checked	|匹配表单中被选中的radio或checkbox元素|
|E::selection	|匹配用户当前选中的元素|
|E:root	|匹配文档的根元素，对于HTML文档，就是HTML元素|
|E:nth-child(n)	|匹配其父元素的第n个子元素，第一个编号为1|
|E:nth-last-child(n)	|匹配其父元素的倒数第n个子元素，第一个编号为1|
|E:nth-of-type(n)	|与:nth-child()作用类似，但是仅匹配使用同种标签的元素|
|E:nth-last-of-type(n)	|与:nth-last-child() 作用类似，但是仅匹配使用同种标签的元素|
|E:last-child	|匹配父元素的最后一个子元素，等同于:nth-last-child(1)|
|E:first-of-type	|匹配父元素下使用同种标签的第一个子元素，等同于:nth-of-type(1)|
|E:last-of-type	|匹配父元素下使用同种标签的最后一个子元素，等同于:nth-last-of-type(1)|
|E:only-child	|匹配父元素下仅有的一个子元素，等同于:first-child:last-child或 :nth-child(1):nth-last-child(1)|
|E:only-of-type	|匹配父元素下使用同种标签的唯一一个子元素，等同于:first-of-type:last-of-type或 :nth-of-type(1):nth-last-of-type(1)|
|E:empty	|匹配一个不包含任何子元素的元素，文本节点也被看作子元素|
|E:not(selector)	|匹配不符合当前选择器的任何元素|

==n的取值==

1，2，3，4，5

2n+1, 2n, 4n-1

odd, even

## `:first-child和:first-of-type的作用和区别`

`A:`

`:first-child` 匹配的是某父元素的第一个子元素，可以说是结构上的第一个子元素。

`:first-of-type` 匹配的是该类型的第一个，类型是指什么呢，就是冒号前面匹配到的东西，比如 p:first-of-type，就是指所有p元素中的第一个。这里不再限制是第一个子元素了，只要是该类型元素的第一个就行了，当然这些元素的范围都是属于同一级的，也就是同辈的。

同样类型的选择器 `:last-child ` 和 `:last-of-type`、`:nth-child(n)`  和  `:nth-of-type(n)` 也可以这样去理解。

## `运行如下代码，解析下输出样式的原因`
```
<style>
.item1:first-child{
  color: red;
}
.item1:first-of-type{
  background: blue;
}
</style>
 <div class="ct">
   <p class="item1">aa</p>
   <h3 class="item1">bb</h3>
   <h3 class="item1">ccc</h3>
 </div>
```

`A:`

![展示效果](/img/QQ20170309-144057@2x.png)

`.item1:first-child`匹配到class是item1的第一个子元素，所以 `<p class="item1">aa</p>` 是第一个子元素 展示位文字红色
`.item1:first-of-type` 匹配到class是item1的第一种标签类型，所以 ` <p class="item1">aa</p>` 为第一个`p`标签，`<h3 class="item1">bb</h3>`为第一个`h3`标签，所以 这两个背景色展示位蓝色。


## `text-align: center的作用是什么，作用在什么元素上？能让什么元素水平居中`

`A:` 

作用是让元素内部 行内元素、display为（table-cell、inline和inline-block）的块级元素水平居中，作用在块级元素上。


