---
layout: post
title: git help
date: '2015-09-12'
cateory: git
tags: [git,github]
---

##说明
>	git 是一个很好的项目协作工具，大多数开源项目已经摒弃svn，使用git，github作为目前最流行的开源code社区，更是容纳了巨多的好项目。掌握git和github的使用，是一个程序员的必要技能。

##基本操作
克隆远程git地址  
`$git clone url`

将所有修改过的工作文件建立库索引    
`$git add . `

提交内容到本地git仓库   
`$git commit`

发布本地git仓库的内容到远程仓库   
`$git push ` 
##tag  

显示所有tag:   
`$ git tag` 

创建一个tag	:  
 `$ git tag 1.0.0` 

补打一个tag:  
`$ git tag -a 1.0.0 9fbc3d0`

删除一个tag:  
`$ git tag -d 1.0.0`

显示tag的信息:  
`$ git show 1.0.0`

发布tag:  
`$ git push origin 1.0.0`

发布所有tag:  
`$ git push origin –tags`
	


