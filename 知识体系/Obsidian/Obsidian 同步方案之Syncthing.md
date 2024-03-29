---
title: Obsidian 同步方案之Syncthing
ctime: 2023-12-20 14:12
aliases: []
tags: []
NID: 
NIW: 
drafts: false
publish: true
status: "[[Seed|🍒]]"
---



> [!tip]- Cue
> 仅三天我弃用坚果云！Obsidian 同步用Syncthing真香
> 
> [[Obsidian]]


自上周 [[Obsidian 同步方案之坚果云|使用坚果云同步 Obsidian]] 以来，发现电脑之间、手机和电脑之间经常出现冲突。少量冲突还可以手动修改，但随着笔记增加，这种额外的工作量变得难以忍受。网友推荐微力同步，了解到微力同步基于开源软件 [[syncthing]]，因此决定上手尝试 syncthing 。微力同步官网有详细的[中文教程](http://www.verysync.com/manual/)，可直接食用。

测试使用主电脑、副电脑和一副小米手机。

### 安装软件

首先到 [syncthing官网](https://syncthing.net/index.html) 下载软件，解压后打开 `syncthing.exe`，浏览器会打开 `http://127.0.0.1:8384/` 界面，初始化时建议在 `设置->图形用户界面` 中设置用户名和密码。手机需要在 google 市场安装软件（微云支持网页下载apk以及更多平台）。

![[Pasted image 20231220075621.png]]
![[Pasted image 20231220075656.png]]

### 添加设备

在主电脑中点击网页右上角 `操作` 下拉选项，点击 `显示ID`。

![[Pasted image 20231220075841.png]]
![[Pasted image 20231220080018.png]]

副电脑点击主面 `添加远程设备`，输入上一步显示的设备ID，输入自定义的设备名称。手机添加设备时可以扫码添加。

![[Pasted image 20231220094757.png]]

添加之后，主电脑会收到添加新设备的请求，点击添加设备，便可以设备列表中看到设备。

![[Pasted image 20231220080441.png]]



### 添加同步文件夹

在主电脑点击 `添加文件夹`，输入文件夹标签和文件夹路径，在共享中勾选副电脑和和手机，注意不要输入加密密码，否则同步的是加密数据，不能正常查看。

![[Pasted image 20231220095733.png]]

等待一段时间后，副电脑和手机会收到添加文件夹的请求，点击添加。手机需要在点击左上角三条杠按钮，点击`网页管理页面`，才能看到添加请求。

![[Pasted image 20231220080609.png]]

到此，三台设备的同步就完成了。

### syncthing 同步效果

在电脑上修改笔记，1分钟之内就能在手机上完成同步。另外，syncthing 可以在局域网上同步，但如果笔记本连接手机热点，关闭网络，是不能同步的。

![[Pasted image 20231220100600.png]]


另外一个问题是，在非局域网下，多台设备同步存在问题。同时添加了副电脑和手机，手机一直没有完成同步，经常会断开链接。将副电脑暂停连接后，手机的同步速度变快。

![[Pasted image 20231220100834.png]]

将三个设备暂停同步目录，在 `第01篇笔记` 中分别添加：
- 主电脑添加：主电脑在 10:20 修改。
- 副电脑添加：副电脑在 10:21 修改。
- 手机添加：手机在 10:21 修改。

之后再开启同步。所有的设备笔记内容都同步为：手机在10:21修改。syncthing 的同步以最后的更改为主，会覆盖冲突的修改。同时保留冲突文件。

![[Pasted image 20231220103909.png]]

如果在副电脑的文件夹共享选项中，将取消共享主电脑和手机，此时主电脑提示远程设备尚未允许分享此文件夹。主电脑中的修改不能共享给副电脑。但如果副电脑取消共享主电脑，但保留共享手机。主电脑的修改会传递给手机，再传给副电脑。

![[Pasted image 20231220103008.png]]

总之，多台设置之前通过共享链同步文件，两个节点之间的响应速度很快，笔记大约在 10s 内可以完成同步，如果笔记间有冲突，以最后的修改为主。多台设备共享，除了初始化时同步文件夹较大外，其它时间用得很丝滑。