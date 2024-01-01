---
title: Quartz 发布 Obsidian_三步发布笔记
ctime: 2024-01-01 15:10
aliases: []
tags: []
NID: 
NIW: 
drafts: false
publish: true
status: "[[Seed|🍒]]"
---

只需三步，[Quartz](https://github.com/jackyzha0/quartz) 就能快速发布你的 [[Obsidian]] 笔记。

一、[[安装 NodeJs]]，选择 `v18.14` 以上版本

二、下载 `quartz` 项目，执行以下命令：

```bash
git clone https://github.com/jackyzha0/quartz.git
cd quartz
npm i
npx quartz create
```

`npx quartz create` 创建项目时，先选择 `Empty Quartz`，后期再添加要发布的笔记。


三、运行 `npx quartz build --serve`，发布网站，通过 [http://localhost:8080/](http://localhost:8080/) 访问。

![[1694094767556.png]]
