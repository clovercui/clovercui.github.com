---
layout:     post
title:      "Linux文件加密之ccrypt"
subtitle:   "用于文件和数据流加密及解密"
date:       2016-08-16
author:     "Clover"
header-img: "img/contact-bg.jpg"
catalog: true
tags:
    - Clover
    - Linux


---
#ccrypt

`ccrypt是为了取代UNIX crypt而设计的，这个实用工具可用于文件和数据流加密及解密。它使用Rijndael密码`

---
## 安装

```
sudo apt-get install ccrypt
# yum install ccrypt
```

---
## 使用

* 加密文件

```
ccencrypt filename
```
输入两次密码

* 解密文件

```
ccdecrypt filename
```

提供加密时输入的同一个密码才能解密


它使用ccencrypt来加密、使用ccdecrypt来解密。一定要注意，加密时，原始文件(tecmint.txt)换成了tecmint.txt.cpt;解密时，加密文件(tecmint.txt.cpt)换成了原始文件(tecmint.txt)。你可以使用ls命令来予以核查
