---
layout: post
title: gradle auto upload
date: '2016-07-27'
cateory: android
tags: [gradle]
---

## 说明
使用android studio 打包或产生渠道包,现在已经很普及,但是打包后还要自行拷贝apk，到发布目录.对于这点繁杂的无脑工作，我们要实现自动化上传发布,解放程序员,解放程序员。接下来看看我们怎么实现。

首先我们要执行gradle assembleRelease打包，然后再后将apk上传到共享或httpserver服务器上。
那如何一气呵成 打包到上传自动话呢，我们可以这样

### doLast方式
    android {
        //.....
         assembleRelease.doLast{
            //apk文件名称
            def fileName = "app-release-signed-apk"
            File outFile = new File("${project.buildDir}/outputs/apk",
            fileName)
            def folder="V${app_version_name}/"
            uploadToSmb(outFile,folder)
            //uploadToHttpServer(outFile,url)
        }
    }

### 上传到共享
    def uploadToSmb(File file,String folder){
        Process p="git version".execute()
        def os_version=p.text
        p.destroy()
        def shareUrl
        if(os_version.contains("windows")){
            shareUrl="//192.168.59.1/APP/"+folder
        }else{
            shareUrl="smb://192.168.59.1/APP/"+folder
        }
        if(file != null){
            def source = file
            if(!new File (shareUrl).exists())
                new File (shareUrl).mkdirs();
            def destination = new File (shareUrl+file.getName())
            if(destination.exists())
                destination.delete()
            source.withInputStream { is ->
                destination << is
            }
            destination.lastModified = source.lastModified()
            logger.error("uploaded "+destination.getAbsolutePath())
        }
    }
### 上传到http server
    def uploadToHttpServer(File file, String url){
        if(url != null && !url.isEmpty()){
            def client = new DefaultHttpClient()
            def post = new HttpPost("${url}");
            def entity = new MultipartEntity()
            def fileBody = new FileBody(file,
            "application/vnd.android.package-archive", file.name)
            entity.addPart("file", fileBody)
            post.entity = entity
    
            post.setEntity(entity);
            def response = client.execute(post);
            if(response.getStatusLine().getStatusCode() == 201)
                println "${file.name} uploaded"
        }
    }
