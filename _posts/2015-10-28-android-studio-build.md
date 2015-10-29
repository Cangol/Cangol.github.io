---
layout: post
title: Android多渠道打包:gradle
date: 2015-10-28
category: android
---


##定义Placeholder
在app的AndroidManifest.xml中定义一个Placeholder。  
value中的“\” 用来将value中的数字转换为字符串（类似excel中“’”的功能）

	<meta-data android:name="CHANNEL_ID" android:value="\ ${CHANNEL_ID_VALUE}"/>
代码中的获取方式

	 public static String getChannnel(Context context) {
	        String  channel = null;
	        ApplicationInfo appInfo;
	        try {
	            appInfo = context.getPackageManager().getApplicationInfo(
	                    context.getPackageName(), PackageManager.GET_META_DATA);
	            channel=appInfo.metaData.getString("CHANNEL_ID");
	        } catch (NameNotFoundException e) {
	        }
	        return channel;
	 }
##定义默认渠道
添加在app的build.gradle文件 android块中	 

	defaultConfig {
		 //其他省略
       //默认是的渠道
       manifestPlaceholders = [CHANNEL_ID_VALUE: "test"]
    }
##定义多个渠道
* 方法一

		 productFlavors {
		 	baidu{
		 		 manifestPlaceholders = [CHANNEL_ID_VALUE: “baidu”]
		 	}
		 	google{
		 		 manifestPlaceholders = [CHANNEL_ID_VALUE: “google”]
		 	}
			qq{
				manifestPlaceholders = [CHANNEL_ID_VALUE: “qq”]
			}
		 }
* 方法二

		 productFlavors {
		 	baidu{}
		 	google{}
		 	qq{}
		 }
		 productFlavors.all { flavor ->
		    flavor.manifestPlaceholders = [UMENG_CHANNEL_VALUE: name]
		}
		
* 方法三  
	从channel文件中按行读取渠道   
	
		productFlavors {
		    def path = "channel.txt"
		    file(path).eachLine { channel ->
		        "$channel" {
		            manifestPlaceholders = [CHANNEL_ID_VALUE: channel]
		        }
		    }
		}
	channel.txt文件内容   
	
		baidu
		google
		qq
* 方法四  
	从属性文件gradle.properties中读取渠道
	
		productFlavors {
		    "${app_channels}".split(',').each { channel ->
		        "$channel" {
		           manifestPlaceholders = [CHANNEL_ID_VALUE: channel]
		        }
		    }
		}
		
	gradle.properties文件设置
	
		app_channels=baidu,google,qq  
##证书和签名
添加在app的build.gradle文件 android块中	  
*  方法一  
	直接写入
	
		signingConfigs {
		        release {
		            keyAlias '******'
		            keyPassword '*'
		            storePassword '*'
		            storeFile file('keystore/*.keystore')
		            }
		}
*  方法二  
	从键盘获取  
	
		signingConfigs {
		    release {
	           storeFile new String(System.console().readPassword("\n\$Enter keystore path: "))	           storePassword new String(System.console().readPassword("\n\$Enter keystore password: "))
	           keyAlias System.console().readLine("\n\$Enter key alias: ")
	           keyPassword new String(System.console().readPassword("\n\$Enter key password: "))
		    }
		}
*  方法三  
从属性文件gradle.properties中获

		signingConfigs {
				    release {
			           storeFile ${app_keystore_filepath}
			           storePassword ${app_keystore_password}
			           keyAlias ${app_keystore_alias}
			           keyPassword ${app_keystore_keypassword}
			        }
			}

##格式化输出apk

	 applicationVariants.all { variant ->
	            variant.outputs.each { output ->
	                def outputFile = output.outputFile
	                if (outputFile != null && outputFile.name.endsWith('.apk')) {
	                    def fileName = "${app_name}_v${defaultConfig.versionName}_${releaseVersionSvn())_${releaseTime()}_${variant.productFlavors[0].name}.apk"
	                    output.outputFile = new File(outputFile.parent, fileName)
	                }
	            }
	        }
以下提供一些APK文件命名的模板参数的相关获取方法
  
*   获取当前时间

		def releaseTime() {
		    return new Date().format("yyyyMMdd", TimeZone.getTimeZone("UTC"))
		}
*  获取svn版本号

		def releaseVersionSvn(){
		    Process p="svn info --xml --incremental".execute()
		    def version=p.text
		    p.destroy()
		    def m=version =~ '.*revision=\"(.*?)\"'
		    if(m.find()){
		        return m.group(1);
		    }else{
		        return null
		    }
		}
*  获取git版本号

		def releaseVersionGit(){
		    Process p="git stash".execute()
		    def version=p.text
		    p.destroy()
		    def m=version =~ '.*at (.*?) '
		    if(m.find()){
		        return m.group(1);
		    }else{
		        return null
		    }
		}
*  获取git序号（git的提交次数号）

		def releaseVersionGitNo(){
		    Process p="git rev-list --all|wc -l".execute()
		    def version=p.text
		    p.destroy()
		    return version;
		}


