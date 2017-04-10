---
title: 【回顾】FCC 高级算法题 （3） —— Inventory Update
excerpt: 我的思路及解法
date: 2017-04-10
tags: FCC
categories: blog
layout: post
---

## 题目

依照新进货库存清单更新旧库存清单，数据都存放在数组中。返回更新后的库存清单，清单按货物字母顺序排序。

[Inventory Update](https://freecodecamp.cn/challenges/inventory-update)

## 思路

### 难点

1.库存货物与新进货物种类的比对。因为清单用二维数组表示，难以直接在其中进行搜索。

2.数组按字母排序的实现

### 解决方法：

1.将清单重新用对象表示，对象属性为对应货品名称，属性值为货品数量。更新完毕之后再从新转换为数组。

2.利用数组对象的排序函数。

## 解法

具体代码如下：

    // 清单转换函数，将清单在数组和对象之间转换
    function turnInv(inv){
        var invMap;
        if(inv instanceof Array){
            invMap = {};
            inv.forEach(function(item){
              invMap[item[1]] = item[0];
            });
        }
        else{
            invMap = [];
            for(var x in inv){
              invMap.push([]);
              invMap[invMap.length - 1].push(inv[x]);
              invMap[invMap.length - 1].push(x);    
            }
        }
        return invMap;
    }
    
    // 更新函数
    function updateInventory(arr1, arr2) {
        // 第一次转换，数组变为对象
        arr1 = turnInv(arr1);
        arr2 = turnInv(arr2);
        // 更新清单
        for(var x in arr2){
            if(arr1[x]){
              arr1[x] += arr2[x];
            }
            else{
              arr1[x] = arr2[x];
            }    
        }
        // 转换 -> 排序 -> 输出
        return turnInv(arr1).sort(function(item1,item2){
            return item1[1] > item2[1];
        });
    }