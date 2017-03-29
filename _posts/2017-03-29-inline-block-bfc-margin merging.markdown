---
layout:     post
title:      "inline-block、BFC、边距合并"
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

# inline-block、BFC、边距合并

## `在什么场景下会出现外边距合并？如何合并？如何不让相邻元素外边距合并？给个父子外边距合并的范例`

`A`

外边距合并[Margin Collapse]：同属于同一BFC的block,垂直方向上都会发生合并

合并规则为：不论对于时父子间还是同级兄弟之间都遵循，同号取绝对值大的为间距，异号向加结果作间距

处理方式：根据产生原因可知，只要元素不在同一BFC之内，就不会发生margin collapase，所以可以overflow非visible， float非none， position非静态和相关， display：inline-block／table-cell... 等脱离文档流的手段，相对来说overflow设置比较简单


## `去除inline-block内缝隙有哪几种常见方法?`

`A:`

inline-block如果不作处理会在个元素之间带来一个空隙

分析原因：这是由于inline元素中，内容的 空格和回车都会显示为一个空的字符导致的，所以解决办法就是想办法去除这个空的字符，可以：

* inline元素之间不加空格跟回车，内容元素直接相邻
* 通过字体处理，包裹的父元素字体大小设置为0，而内容的元素字体大小正常
* 通过margin=-4来处理
* 最好的方式就是，不用inline-block而使用float替代 

## `容器使用overflow: auto| hidden撑开高度的原理是什么？`

本质上来说就是因为触发建立了BFC，而BFC就是能让上下左右都包裹住哪怕是浮动的元素

## `BFC是什么？如何形成BFC，有什么作用?`

`A:`

>Boxes in the normal flow belong to a formatting context, which may be block or inline, but not both simultaneously. Block-level boxes participate in a block formatting context. Inline-level boxes participate in an inline formatting context.

在w3c规范中这样说道，每个文档流中的盒子必定属于一个格式化环境，要么块级格式化环境，要么行内格式化环境，其中，块级元素在块级格式化环境中，行内元素在行内格式化环境中
其实HTML根元素html就是一个默认的块级格式化上下文，因为当页面的内容在视窗显示不下的时候，会出现滚动条，意味着html根元素的overflow属性的值为auto
那么块级格式化上下文是什么呢，通俗来说，就是一个独立的布局环境，外面的元素不能影响里面的元素，里面的元素也不能影响外面的元素，那么BFC的布局规则是怎样的呢

* BFC布局规则

	1. 内部的BOX会在垂直方向，一个接一个地放置
	2. BOX垂直方向的距离由margin决定。属于同一个BFC的两个相邻的BOX的margin会发生重叠
	3. 每个元素的margin-box的左边与包含块的border-box的左边相接触（对于从左往右的格式，反之相反）。及时存在浮动也是如此
	4. BFC的区域不会与float box重叠
	5. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此
	6. 计算BFC高度时，浮动元素也参与计算

* 如何触发BFC

>Floats, absolutely positioned elements, block containers (such as inline-blocks, table-cells, and table-captions) that are not block boxes, and block boxes with 'overflow' other than 'visible' (except when that value has been propagated to the viewport) establish new block formatting contexts for their contents.

浮动元素，绝对定位元素，块级容器但是不是块级盒子（例如：display的值为inline-block、table-cell、table-caption），块级元素的overflow属性的值不为visible

* BFC的作用

    1. 清除内部浮动
    2. 自适应两栏布局
    3. 阻止垂直外边距合并

BFC的用法都体现了规则的第5条：

>BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此

因为BFC内部和外部互不影响，所以当外部元素浮动时，BFC通过自身变窄不与浮动元素重叠来消除的影响，而当内部元素浮动时，为了不影响外部元素，所以把浮动元素的高度计算在内，阻止margin合并也是为了内部元素不与外部元素互相影响

## 代码

### `实现如下效果的导航条`

![](/img/a777a42b7fc6a7c3b4a8ff0afa88fb76f8cb75d4.gif)

