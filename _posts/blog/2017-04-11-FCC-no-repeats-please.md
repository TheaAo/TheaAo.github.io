---
title: 【回顾】FCC 高级算法题（4） —— No repeats please
excerpt: 我的思路和解答
date: 2017-04-11
tags: FCC
categories: blog
layout: post
---

## 题目

传入一个字符串，利用其中字符重新排列，返回没有单个字符连续重复的字符串的个数。

[No repeats please](https://freecodecamp.cn/challenges/no-repeats-please)

## 思路

如果利用穷举法，先列出所有的排列，再剔除含连续重复字符的排列。耗时太长，效率太低。对于这道题目，我们观察到测试数据的最大重复次数不超过两次，于是取巧把这道题转换为如下关于排列的数学题：

>现有一长度为 *n* 的字符串，其中有 *m* 个字符有重复，重复次数最多为两次, 求最终不含连续重复字符的排列个数。

易知，总排列数为 $$A_n^n$$，含连续重复字符的排列个数为 

$$m A_{2}^{2} A_{n-1}^{n-1} - (m-1) A_p^p (A_{2}^{2})^m$$

其中，减号后面是之前重复计算了的排列数，*p* 为原字符串剔除所有多余的重复字符后的剩余字符个数，

$$p = n - m$$

所以最终结果即为

$$A_n^n - m A_{2}^{2} A_{n-1}^{n-1} - (m-1) A_p^p (A_{2}^{2})^m$$

## 解答

实际代码如下：

    // 阶乘函数 
    function fact(num){
        if(num <= 1){
            return 1;
        }
        else{
            return num * fact(num-1);
        }
    }
    // 主体函数
    function permAlone(str) {
        var n = str.length;  // 总字符个数
        var m = 0;  // 重复字符个数
        var counter = {}; // 每个字符出现次数的计数器
        var permRepeat; // 重复排列个数
    
        str = str.split('');
        
        // 统计每个字符的出现次数
        str.forEach(function(item){
            if(counter[item]){
            counter[item] += 1;
          }
          else {
            counter[item] = 1;
          }
        });
        
        // 筛选出重复字符的重复次数
        for(var x in counter){
          if(counter[x] > 1){
            if(counter[x] == n){
                return 0;   // 如果字符串由一个字符重复而成，返回零
            }              
              m++;
          }
        }
      
        // 求单个字符连续重复排列数
        permRepeat = 2 * m * fact(n-1) - (m - 1) * fact(n - m) * Math.pow(2,m);
      
        return fact(n) - permRepeat;
    }

## 总结

但是这种算法的应用范围太狭窄了，仅限于字符重复次数不超过两次的情况下。如果要完全实现题干的要求，目前我能想到的方法只有穷举。

思路大致如下：

1.列举出所有可能的排列
2.对全部排列进行筛选，留下符合要求的排列
3.统计 2 中排列数量，输出结果

具体代码如下：

    // 阶乘函数
    function fact(num){
        if(num <= 1){
            return 1;
        }
        else{
            return num * fact(num-1);
        }
    }
    // 验证数组是否为空
    function isEmpty(arr){
        return arr.every(function(item){
            return item === '';
        });
    }
    // 验证排列是否有连续重复字符
    function isRepeat(str){
        for(var i = 0; i < str.length; i++){
            if(str[i] == str[i+1]){
                return true;
            }
        }
        return false;
    }
    // 主体函数
    function permAlone(str) {
        var strA;
        var perm;  // 存放单个排列
        var numPerm;  // 存放单个排列的编号
        var allPerm = [];  // 存放所有排列
        var allNumPerm = [];  // 存放所有排列的编号
        var index;
        var all = fact(str.length); // 所有排列数
        
        // 穷举过程
        // 以排列集合中的数量判断是否枚举了全部排列
        while(allPerm.length < all){   
            strA = str.split('');
            numPerm = '';
            perm = '';
    
            // 单次排列实现过程
            while(!isEmpty(strA)){
                do{
                    index = Math.floor(Math.random()*str.length);
                }while(!isEmpty(strA) && strA[index] === '');
                numPerm += index;
                perm += strA[index];
                strA.splice(index,1,'');
            }
    
            // 如果单次排列的编号不在数组中，则认为举出了一个新的排列
            // 记录这个新的排列和其编号
            if(allNumPerm.indexOf(numPerm) == -1){  
                allNumPerm.push(numPerm);
                allPerm.push(perm);
            }
        }
        // 穷举结束后进行筛选
        var alonePerm = allPerm.filter(function(item){
            return !isRepeat(item);
        });
    
        return alonePerm.length;
    } 

以上代码无法通过该题页面的测试，因为在输入字符串长度为 7 时，页面会以可能存在无限循环的风险阻止运行。但我自己测试，虽然耗时较长但是可行（而且直接跑比在题目页面跑也要快一点点），所以也把代码贴上来。

可能我穷举的实现算法不够有效，如果以后发现更有效的实现方法，会再贴上来。