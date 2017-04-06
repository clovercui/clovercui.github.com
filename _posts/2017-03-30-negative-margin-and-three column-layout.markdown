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

`A:`

圣杯布局是一个两边在父容器里顶宽，中间自适应的布局并且中间主内容首先渲染。设置三个div浮动，左右两栏加上负margin使其与中间栏并排。然后中间栏设置左右padding，再对两个边栏使用position: relative属性并设置left定位使其不遮挡中间栏。

1. 写好一个三栏HTML结构
	![圣杯布局HTML结构](/img/2155778-17f6d5af52b85b31.png)
2. 使用浮动与负margin写好一个三栏并排的布局
	![三栏并排](/img/2155778-0f4b4e9a132b3e6c.png)
3. 给父容器设置左右padding，然后给两个边栏设置position: relative; 并设置left值定位到父容器的两边顶宽
	![圣杯布局](/img/2155778-b7faed23e04b72fa.png)
4. 设置圣杯布局的两栏布局只需要删除父容器的右内边距
	![删除父容器右内边距形成圣杯两栏布局](/img/2155778-84f6223248a67ed5.png)

## `双飞翼布局的原理? 实现步骤?`

`A:`

双飞翼布局也是一个两边在父容器里顶宽，中间自适应的布局并且中间主内容首先渲染。设置三个div浮动，左右两栏加上负margin使其与中间栏并排的布局。区别在于双飞翼布局中间的div设置了子div来放置内容，在这个子div里设置了margin来给两边的div留出了位置

1. 写好一个三栏HTML结构，与圣杯不同的是在主内容里增加一个子div
	![双飞翼HTML结构](/img/2155778-502dedc533521aac.png)
2. 使用浮动与负margin写好一个三栏并排的布局
	![双飞翼三栏并排](/img/2155778-95a81eef83a74dae.png)
3. 给中间栏设置左右margin值形成双飞翼布局
	![给中间栏设置左右margin值形成双飞翼布局](/img/2155778-536227648f0b26f4.png)
4. 双飞翼布局形成两栏布局只需要删除一边侧栏并且删除主内容的一边外边距
	![删除一栏和一侧外边距](/img/2155778-12036b91f4e4d094.png)



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

`代码`

```html
    <!DOCTYPE html>
    <html>
        <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
        <title>圣杯布局</title>
        <meta name="description" content="">
        <meta name="keywords" content="">

        <!--css引入区-->
        <link href="" rel="stylesheet">
        <!--js引入区-->
        <script type="text/javascript" src=""></script>
        <style type="text/css">

        html,body,ul,p,h1,h2,h3{
            margin:0px;
            padding:0px;
        }
            .contain{

                background: #ccc;
                padding:0 200px;
            }

            .main{
                float:left;
                width:100%;
                height:100%;
                background: red;


            }
            .aside{
                float:left;
                background: yellow;
                width:200px;
                height:200px;
                margin-left:-100%;
                position: relative; 
                left: -200px;
            }

            .bside{
                background: blue;
                width:200px;
                height:200px;
                float:left;
                margin-left:-200px;
                position: relative; 
                left: 200px;
            }

             .clear::after{
                content: "";
                display: block;
                clear:both;
           }

        </style>
        </head>
        <body>
            <div class="contain clear">
            这里是主题
                <div class="main">
                     这里是内容<br>
                        这里是内容<br>这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                         这里是内容<br>
                        这里是内容<br>这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                </div>
                <div class="aside"></div>
                <div class="bside"></div>
            </div>


        </body>
    </html>
```


### `双飞翼布局`

> [双飞翼布局DEMO](/demo/双飞翼布局.html)

```html
    <!DOCTYPE html>
    <html>
        <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
        <title>双飞翼布局</title>
        <meta name="description" content="">
        <meta name="keywords" content="">

        <!--css引入区-->
        <link href="" rel="stylesheet">
        <!--js引入区-->
        <script type="text/javascript" src=""></script>
        <style type="text/css">

        html,body,ul,p,h1,h2,h3{
            margin:0px;
            padding:0px;
        }
            .contain{


                background: #ccc;

            }

            .main{
                float:left;
                width:100%;
                height:100%;


            }
            .warp{
                /*width:100%;*/
                height:100%;
                margin:0 200px;
                background: red;
            }
            .aside{
                float:left;
                background: yellow;
                width:200px;
                height:200px;
                margin-left:-100%;          
            }

            .bside{
                background: blue;
                width:200px;
                height:200px;
                float:left;
                margin-left:-200px;

            }

             .clear::after{
                content: "";
                display: block;
                clear:both;
           }

        </style>
        </head>
        <body>
            <div class="contain clear">
             <p>这里是整体</p>
                <div class="main">
                    <div class="warp">
                        这里是内容<br>
                        这里是内容<br>这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                         这里是内容<br>
                        这里是内容<br>这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                        这里是内容<br>
                    </div>
                </div>
                <div class="aside"></div>
                <div class="bside"></div>
            </div>
        </body>
    </html>
```