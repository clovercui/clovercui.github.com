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
|    !   |  [if !IE]  |   The NOT operator. This is placed immediately in front of the feature, operator, or subexpression to reverse the Boolean meaning of the expression.
NOT运算符。这是摆立即在前面的功能，操作员，或子表达式扭转布尔表达式的意义。     |
|   lt    |  [if lt IE 5.5] |    The less-than operator. Returns true if the first argument is less than the second argument.
小于运算符。如果第一个参数小于第二个参数，则返回true。    |
|lte      | [if lte IE 6] |   The less-than or equal operator. Returns true if the first argument is less than or equal to the second argument.
小于或等于运算。如果第一个参数是小于或等于第二个参数，则返回true。     |
|  gt    | [if gt IE 5] |    The greater-than operator. Returns true if the first argument is greater than the second argument.
大于运算符。如果第一个参数大于第二个参数，则返回true。    |
|  gte   |[if gte IE 7] |    The greater-than or equal operator. Returns true if the first argument is greater than or equal to the second argument.
大于或等于运算。如果第一个参数是大于或等于第二个参数，则返回true。    |
|  ( )   | [if !(IE 7)] |   Subexpression operators. Used in conjunction with boolean operators to create more complex expressions.
子表达式运营商。在与布尔运算符用于创建更复杂的表达式。     |
|  &    |[if (gt IE 5)&(lt IE 7)]|   The AND operator. Returns true if all subexpressions evaluate to true
AND运算符。如果所有的子表达式计算结果为true，返回true     |
|  ==|==    | ==[if (IE 6)|(IE 7)]== |    The OR operator. Returns true if any of the subexpressions evaluates to true.
OR运算符。返回true，如果子表达式计算结果为true    |