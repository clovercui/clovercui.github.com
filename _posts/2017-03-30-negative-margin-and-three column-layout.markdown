---
layout:     post
title:      "负边距、三栏布局"
subtitle:   ""
date:       2017-03-30
author:     "Clover"
header-img: "img/education_large_2x.jpg"
catalog: true
tags:
    - Clover
    - CSS
    - HTML
    - 前端
---

# 负边距、三栏布局

## `负边距在让元素产生偏移时和position: relative有什么区别?`

`A:`

* position:relative,元素偏移后脱离的文档流，但元素原来的位置仍然占据文档流中，不会影响之前的文档流，其之后的元素仍然会认为定位元素在文档流中而跟随在后面。

![](/img/2155778-f3484ab45d48a5b5.png)

* 负margin：元素偏移后并没有脱离文档流，位置变化影响了文档流，之后的元素按文档流的排列特征也会跟着偏移
![](/img/2155778-d95972ee88a29c11.png)

## `使用负 margin 形成三栏布局有什么条件?`

`A:`

1. 必须设置浮动
2. 父容器下有三个块级元素
3. 中间的子元素宽度设置为父容器宽度的100%
4. 左右两栏的子元素设置负边距

## `圣杯布局的原理是怎样的? 简述实现圣杯布局的步骤`


## `双飞翼布局的原理? 实现步骤?`


## 代码


### `用浮动、负边距实现如下效果`

[参考](http://js.jirengu.com/fag/2/edit)

`代码`

```html
    <style>

    .demo{
    height:500px;
    border:1px solid #38b1da;
    overflow:auto;
    }
    ul{
    list-style:none;
    padding:0px;
    margin:0px;
    }
    .content{
        width:600px;
        margin:120px auto;
    }
    .main{
    width:600px;
    background:#ccc;
    }

    .clear:after{  /*清除浮动，让父亲撑开高度*/
          content: '';
          display: block;
          clear: both;
        }

    .ulnav li{
        float:left;
        width:192px;
        margin-left:12px;
        height:100px;
        margin-top: 10px;
        background:green;
    }
    .ulnav{
        margin-left: -12px;
        padding-bottom: 10px;
    }

    </style>

    <div class="demo">
        <div class="content">
            <div class="main">
                <ul class="ulnav clear">
                    <li>1</li>
                    <li>2</li>
                    <li>3</li>
                    <li>4</li>
                    <li>5</li>
                    <li>6</li>
                </ul>
            </div>

        </div>
    </div>

```



<style>

.demo{
height:500px;
border:1px solid #38b1da;
overflow:auto;
}
ul{
list-style:none;
padding:0px;
margin:0px;
}
.content{
	width:600px;
    margin:120px auto;
}
.main{
width:600px;
background:#ccc;
}

.clear:after{  /*清除浮动，让父亲撑开高度*/
      content: '';
      display: block;
      clear: both;
    }

.ulnav li{
	float:left;
	width:192px;
    margin-left:12px;
    height:100px;
    margin-top: 10px;
    background:green;
}
.ulnav{
    margin-left: -12px;
    padding-bottom: 10px;
}

</style>

<div class="demo">
	<div class="content">
    	<div class="main">
        	<ul class="ulnav clear">
            	<li>1</li>
                <li>2</li>
                <li>3</li>
                <li>4</li>
                <li>5</li>
                <li>6</li>
            </ul>
        </div>
    
    </div>
</div>

### `圣杯布局`

> [圣杯布局demo](/demo/圣杯布局.html)

### `双飞翼布局`

> [双飞翼布局DEMO](/demo/双飞翼布局.html)