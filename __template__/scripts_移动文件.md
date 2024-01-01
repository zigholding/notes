---
title: scripts_移动文件
ctime: 2023-12-30 17:31
aliases: []
tags: []
NID: 
NIW: 
drafts: false
publish: true
status: "[[Seed|🍒]]"
---


[[this.app.vault]]
[[this.app.fileManager]]

```dataviewjs
console.log(this.app);
```

```dataviewjs
let msg = "-----移动文件-----<br><br>";
dv.span(msg);
console.log(msg);

let fs = require('fs');
let vault_api = this.app.vault;
let fileManager_api = this.app.fileManager;

console.log(fs);
// console.log(vault_api);
// console.log(fileManager_api);

function is_publish_file(path,verbose=false){
	let meta = dv.page(path);
	if(verbose){
		console.log(path,meta);
	}
	if(!meta){
		return false;
	}

	if(!meta.publish){
		if(verbose){console.log("!meta.publis:",!meta.publish)}
		return false;
	}
	if(meta.draft){
		if(verbose){console.log("meta.draft:",meta.draft)}
		return false;
	}
	return true;
}

function get_publish_files(aspage=false){
	let files = vault_api.getMarkdownFiles();
	let nfiles = files.filter((file)=>is_publish_file(file.path));
	if(aspage){
		return nfiles.map((file)=>dv.page(file.path));
	}
	return nfiles;
}

function get_attachments_of_files(files){
	let pages = files.map((file)=>dv.page(file.path));
	let res = new Set();
	for(let page of pages){
		if(!page){continue;}
		for(let tmp of page.file.outlinks){
			if(!dv.func.endswith(tmp.path,".md")){
				res.add(tmp.path);
			}
		}
	}
	return res;
}

function move_file(src,target){
	let slink = dv.func.link(src);
	let sfile = vault_api.getAbstractFileByPath(slink.path);
	
	if(!sfile){
		dv.span("<br>原文件不存在: "+src);
		return false;
	}

	if(dv.func.endswith(target,"/")){
		target = target + sfile.name;
	}
	
	let tlink = dv.func.link(target);
	let tfile = vault_api.getAbstractFileByPath(tlink.path);
	if(tfile){
		dv.span("<br>目标文件已存在: "+target);
		return false;
	}

	console.log(sfile);
	if(!dv.func.endswith(target,"."+sfile.extension)){
		dv.span("<br>目标文件类型不一致: "+target);
		return false;
	}
	let dst = target;
	if(!target.includes("/")){
		dst = sfile.parent.path+"/"+target;
	}
	dv.span("<br>"+slink.path+"->"+dst);
	fileManager_api.renameFile(sfile,dst);
	
	return true;
	
}

function move_publish_attachments(
	dst_folder="__public__",src_folder="__files__"
){
	if(!dv.func.endswith(dst_folder,"/")){dst_folder=dst_folder+"/"};
	if(!dv.func.endswith(src_folder,"/")){src_folder=src_folder+"/"};

	dv.span("<br>---将公开附件移动到 dst_folder 中---<br>");
	let files = get_publish_files();
	let attachments = get_attachments_of_files(files);
	for(let tmp of attachments){
		if(!dv.func.startswith(tmp,dst_folder)){
			move_file(tmp,dst_folder);
		}
	}

	dv.span("<br><br>---将 dst_folder 中非发布的附件移到 src_folder---<br>");
	let afiles = vault_api.getFiles().filter(
		(file)=>dv.func.startswith(file.path,dst_folder)
	).map(
		(file)=>file.path
	).filter(
		(file)=>!attachments.has(file)
	);
	
	for(let tmp of afiles){
		move_file(tmp,src_folder);
	}
}


function copy_file(src, dst) {

	if(!fs.existsSync(src)){
		dv.span("<br>原文件不存在: "+src);
		return;
	}

	if(fs.existsSync(dst)){
		if(fs.statSync(dst).mtimeMs>=fs.statSync(src).mtimeMs){
			console.log("<br>目标文件新于旧文件: "+src);
			return;
		}
		fs.unlinkSync(dst);
		
	}

	fs.copyFileSync(src,dst);
}

function copy_file_between_vault(path,xvault){
	let root = vault_api.adapter.basePath;
	let src = root + "/" + path;
	let dst = xvault + "/" + path;
	copy_file(src,dst);
}

function copy_publish_vault(xvault){
	dv.span("Copy to: "+xvault);
	let files = get_publish_files();
	let attachments = get_attachments_of_files(files);
	console.log(files);
	for(let file of files){
		copy_file_between_vault(file.path,xvault);
	}

	for(let file of attachments){
		copy_file_between_vault(file,xvault);
	}

}

function run(mode){

	// 将当前 vault 下的公开笔记附件移到 __pulbic__ 下
	if(mode==1){
		move_publish_attachments();
	}

	// 将当前 vault 下的公开笔记附件移到 __pulbic__ 下
	if(mode==2){
		let xvault = "D:/iLanix/website/quartz/content";
		copy_publish_vault(xvault);
	}

	// 将当前 vault 下的公开笔记附件移到 __pulbic__ 下
	if(mode==3){
		let xvault = "D:/iLanix/website/notes";
		
		copy_publish_vault(xvault);
	}
	
}

console.log(fs);
run(3);
//console.log(get_publish_files());

console.log(msg);
```

