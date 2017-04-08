---
titile: FCC 高级算法题(1) -- Validate US Telephone Numbers
excerpt: 我的思路及解法
tags: FCC 
date: 2017-04-08
categories: blog
layout: post
---

FCC 全称 FreeCodeCamp, 是一个开源的编程社区。前不久我在上面完成了高级算法题这一部分。第一次做，很多地方思路很乱。也有很多冗余代码，于是决定重新做一遍。可惜在做第一遍的过程中因为写出了死循环导致一打开页面浏览器就崩溃，无奈删除了所有记录，没有办法前后对比代码。

[FCC 中文站](https://freecodecamp.cn)

[FCC 英文站](https://www.freecodecamp.com)

## 题目

[Validate US Telephone](https://freecodecamp.cn/challenges/validate-us-telephone-numbers)

输入字符串，判断其格式是否是有效的美国电话号码，返回判断结果。

## 思路

这道题目不算难，有两个关键点：

1. 利用举的例子自行总结出有效格式是什么；
2. 能够通过正则表达式表现有效格式。

根据给出的例子，有效格式为 [国家代码]\[三位数区号][七位数电话号码]，国家代码确定为 1 ，三位数区号外边可以包裹括号，七位数电话号码可以拆成三位+四位。各部分之间没有间隔或以连字符或空格为间隔。

暂不考虑国家代码，按部分写出正则表达式再连接起来。如果太繁琐，可以用一个数组存放多个正则表达式。

我的正则表达式数组为

    var re = [/^\d{3}([-\s]|\B)\d{3}([-\s]|\B)\d{4}$/,/^\(\d{3}\)([-\s]|\b)\d{3}([-\s]|\B)\d{4}$/];

## 解法

完整代码如下：

    function telephoneCheck(str) {
        // 写出正则表达式
        var re = [/^\d{3}([-\s]|\B)\d{3}([-\s]|\B)\d{4}$/,/^\(\d{3}\)([-\s]|\b)\d{3}([-\s]|\B)\d{4}$/];
    
        // 如果有国家代码
        if(str[0] ==1){
            str = str.slice(1);
            str = str.trim();
        }
    
        // 返回验证结果
        return re.some(function(item){
            return item.test(str);
        });
    }

## 关于 JavaScipt 正则表达式


你可以在[这里](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)了解更多。