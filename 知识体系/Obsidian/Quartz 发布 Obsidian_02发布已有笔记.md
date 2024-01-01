---
title: Quartz 发布 Obsidian_发布已有笔记
ctime: 2024-01-01 15:11
aliases: []
tags: []
NID: 
NIW: 
drafts: false
publish: true
status: "[[Seed|🍒]]"
---


如果你已经记录了大量笔记，那么重要整理要发布的笔记可能让你望而生畏。

是时候展示真正的技术了，仅需几个改动，让你发布已有笔记。

在 `npx quartz create` 时，选择第三个选项 `Chose how Quartz should resolve links..`，然后输入或拖入已有的 Obsidian 库，`quartz` 将关联你的笔记库。

![[Pasted image 20231230110009.png]]

接下来，设置要发布的笔记。用文本编辑器打开 `quartz.config.ts` 。

一种方式是根据笔记过滤，设置项位于  `config->plugins->filters` ：
- `Plugin.RemoveDrafts()`：默认配置，过滤笔记元数据  `drafts: true` 的笔记。这种方式默认发布所有笔记，要对个人笔记添加元数据；
- `Plugin.ExplicitPublish()` ：与官方发布类似，发布 `publish : true` 的笔记；

为保险起见，发布 `drafts: false`  且 `publish: true` 的笔记。最终设置：
- `filters: [Plugin.RemoveDrafts(),Plugin.ExplicitPublish()]`

另一种方式是根据文件路径过滤，设置项位于 `config->ignorePatterns`：
- `some/folder` ：过滤文件夹 `some/folder`
- `*.md` ：过滤所有带有 `.md` 扩展名的文件
- `!*.md`：过滤所有没有 `.md` 扩展名的文件
- `**/private`：过滤任何嵌套级别 `private` 命名的任何文件

例如，过滤插件目录、模板目录、垃圾箱目标和个人目录：
- `ignorePatterns: [".obsidian",".trash","__template__","100一人之国"]`


`quartz.config.ts` 中的其它配置：
- `pageTitle`：网站标题，点击回到主页

![[Pasted image 20231230201739.png]]

### 零碎的事项


`quartz` 的主面是根目录下的 `index.md` 笔记。



`Quartz` 将 `/content` 下的笔记转换为 `/public` 下的 `html` 页面，主页为 `/content/index.md`。`/content` 下的内容有变化时，`Quartz` 能自动识别并发布。


错误：
- `duplicated mapping key`：元数据有重复字段
- `no such file or directory`：文件名中包含 emoji
- `URI malformed`：有些