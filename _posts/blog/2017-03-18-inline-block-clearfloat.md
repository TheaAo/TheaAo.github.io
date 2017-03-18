---
title: 使用 inline-block 包围浮动子元素
excerpt: 以及其他三种包围浮动子元素的方法
date: 2017-03-18
tags: CSS float
categories: blog
layout: post
---

在设置导航栏时，常用无序列表——通过设置列表样式去除列表前面的条目标记。如果导航栏在页眉部分，往往通过将列表条目设置为浮动的方式使其并列在一排。但是元素被设置为浮动后便脱离原本的文档流，此时若父元素中无其他元素，则父元素不能包住子元素。这对于整个页面的布局会有影响。

通过设置父元素 `display` 的值为 `inline-block` 可以解决这个问题。

页面标签结构如下：

    <div class="container">
        <div class="float"></div>
    </div>

CSS 样式设置如下：

    .container {
        border: 1px solid;
        display: inline-block;
        padding: 2px;
        width: 50%;
    }
    .float {
        width: 100px;
        height: 100px;
        border: 1px solid orange;
        float: left;
    }

边框设置是为了更直观地查看父元素对浮动子元素的包含情况。设置完成后，容器将紧紧包围住浮动的子元素。

关于 `inline-block`, MDN Web 技术文档里的说明如下

>该元素生成一个块状盒，该块状盒随着周围内容流动，如同它是一个单独的行内盒子（表现更像是一个被替换的元素）

需要注意的是，重新设置 `display` 的值后，对于容器的父元素而言，容器是一个行内元素。所以容器居中无法通过设置 `margin` 为 `auto` 实现，简单设置容器父元素的 `text-align` 为 `center` 即可。

## 另外三种父元素包围浮动子元素的方法

### 1. 设置父元素 `overflow` 为 `hidden`

### 2. 将父元素也设置为浮动

### 3. 在浮动子元素后添加一个非浮动的子元素

对于该子元素需要设置为浮动清除。HTML 结构如下：

    <div class="container">
        <div class="float"></div>
        <div class="clearfloat"></div>
    </div>

CSS 设置如下：

    .clearfloat {
        clear: left;    // clear: both 也可
    }

如果你不想在 HTML 页面中加入无意义的标签，也可以通过 CSS 伪元素给容器添加一个 `clearfix` 类来实现。CSS 代码如下：

    .clearfix ::after {
        content: ".";
        display: block;
        height: 0;
        clear: both;
        visibility: hidden;
    }

