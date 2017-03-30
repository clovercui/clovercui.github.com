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

## `使用负 margin 形成三栏布局有什么条件?`

## `圣杯布局的原理是怎样的? 简述实现圣杯布局的步骤`

## `双飞翼布局的原理? 实现步骤?`

## 代码

### `用浮动、负边距实现如下效果` 

[参考](http://js.jirengu.com/fag/2/edit)

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
