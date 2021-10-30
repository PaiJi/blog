---
title: hexo主题开发后记
date: 2021-10-23 00:48:37
tags: ['hexo','主题开发']
categories: '心得'
permalink: 'hexo-theme-dev-note/'
photos:
---
写这个主题的时候还是遇到了一点奇奇怪怪的坑，放在这里给自己立个警示牌，如果能帮到更多人就好了。
<!-- more -->

整个开发过程中主要就一个感想 “Hexo的文档不太行”，说得不清楚、压根没提到、还有过时的内容。大部分时候我都在对着文档干瞪眼和谷歌。这里感谢 molunerfinn的 [Hexo主题开发经验杂谈](https://molunerfinn.com/make-a-hexo-theme/#%E9%A1%B5%E9%9D%A2) 和他的Next主题，遇到问题都先看看别人的源码就大概明白了。

### 禁用Archive生成
没给Archives相关页面写样式，也不觉得有什么需要，就暂时禁用了，在 `hexo@5.4.0` 这个版本，禁用的办法是在根目录的_config.yml中加入
```
//有效
archive:
  enabled: false
//无效
archive_generator:
  enabled: true
```

### Page.photos 
按照文档描述，`page.photos`返回文章的照片，是一个数组，实测在 渲染文章 的时候，即便没有顶部配置photos字段也会拿到一个空数组，而在 渲染页面 时，没有顶部配置photos字段的情况下会返回null。

### 代码高亮
hexo 默认的配置是无法让 highlight.js 的主题文件生效的，需要改成这样。同时需要yarn clean才能让这个配置生效，单纯yarn server看不到变化，不知道是个人原因还是什么情况🤨
```
# _config.yml
highlight:
  enable: true
  line_number: false
  wrap: false
  hljs: true
```

### post-link 不能忽略路径
UPDATE:
怪事，隔日发现这个函数又能正常工作了，明明写主题那几天怎么都不行😑。
~~这个其实不能算主题开发遇到的，是写作的时候遇到的，官方文档称“在使用此标签时可以忽略文章文件所在的路径或者文章的永久链接信息”。还举了两个栗子，然而实际上如果不是在同目录下的文章，就是找不到。不知道是不是我理解得有问题。还是得写成~~ 
```
{% post_link 2020/recommand-spotify %}
```

### 图片的caption实现
WordPress用户都知道可以给插入的图片写一个描述，看hexo 导入的时候没处理好，谷歌了一下发现可以这样做
```
![](path_to_image)
*image_caption*
```
hexo会生成
```
<p>
    <img/>
    <br>
    <em>
</p>    
```
用CSS稍微处理一下就好了。

> 来源： https://stackoverflow.com/questions/19331362/using-an-image-caption-in-markdown-jekyll