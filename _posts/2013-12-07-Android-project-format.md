---
layout: post
title: Android project format
date: '2013-12-07'
---

再次规范Android App项目使用的相关Java代码、xml文件、其他资源文件、样式、id、vcs工具的规范，已统一风格，提高可读性.






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

  
    