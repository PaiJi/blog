---
title: 《死亡搁浅》 EPIC 与 Steam 存档转移
date: 2023-1-15 17:13:12
tags: ['死亡搁浅','steam','epic','存档']
categories: ['游戏']
permalink: /death-stranding-arcvhive-transfer/
photos: https://qcloud-cdn-static.lonepixel.cn/blog/2023/Death%20Stranding%202022_12_29%201_07_51.png
---
买了Steam版的死亡搁浅却发现在EPIC的存档不能通用？来看看这篇文章！
<!-- more -->
# 前置知识

## 本体
《死亡搁浅》的存档文件夹结构目录如下：
 * 23个自动存档（0-22）
 * 23个手动存档（0-22）
 * 23个快速存档（0-22）
 * profie目录（记录你的配置和个人数据）
 * Steam版会多一个 steam_autocloud.vdf 文件，里面只有你的 Steam Account ID信息。

## Steam

《死亡搁浅导演剪辑版》Steam版的游戏存档位置位于 `Users\[userName]\AppData\Local\KojimaProductions\DeathStrandingDC` 下的一个以纯数字命名的文件夹中，这个数字同时也是你的Steam Account ID（有网站管这个叫 ID3）。普通版则位于 `Users\[userName]\AppData\Local\KojimaProductions\DeathStranding`下。

## EPIC
而EPIC版的游戏存档目录位于 `Users\[userName]\AppData\LocalLow\KojimaProductions\DeathStrandingDC` 下的一个以随机字符组成命名的文件夹中，这个命名肯定也是EPIC的某种Account ID了，普通版也位于不带DC的父目录中。

# 核心操作

如果你直接对拷游戏存档，启动游戏时会报错 `Save Data is corrupted, and cannot be loaded.`, 但其实你可以在游戏的加载点选择界面看到不管是游戏章节进度还是截图都是完全正常的，这也说明存档其实是可以通用的（我觉得程序员也不会闲着蛋疼为两个平台开发截然不同的存档存储结构）。

经过一番蹩脚英语激情搜索，找到了一个可用方法：用任意 Hex编辑器（没有就推荐HxD），打开`DeathStranding.exe` ，搜索 `75 05 41 C6 46 3A 11 48 8D 8D` 然后替换为 `EB 05 41 C6 46 3A 11 48 8D 8D`，重新启动游戏，加载最新的一个存档，成功进入游戏后再保存一次存档，最后还原对游戏主程序的修改即可。

# 但是

需要注意的是这样操作只会“修复”你选中加载后保存的那个存档，其余的存档依然不可用，当然你也可以选择不将游戏主进程的修改还原，这样存档校验就一直处于禁用状态，不过经过修改的游戏主程序可能无法通过下次Steam更新的文件校验就是（虽然我怀疑这个游戏后续是否还会release patch）。

## 成就

还有大家关心的成就问题，一句话总结就是：还可以被触发的成就会被记录，如果是计数类的成就会将过往数据计算在内：如查询100篇访谈。如果已经不能被触发，最典型的就是已经打通的章节成就，是不会激活的。建议全成就爱好者做好二周目的准备。

# 来源
本文的核心原理来自 
> https://www.youtube.com/watch?v=Yn5h5nB4pqQ
 