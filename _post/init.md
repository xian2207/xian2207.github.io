---
layout:     post
title:      Git指令整理
subtitle:   不适合阅读的整理的一些个人常用的 Git 指令
date:       2017-02-15
author:     BY
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - 终端
    - Git
---

>随便整理的一些自用的Git指令


# GitHub创建仓库提示代码

	echo "# 项目名" >> README.md
	git init
	git add README.md
	git commit -m "first commit"
	git remote add origin git@github.com:qiubaiying/项目名.git
	git push -u origin master

若仓库存在直接push

	git remote add origin git@github.com:qiubaiying/test.git
	git push -u origin master


# 常用操作

#### 创建仓库（初始化）
	在当前指定目录下创建
	git init
	
	新建一个仓库目录
	git init [project-name]
	
	克隆一个远程项目
	git clone [url]
	
#### 添加文件到缓存区

	添加所有变化的文件
 	git add .

	添加名称指定文件
	git add text.txt