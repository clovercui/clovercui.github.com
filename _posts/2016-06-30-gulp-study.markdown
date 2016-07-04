---
layout:     post
title:      "Gulp-前端自动化构建工具"
subtitle:   "Gulp学习笔记"
date:       2016-06-30
author:     "Clover"
header-img: "img/404-bg.jpg"
catalog: true
tags:
    - Clover
    - Web
    - HTML
    - Gulp
    
---

# Gulp-Web前端自动化开发工具

---

### 入门指南

####1. 全局安装 gulp

```
$ npm install --global gulp
```

####2. 作为项目的开发依赖（devDependencies）安装

```
$ npm install --save-dev gulp
```

####3. 在项目根目录下创建一个名为 gulpfile.js 的文件

```
var gulp = require('gulp');

gulp.task('default', function() {
  // 将你的默认的任务代码放在这
});
```   

####4. 运行 gulp：
