##请求授权与AFNetworking框架的使用

![](/assets/Snip20171028_1.png)
封装一个继承NSObject的文件切换window的工具类//也可以做成UIWindow扩展
```objetivec
UIWindowTool.m

#import "FXTaBarController.h"
#import "FXNewFeatureViewController.h"

@implementation UIWindowTool

//切换窗口
+(void)switchWindow
{
    UIWindow *window=[UIApplication sharedApplication].keyWindow;    /////////////////////// ------判断新特性---------  //////////////////////
    //获取当前程序的版本号
    NSString *lastBundleVersion=[[NSUserDefaults standardUserDefaults] objectForKey:@"CFBundleVersion"];
    
    //获取info.plist文件
    NSDictionary *info=[NSBundle mainBundle].infoDictionary;
    
    //获取键值
    NSString *currentBundleVersion=info[@"CFBundleVersion"];
    
    if([lastBundleVersion isEqualToString:currentBundleVersion])
    {
        //实例化taber控制器
        FXTaBarController *taBer=[[FXTaBarController alloc] init];
        window.rootViewController=taBer;
    }else
    {
        //将版本不同的存入沙盒
        [[NSUserDefaults standardUserDefaults] setObject:currentBundleVersion forKey:@"CFBundleVersion"];
        
        //写入沙盒
        [[NSUserDefaults standardUserDefaults] synchronize];
        
        //创建TabBarController，它是程序底部的标签页,是一个容器
        window.rootViewController=[[FXNewFeatureViewController alloc] init];
    }
    
    /////////////////////// ------判断新特性---------end  ///

}
//---------------------UIWindowTool.m----end


//切换窗口
 +(void)switchWindow;
//---------------------UIWindowTool.h----end
```
在AppDelegate中




```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
...
    //判断沙盒是否存在账号
    NSString *doc = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject];
    //拼接文件名
    NSString *docPath = [doc stringByAppendingPathComponent:@"HFX.plist"];
    //读取账号
    NSDictionary *account=[NSDictionary dictionaryWithContentsOfFile:docPath];

    //提前指定根控制器，否则会奔溃
    [self.window makeKeyAndVisible];
    
    //如果账号存在
    if(account)
    {
        [UIWindowTool switchWindow];
        
    }else
    {
        
        self.window.rootViewController=[[FXOauthViewController alloc] init];
        
    }
...
```






