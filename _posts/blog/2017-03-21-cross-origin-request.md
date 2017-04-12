----
title: No "Access-Control-Allow-Origin" header 问题解决
excerpt: Ajax 请求跨域问题及解决办法
date: 2017-03-21
categories: blog
layout: post
tags: Ajax JSON
published: false
---

使用 Ajax 技术可以在不重新加载页面的情况下动态更新页面数据。在 [LocalWeather][1] 和 [WikipediaViewer][2] 两个项目中，我通过浏览器 XHR 对象向远端服务器发出 Ajax 请求，得到数据并显示在页面上。然而无法获得数据。控制台错误信息显示
    
    XMLHttpRequest cannot load ***. No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'https://theaao.github.io' is therefore not allowed access.

查了一下，因为 Ajax 使用有条件，受到[浏览器同源策略][3]的约束。

简单说，就是使用 Ajax 获取数据的条件是双方 URI 必须是“同源”的，即

- 相同协议
- 相同主机
- 相同端口

三者缺一不可，否则浏览器会阻止数据的返回。然而 Ajax 请求跨域的情况并不少，因此相应也有不少解决策略。

## 1.CORS

CORS 全称为“跨域资源共享(Cross-Origin Resource Sharing)”,是 w3c 为解决 Ajax 跨域请求推出的一项标准。

我们知道 Web 中客户端和服务器之间的通信是依据 HTTP 协议进行的。HTTP 报文首部字段包含了一系列表示请求和响应的各种条件和属性的各类首部。在发起 Ajax 请求时，如果是跨域的，则会在首部加上一个 Origin 字段，字段值为客户端 URI。例如
        
        Origin: http://theaao.github.io

服务器将据此作出回应，决定是否允许请求。

CORS 目前由浏览器自动实现，不需要用户额外设置什么。

根据浏览器错误提示，我的请求源大概不在服务器许可范围内。猜想要解决这个问题应该需要更改服务器设置，在客户端无法解决。因此我采用了另一种方法，使用 JSONP 来解决这个问题。

## 2.JSONP

在 html 页面动态插入 `script` 标签，将标签的 `src` 属性值设置为请求目的服务器地址，地址中需要包含一个指定名字的回调函数。服务器收到请求后，会将数据作为回调函数的参数传回。具体代码如下：
    
    function addJSTag(src){
        var script = document.createElement("script");
        script.src = src;
        document.body.appendChild(script);
    }
    function getResponse(data){
        //do something with data
    }
    addJSTag(src+"&callback=getResponse");

JSONP 不需要对服务器进行改动。

除了这两种之外，还有其他方案。

## 3.图像 Ping
## 4.Web Socket

**参考资料**
\[1\] [跨域资源共享 CORS 详解][4]
\[2\] [HTTP 访问控制][5]
\[3\] [JavaScript 高级程序设计第 21 章][6]

[1]:https://theaao.github.io/LocalWeather/
[2]:https://theaao.github.io/WikiViewer/
[3]:http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html
[4]:https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS
[5]:http://www.ruanyifeng.com/blog/2016/04/cors.html
[6]:https://book.douban.com/subject/10546125/
