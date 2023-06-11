---
title: 都2023年了，别再用 Yarn v1 啦
date: 2023-06-11 12:49:36
tags:
categories: [FrontEnd]
permalink: dont-use-yarn-v1-anymore/
photos:
---
最近在 Cloudflare Page 上运行 Next.js 时，log很无情的抛了一堆错，其中之一的原因是，CloudFlare Page 默认以 Yarn3 和 Node.js 18 作为builder runtime，而cli尖锐地指出：检测到yarn v1。这下我坐不住了，什么，我在用v1，我不是在用v2吗😧？
<!-- more -->
## 你以为的
一番调查后我才发现，并不是你用上了 Node.js 16.10 之后，通过

```bash
corepack enable
```

启用 Yarn 就算吃上 Yarn V2了。这个页面的顶部的横幅让我误以为用这个方式安装就是“现代化的版本”🤦：
>This documentation covers modern versions of Yarn.  For 1.x docs, see classic.yarnpkg.com.


## 新版好处都有啥

看了一圈文档，v2最大的变化就是 [PnP](https://yarnpkg.com/features/pnp)，毕竟 传统的 node_modules 目录形式在发展这么多年后可谓是饱受诟病，最大的痛点就是：体积IO两开花，开发者磁盘苦不堪言。也正因如此才导致了pnpm的诞生。

而用上了 PnP后，这个问题能得到有效的优化，从安装速度到IO到启动都有了很大的提升。具体的内容都在这里👉 [PnP](https://yarnpkg.com/features/pnp)。

## 开干！
开始前请先确保你的 yarn v1 已经是最新版，这么多年了不会有人连 yarn v1 都不是最新吧。

官方的迁移帮助里说从v1升级到v2，只需要跑这行命令设置版本就行：
```bash
yarn set version berry
```

看到如下结果就说明已经成功安装v3，你也可以用 `yarn -v` 看看版本号。
```
➤ YN0000: Retrieving https://repo.yarnpkg.com/3.6.0/packages/yarnpkg-cli/bin/yarn.js
➤ YN0000: Saving the new release in .yarn/releases/yarn-3.6.0.cjs
➤ YN0000: Done in 0s 549ms
```

此时，你的项目根目录中应该出现了一个 .yarnrc.yml 文件，里面包含了一行默认配置：
```
yarnPath: .yarn/releases/yarn-3.6.0.cjs
```

根据官方文档，PnP结构是可选的，你可以通过这样写来继续使用以前的 node-modules，以便于平滑过渡: 
```
nodeLinker: "node-modules"
```

但是既然追求刺激，我当然是一路到底：
```
nodeLinker: "pnp"
```
(或者直接不加这行，因为PnP是 Yarn v2之后的默认行为)

然后运行 `yarn install`，node_modules 目录会被删除，取而代之的是全新的 .yarn 目录， 你马上就会注意到，新的 .yarn 目录的体积较原始相比，小了不少。

最后，面对这些新出现的海量文件，你需要更新一下你的 `.gitignore`，请参考这篇官方文档。[which-files-should-be-gitignored](https://yarnpkg.com/getting-started/qa#which-files-should-be-gitignored)。

## 有些不太一样
### Yarn global 已被替换为 Yarn dlx
根据官方文档[Renamed](https://yarnpkg.com/getting-started/migration#renamed)的描述，`yarn global` 已经被替换为了`yarn dlx`，用来执行那些只运行一次的代码。看起来这就是yarn版的 `npx`。
### 使用yarn run而不是  node_modules/.bin
这个目录都已经灰飞烟灭了，请改用 `yarn run`。
### 使用 yarn node 而不是直接 node 来运行你的脚本
简单来说以前我们会直接运行JS文件，像这样：
```node
node main.js
```
但是有了PnP之后，查找依赖的方式发生了变化，你得先把依赖描述文件注入到运行时里。如果你使用 `yarn script`，这个步骤是自动的。

因此，现在请改用 :
```node
yarn node main.js
```


## 新的很好，但是不动更好
根据[官方文档](https://yarnpkg.com/features/pnp/#incompatible)，如果你在使用 react-native 或者 flow，那么你还得用传统的 `node_modules`，你可以简单的躺平——或者，安装新版的yarn但是将:
```
nodeLinker: "node-modules"
```
原汁原味的 node-modules，现已通过 Yarn berry 全新呈现。

## 最后
其实 Yarn V2 首个正式版本是在2020年发布的，距离写下这篇文章已经过去了三年，而最新的版本已经到了 Yarn 3.6，但是总感觉普通开发者对于升级 Yarn 版本这件事积极性不高，毕竟 1.X 什么都能做，“又不是不能用”，感觉就算有一天yarn 一路飙到了 V100，世界上还是有很多项目在用1.X🤔️。