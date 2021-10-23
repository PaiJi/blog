---
title: Kindle地狱，Calibre制作电子书踩坑实录
tags:
  - calibre
  - ebook
  - kindle
categories:
  - - 玩具箱
date: 2018-04-26 20:58:19
permalink: 'calibre-make-ebook/'
---

> Booklover's best friend.——Amazon

这个friend在加帕里公园可能不怎么招人喜欢，如果可以的话希望天蓝怪送他一程。 本文主要讲述了如果你想要制作一本能在kindle上完美显示封面和目录的电子书，所要踩多少坑的故事。
<!-- more -->
## 0.基础知识补习

*   Kindle的邮件推送支持mobi，不支持azw3和equb，哪怕你加Convert也不行，不不不，多贵的kindle也不行。
*   直接把azw3放进Kindle的document目录，无法显示封面。
*   制作好的azw3格式电子书直接转mobi下发至kindle，可能会遭遇封面大小崩坏。

## 1.azw3还是equb？

这个问题的前提是，我们假定你只有一个txt文档，如果你是从其它地方找来的equb，本文可能对你没有什么作用...... 在你第一次打开Calibre并且开箱即用的时候，你可能会遇到的第一个问题是，要转换成什么格式好呢？Calibre提供了一大堆格式给你，但其实！只有azw3和equb格式是可编辑内容的！后续Calibre会提示你，我只能编辑这两种格式哦科科。但是当你看到这个提示的时候一般为时已晚...... [![2018-04-26_201310.jpg](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_201310.jpg)](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_201310.jpg) [![2018-04-26_201153.jpg](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_201153.jpg)](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_201153.jpg) 如果你有一个实体的Kindle，**并且你不在乎这本电子书能否进行多设备、进度、笔记同步，**azw3格式当然是最方便的，制作完成后可以通过Calibre+USB连接Kindle的方式直接导入，这个方法可以看到封面。 如果你打算在Kindle Android、Kindle iPhone上阅读您的电子书或者你有**多设备、进度、笔记同步**的需求，那可以优先选择equb，因为反正最后都要转成mobi才能上传到Kindle 图书馆。 _Tips:Azw3格式不能重命名章节html文件的名称，只能删除重建，equb可以。_

## 2.添加图书基本信息，考据党和强迫症的大满足！

这一步简直可以说是最轻松的一步，考据党可以轻松的从中亚、日亚上找到ISBN、作者信息等等，上述操作甚至可以直接在线导入，不过对于没有在售的图书就没啥效果，有个豆瓣插件，默认没有开启，看大家的反馈，豆瓣的信息不是很全，我觉得还是自己来吧。当然，如果你不怕被骂的话，你甚至还可以在图书名称和简介里面加私货。比如《Yes Game Yes Life》第X卷 by XXX 爱心制作（呕）。最后加个封面啥的，图书基本信息就算搞定啦。请注意，Kindle并不会去强制缩放图书封面大小，所以Kindle里的图书高低长宽都不一样，强迫症看了想戳瞎双眼，因此请一定确保你的系列图书的封面都是固定大小。 \[caption id="" align="aligncenter" width="1708"\][![2018-04-26_201540.jpg](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_201540.jpg)](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_201540.jpg) 证明考据党对原作的爱的时候到了\[/caption\] 哦shit我才发现连Kindle商店里的封面大小都不一样，要死了

## 3.是元素的魔法？不，只是单纯的不同html文件···

当普通txt导入Calibre之后，你会发现，这个所见即所得编辑器好像很眼熟啊...这个,h1h2,h3元素，还有熟悉的格式......没错，就是我，HTML哒！！书内所有的什么行距加粗斜线都是通过HTML+CSS来实现的......总有一种人生何处不相逢的感觉呢- - 所以你也就会发现，如果你想强制翻页的话，并不是有一个元素叫<next page>，而是，这个章节html到此为止了.......... 因此，如果你是一个想要自己做目录，并且是可以跳转的真·目录的话，你就要开始把这一整个html拆成一章一章，关于作者，出版信息，封面也都一样啦......工作量一点也不小啊。 还有，没有实体按键的Kindle，如果想要尽量减少触摸屏幕的次数，就要调低行距，少用大间距。毕竟看完一本电子书之后我觉得指纹都快磨掉了！！

