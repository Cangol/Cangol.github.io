---
layout: post
title: Android project format
date: '2013-12-07'
---
优秀的代码，甚至可以不需要注释。项目开发，特别是协同和开源，代码一定要规范，已提高可读性。  
我们坚信：约束优于配置。  
以下列出我们的Android App项目代码基本规范(欢迎指正，仅供参考)    
包括：Java代码、
资源文件命名规范（color、drawable、string、id、style）、vcs工具（svn和git）的注释规范.
                                                                                     


##res资源命名规范
不分模块的项目无“模块”前缀，不强调通用资源的项目无“模块”前缀

	标准： 模块_组件_功能_类别_状态

###color selector
	模块_组件_类别_状态
###drawable selector
	模块_组件_功能_类别_状态
###string
	模块_功能_类别_状态
	
###layout 命名规范
	模块_功能_类别_状态
	activity_功能_状态
	fragment_功能_状态
	layout_功能_状态
	view_功能_状态
	page_item_功能_状态
	listview_item_功能_状态
	gridview_item_功能_状态
	tabview_item_功能_状态
	gallery_item_功能_状态
	
###id
	模块_组件_功能_类别_状态
	textview_name_red_disable
	imageview_name_red_default
	
###style
	模块_组件_功能.类别.状态
	TextView_Menu.White
	
## Java
###包名及类
	mobi.cangol.mobile.app.模块.activity
		activity类
	mobi.cangol.mobile.app.模块.fragment
	   fragment类
	mobi.cangol.mobile.app.模块.adapter  
		adapter类   
	mobi.cangol.mobile.app.模块.view 
		自定义view    	
	mobi.cangol.mobile.app.模块.api  
		API交互类	    
	mobi.cangol.mobile.app.模块.model    
		model类 
	mobi.cangol.mobile.app.模块.stat
		统计类  	
	mobi.cangol.mobile.app.模块.utils    
		utils类 
	mobi.cangol.mobile.app.模块.xxx  
		其他功能性类,如视频 

### 命名规范
	1.成员变量：驼峰命名法或m变量命名法
	2.静态变量：（小写）驼峰命名法或m变量命名法
	3.常量：（大写）功能_状态 
	其他遵循java基本规范
	
##SVN、GIT 注释规范
	+):新增资源
	-):减少资源
	*):修改
	!):fix bug no.
	B):分支
	T):标记
	M):合并

  
    