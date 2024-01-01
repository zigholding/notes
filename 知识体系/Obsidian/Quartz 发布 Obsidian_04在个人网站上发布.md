---
title: Quartz 发布 Obsidian_在个人网站上发布
ctime: 2024-01-01 14:45
aliases: []
tags: []
NID: 
NIW: 
drafts: false
publish: true
status: "[[Seed|🍒]]"
---


公众号中插入代码的阅读体验太差了。刚好年前备案的域名审核通过，在 2024 年的第一天，网站和大家见面。用的方式就是 Quartz 发布 Obsidian 笔记。

这里介绍下主要配置。
- 服务绑定网站
- 快速发布笔记



在 GitHub 上新建项目 notes 用于保存公开笔记。将  Quartz 中的 content 绑定到 notes 仓库中。代码如下：

```
git clone https://github.com/jackyzha0/quartz.git
cd quartz
npm i
npx quartz create # 选择 Empty Quartz

rm .git -rf # 删除 git 配置

cd content
git remote add origin git@github.com:zigholding/notes.git
git pull origin master
npx quartz build --serve
```

利用 Nginx 将网站请求转发到 `localhost:8080`。

```config
server {
	listen 443 ssl;
	server_name zigholding.com;
	ssl_certificate /etc/nginx/zigholding.com_bundle.crt;
	ssl_certificate_key /etc/nginx/zigholding.com.key;
	ssl_session_timeout 5m;
	ssl_protocols TLSv1.2 TLSv1.3;
	ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
	ssl_prefer_server_ciphers on;

	listen       80 default_server;
	server_name  www.zigholding.com;
	include /etc/nginx/default.d/*.conf;

	location / {
		proxy_pass http://localhost:8080;
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
	}
}
```


