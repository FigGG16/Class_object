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

