---
title: Dataview 索引笔记
ctime: 2023-11-29 14:34
aliases: 
tags: 
NID: 515.4
NIW: 
status: "[[Seed|🍒]]"
drafts: false
publish: true
---

还有用文件夹？快试试 DataView 创建索引笔记

怎么查看你的笔记？点击文件夹一层层查找，或者通过标签，再或者想起相关笔记后查看它的双链。

等等，我现在有上千条笔记，我需要在一个页面中查看特定主题但散落在不同文件夹的笔记，最好还能看到每个笔记的修改时间、字数和某些状态。文件夹、标签、双链等基础功能已不足以管理繁杂的笔记。

当我抓耳挠腮时，DataView 自告奋勇站了起来说：”让我来吧，像数据库一样管理你的笔记，展示你所需的一切。"

例如，我想罗列下列条件的笔记：
- 在 `笔记方法论` 文件夹下；
- 或打有 `#笔记方法论` 标签的；
- 或链接了笔记 `[[笔记方法论]]` 。

在 `笔记方法论` 下添加笔记 `🧭IDX-Folder`，插入DataView 查询代码，以表格罗列出 `笔记方法论` 相关的笔记，并给出修改时间、笔记字数。

![[Pasted image 20231129182203.png]]

`🧭IDX-Folder` 会罗列其所在文件夹下所有笔记（包含子文件夹）、以文件夹为标签或链接文件夹同名笔记的所有笔记。可以通过 `move current fle to another folder` 命令移动到指定文件夹下，查看不同文件夹的索引文件。例如，移动到 Obsidian 文件夹下将显示 Obsidian 相关笔记索引。
![[Pasted image 20231129182734.png]]

> [!NOTE]- DataView 查询代码
> ```dataview_text
> TABLE without ID
> 	file.link as 标题, 
> 	status as 状态,
> 	dateformat(file.mtime,"MM-dd") as 修改,
> 	round(file.size/3) as 字数
> WHERE (
> 	(
> 		contains(
> 			file.folder, 
> 			this.file.folder
> 		)
> 	) or
> 	(
> 		contains(
> 			file.tags,
> 			reverse(
> 				split(this.file.folder,"/")
> 			)[0]
> 		)
> 	) or
> 	(
> 		contains(
> 			file.outlinks,
> 			link(
> 				reverse(
> 					split(this.file.folder,"/")
> 				)[0]
> 			)
> 		)
> 	) or
> 	(
> 		contains(
> 			file.inlinks,
> 			link(
> 				reverse(
> 					split(this.file.folder,"/")
> 				)[0]
> 			)
> 		)
> 	)
> ) and 
> file.name!=this.file.name and  !contains(file.name,".excalidraw")
> SORT status,file.mtime desc
> LIMIT 100
> ```
> 

不用做任何更改，复制代码到你的笔记中，即可实现上述功能。下面是主要代码的介绍：
- `round(file.size/3) as 字数`：Obsidian 中以 utf-8 编码，1 个汉字占 3  byte；
- `reverse( split(this.file.folder,"/") )[0]`：当前笔记所在目录名称；
- `link(name)`：`[[name]]`对应的链接
- `status as 状态`：笔记定义的 YAML 元数据，`status` 用于标注当前笔记的状态；

使用 DataView 索引笔记的另一个主要原因是为笔记建立编码。

![[为笔记添加编码]]

![[Pasted image 20231129215543.png]]


### 根据索引查看前后笔记



```dataview
TABLE without ID
	file.link as 前一个笔记,
	status as 状态,
	dateformat(file.mtime,"MM-dd") as 修改,
	round(file.size/3) as 字数
	
WHERE (
		contains(
			file.folder, 
			this.file.folder
		)
) and NID < this.NID
SORT NID desc
LIMIT 1
```

```dataview
TABLE without ID
	file.link as 后一个笔记,
	status as 状态,
	dateformat(file.mtime,"MM-dd") as 修改,
	round(file.size/3) as 字数
	
WHERE (
		contains(
			file.folder, 
			this.file.folder
		)
) and NID > this.NID
SORT NID
LIMIT 1
```



Cue：
- [[笔记方法论]]
- [[Obsidian]]