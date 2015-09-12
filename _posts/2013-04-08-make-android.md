---
layout: post
title: make android apk
date: 2013-04-08
category: android
---

生成R.java   
`aapt p -f -m -J gen -S res -I D:\dev\android-sdk\platforms\android-17\android.jar -M AndroidManifest.xml`

.aidl转成.java文件

编译class 包括R.java   
`javac -classpath D:\dev\android-sdk\platforms\android-17\android.jar;libs\zxing-core.jar -d bin\classes src\mobi\cangol\qrcode\*.java gen\mobi\cangol\qrcode\R.java`

class文件转dex    
`dx --dex --output=D:\workspace\QRcode\bin\classes.dex D:\workspace\QRcode\bin;D:\workspace\QRcode\libs\zxing-core.jar`

生成apk    
`aapt p -f -F bin\QRcode.apk -v -u -z -M AndroidManifest.xml -S res -A assets -I D:\dev\android-sdk\platforms\android-17\android.jar`

添加classes.dex 到apk (cd bin)   
`aapt add QRcode.apk classes.dex`

apk签名  
`jarsigner -verbose -keystore Test.keystore -storepass 111111 -keypass 1111111 -signedjar bin\QRcode_signed.apk bin\QRcode.apk test`

