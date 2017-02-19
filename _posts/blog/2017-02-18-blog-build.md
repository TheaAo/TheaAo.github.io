---
layout: post
title: 利用 Jekyll 和 GitHub Pages 创建自己的个人博客
excerpt: 一个小白搭建博客的坎坷路
date: 2017-02-18
categories: blog
tags: Jekyll GitHub-Pages
---

开始学习前端开发后，好多次都是在别人博客里搜索到问题的解决办法。于是觉得有一个个人博客很不错，可以及时记录下自己的学习总结。但是希望这个博客干净整洁，没有乱七八糟的广告。

看到可以用 GitHub Pages 和 Jekyll 搭建一个完全属于自己的个人博客后，觉得超级棒，于是速度学起来。虽然现在回想觉得过程很简单，但还是遇到了很多奇奇怪怪的问题。如果你和我一样是个啥都不懂的小白，希望这篇文章可以帮到你。

说明：整个操作环境为 win7 x86 系统

## GitHub 和 Jekyll 都是啥？

关于 GitHub Pages 和 Jekyll 的介绍可以直接看官方说明：

- [GitHub Pages - Websites for you and your projects] [1]
- [Jekyll] [2]

如果你看过《化身博士》，你一定知道 Dr.Jekyll 的故事。不过我并不知道这二者是否有联系。（\*^_^\*）

简单说，Jekyll 是一个静态网站生成器，它可以将纯文本格式的文件转换成可发布的静态网点，这个网点就是我们所说的个人博客了。假如一切顺利，成功安装 Jekyll 并搭建好自己的博客后，在浏览器中输入 `localhost:4000` 你就可以看到你的博客在本地运行了。

如果你搭建博客的目的只是希望自己的记录以一种更美好的形式呈现出来，仅供自娱而不需要其他人访问，那么安装 Jekyll 就可以满足你的需求了。但假如你想将博客发布到网上，GitHub Pages 可以提供帮助。

>GitHub Pages is a static site hosting service

GitHub Pages 是 GitHub 所提供的静态网站托管服务。意思是，你将之前搭建好的博客部署到 GitHub 上之后，通过 Github Pages 就可以在互联网上浏览你的博客啦！使用 GitHub 的好处包括：

- 免费
- 不错的访问速度
- 无限容量

是不是很棒！

## 除此之外你还要了解啥？

Markdown 是一门简单好用的标记语言，有如下优点：

- 纯文本格式，兼容性极佳
- 用简单的方式保留了样式设置，使你写作时可以专注于文字内容本身
- 具有很强的可读性
- 易于被转换成 HTML 语言

尽管它的使用人群大概以程序猿居多，但它的语法非常简单，即使不懂编程也完全能够轻松掌握。

[Markdown 语法说明（简体中文版）][3]

## 让我们开始吧！

### 1. 准备工作

你需要有一个 GitHub 账号，去[官网][4]注册吧！

注册完成后，创建一个新的仓库(repository), 命名为 *`yourgithubname.github.io`*, 将其 `clone` 到本地。

### 2.安装 Jekyll

本文一开始贴出的 Jekyll 官方文档中其实已经包括了从安装到设置到使用完整的一系列说明。以下是安装部分的重点。（以 Windows 系统为例）因为我使用的是 Windows 系统，而它并非 Jekyll 官方支持的平台。不过还是有办法使它运行在 Windows 系统上的。参见：

[Jekyll 运行在 Windows 系统][5]

- 下载安装 [Ruby][6] 和 [Ruby DevKit][7]

    安装 Ruby 时出现以下窗口，请记得勾选 "Add Ruby excutable to your PATH" 后再确认安装。

    ![Ruby_install](/images/20170218/ruby_install.png)

- 初始化 DevKit 并将它和 Ruby 绑定

    打开 *window powershell* 命令行工具，进入 DevKit 的安装目录，输入以下指令进行初始化：

    ruby dk.rb init

    输入以下指令将 DevKit 与 Ruby 绑定

        ruby dk.rb install

    一切顺利的话，在命令行中输入 `ruby -v` 可以看到版本信息。

- 安装 Jekyll Gem

    使用 Ruby Gem 安装 Jekyll 非常简单，你只需要在命令行中输入

        gem install jekyll

    然后按下回车键就搞定了。

### 3.安装 GitHub Pages

请参考[安装 GitHub Pages][8]

- 安装 Nokigori 软件包

    GitHub Pages 运行时需要这个软件包，依然使用 Gem 安装，直接输入以下指令即可
    
        gem install nokigori --^

- 安装 GitHub Pages

    在命令行界面安装 `Bundler`, 输入

        gem install bundler 

    在之前保存在本地的仓库的根目录下新建一个不带任何后缀的名为 `Gemfile` 的文件，在文件中输入下面两行信息并保存。

        source "https://rubygems.org"
        gem "github-pages"
        gem "wdm",">=0.1.0"

    切换到命令行界面，输入以下命令安装 GitHub Pages
    
        bundle install

    安装成功！

### 4.挑选主题模板

这时你的仓库里什么都没有，你需要自己编写搭建博客的文件，并将它们放入其中。

或者你可以采用别人的模板来编写自己的内容，你可以在以下网站寻找合意的主题模板。

[Jekyll 主题模板][9]

将中意的模板下载下来，按照模板的说明进行个人化的修改。千万记得最重要一点是将 `_config.yml` 中的 `url` 的内容修改为 `https://yourgithubname.github.io` 。

### 5.试运行

打开命令行界面，进入仓库的根目录，输入 `bundle exec jekyll serve`

![Jekyll_run_command](/images/20170218/jekyll.png)

打开浏览器，输入 `localhost:4000` 你就能在本地看到你的博客啦！

将本地仓库 `push` 到远端，在浏览器地址栏输入 `https://yourgithubname.github.io`, 你就能在线看到你的博客啦！

## 我遇到过的那些奇奇怪怪的问题

虽然我写了这么多，但是首先还是建议查看官方说明文档。我在安装过程中遇到的很多问题其实按照官方给出的说明都能顺利解决。

安装完成之后遇到了两个小问题：

1. 博客本地运行正常，部署到 GitHub 后所有样式都没有了

    网上查到的几乎都是 [vendors 文件的命名问题][10]，但是我的不是这样。后来打开 chrome 的开发者工具查看它的报错才解决了问题。具体问题是什么记不清了，只是提供一个方法。如果碰到本地运行正常，部署后出现问题可以这样去查看确切的问题，然后 google 之，解决之。

2. 写了新文章后运行发现文章不能读取，问题如下图：

    ![problem](/images/20170218/problem_n.png)

    发现是头信息有误，修改后问题就解决了。

    其实遇到问题不用慌，查看问题是什么，google 一下，按照解决办法一条一条试过去，多半是能够解决的。



- **后记**：虽然还蛮喜欢现在这个模板，但是还是希望自己的个人博客是由自己亲手设计并实现的。所以还要继续通过研究现在这个模板学习 Jekyll, 不要偷懒，要早日实现想法哦！

[1]: https://pages.github.com/
[2]: http://jekyllcn.com/
[3]: http://wowubuntu.com/markdown/
[4]: https://github.com/
[5]: http://jekyll-windows.juthilo.com/
[6]: http://rubyinstaller.org/downloads/
[7]: http://rubyinstaller.org/downloads/
[8]: http://jekyllcn.com/docs/windows/#如何安装github-pages 
[9]: https://github.com/jekyll/jekyll/wiki/Themes
[10]: https://github.com/blog/2277-what-s-new-in-github-pages-with-jekyll-3-3
