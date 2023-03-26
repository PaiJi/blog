---
title: 限制Spotify的缓存大小
tags: []
categories:
  - - uncategorized
date: 2019-06-05 15:25:57
permalink: 'limit-spotify-cache-size/'
photos: ['https://images.pexels.com/photos/1113804/pexels-photo-1113804.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=2']
---
一个三分钟生活小妙招，教你限制Spotify的缓存文件夹体积。
<!-- more -->

这功能明明是做了的，但是没有UI界面不在设置里，不是很理解Spotify的思路？

anyway，根据用户论坛的讨论贴，对于Windows用户，路径是：

 %USERPROFILE%\\AppData\\Roaming\\Spotify 

对于Mac用户，路径是：

 ~/Library/Application Support/Spotify/

找到目录里的prefs文件，用随便一个文件编辑器打开，只要不是记事本......在最后加上

```
storage.size=1024
```

这里的单位是MB，请自行调整。操作前请先关闭Spotify。