## 4.生成一个你其实不会怎么用但是必须要有的目录

其实如果每一章的第一行就是章节名称的话非常好办啦，Calibre可以一键生成，但是请注意，对于一本电子书来言，一般会有两个目录，一个目录是阅读器会读取的目录信息，直接展示在菜单里，安卓Kindle客户端是在侧边栏里面。另外一个则是在电子书内 单独一页体现的内联目录。对于后者而言，你可以在生成第一种目录后直接插入，不过一般不咋好看就是了，我还是倾向于直接在已有的目录上加<a>标签。 \[caption id="" align="aligncenter" width="952"\][![2018-04-26_202142.jpg](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_202142.jpg)](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_202142.jpg) 鸡排自己最喜欢这个\[/caption\] \[caption id="" align="aligncenter" width="928"\][![2018-04-26_202324.jpg](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_202324.jpg)](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_202324.jpg) 目录其实是这样的\[/caption\] 那个地址后面带的乱码我也不清楚咋回事，好像保存之后再打开编辑器就有了Orz，有时间我再研究研究。

## 5.请注意，地形障碍，地形障碍...

其实这一步已经很多余，当你的电子书做到这一步的时候已经差不多了，插上Kindle，用Calibre同步进去就能看个爽，封面也是OK的。 但是如果你非要把你的图书上传至亚马逊图书馆，又或者，你目前根本没有Kindle，只有个安卓客户端，而且你发现**用Calibre转换后的mobi 封面崩了！！！DAMN IT! 那么，下面的奇怪教程可能是你需要的......** 首先感谢一下亚马逊这家应该被天蓝怪教训一下的公司，因为对于自己家的azw3文件，他们自己的官方转换工具是这么说的。挺好的，自家公司不认自己格式。 [![2018-04-26_202951.jpg](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_202951.jpg)](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_202951.jpg)   根据多次浪费时间转换文件再发邮件给Kindle的心酸试错，只要KP3工具说你太旧，你转换出来的mobi文件封面就一定是崩的，所以这里我们要走一条简直莫名其妙的道路。回到开头，我建议读者如果能够将就将就，azw3格式是坠吼的，USB直接导入kindle要什么自行车！！！但是你都看到这里了，你肯定不是那号人......那么，我们应该请出最后的主角，equb了。请在Calibre中，将你的azw3格式的图书转换为equb，然后在KP3工具中打开equb格式的图书。 [![2018-04-26_203839.jpg](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_203839.jpg)](https://qcloud-cdn-static.lonepixel.cn/blog/2018-04-26_203839.jpg) 看到没有！他不挑食了！他吃饭了！他动了！ 最后，文件->导出，用你颤抖的双手把这个mobi文件以附件形式发到你的Kindle指定官方联系邮箱里................ 也就是说，我们的转换之路是这样的，txt->azw3->equb->mobi 哦我亲爱的华生，生活对于我们真是有些小严苛呢。 最后： \[caption id="" align="aligncenter" width="2868"\][![IMG_20180426_205222.jpg](https://qcloud-cdn-static.lonepixel.cn/blog/IMG_20180426_205222.jpg)](https://qcloud-cdn-static.lonepixel.cn/blog/IMG_20180426_205222.jpg) 用一张1.4M的图片塞爆你的浏览器\[/caption\]

## #感谢以下大佬的文章

*   [Kindle封面的大小问题](https://aimingoo.github.io/1-1729.html)
*   [Kindle官方转换工具](https://bookfere.com/post/92.html)