```html
    <!--
        @authors Clover (clover_cui@163.com)
        @date    2017-03-28 17:03:08  
    -->
    <!DOCTYPE html>
    <html>
        <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
        <title>导航</title>
        <meta name="description" content="">
        <meta name="keywords" content="">

        <!--css引入区-->
        <link href="" rel="stylesheet">
        <!--js引入区-->
        <script type="text/javascript" src=""></script>
        <style type="text/css">
            body, ul, li, p, h1, h2, h3, h4, h5, h6 {
                margin: 0;
                padding: 0;
            }

            body {
                font: 14px/1.5 "Microsoft YaHei","微软雅黑",sans-serif;
                color: #656565;
            }
            ul{
                list-style: none;
            }
            .contain{
                margin:20px auto;
                height: 300px;
                width:900px;
                border: 1px solid #333;
            }
            .head{
                height:50px;
                background-color: #ccc;
            }
            .nav a{
                color: #656565;
                text-decoration: none;
                display: block;
                padding: 0 20px;
                line-height: 50px;
            }

            .nav>li{ 
                margin:0 20px;              
                float: left;
                position: relative;
            }
            .nav li:hover{
                background-color: #333;

            }
           .nav li:hover>a{
              color:#fff;
           }

            .nav ul{
                display: none;
                /*border: 1px solid #ccc;
                border-top:0px;*/
                position: absolute;
                left:0;
                top:100%;
                background-color: #eee;
            }
             .nav li:hover ul{
                display:block;

             }


            .clear:after{
                content:'';
                display:block;
                clear: both;
            }
        </style>
        </head>
        <body>
            <div class="contain">
            <div style="text-align: center;"><h1>___导航栏演示____<h1></div>
                <div class="head">
                    <ul class="nav clear">
                        <li>
                            <a href="#">前端</a>
                            <ul>
                                <li><a href="#">JavascriptJavascript</a></li>
                                <li><a href="#">CSS</a></li>
                                <li><a href="#">HTML</a></li>
                            </ul>
                        </li>
                        <li>
                            <a href="#">服务端</a>
                            <ul>
                                <li><a href="#">PHP</a></li>
                                <li><a href="#">Node</a></li>
                                <li><a href="#">Java</a></li>
                            </ul>
                        </li>
                        <li>
                            <a href="#">前端工具</a>
                             <ul>
                                <li><a href="#">Sublime</a></li>
                                <li><a href="#">Git</a></li>
                                <li><a href="#">Webstorm</a></li>
                            </ul>
                        </li>
                        <li>
                            <a href="#">HTML</a>
                             <ul>
                                <li><a href="#">Javascript</a></li>
                                <li><a href="#">CSS</a></li>
                                <li><a href="#">HTML</a></li>
                            </ul>
                        </li>
                        <li>
                            <a href="#">NODE</a>
                             <ul>
                                <li><a href="#">Javascript</a></li>
                                <li><a href="#">CSS</a></li>
                                <li><a href="#">HTML</a></li>
                            </ul>
                        </li>
                    </ul>

                </div>
            </div>
        </body>
    </html>
```


[导航条示例](/demo/navdemo.html)

### `利用BFC的原理实现两栏布局`

```html
    <!--
        @authors Clover (clover_cui@163.com)
        @date    2017-03-29 14:45:17  
    -->
    <!DOCTYPE html>
    <html>
        <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
        <title>两栏布局</title>
        <meta name="description" content="">
        <meta name="keywords" content="">

        <!--css引入区-->
        <link href="" rel="stylesheet">
        <!--js引入区-->
        <script type="text/javascript" src=""></script>
        <style>

        html,body,p,h1,h2{
                margin:0;
                padding:0;
            }
            #header{
                height:40px;
                background: red;
            }
            #content .aside{
                background: blue;
                min-height:100px;
                width: 200px;
                float:left;
            }
            #content .main{
                /*margin方法*/
                /*margin-left:200px;*/
                /*bfc*/
                overflow: auto;
                background: pink;
                min-height:500px;
            }
            #footer{
                height:40px;
                background: green;
            }
           .clear{
                content: "";
                display: block;
                clear:both;
           }


        </style>
        </head>
        <body>
            <div id="header">
            头部
            </div>
            <div id="content">
                <div class="aside">我是左边栏</div>
                <div class="main">我是 主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是 主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域我是主体区域</div>
            </div>
            <div id="footer">
            尾部
            </div>
        </body>
    </html>
```


[两栏布局](/demo/两栏布局.html)


## 参考
[w3cplus-理解 bfc 和边距合并1](http://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html)

[w3c-bfc2](http://www.w3.org/TR/CSS2/visuren.html#block-formatting)

[w3c-边距合并1](http://www.w3.org/TR/CSS2/box.html#collapsing-margins)


