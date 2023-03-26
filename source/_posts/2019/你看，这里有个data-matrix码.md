---
title: 你看，这里有个Data Matrix码
tags: []
categories:
  - - Diary
date: 2019-04-26 18:28:09
permalink: 'a-data-matrix-code/'
photos: ['https://images.pexels.com/photos/785418/pexels-photo-785418.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=2']
---

早上被人戳醒，对方发来一个奇怪的二维码：“这啥玩意啊，我也想生成一个，怎么解码再生成就不一样了呢”。怀着“这种小事我用草料二维码分分钟解决给你看的心情，我从床上爬了起来，然后就把早餐时间赔了进去......

![](https://qcloud-cdn-static.lonepixel.cn/blog/2019/a-data-matrix-code/data-matrix-code.jpg)
<!-- more -->
## 0.什么是Data Matrix码？

>   
> **Data Matrix**（数据矩阵），是一种由[黑色](https://zh.wikipedia.org/wiki/%E9%BB%91%E8%89%B2)、[白色](https://zh.wikipedia.org/wiki/%E7%99%BD%E8%89%B2)的色块（单元格）以[正方形](https://zh.wikipedia.org/wiki/%E6%AD%A3%E6%96%B9%E5%BD%A2)或[长方形](https://zh.wikipedia.org/wiki/%E7%9F%A9%E5%BD%A2)组成的[二维条码](https://zh.wikipedia.org/wiki/%E6%9D%A1%E5%BD%A2%E7%A0%81)（也可称[矩阵](https://zh.wikipedia.org/wiki/%E7%9F%A9%E9%98%B5)），于1994年8月由美国国际资料公司所研发出，主要用于零件、[印制电路板](https://zh.wikipedia.org/wiki/%E5%8D%B0%E5%88%B6%E7%94%B5%E8%B7%AF%E6%9D%BF)等等，美国国际资料公司于2008年被Microscan公司收购。[\[1\]](https://zh.wikipedia.org/wiki/Data_Matrix#cite_note-1)[\[2\]](https://zh.wikipedia.org/wiki/Data_Matrix#cite_note-2)[\[3\]](https://zh.wikipedia.org/wiki/Data_Matrix#cite_note-3)被编码的信息可能是文本或数字数据。数据大小通常是几个至1556[字节](https://zh.wikipedia.org/wiki/%E5%AD%97%E8%8A%82)。被编码数据的长度决定了矩阵中色块的数量。编码时经常使用[纠错码](https://zh.wikipedia.org/wiki/%E5%89%8D%E5%90%91%E9%8C%AF%E8%AA%A4%E6%9B%B4%E6%AD%A3)来增加可靠性：即便一个或多个色块被损坏而不可读，里面的信息仍然可被读取。一个数据矩阵可以存储最多2,335个[数字或字母](https://zh.wikipedia.org/wiki/%E6%96%87%E6%95%B8%E5%AD%97)。
> 
> 来自维基百科

说白了，这种码可能有的小伙伴见过，在PCB元器件或者比较小的电子设备上蚀刻而成，因为其最大的优势就是

>   
> 该编码能在2或3平方毫米的面积上编码50个字符，且在20%对比度下仍然可读。 绘制与读取系统的保真度是其唯一的限制。美国电子工业联盟（EIA）建议使用Data Matrix标注小型电子组件。
> 
> 还是从维基百科拖来的

不知道比QR码高到哪里去了！某些公交车上的二维码建议学习一个！

## 1.奇怪的字符

根据小伙伴介绍，这个DM码用微信扫描后能拉起小程序并传递数据。这里插曲一下，鸡排做过简单的小程序开发，没见过用这种数据就能拉起小程序的，考虑到该小程序的主体正是深圳腾讯计算机有限公司，怀疑是微信特权。

![DM码](https://qcloud-cdn-static.lonepixel.cn/blog/2019/a-data-matrix-code/data-matrix-code.jpg)
*用微信扫我*

接下来，当然是还原字符。分别使用了多款二维码扫描器对该DM码进行解析，Google play上的两款二维码扫描器虽然都成功认出了这个是DM码，但是得出的结果却略有差异：一款输出了一串数字，另一款在数字的开头和中间有一个空格——有什么东西没显示出来。打开电脑，一顿谷歌，找到了一个可以显示编码数据的在线解码网站，显示编码数据如下：

```
Raw text
�010729010483057010A0515-020�1118041517191013218835100515020417
Raw bytes
e8 83 89 9f 83 86 d5 87   c8 8c 42 87 91 2e 84 31
e8 8d 94 86 91 93 95 8c   8f 97 da a5 8c 87 91 84
86 93 81 b5 
```

破案了！你看这不就是十六进制+ASCII，那个显示不出来的东西一定就是导致无法转换回DM码的元凶！这个第一行和第二行的e8，看起来很可疑啊！先记在小本本上！  

## 2.直接转换，我不要面子的啊？

那么，剩下的十六进制就直接对应原文数字了吗？掏出田牌计算器摁了两下，发现数据都对不上，83十六进制转换回去是131 ，肯定不对。赶紧看看DM到底是怎么处理数据的：

>   
> DataMatrix编码的第一步骤，需要将原始信息转换成DataMatrix的码字，生成的码字范围（0,255）即unsigned char。通常的编码方式为Ascii编码，将原始字符+1即生成码字；同时为了压缩码长，若其中含有连续的两位数字，则将其+130后，生成一个unsigned char。如果要进一步压缩码长，还可以混合其他的编码方式：
> 
> 这里本来该有张图不过对本文没啥帮助就省略掉吧啊哈哈哈
> 
> 来自 [DataMatrix编码1——生成码字](http://blog.sina.com.cn/s/blog_4572df4e01019vx2.html)

中文搜索结果里，只有这一篇文章被孤零零的反复copy，好在它确实有效的解决了燃眉之急：字符+1，连续两位数字+130，83的十六进制是131，减去130是01，89的十六进制是137，减去130是07，确实符合了上面的Raw text。那么唯一剩下的，只有神秘的e8。

啊可能有小伙伴会问最后多出来的那几个十六进制数，查询文档进一步可知，生成MD码需要把码字的长度”补充“到一个固定长度上，这是为了避免在生成出来的MD码上出现大量空白，而具体补充到多长要依据实际有效字长查表。因此后面多出来的十六进制数就是根据算法填充的。

## 3.你以为我是ASCII字符，其实我是...

那么，最后的谜题，就在于这个e8身上。e8到底是什么？知道了它的本体就能把它重新编码，生成正确的二维码图。

根据上面初步了解到的规则，e8肯定不是按照数字来转换的，那就是字符喽？也就是说，原始数据是e8-1=e7——（ ç）！

鸡排高兴的把新的字符串组装了起来，生成的MD码却失败了......再对生成的MD码进行解码，发现e7被转换为：

```
Raw text
ç
Raw bytes
eb 68 81 
```

根本不是我们要的e8！思前想后，只能确定一件事：这个e8极有可能是某种约定的标识符，它的转换可能不遵循ASCII，甚至这个e8可能不按照16进制来进行解读.......那么这个e8在 DataMatrix的世界里，到底代表了什么？直接查找DataMatrix+e8这个关键词，看起来没有任何有价值的线索。

那么看看 DataMatrix的技术实现手册？也许里面有别的东西......一份76页的PDF（链接[戳我](https://www.gs1.dk/media/1233/gs1_datamatrix_introduction_and_technical_overview.pdf)），然而在里面，e8只出现在了多进制转换表格里。一无所获！简直浪费时间，我花了2个小时了解了DM码的故事，这挺不错，但是我果然还是应该先点个外卖填饱肚子，再去写写毕设！

我真的去点了个外卖........  
插播广告：复联4：终局之战正在热播（写这篇文章时），赶紧购买电影票吧！  
在这里鸡排给大家点一首复联主题曲，如果你看不到说明你无法顺利访问spotify

https://open.spotify.com/track/5SXsXjVJCWeJuf7FHvgBYR?si=H17go6jRT66WFkSSsSxIRA

点完外卖回来后，滚了两下手册，发现在进制转换表的下一页有一个表

```
Protocol used to encode ASCII in Data Matrix ECC 200
Extracted from the standard ISO/IEC 16022
Table 2 - ASCII encodation values
```

好像是我要找的东西！！定义的协议标识！一看表，是用十进制进行约定的，e8对应的十进制是232，查表可知，这个东西叫 **FNC1**

谷歌FNC1，立刻得到了解释：

> FNC1，全称是Function 1 Symbol Character，是GS1-128或者GS1-DataMartrix条形码编码中的第一个符号字符。  
> FNC1是Code 128字符集中的一个字符，是个特殊字符，在某些情况下，起到一定控制的作用。  
> FNC1不是ASCII字符集中（可见或非可见）的字符。  
> 需要多说明一点的是，ASCII中的，共0-0xFF,256个字符，其中0-0x1F共32个控制字符，叫做不可见字符，余下0x20-0xFF就是我们常见的大小写字母，数字，常见符号等等，称作可见字符。  
> 而FNC1本身就不是属于ASCII中的，所以也不是ASCII中的那种不可见的控制字符。对此，需要特别注意，不要再搞混淆了。
> 
>   
> [https://www.crifan.com/files/doc/docbook/symbology\_gs1128/release/html/symbology\_gs1128.html#ch05\_fnc1\_details](https://www.crifan.com/files/doc/docbook/symbology_gs1128/release/html/symbology_gs1128.html#ch05_fnc1_details)

## 4.烤肉饭真好吃

知道了e8的真面目 **FNC1**，剩下的问题就是：如何输入它？既然这是一个不可见字符，自然也没法复制，鸡排已经开始思考单片机课程上的操作：去拼接十六进制结果。但是这个操作很明显不适用于当下场景......

最后的解决方案倒也惊人的简单：在这一堆DataMartix码生成网站里，有一家[网站](https://barcode.tec-it.com/zh/DataMatrix?data=%C3%A7010729010483057010A0515-020%5CF1118041517191013218835100515020417)专业的提供了 **_评估转义序列 ： 见_** [**_条形码参考_**](https://www.tec-it.com/download/PDF/Barcode_Reference_EN.pdf)**_: 使用 \\F 为 FNC1, \\t 为 TAB, \\n 为ENTER_**

将\\F添加进原文字符，生成MD码，掏出你的大手机，用微信一扫，拉起小程序，再解码查看，就是我们要的e8，和开头的原图完 全 一 致。

写了这么多废话，本文到这里就结束鸟。花了两个小时搞了个这个，个人感觉很有意思，乘着还没忘记过程赶紧写了下来。

最后，烤肉饭真好吃！复联4挺不错的！如果不咕的话，下一篇就来讲讲流媒体吧！

## 5.本文用到的网站

*   可以显示编码数据的解码网站[zxing.org](https://zxing.org/w/decode.jspx)！
*   能输出e8的编码网站[TEC-IT](https://barcode.tec-it.com/zh/DataMatrix?data=%C3%A7010729010483057010A0515-020%5CF1118041517191013218835100515020417)！
*   还是有好东西的[新浪博客](http://blog.sina.com.cn/s/articlelist_1165156174_0_1.html)，感谢这位大佬，再鄙视一下某C字开头的中文毒瘤社区，不要copy了。
*   GS1-128条形码和相关的AI及FNC1的[详解](https://www.crifan.com/files/doc/docbook/symbology_gs1128/release/html/symbology_gs1128.html#ch05_fnc1_details)—— Crifan Li
*   DataMartix[手册](https://www.gs1.dk/media/1233/gs1_datamatrix_introduction_and_technical_overview.pdf)