---
title: JiPai.Moe迁移到Google Cloud Platform 一说
tags: ['']
categories: ['运维']
date: 2017-07-01 10:48:25
permalink: 'move-to-gcp/'
---

JiPai.Moe最开始注册并运行在50vz的洛杉矶机房VPS上，是和 _[NekoTora](http://flag.moe)_ 一起进行的合租，最开始的时候速度还不错，数月后访问质量开始下降，严重影响了自己写博的心情...(明明就是因为懒) 上代JiPai.Moe 在 _[NekoTora](http://flag.moe)_ 的安利下是用Typecho+MySQL搭建的，虽然说已经多年没有更新stable版本，但是还能将就用用，要什么自行车！直到有一天我打不开后台.......而我的SSH不但又卡而且我还忘了密码......于是便萌生了迁移的想法......
<!-- more -->
一开始打算放在腾讯云上，但是备案折腾起来有点麻烦，而如果不备案的话国内的CDN也用不了，遂把目光投向国外。海外知名VPS厂商的话我觉得肯定是DigitalOcean（这家验证edu邮箱后可以得到50$代金卷，**有效期一年**） 和Linode以及Vultr，前两者提供了最低每月5$的套餐，后者提供了最低每月2.5$的套餐，但是即便如此，按照市值汇率计算，每月也要付出接近20元人民币的服务器开销，身为一个濒临破产边界线的过期鸡排，对价格是非常敏感的！ 恰在此时，Google Cloud Platform宣布新用户免费体验一年，赠送300$使用，于是决定，先GCP怼爆，明年迁移到DO，其他的有钱再说！ 浦发的大学生VISA卡还是不错的，直接通过Google Wallet验证，没有翻车。 先说配置，开的是GCP的Micro机型。每月大概5刀，流量另算，现在想想太勤俭节约啦！><

> ###### 机器类型
> 
> f1-micro（1 个 vCPU，0.6 GB 内存）
> 
> ###### CPU 平台
> 
> Intel Ivy Bridge
> 
> ###### 地区
> 
> asia-east1-a

开机子的时候一头雾水，一路下一步，现在想起来给自己后面埋了很多雷，首先我选了CentOS6.0，其次我没设置SSH密码 （修正：GCP默认不允许使用密码登陆，创建实例时请添加自己的公钥），磁盘也只选了默认的10G。随后部署的时候就发现自己的Xshell和WinSCP无法以root帐户登陆，好在谷歌的网页版Shell实在好用，我也懒得去配自己的shell了（后来觉得太麻烦还是去把允许密码登陆开启了）。装好LNMP后，直连IP却发现根本打不开，首先以为IP被关照了，后来又察觉并不是，在排除了Nginx异常，安装失败等因素后，我在GCP控制面板里找到了防火墙设定，在创建实例的时候，**请勾选允许HTTP/HTTPS选项**，否则默认不放行web相关的端口，在控制面板正确配置后访问正常。

接下来添加vhost和上传文件，这个时候遇上了Linux常见的权限问题，在一顿暴力chown后被``cannot remove `.user.ini': Operation not permitted``

难住了。再一查，居然是用chatter指令锁定了，解锁就好：`chattr -i` `/home/wwwroot/yoursite/``.user.ini`

详细资料戳[这里](http://www.dayanmei.com/lnmp-delete-user-ini/)

最后有非常重要的一点，GCP默认开出来的实例，是分配的动态IP，一定要去网络里面把这个IP固定一下，否则重启后IP会发生变化，免费300刀的用户，只有一个静态IP的配额。

 敲黑板，GCP要点解析！上面太长不看直接看这里就好

*   GCP实例默认不允许用户名密码组合登录，需要配置Public key，如果你没配好的话请用web shell上去补救。
*   开出来的实例记得去绑定自己的动态IP。
*   创建实例时请勾选HTTP/HTTPS，否则请自己到防火墙规则中放行相关端口。后台面板防火墙不等于系统内置的Firewall！