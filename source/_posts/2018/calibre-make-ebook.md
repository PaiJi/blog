---
title: Calibre制作电子书踩坑实录
tags:
  - calibre
  - ebook
  - kindle
categories:
  - - 玩具箱
date: 2018-04-26 20:58:19
permalink: 'calibre-make-ebook/'
photos: ['https://images.pexels.com/photos/59143/pexels-photo-59143.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=2']
---

> Booklover's best friend.——Amazon

本文主要讲述了如果你想要制作一本能在kindle上完美显示封面和目录的电子书，所要踩多少坑的故事。
<!-- more -->
## 0.基础知识补习

*   Kindle的邮件推送支持mobi，不支持azw3和equb，哪怕你加Convert也不行，不不不，多贵的kindle也不行。
*   直接把azw3放进Kindle的document目录，无法显示封面。
*   制作好的azw3格式电子书直接转mobi下发至kindle，可能会遭遇封面大小崩坏的问题。

## 1.azw3还是equb？

这个问题的前提是，我们假定你只有一个txt文档，如果你是从其它地方找来的equb，本文可能对你没有什么作用。 

在你第一次打开Calibre并且使用默认配置的时候，你可能会遇到的第一个问题是，要转换成什么格式好呢？Calibre提供了一大堆格式给你，但其实只有azw3和equb格式是可编辑内容的。后续Calibre会提示你，我只能编辑这两种格式哦科科。但是当你看到这个提示的时候一般为时已晚...... 

![](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_201153.jpg)
*丰盛的选项*
![](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_201310.jpg)
*早说啊你*


如果你有一个实体的Kindle，**并且你不在乎这本电子书能否进行多设备、进度、笔记同步，**azw3格式当然是最方便的，制作完成后可以通过Calibre+USB连接Kindle的方式直接导入，这个方法可以看到封面。 如果你打算在Kindle Android、Kindle iPhone上阅读您的电子书或者你有**多设备、进度、笔记同步**的需求，那可以优先选择equb，因为反正最后都要转成mobi才能上传到Kindle 图书馆。 _Tips:Azw3格式不能重命名章节html文件的名称，只能删除重建，equb可以。_

## 2.添加图书基本信息

这一步相对来说可能比较轻松，主要是利用搜索引擎从各种渠道找到图书信息，你甚至可以直接在线导入，不过可想而知不是百试百灵的。软件也自带豆瓣插件，默认没有开启，应该是能抓取到豆瓣对应图书的信息，可以按需食用。填好基本信息后可以加个封面啥的，丰富一下显示内容。

请注意，Kindle并不会去强制缩放图书封面大小，所以Kindle里的图书高低长宽都不一样，强迫症看了想戳瞎双眼，因此请一定确保至少你自己做的的系列图书的封面都是固定大小。
![](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_201540.jpg)
*根本不会有人看的meta信息*

## 3.打开看看里面装的到底是啥

当普通txt导入Calibre之后，前端同学可能会觉得这个所见即所得编辑器好像很眼熟，这些元素命名，这，这不就是HTML和CSS么！真是人生何处不相逢。 所以马上就能联想到，原来目录和章节的实现其实就是在不同的html文件间切换。因此，如果你想要自己做目录，并且是可以跳转的真·目录的话，你就要开始手动把整篇txt拆成一章一章的内容，关于作者，出版信息，封面直接沿用本体的就可以。主要是拆分的工作量一点也不小。以及，没有实体按键的Kindle，如果想要尽量减少触摸屏幕的次数，就要调低行距，少用大间距。毕竟你肯定不想看完一本电子书之后觉得指纹都快磨掉了吧。

## 4.生成一个目录

其实如果每一章的第一行就是章节名称的话就非常好办啦，Calibre支持一键生成，但是对于一本电子书来言，一般会有两个目录，一个目录是阅读器会读取的目录信息，直接展示在菜单里，安卓Kindle客户端是在侧边栏里面。另外一个则是在电子书内 单独一页体现的内联目录。对于后者而言，你可以在生成第一种目录后直接插入，不过一般不咋好看就是了，我还是倾向于直接在已有的目录上加\<a>标签。 
![](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_202142.jpg) 
*最喜欢的偷懒按钮*
![](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_202324.jpg) 
*目录其实长这样* 

## 5. 不必要，但是难受

看到这里的时候，你自己的电子书差不多已经做好了。插上Kindle，用Calibre同步进去就能看个爽，封面也是OK的。 但是当你把图书上传至亚马逊图书馆，或者是用安卓客户端打开的时候你会发现 **用Calibre转换后的 mobi 封面崩了**。 

原因大概是Calibre创建出的文件内部的版本号过低？没有深究，反正他们自己的官方转换工具是这么说的。 
![](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_202951.jpg)   
根据多次浪费时间转换文件再发邮件给Kindle的试错，只要KP3工具说你太旧，你转换出来的mobi文件封面就一定是崩的，所以我们稍微走一下弯路。回到开头，我建议读者如果能够将就将就，azw3格式是最好的，USB直接导入kindle也可以直接看。如果你非要做出一个完美的mobi文件，首先需要在Calibre中将azw3格式的图书转换为equb，然后在KP3工具中打开equb格式的图书,最后再从文件->导出。
![](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_203839.jpg)
*成了！*
现在得到的mobi文件就是可被kindle完美识别的版本，具体的格式转换路径如下：txt->azw3->equb->mobi 
最后的成品如下： 
![](https://qcloud-cdn-static.lonepixel.cn/blog/IMG_20180426_205222.jpg) 
*用一张1.4M的图片塞爆你的浏览器*

## #感谢以下大佬的文章

*   [Kindle封面的大小问题](https://aimingoo.github.io/1-1729.html)
*   [Kindle官方转换工具](https://bookfere.com/post/92.html)