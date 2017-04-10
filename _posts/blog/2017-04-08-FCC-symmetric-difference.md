---
title: FCC 高级算法题 （2）—— Symmetric Difference
excerpt: 思路及解法
tags: FCC
date: 2017-04-09
categories: blog
layout: post
---
[FCC 中文社区](https://freecodecamp.cn)
[FCC 英文社区](https://freecodecamp.com)

## 题目

创建一个函数，参数为 N 个数组（N 仅出现在>=2），对这些数组进行对等差分运算，返回结果。

[Symmetric Difference](https://freecodecamp.cn/challenges/symmetric-difference)

## 思路

重点：

1.两数组对等差分运算的实现
2.数组去重

### 对等差分运算的实现

1.将数组 2 分成两个数组，一个存放共有的元素，另一个存放独有的元素，使用过滤器实现；
2. 过滤数组 1，得到只含独有元素的数组；
3. 拼接两个独有数组，得到差分结果。

### 数组去重

1.初始化一个新的空数组用以存放去重后的数组；
2.遍历待去重数组，检查元素是否已在新数组中；
3.若无，加入新数组。

## 解法

具体代码如下：

    // 去重函数    
    Array.prototype.unique = function(){
      var arr = [];
      this.forEach(function(item){
        if(arr.indexOf(item) == -1){
          arr.push(item);
        }
      });
      return arr;
    };
    
    // 两个数组对等差分运算函数
    function symTwo(arr1,arr2){
      var arr2f1 = arr2.filter(function(item){
        return arr1.indexOf(item) != -1;
      });
      var arr2f2 = arr2.filter(function(item){
        return arr1.indexOf(item) == -1;
      });
      arr1 = arr1.filter(function(item){
        return arr2f1.indexOf(item) == -1;
      });
      return arr1.concat(arr2f2).unique();
    }
    // 多个数组对等差分运算函数
    function sym(args) {
      // 一个简单的参数输入检验    
      if(args.length < 2){
        return "Error Arguments!"
      }
      // 真正运算
      var argsArr = Array.from(arguments);
      argsArr = argsArr.reduce(function(prev,curr){
        return symTwo(prev,curr);
      });
      return argsArr;
    }