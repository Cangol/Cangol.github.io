---
layout: post
title: tcpdump help
date: '2012-11-12'
category: android
tags: [tcpdump]
---

开始抓包  
`/data/local/tcpdump -p -vv -s 0 -w /sdcard/capture.pcap`

导出抓包文件  
`adb pull /sdcard/capture.pcap d:`