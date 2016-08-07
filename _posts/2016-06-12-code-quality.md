---
layout: post
title: code-quality
date: '2016-06-12'
cateory: gradle
tags: [gradle]
---

#如何编写高质量代码（Code Quality） 

	如何编写高质量高规格的代码，一直是我们大多数程序员的追求之一。   
	编写高质量代码：以减少代码的警告和错误，提高代码的易用性和安全性。
	可提高代码的可读性，提高代码与技术在团队中的传承；
	
	那如何编写高质量代码呢？我们除了自我规范，还可以借助工具Code Quality的三剑客：    
	checkstyle,findbugs,pmd等，还其他一些工具，这里只介绍这三种。
	
##checkstyle
检查代码是否符合规范，有很多个大公司的规范模板，可以自行搜索

##findbugs
一个静态分析工具，它检查类或者 JAR 文件，将字节码与一组缺陷模式进行对比以发现可能的问题。     
即依据bug patterns检测代码中存在的bug

##pmd
* 潜在的bug：空的try/catch/finally/switch语句
* 未使用的代码：未使用的局部变量、参数、私有方法等
* 可选的代码：String/StringBuffer的滥用
* 复杂的表达式：不必须的if语句、可以使用while循环完成的for循环
* 重复的代码：拷贝/粘贴代码意味着拷贝/粘贴bugs
* 循环体创建新对象：尽量不要再for或while循环体内实例化一个新对象
* 资源关闭：Connect，Result，Statement等使用之后确保关闭掉

##集成使用
gralde 中已集成了checkstyle，findbugs，pmd。只需要很简单的配置就可以在项目中使用

引入相关插件

	apply plugin: 'checkstyle'
	apply plugin: 'findbugs'
	apply plugin: 'pmd'
	
配置checkstyle

	task checkstyle(type: Checkstyle) {
	    configFile file("config/quality/checkstyle/checkstyle.xml")
	    configProperties.checkstyleSuppressionsPath = file("config/quality/checkstyle/suppressions.xml").absolutePath
	    source 'src'
	    include '**/*.java'
	    exclude '**/gen/**'
	    classpath = files()
	}
配置checkstyle
	
	task findbugs(type: FindBugs, dependsOn: assembleDebug) {
	
	    ignoreFailures = false
	    effort = "max"
	    reportLevel = "high"
	    excludeFilter = new File("config/quality/findbugs/findbugs-filter.xml")
	    classes = files("build/intermediates/classes")
	
	    source 'src'
	    include '**/*.java'
	    exclude '**/gen/**'
	
	    reports {
	        xml.enabled = false
	        html.enabled = true
	        xml {
	            destination "build/reports/findbugs/findbugs.xml"
	        }
	        html {
	            destination "build/reports/findbugs/findbugs.html"
	        }
	    }
	
	    classpath = files()
	}

配置pmd

	task pmd(type: Pmd) {
	    ignoreFailures = false
	    ruleSetFiles = files("config/quality/pmd/pmd-ruleset.xml")
	    ruleSets = []
	
	    source 'src/main'
	    include '**/*.java'
	    exclude '**/gen/**'
	
	    reports {
	        xml.enabled = false
	        html.enabled = true
	        xml {
	            destination "build/reports/pmd/pmd.xml"
	        }
	        html {
	            destination "build/reports/pmd/pmd.html"
	        }
	    }
	}

当让这里有已近配置好的，可直接拿去用。   
https://github.com/vincentbrison/vb-android-app-quality