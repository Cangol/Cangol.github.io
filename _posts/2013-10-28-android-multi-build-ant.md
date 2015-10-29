---
layout: post
title: Android多渠道打包:ant
date: 2013-10-28
category: android
---

ant打包适用于eclipse等情况

##修改AndroidManifest
app的AndroidManifest.xml中定义一个meta-data “CHANNEL_ID”
value中的“\” 用来将value中的数字转换为字符串（类似excel中“’”的功能）

	<meta-data android:name="CHANNEL_ID" android:value="\ CHANNEL_ID_VALUE"/>

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

    }
##定义多个渠道

项目根目录下新建channel.txt，内容如下，多个渠道用逗号隔开

	testa,weba,baidu
##生成build文件
在项目根目录下执行，会生成了这些文件（build.xml,ant.properties,custom_rules.xml）

	ant android project update
	
修改build.xml,加入如下代码

	 <import file="custom_rules.xml" optional="true" />

	<property file="channel.txt"/>
		<tstamp>
				<format property="TODAY" pattern="yyyyMMdd"/>
		</tstamp>
		<property name="ant.project.name" value="${app.name}-${app.version}-${app.svnv}-${TODAY}-${channel_id}">
	</property>

在ant.properties中配置相关信息

	key.store=*.keystore
	key.alias=******
	key.store.password=******
	key.alias.password=******
	app.name=APP
	app.version=1.0.0
	
	output.dir=D:\build
	ant-contrib.dir=D:/dev/ant-contrib-1.0b2.jar

修改或新建custom_rules.xml，内容如下

	<?xml version="1.0" encoding="UTF-8"?>
	<project name="custom_rules">
	     <tstamp>
				<format property="TODAY" pattern="yyyyMMdd"/>
		 </tstamp>
		 <target name="-pre-build">
	        <copy file="AndroidManifest.tmpl" tofile="AndroidManifest.xml" overwrite="true"/>
	        <replace file="AndroidManifest.xml" token="SDK_CHANNEL_ID" value="${channel_id}"/>
	        <echo level="info">replace SDK_PARTNER ${channel_id}</echo>
	        <replace file="AndroidManifest.xml" token="SDK_BUILD_DATE" value="${TODAY}"/>
	        <echo level="info">replace SDK_BUILD_DATE ${TODAY}</echo>
	        <replace file="AndroidManifest.xml" token="SDK_BUILD_SVNV" value="${app.svnv}"/>
	        <echo level="info">replace SDK_BUILD_SVNV ${app.svnv}</echo>
	    </target>
	</project>

新建build_all.xml,内容如下

	<?xml version="1.0" encoding="UTF-8"?>
	<project default="help">
	    <!-- ************************* Help ************************ -->
	    <target name="help">
	        <echo>Android Muilit Ant Build. Available targets:</echo>
	        <echo>   help:			Displays this help.</echo>
	        <echo>   build_all:     Muilit build all ids app</echo>
	        <echo>   build_copyto:  copy apk to build.</echo>
	        <echo>   build_clean:   delete apk bin</echo>
	    </target>
	    <!-- ************************* build_copyto ************************ -->
	    <target name="build_copyto" depends="build_all">
	        <property file="ant.properties"/>
	        <delete dir="${output.dir}" includeEmptyDirs="true" includes="${app.name}-${app.version}-*"/>
	        <copy todir="D:\build" verbose="true">
	            <fileset dir="bin" >
	                <include name="${app.name}-${app.version}-*-release.apk" />
	            </fileset>
	             <globmapper from="*-release.apk" to="*.apk"/>  
	        </copy>
	   </target>
	    <!-- ************************* build_clean ************************ -->
	    <target name="build_clean" depends="build_copyto,build_all">
	        <property file="ant.properties"/>
	        <delete verbose="true" includeEmptyDirs="true" > 
	             <fileset dir="bin" >
	                <include name="${app.name}-${app.version}-*-unaligned.apk" />
	                <include name="${app.name}-${app.version}-*-unsigned.apk" />
	                <include name="${app.name}-${app.version}-*-unsigned.apk.d" />
	                <include name="${app.name}-${app.version}-*.ap_" />
	                <include name="${app.name}-${app.version}-*.ap_.d" />
	                <include name="${app.name}-${app.version}-*-release.apk" />
	            </fileset>
	        </delete>
	   </target>
	    <!-- ************************* build_all ************************ -->
	   <target name="build_all">
		   <property name="ant-contrib.jar" location="${ant-contrib.dir}" />
		   <taskdef resource="net/sf/antcontrib/antcontrib.properties" classpath="${ant-contrib.jar}" />
		   <loadfile property="ids" srcFile="channel.txt">
			        <filterchain> 
			            <tokenfilter delimoutput=","/>
			        </filterchain>
	   		 </loadfile>
		    <foreach target="build_one" list="${ids}" delimiter="," param="id">
		    </foreach>
	    </target>
	   <target name="build_one">
	        <echo message="start build channel_id=${id}"/>
	        <!---->
	        <ant antfile="build.xml" target="release">
	            <property name="channel_id" value="${id}"/>
	        </ant>
	   </target>
	</project>

ant需要依赖的包ant-contrib-1.0b2.jar，请自行下载,并配置到classpath
[ant-contrib](http://sourceforge.net/projects/ant-contrib/)
##执行打包
最后ant运行build_all.xml文件,即可进行多渠道打包
