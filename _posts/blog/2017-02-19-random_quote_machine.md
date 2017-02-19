---
layout: post
title: 随机名言生成器
excerpt: FCC课程作业——随机名言生成器
tags: FCC-课程项目 jQury JSON
date: 2017-02-19
categories: blog
---

[FCC(Free Code Camp)][1] 是 GitHub 上的一个开源项目，免费为想要学习编程的人提供课程。课程以关卡形式发布，每阶段课程完成后会有相关的项目作业。我在不久前开始学习它的课程，决定在每个项目完成后进行总结，以期更好的巩固所学。如果你英文不够好，这儿是他们的中文社区——[ FCC 中文社区][2]。

以下是关于本项目的正式介绍。

# 项目介绍

- 项目名称
    + 随机名言生成器

- 项目功能
    + 每次点击按钮随机生成一条名言
    + 支持将内容发布到社交网站
    + 说明：
        项目例子是英文名言，我对呈现内容做了一些改动，最终实现的应该是随机诗词生成器，而且都是苏东坡诗词。(\*^_^\*)

- 项目实现效果

    ![随机诗词生成器](/images/random_quote_machine/random_quote_machine.png)

    在线查看：

    [随机诗词生成器][3]

    [源码](https://github.com/TheaAo/Random_Quote_Machine)

# 实现过程

## -界面

用 HTML 编出页面框架。从图中可以看出整个页面比较简单，这一步不多说，页面结构大致如下。

![DOM 结构](/images/random_quote_machine/dom.png)

标签和社交网站图标使用自 [Font Awesome][4] 的图标库。

## -CSS

用 CSS 对页面进行布局和美化。由于颜色和内容都是动态设置的，仅此这一步主要就是对页面的各个框框的布局进行设置，以及稍微设置了一下按钮的样式。

## - 功能

功能实现分成三步：

###### 1. 随机呈现诗词

- 建了一个 json 格式的诗词库，手动输入了三十首东坡词。
- 使用 jQuery 的 getJSON 函数从库里取到数据
- 使用随机函数生成索引，依照索引从库里取到内容并呈现

###### 2. 动态效果

- 建了一个数组用来存放颜色
- 使用 jQuery 的函数实现淡入淡出

最开始我使用的是 `fadeIn()` 和 `fadeOut()` 函数，发现这样背景颜色在切换时会出现空白，从而会有闪屏的效果，使眼睛很不舒服。要实现平滑切换，有两种方法：

1. 使用 jQuery 的 `animate()` 方法。

    但是 jQuery 核心库不支持有关颜色的动画效果，需要额外加载插件。可以通过[jQuery 插件库][5] 查找合意的插件的 CDN 地址。

2. 这是我最开始的实现方法。

考虑到 `fadeOut()` 实际上是对元素进行了 `display: none` 这样的一个设置。于是我在 html 页面的 `container` 里加了一个名为 background 的 `div` 用作背景，让他调用 `fadeToggle()` 函数。当它消失时 `container` 的颜色显现出来，当它出现时覆盖 `container` 。就像百叶窗一样。这样的切换就是平滑的了。

这个方法当然有些傻，不过在实现过程中我到因此注意到了之前不曾在意的 position 属性值的区别。

    position    static      默认值，没有定位，忽略 top/right/bottom/left 值
                fixed       以窗口为参照，利用 top/right/bottom/left 确定位置
                absolute    以 position 值不是 static 的第一个父节点为参照，定位同上
                relative    相对定位，以元素本来位置为参照点确定位置
                inherit     继承父节点定位置

此前我从没有在意过它，所以理所当然，它的值始终都是默认的 `static` 。但是这样的话，我的诗词卡片就没有办法置于 background 的中间了。所以设置 container 的 position 值为 fixed，card 和 background 的对应值为 absolute 就可以使二者重叠了。

###### 3. 分享功能实现

使用 JavaScript 的窗口对象在新窗口打开对应的页面即可。

# 总结

- **问题**
    + Chrome 跨域请求支持问题
    
    ![Chrome 跨域请求支持问题](/images/random_quote_machine/cross_origin_request_error.png)

    在从 JSON 文件里取数据时遇到的问题。因为在浏览器打开本地文件时，地址栏的是 `file:///` 开头，但是 `getJSON()` 的跨域请求并不支持此种协议。

    [解决办法][6]很简单，但是我也要补补网络传输协议相关的知识了。

- **所得**
    + CSS 定位
    
        像前文所述那样，因为一个“笨办法”了解到了被自己忽略的知识点。是时候重新看一遍基础教程，看看还忽略了什么了。

    + JSON
    
        之前尽管看了教程，还是对 JSON 是什么云里雾里，而通过这个小项目，总算明白了。按我的理解，JSON 是一种数据储存的格式，这种格式和 JavaScript 的对象格式一样，所以可以被其当成对象读取。

[1]:https://www.freecodecamp.com
[2]: https://freecodecamp.cn
[3]: http://codepen.io/TheaAo/full/JEwZNG/
[4]: http://fontawesome.dashgame.com/
[5]: http://www.jq22.com/cdn/
[6]: http://www.daxiblog.com/2016/09/16/%E8%AE%A9%E6%96%B0%E7%89%88chrome%E6%94%AF%E6%8C%81%E6%9C%AC%E5%9C%B0%E8%B7%A8%E5%9F%9F%E8%AF%B7%E6%B1%82%E8%B0%83%E8%AF%95/

