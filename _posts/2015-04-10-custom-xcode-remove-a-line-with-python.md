---
layout: post
title: xcode自定义“删除一行”快捷键的python脚本
category: iOS
---

一个xcode自定义“删除一行”快捷键的python脚本

{% highlight python %}
# -*- coding:utf8 -*-
#
# 修改文件权限
# 添加自定义的ctr+D删除到xcode中
# 参考：
#   http://hi.baidu.com/castiello_ybx/item/20e9945932516ac89e2667ea
#   http://stackoverflow.com/questions/13045593/using-sudo-with-python-script
#

import os, sys, stat
from biplist import *

def changeChmod(sudoPassword, command):
    os.system('echo %s|sudo %s' % (sudoPassword, command))

def myFun():
    # os.chmod("/Applications/Xcode.app/Contents/Frameworks/IDEKit.framework/Resources/", stat.S_IRWXU)
    # os.chmod("/Applications/Xcode.app/Contents/Frameworks/IDEKit.framework/Resources/IDETextKeyBindingSet.plist", stat.S_IRWXU)
    # 相关文件路径
    dir_path = "/Applications/Xcode.app/Contents/Frameworks/IDEKit.framework/Resources/"
    plist_path = dir_path+"IDETextKeyBindingSet.plist"

    # 修改文件权限
    command1 = "sudo chmod 666 "+plist_path
    command2 = "sudo chmod 777 "+dir_path
    # os.system('echo %s|sudo %s' % (sudoPassword, command))
    changeChmod('这里写你的电脑密码', command1)
    changeChmod('这里写你的电脑密码', command2)

    # 添加自定义的ctr+D删除到xcode中
    plist = readPlist(plist_path)
    plist['Deletions']['my delete the line'] = 'moveToEndOfLine:, deleteToBeginningOfLine:, deleteToEndOfParagraph:'
    writePlist(plist, plist_path)

if __name__ == '__main__':
    myFun()

{% endhighlight %}

使用场景：
每次更新xcode后（apple更新xcode的频率真心快）
用法：
创建空文件，粘贴代码并保存为.py文件，在控制台cd到文件目前目录，执行 python 文件名.py ,重启xcode。
