---
title: 【总结】正则表达式中限定字符和多选结构的作用范围
excerpt: 一个还是一串？
date: 2017-04-10
tags: 正则表达式
categories: blog
layout: post
---

JavaScript 正则表达式中的限定字符及其描述（加一个多选结构）

|限定符|描述|
|:----:|:----:|
|?|x = 0 or x = 1|
|+|x >= 1|
|*|x >= 0|
|{n}| x = n|
|{n,}| x >= n|
|{n,m}| n<= x <=m|
|\||表示两个中的一个|

它们的作用范围究竟是符号本身前的一串字符还是一个字符？

可以理解为它们的作用范围皆为最近的一组字符，但是它们对于“组”的定义不同。

1. 有分组符 `()` 时，毋庸置疑，括号内为一组；
2. 无分组符 `()` 时，对于限定字符，单个描述为一组，对于 `|` 往前截止到开头或左括号 *(*，往后截止到结尾或右括号 *)* 为一组。
    举例：

    1. 对于限定字符,正则表达式 `/abc*/` 中 `*` 限定的仅为字母*c*, 因此匹配的字符串为 *ab/abc/abccc* 等，不匹配 *abcabc*; 
    2. 对于多选结构，正则表达式 `/abc|def/` 匹配的是字符串 *abc* 或 *def*,不匹配 *abcef/abdef*。
    更复杂一点的例子，正则表达式 `/^\d{2}|\d{4}[\dX]$/` 的可以匹配 *以两个数字开头* 和 *以四个数字+一个数字或字母X结尾* 的字符串。

        var re = /^\d{2}|\d{4}[\dX]$/;

        var str1 = 12abcd;
        var str2 = abcd12345;
        var str3 = abcd1234X;
        var str4 = 12345;
        var str5 = 12x;

        re.exec(str1);  // 12
        re.exec(str2);  // 12345
        re.exec(str3);  // 1234X
        re.exec(str4);  // 12
        re.exec(str5);  // 12

    要想匹配 *两个或四个数字开头+一个数字或字母X结尾* 的字符串，需要将正则表达式改为 `/^(\d{2}|\d{4})[\dX]$/`

推荐关于正则表达式的博文：

[正则表达式前端使用手册](http://louiszhai.github.io/2016/06/13/regexp)