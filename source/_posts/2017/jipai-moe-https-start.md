---
title: JiPai.Moe全站了开启HTTPS！
tags: ['website']
categories: ['运维']
date: 2017-07-01 17:03:33
permalink: 'jipai-moe-https-start/'
---

现如今，网站不配个SSL，都不好意思出门和大佬交换友链了！当然最重要的原因也是因为国内网络环境恶化，更何况Google也表示不带SSL玩的话我们就不带你玩了，所以JiPai.Moe也算是姗姗来迟，部署了SSL，至于HSTS，暂时没有开启的打算。
<!-- more -->
SSL证书的话使用了TrustAsia联合腾讯云提供的免费单域名一年证书，申请在腾讯云那边完成，用DNS验证或者上传文件验证域名所有人就好。 LNMP提供了方便的开启SSL证书的办法，只要按照操作提示填写域名，证书和key的存放位置，就会自动配置好nginx设置并且reload。 这样自动配置好的HTTPS有一个问题就是不会强制跳转，所以我们要在nginx的配置文件里加一句

> `rewrite ^(.*)$  https://$host$1 permanent;` $host是请求的主机名， $1是第一个匹配的结果，permanent是返回永久重定向的HTTP状态301。 具体的话请看[这里](http://seanlook.com/2015/05/17/nginx-location-rewrite/)

这样的配置方法对于只有一两个子域名的人来说还算方便，但是如果对于我来说可能随时就会开一个新的子域，为其应用HTTPS就会很不方便，要重新申请证书，验证域名，然后改nginx配置，所以估计对于不重要的域名，会开始使用Let's Encrypt,听说有自动续期脚本可以用，也不用操心三个月有效期的问题，唯一的不足可能就是可能在某些低版本chrome上得不到承认。 有关HTTPS更多的知识，请看[Jerry Qu大佬的博客](https://imququ.com/post/sth-about-switch-to-https.html#toc-2)