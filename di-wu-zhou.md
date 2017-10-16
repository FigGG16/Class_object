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

