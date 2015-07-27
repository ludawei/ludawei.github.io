---
layout: post
title: iOS开发-这几天遇到的问题
category: iOS
---

这几天遇到的几个比较常见的问题及解决方法（记录一下）

####1，ios <Error>: ImageIO: CGImageReadCreateDataWithMappedFile  'open' failed
程序没有错误，逻辑也没有问题；

最后把
UIImage *image = [UIImage imageWithContentsOfFile:imagePath];

改成

NSData *data = [NSData dataWithContentsOfFile:imagePath];

 UIImage *image = [UIImage imageWithData:data];

问题解决；

####2，真机调试时，The entitlements specified in your application’s Code Signing Entitlements file do not match those specified in your provisioning profile.

解决方法：

>1）检查Target->Capabilities，修改掉其中的错误；

>2）若没有找到错误，则重启xcode后，重复上面1）操作；

>3）若还没有错误，则删除手机上应用，再试；

####3，ios推送中加小图片->[apple push emoji](http://stackoverflow.com/questions/16649050/emojis-support-in-apple-push-notification), 参考http://code.iamcal.com/php/emoji/，http://www.emoji-cheat-sheet.com/
![](/assets/images/屏幕快照 2015-03-16 下午3.55.34.png)

####4，[iOS 定位坐标不准确的相关整理及解决方案汇总](http://blog.csdn.net/demo_qiao/article/details/43667317) CLLocation+Sino

####5，2015年3月起，要求关闭所有App内的检查更新功能，苹果App Store将向用户自动提示更新，新提交审核版本如果保留检查更新入口审核时将被拒绝

####6，MKMapView加载时，控制台提示 “mapview More than xxx names on road . Data is probably bad. Please open a radar”，还未找到解决方法。
