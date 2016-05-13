---
layout: post
title: 响应式编程ReactiveCocoa学习
category: iOS
---

ReactiveCocoa是一个将函数响应式编程范例带入Objective-C的开源库。由Josh Abernathy和Justin Spahr-Summers在对GitHub for Mac的开发过程中建立。上周，[ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa)发布了其1.0 release，达到了第一个重要里程碑。

函数响应式编程(Functional Reactive Programming a.k.a FRP)是思考软件将输入转化为输出在时间上的持续过程的一种方式。

以上引自：http://nshipster.cn/reactivecocoa/

RAC代码：
```objc
// textField 每次内容变化，都会log；
[[textField rac_textSignal] subscribeNext:^(NSString *newName) {
    NSLog(@"%@", newName);
}];

// 等价的delegate
-(BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string
{
    NSLog(@"%@", textField.text);
    return YES;
}
```

它也可以这样用：
```objc
UIButton *button = [[UIButton alloc] initWithFrame:CGRectMake(20, 140, 100, 35)];
[button setTitle:@"click" forState:UIControlStateNormal];
[self.view addSubview:button];

[[button rac_signalForControlEvents:UIControlEventTouchUpInside] subscribeNext:^(id x) {

    NSLog(@"%@", x);

    UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"" message:@"Alert" delegate:nil cancelButtonTitle:@"YES" otherButtonTitles:@"NO", nil];
    [[alertView rac_buttonClickedSignal] subscribeNext:^(NSNumber *indexNumber) {
        if ([indexNumber intValue] == 1) {
            NSLog(@"you touched NO");
        } else {
            NSLog(@"you touched YES");
        }
    }];
    [alertView show];
}];
```

目前来看，RAC是以KVO为基础的，不同于delegate的消息框架，可以提高写代码的连贯性。

参考如下：  
1，http://nshipster.cn/reactivecocoa/  
2，http://limboy.me/ios/2013/12/27/reactivecocoa-2.html
