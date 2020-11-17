---
title: Java学习笔记
date: 2020/10/29 17:08:52
comments: false
cover: https://i.loli.net/2020/10/29/CnS9qr2btcygmGB.png
coverHeight: 750
coverWidth: 1200
categories:
   - 笔记
tags:
   - Java
copyright: '<strong>版权声明：</strong>本文采用 <a href="https://creativecommons.org/licenses/by-nc-sa/3.0/cn/deed.zh" target="_blank">CC BY-NC-SA 3.0 CN</a> 协议进行许可'
---

*总结自己作为菜鸡在Java学习过程中遇见的新的发现，可能是错误的，随着学习深入，会进行再思考。*

<!--more-->

1. 继承中的super

   经过实践，关于视频教程中，“子类构造方法默认访问父类无参构造方法，若父类没有无参构造方法，则会报错。”描述并不完整，应为：*“当父类构造方法全为带参时，子类必须带有构造方法，且必须使用super(参数)，否则报错。”*而父类没有构造方法，仅有成员方法时，并不会出现上述情况。