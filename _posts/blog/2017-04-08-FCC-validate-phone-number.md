---
title: 【回顾】FCC 高级算法题 （1） —— Validate US Telephone Numbers
excerpt: 我的思路及解法
tags: FCC 
date: 2017-04-08
categories: blog
layout: post
---
第一遍做 FreeCodeCamp 的高级算法题时，思路比较混乱，代码写得也乱。继续学习 JavaScript 之后决定重做一遍，整理思路，并把它和解法都记录下来。因为清理过浏览器记录，所以第一遍的代码没有保存，无从对比分析。如果以后看到更优的思路或代码，会再次更新。

[FCC 中文站](https://freecodecamp.cn)

[FCC 英文站](https://www.freecodecamp.com)

## 题目

输入字符串，判断其格式是否是有效的美国电话号码，返回判断结果。

[Validate US Telephone](https://freecodecamp.cn/challenges/validate-us-telephone-numbers)

## 思路

### 关键点

1.利用举的例子自行总结出有效格式是什么；

2.能够通过正则表达式表现有效格式。

### 具体分析

根据给出的例子，电话号码可以分为以下几个部分：

- 国家代码——可有可无，有则必须为 1 。

- 区号——三位数，与国家代码之间间隔可有可无，有则间隔符只能为空格或连字符；外层包裹括号可有可无，有则必须成对。

- 电话号码——七位数，与区号之间间隔可有可无，有则只能为空格或连字符；自身可以连写或拆分为三位+四位的形式，若拆分，中间间隔符也只能为空格或连字符。

用正则表达式将以上描述表达出来。

因为括号涉及到配对的问题，最终我将正则表达式拆分为有括号和无括号两个版本，放在数组里。无括号版正则表达式如下：

    var re = /^1?[-\s]?\d{3}[-\s]?\d{3}[-\s]?\d{4}$/;

## 解法

完整代码如下：

    function telephoneCheck(str) {
        var re = [/^1?[-\s]?\d{3}[-\s]?\d{3}[-\s]?\d{4}$/,/^1?[-\s]?\(\d{3}\)[-\s]?\d{3}[-\s]?\d{4}$/];
        return re.some(function(item){
            return item.test(str);
        });
    }

也可以：

    function telephoneCheck(str) {
        var re = /^1?[-\s]?\d{3}[-\s]?\d{3}[-\s]?\d{4}$|^1?[-\s]?\(\d{3}\)[-\s]?\d{3}[-\s]?\d{4}$/;
        return re.test(str);
    }

目前，我没办法写出更简洁的正则表达式了，如果发现更简洁的表达方式会再更新。

## 关于 JavaScipt 正则表达式

[RegExp](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)

[正则表达式前端使用手册](http://louiszhai.github.io/2016/06/13/regexp)