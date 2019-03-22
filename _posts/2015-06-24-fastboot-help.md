---
layout: post
title: fastboot help
date: '2015-04-24'
cateory: rom
tags: [android,moto]
---

刷写gpt分区   
`fastboot flash partition gpt.bin`

刷写system分区（这个就是Android系统了）：  
由于Moto为解决分区过大刷机容易导致出错，所以采用了分段式的方法。先看看底包中有几个分段，刷机时，方法还是一致的，只不过要从分段0开始，按次序刷到最后一个分段。
`fastboot flash system system.img_sparsechunk.0`

刷写recovery分区（大家常用的卡刷模式所在分区）:
`fastboot flash recovery recovery.img`

刷写boot分区（内核）   
`fastboot flash boot boot.img`

刷写modem分区（基带）   
`fastboot flash modem NON-HLOS.bin`

刷写fsg分区（射频表）    
`fastboot flash fsg fsg.mbn`

刷写sdi分区（和基带有关的）   
`fastboot flash sdi sdi.mbn`

刷写motoboot镜像：（这个是bootloader的组合镜像包，简称BL，最好不要乱刷！这个只能升级不能降级这个必须与gpt版本一致才能刷进去，。并且刷这个容易变砖！）
`fastboot flash motoboot motoboot.`

刷写data分区：（用于清空data分区等）  
`fastboot flash userdata userdata.img`

刷写logo分区（开机第一屏）  
`fastboot flash logo logo.bin`

fastboot erase <要清空的分区名>  
清空data分区：（此命令会清除data、sdcard两个分区，如果内置存储有重要的东西，不要用此命令，请在第三方recovery中进行WIPE操作）   
`fastboot erase userdata`

清空cache分区  
`fastboot erase cache`

清空customize分区  
`fastboot erase customize`

清空modemst1 ：（基带缓存分区，两个分区互补加密，破解3G其实就是改的这两个分区）    
`fastboot erase modemst1`

清空 modemst2   
`fastboot erase modemst2`

清空clogo ：（自定义开机第一屏，官方推送开机动画和定制开机标语的，改的就是这个分区）  
`fastboot erase clogo`

清空data、cache、sdcard 三个分区  
`fastboot -w`

清空DDR分区  
`fastboot erase DDR`

设置开机启动项为fastboot  
`fastboot oem fb_mode_set`

清除开机启动项为fastboot   
`fastboot oem fb_mode_clear`

获取手机Bootloader代码  
`fastboot oem get_unlock_data`

解锁bootloader  
`fastboot oem unlock AS3VDFM45GHLKPUIKN34`

获取手机的全部信息    
`fastboot getvar all`

引导启动外部镜像  
`fastboot boot xxxxxx.img`
