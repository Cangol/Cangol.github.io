---
layout: post
title: gradle auto upload
date: '2016-07-27'
cateory: android
tags: [gradle]
---

##说明
>	实现gradle assembleRelease打包后将 apk上传到共享或server服务器上。

##上传到共享
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
            def destination = new File (shareUrl+file.getName().replace("autotest","debug"))
            if(destination.exists())
                destination.delete()
            source.withInputStream { is ->
                destination << is
            }
            destination.lastModified = source.lastModified()
            logger.error("uploaded "+destination.getAbsolutePath())
        }
    }
