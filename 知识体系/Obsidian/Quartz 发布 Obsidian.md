---
title : Quartz 发布 Obsidian
ctime : 2023-09-07 17:46
mtime : 2023-09-07 17:49
aliases : [Quartz]
tags : []
NIW : 
drafts: false
publish : true
---


## 三步发布 Obsidian 笔记

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

## 发布已有笔记


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



## 快速添加元数据

在 `__template__` 文件夹下新建文件 `quartz_pulbish`，内容如下：

```md

drafts: false
publish: true
```

在  [[Templater]] 设置页面，点击 `Template Hotkeys` 下 `Add new hotkey for tempalte`，输入 `__template__/quartz_pulbish.md` ，为插入发布所需的元数据创建快捷键。

在 [[cMenu]] 设置页面，点击 `cMenu commands` 中的加号按钮，选择 `Templater:Insert__template__/quartz_pulbish.md`，添加 `cMemu` 按钮。

现在，可以通过快捷键或点击图标插入元数据，快速发布笔记。


## 发布到 [[GitHub Pages]]

[[GitHub Pages]] 是 GitHub 提供的一项功能，用于托管和发布静态网页。它允许用户将他们的代码仓库转化为在线可访问的网站，并可以通过 GitHub 提供的域名（username.github.io）或自定义域名进行访问。

在 [[GitHub]] 上新建项目 `quartz`，点击 `Setting->Pages`，将 `Build and deployment` 下的 `source` 设置为 `Github Action`。

`cd` 到 `quartz` 目录，将 `github` 地址更改为自己的项目地址。

```
git remote -v
git remote remove origin
git remote remove upstream
git remote add origin git@github.com:zigholding/quartz.git
```

[[github 配置SSH]]，运行 `npx quartz sync` 同步。

初化里可能不成功，提示 ` couldn't find remote ref v4`，手动推送 `git push origin v4`。

现在，就可以在 [zigholding.github.io/quartz](https://zigholding.github.io/quartz/) 访问了。

