---
layout: post
title: iOS开发中用到的开源库（整理）
category: iOS
---

本着不重复造轮子的原则，app开发中或多或少用各种的开源库，用对了开源库，往往能**代码风骚、效率恐怖**~

做iOS也有几年了，整理一下开发中常用到的开源库。

1，[AFNetworking](https://github.com/AFNetworking/AFNetworking)&nbsp;在iOS开发中使用非常多网络开源库,适用于iOS以及Mac OS X。架构良好，模块清晰，api丰富，绝对是iOS开发中网络库的不二之先。
<div class="message">
*AFNetworing*是mattt大神的开源库，mattt同时也是[nshipster](http://nshipster.com/)的组织者，上面有很多关于iOS的技术文章。
</div>
简单使用示例：
{% highlight objc %}
AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
[manager GET:@"http://example.com/resources.json" parameters:nil success:^(AFHTTPRequestOperation *operation, id responseObject) {
    NSLog(@"JSON: %@", responseObject);
} failure:^(AFHTTPRequestOperation *operation, NSError *error) {
    NSLog(@"Error: %@", error);
}];
{% endhighlight %}

2，<a href="https://github.com/rs/SDWebImage">SDWebImage</a>&nbsp;ios开发的图片下载缓存库，实现<strong>懒加载</strong>的得力助手。
<div class="message">
不过新版本的AFNetwording集成了这功能，而且api一样，所以现在基本用不单独用这个库了。
</div>
简单使用示例：
{% highlight objc %}
[cell.imageView setImageWithURL:[NSURL URLWithString:@"http://www.domain.com/path/to/image.jpg"]
               placeholderImage:[UIImage imageNamed:@"placeholder.png"]
                      completed:^(UIImage *image, NSError *error, SDImageCacheType cacheType) {
                      ... completion code here ...
                      }];
{% endhighlight %}

3，<a href="https://github.com/jdg/MBProgressHUD">MBProgressHUD </a>&nbsp;集加载等待、进度条、提示功能于一体的指示器库。
<div class="message">
基于它可以非常简单的生成类型Android的toast的提示效果；
{% highlight objc %}
+ (void)showHUDNoteWithText:(NSString *)text
{
    MBProgressHUD *hud = [[MBProgressHUD alloc] initWithWindow:[UIApplication sharedApplication].keyWindow];
    hud.mode = MBProgressHUDModeText;
    hud.removeFromSuperViewOnHide = YES;
    [[UIApplication sharedApplication].keyWindow addSubview:hud];
    hud.labelText = text;
    [hud show:YES];
    [hud hide:YES afterDelay:1.5];
}
{% endhighlight %}
</div>
![gif](../images/1月-22-2015-11-34.gif)

4，[Reachability](https://developer.apple.com/library/ios/samplecode/Reachability/Introduction/Intro.html)&nbsp;`apple`官方给出的在iOS设备上检测网络的库，可以检测网络的连通性，也可以监听网络的变化（由3g到wifi等情况）。
简单使用示例：
{% highlight objc %}
-(int)checkNetWork {
    int isExistenceNetWork;
    Reachability *reachability = [Reachability reachabilityWithHostName:@"www.baidu.com"];
    switch ([reachability currentReachabilityStatus]) {
        case NotReachable://无网络
            isExistenceNetWork = 0;
            break;
        case ReachableVia3G://使用3G或RPRS
            isExistenceNetWork = 1;
            break;
        case ReachableVia2G://使用2G或RPRS
            isExistenceNetWork = 1;
            break;
        case ReachableViaWiFi://使用WiFi
            isExistenceNetWork = 2;
            break;
    }
    return isExistenceNetWork;
}
{% endhighlight %}

5，<a href="https://github.com/gotosleep/JASidePanels">JASidePanels</a>&nbsp;左右侧滑的UIViewController容器;
<div class="message">
这样左右侧滑效果在一两年前，相当流行啊！现在好像差不多审美疲劳了~
</div>
![图片](https://camo.githubusercontent.com/c7764cc1a74c7a9be35fa09f350adb5622e3eb49/68747470733a2f2f696d672e736b697463682e636f6d2f32303132303332322d6478366b36393537377261333777776771676d73676b737170782e6a7067)

6，分享组件&nbsp;&nbsp;&nbsp;国内有好多平台，用过的有：
<ul>
<li>[友盟](http://www.umeng.com/social)</li>
<li>[mob](http://www.mob.com/)</li>
<li>[百度](http://share.baidu.com/)</li>
</ul>

7，<a href="https://github.com/BradLarson/GPUImage">GPUImage</a>&nbsp;&nbsp; GPUImage是Brad Larson在github托管的一个开源项目，项目实现了图片滤镜、摄像头实时滤镜，该项目的优点不但在于滤镜很多，而且处理效果是基于GPU的，比使用CPU性能更高。
<div class="message">
部分滤镜介绍参考[这里](http://www.cnblogs.com/bunsman/archive/2013/06/06/3121406.html)
</div>
![filterImage](../images/屏幕快照2015-01-22下午2.16.00.png)
简单使用示例：
{% highlight objc %}
GPUImageSepiaFilter *stillImageFilter2 = [[GPUImageSepiaFilter alloc] init];
UIImage *quickFilteredImage = [stillImageFilter2 imageByFilteringImage:inputImage];
{% endhighlight %}

8，<a href="https://github.com/feuvan/opencore-amr-iOS">opencore-amr</a>&nbsp;&nbsp;能将声音转化为amr格式，amr格式十多Ｋ大小就可以长达一分钟的内容，便于网络上传使用。
<div class="message">
编译好的带arm64的库见<a href="https://github.com/ondev/AMR-for-iOS7">这里</a>或者<a href="https://github.com/guange2015/ios-amr">这里</a><br />
因为源码是c语言的，故调用时用到中间函数，示例见<a href="https://github.com/guange2015/ios-amr">这里</a>
</div>

9，<a href="https://github.com/jamztang/JTGestureBasedTableViewDemo">JTGestureBasedTableView</a>&nbsp;&nbsp;对UITableViewCell手势的监听操作，个人用到的是它的“长按cell重新排序”的功能；
<img src="https://raw.githubusercontent.com/jamztang/JTGestureBasedTableViewDemo/master/demo4.png" alt="Drawing" style="width: 200px;"/>

10，<a href="https://github.com/piemonte/PBJVision">PBJVision</a>&nbsp;&nbsp;PBJVision是iOS摄像头引擎，支持按住屏幕后录制视频和拍照。
<div class="message">
拍摄界面可自定义，拍摄视频尺寸可自定义，功能很强大，谁用谁知道。
</div>
{% highlight objc %}
-(void)viewDidLoad:(Bool)animated
{
	[super viewDidLoad:animated];

	// preview and AV layer
    _previewView = [[UIView alloc] initWithFrame:CGRectZero];
    _previewView.backgroundColor = [UIColor blackColor];
    CGRect previewFrame = CGRectMake(0, 60.0f, CGRectGetWidth(self.view.frame), CGRectGetWidth(self.view.frame));
    _previewView.frame = previewFrame;
    _previewLayer = [[PBJVision sharedInstance] previewLayer];
    _previewLayer.frame = _previewView.bounds;
    _previewLayer.videoGravity = AVLayerVideoGravityResizeAspectFill;
    [_previewView.layer addSublayer:_previewLayer];
}

- (void)_setup
{
    _longPressGestureRecognizer.enabled = YES;

    PBJVision *vision = [PBJVision sharedInstance];
    vision.delegate = self;
    vision.cameraMode = PBJCameraModeVideo;
    vision.cameraOrientation = PBJCameraOrientationPortrait;
    vision.focusMode = PBJFocusModeContinuousAutoFocus;
    vision.outputFormat = PBJOutputFormatSquare;

    [vision startPreview];
}
{% endhighlight %}

11，<a href="https://github.com/enormego/EGOTableViewPullRefresh">EGOTableViewPullRefresh</a>&nbsp;&nbsp;最有名的iOS列表下拉刷新控件。
<div class="message">
源码可任意自定义，修改出各种样式的下拉刷新样式~
</div>

12，<a href="https://github.com/CEWendel/SWTableViewCell">SWTableViewCell</a>&nbsp;&nbsp;UITableViewCell右滑删除，效果十分赞~
![图片](https://camo.githubusercontent.com/c138fcd3df24ae1d91f8bf6feb51a1cf111606a4/687474703a2f2f692e696d6775722e636f6d2f6e6a4b436a4b382e676966)

13，<a href="http://developer.baidu.com/wiki/index.php?title=docs/cplat/media/voice">百度语音服务</a>&nbsp;&nbsp;这个服务能帮你把语音变成文本。

简单使用示例：
{% highlight objc %}
- (void)sdkUIRecognition
{
    // 创建识别控件
    self.recognizerViewController = [[BDRecognizerViewController alloc] initWithOrigin:CGPointMake(9, 128) withTheme:[BDTheme lightBlueTheme]];
    self.recognizerViewController.delegate = self;
    //    self.recognizerViewController = tmpRecognizerViewController;

    // 设置识别参数
    BDRecognizerViewParamsObject *paramsObject = [[BDRecognizerViewParamsObject alloc] init];

    // 开发者信息，必须修改API_KEY和SECRET_KEY为在百度开发者平台申请得到的值，否则示例不能工作
    paramsObject.apiKey = BAIDU_API_KEY;
    paramsObject.secretKey = BAIDU_SECRET_KEY;

    // 设置是否需要语义理解，只在搜索模式有效
    paramsObject.isNeedNLU = NO;

    // 设置识别语言
    paramsObject.language = BDVoiceRecognitionLanguageChinese;

    // 设置识别模式，分为搜索和输入
    paramsObject.recognitionProperty = EVoiceRecognitionPropertySearch;

    // 开启联系人识别
    //paramsObject.enableContacts = YES;

    // 设置显示效果，是否开启连续上屏
    paramsObject.resultShowMode = BDRecognizerResultShowModeWholeShow;

    // 设置提示音开关，是否打开，默认打开
    paramsObject.recordPlayTones = EBDRecognizerPlayTonesRecordForbidden;

    paramsObject.isShowTipAfter3sSilence = NO;
    paramsObject.isShowHelpButtonWhenSilence = NO;
//    paramsObject.tipsTitle = @"可以使用如下指令记账";
//    paramsObject.tipsList = [NSArray arrayWithObjects:@"我要记账", @"买苹果花了十块钱", @"买牛奶五块钱", @"第四行滚动后可见", @"第五行是最后一行", nil];

    [self.recognizerViewController startWithParams:paramsObject];
}
{% endhighlight %}

14，高德地图sdk，百度地图sdk&nbsp;&nbsp;&nbsp;这两个地图sdk的api基本一样，都包含了`apple`自己的MapKit的所有api.
<div class="message">
这两地图提供了一些额外的api，如地图覆盖物图层MAGroundOverlay等。<br />
用法见官网吧。
</div>

15，<a href="https://github.com/ninjinkun/NJKWebViewProgress">NJKWebViewProgress</a>&nbsp;&nbsp;是一个 UIWebView 的进度条接口库，UIWebView 本身是不提供进度条的。样式与Safari差不多。
<img src="https://camo.githubusercontent.com/082fc708cc461dc53832b7d14d5affdf475dd57b/68747470733a2f2f7261772e6769746875622e636f6d2f6e696e6a696e6b756e2f4e4a4b5765625669657750726f67726573732f6d61737465722f44656d6f4170702f53637265656e73686f742f73637265656e73686f74312e706e67" alt="Drawing" style="width: 200px;"/>
{% highlight objc %}
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.

    self.view.backgroundColor = [UIColor whiteColor];

    _progressProxy = [[NJKWebViewProgress alloc] init];
    _progressProxy.webViewProxyDelegate = self;
    _progressProxy.progressDelegate = self;

    CGFloat progressBarHeight = 2.f;
    CGRect navigaitonBarBounds = self.navigationController.navigationBar.bounds;
    CGRect barFrame = CGRectMake(0, navigaitonBarBounds.size.height - progressBarHeight, navigaitonBarBounds.size.width, progressBarHeight);
    _progressView = [[NJKWebViewProgressView alloc] initWithFrame:barFrame];
    _progressView.autoresizingMask = UIViewAutoresizingFlexibleWidth | UIViewAutoresizingFlexibleTopMargin;

    _webView = [[UIWebView alloc] initWithFrame:CGRectMake(0, 0, self.view.width, self.view.height)];
    _webView.delegate = _progressProxy;
    [self.view addSubview:_webView];

    [self loadUrl];

    // other inits...
}

- (void)viewWillAppear:(BOOL)animated
{
    [super viewWillAppear:animated];

    // self.navigationController.navigationBarHidden = NO;
    [self.navigationController.navigationBar addSubview:_progressView];
}
{% endhighlight %}

16，<a href="https://github.com/bmorton/ZBarSDK">ZBarSDK</a>&nbsp;&nbsp;条形码和二维码扫描库。
<div class="message">
支持arm64的库到<a href="https://github.com/null09264/ZBarSDK-for-iOS">这里</a>下载<br />
使用方法与调用相机差不多，delegate代码比较多，示例自行google吧。
</div>

17，<a href="http://developer.baidu.com/cloud/push">百度推送</a>&nbsp;&nbsp;可管理，更简便的推送功能。
<div class="message">
使用的的实质其实只是用百度推送服务器做了一次中转
<ul>
<li>未使用：我们的服务器->apns->iOS设备</li>
<li>使用后：我们的服务器->百度云推送服务器->apns->iOS设备</li>
</ul>
使用的好处是，它提供用户分组，可以单播，分组播等，也提供了统计功能。
</div>

18，微信支付，银联支付
<div class="message">
这两个api接口要想成功调用，都必须申请各自平台支付商户号。
其中银联的sdk也得申请完了，人家才会给。

客户端代码比较简单了，所有订单，流水号全在服务上生成完了，参数扔给客户端，
客户端调起各平台的支付界面，就没你事了，成功与否在delegate中找。

支付宝虽然没实际用过，但想来流程也应该差不多~
</div>
部分示例代码：
{% highlight objc %}
-(void)wxPay
{
    TJHttpCmdWXPayData *cmd = [TJHttpCmdWXPayData cmd];
    cmd.payType = @"wx";
    cmd.sign = self.data.sign;
    cmd.tokenId = [TJUserManager sharedInstance].tokenId;
    [cmd setSuccess:^(id object){
        NSLog(@"%@", object);

        NSDictionary *dict = [object objectForKey:@"payData"];

        self.payTradeNo = [dict objectForKey:@"tradeNo"];

        // 调起微信支付
        PayReq *request   = [[PayReq alloc] init];
        request.partnerId = [NSString stringWithFormat:@"%@", dict[@"partnerid"]];
        request.prepayId  = [NSString stringWithFormat:@"%@", dict[@"prepayid"]];
        request.package   = [NSString stringWithFormat:@"%@", dict[@"packages"]];      // 文档为 `Request.package = _package;` , 但如果填写上面生成的 `package` 将不能支付成功
        request.nonceStr  = [NSString stringWithFormat:@"%@", dict[@"noncestr"]];
        request.timeStamp = [[NSString stringWithFormat:@"%@", dict[@"timestamp"]] integerValue];

        request.sign = [NSString stringWithFormat:@"%@", dict[@"sign"]];

        // 在支付之前，如果应用没有注册到微信，应该先调用 [WXApi registerApp:appId] 将应用注册到微信
        if ([WXApi safeSendReq:request]) {
            NSLog(@"success~~");
        }
        else
        {
            NSLog(@"fail~~");
        }

    }];
    [cmd setFail:^(AFHTTPRequestOperation *response){
        LOG(@"失败%@", response);

    }];
    [cmd startRequest];
}

-(void)upPay
{
    TJHttpCmdWXPayData *cmd = [TJHttpCmdWXPayData cmd];
    cmd.payType = @"up";
    cmd.sign = self.data.sign;
    cmd.tokenId = [TJUserManager sharedInstance].tokenId;
    [cmd setSuccess:^(id object){
        NSLog(@"%@", object);

        NSDictionary *dict = [object objectForKey:@"payData"];
        self.payTradeNo = [dict objectForKey:@"tradeNo"];

        NSString *tn = [dict objectForKey:@"tn"];
        NSString *mode = [dict objectForKey:@"model"];

        // 从服务器请求回来tn
        if ([UPPayPlugin startPay:tn mode:mode viewController:self delegate:self]) {
            NSLog(@"success~~");
        }
        else
        {
            NSLog(@"fail~~");
        }
    }];
    [cmd setFail:^(AFHTTPRequestOperation *response){
        LOG(@"失败%@", response);

    }];
    [cmd startRequest];
}
{% endhighlight %}

还有一些用过的，但不常用，就不介绍了。先到这里，以后有再添加。
