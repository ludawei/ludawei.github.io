
---
layout: post
title: iOS 7新增三种后台测试
<!-- excerpt: ios打包测试工具 - [浦公英](http://www.pgyer.com/) -->
category: iOS
---

原文：https://app.yinxiang.com/l/ADSjzqzqk85Hh5xU33rnIAF-caef_ME7OEo

想实现这样一个功能：
某个视频类app中，一个上班族用户正在追某个感兴趣的视频，TA想每天下班回家后，TA追的那些个视频，在他上班期间已经下载完成了，TA回到家后不用下载了，直接可以看了。

实现想法：
1，当某个视频节目刚更新完后，会远程通知给客户端，客户端收到通知后，根据内容，后台下载这个视频节目；（这是基于远程推送的想法）
2，每天下班前的某个时刻点，进行一次远程获取数据，客户端根据数据，开一个下载队列，依次下载这些视频节目；（这是基于远程获取的想法）

1）知识准备：
1，后台获取（fetch）；
2，后台推送（notifications）；
3，后台下载（BackgroundTransfer）；

2）测试说明：
1，一条远程通知实际上只是一条普通的带有 content-available 标志的推送通知。
(注：普通的推送，如果应用在后台，程序是没有回调的，但是远程通知有回调)；

2，一个新的远程通知测试方法get：http://nomad-cli.com/#houston

3，远程下载apple demo:https://developer.apple.

com/library/ios/samplecode/SimpleBackgroundTransfer/Listings/SimpleBackgroundTransfer_APLViewController_m.html
(注：测试远程下载时，断点测试法和NSLog测试法不可用，可用本地通知法配合文件查询法来进行测试)

4， 远程下载时，这个sessionConfiguration的identifier很重要， com.company.XXX  这里，这个com.company.这个一定要和 bundle identifier 里面的一致，否则ApplicationDelegate 不会调用handleEventsForBackgroundURLSession代理方法


测试demo：https://github.com/ludawei，m/TestMultaMask.git

3）测试结果：
远程推送的想法demo中已经实现；
远程获取的想法没试，主要问题是下载队列的设计，个人感觉借助afnetworking库，应该问题不大；