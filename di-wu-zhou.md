##开始微博实现跳转标签控制器


```objectivec
-(void)setUpLastImage:(UIImageView*)imag
{
...
 //跳转标签控制器
   [startBtn addTarget:self action:@selector(startBtnClick) forControlEvents:UIControlEventTouchUpInside];
...
}

-(void)startBtnClick
{
   UIWindow *window=[UIApplication sharedApplication].keyWindow;
   
   window.rootViewController=[[FXTaBarController alloc] init];


}


```

##解决新特性界面重复显示
工作流程

![](/assets/Snip20171016_1.png)



在AppDelegate文件中判断
```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
...
    //获取当前程序的版本号
    NSString *lastBundleVersion=[[NSUserDefaults standardUserDefaults] objectForKey:@"CFBundleVersion"];
    
    //获取info.plist文件
    NSDictionary *info=[NSBundle mainBundle].infoDictionary;
    
    //获取键值
    NSString *currentBundleVersion=info[@"CFBundleVersion"];
    
    //打印沙盒路径
    NSString *path=NSHomeDirectory();
    
    //判断版本号
    if([lastBundleVersion isEqualToString:currentBundleVersion])
    {
        //实例化taber控制器
        FXTaBarController *taBer=[[FXTaBarController alloc] init];
        self.window.rootViewController=taBer;
    }else
    {
        //将版本不同的存入沙盒
        [[NSUserDefaults standardUserDefaults] setObject:currentBundleVersion forKey:@"CFBundleVersion"];
        
        //写入沙盒
        [[NSUserDefaults standardUserDefaults] synchronize];
        
        //创建TabBarController，它是程序底部的标签页,是一个容器
        self.window.rootViewController=[[FXNewFeatureViewController alloc] init];
    }
    
}
...
```
##程序请求流程
![](/assets/Snip20171016_3.png)


什么是Oauth授权？
比如QQ，当我们登录CSDN时选择QQ登录，那么出现的QQ界面是腾讯提供的，属于第三方，登录之后就得到了腾讯的授权，就可以拿到腾讯的数据

开发步骤

![](/assets/Snip20171016_4.png)

##微博web请求注意点
![](/assets/Snip20171023_1.png)

请求页面代码


```
- (void)viewDidLoad {
    [super viewDidLoad];
    //创建一个UIWebView对象
    UIWebView *webView=[[UIWebView alloc]init];
    //frame
    webView.frame=self.view.bounds;
    //把webView作为控制器view的子控件
    [self.view addSubview:webView];
    //urlStr
    NSString *urlStr=@"https://api.weibo.com/oauth2/authorize?client_id=116051389&redirect_uri=http://";
    //authorize?授权地址
    //client不能为空
    //redirect_uri 回调地址
    
    //创建url
    NSURL *url=[NSURL URLWithString:urlStr];
    //创建请求对象
    NSURLRequest *request=[NSURLRequest requestWithURL:url];
    //发送请求
    [webView loadRequest:request];
    
    webView.delegate=self;
}

```
##使用代理的方式截取指令 获取access Token


```
-(BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType
{
    NSString *url = request.URL.absoluteString;
    
    //  截取code令牌
    NSRange range=[url rangeOfString:@"code="];
    
    if(range.length!=0)
    {
       //已授权
        NSInteger fromIndex = range.location+range.length;
        
        NSString *code = [url substringFromIndex:fromIndex];
        
        NSLog(@"XXX%@",code);
    }


    return YES;
}

```







