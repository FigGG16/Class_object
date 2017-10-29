##请求授权与AFNetworking框架的使用
>处理流程
![](/assets/Snip20171028_1.png)
封装文件切换window的分类
```objectivec
UIWindow+Extension.m

#import "UIWindow+Extension.h"
#import "FXTaBarController.h"
#import "FXNewFeatureViewController.h"

@implementation UIWindow (Extension)
//切换窗口
+(void)switchRootViewController
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

@end
    
    /////////////////////// ------判断新特性---------end  ///

}
//---------------------UIWindow+Extension.m----end


//切换窗口
//切换窗口
+(void)switchRootViewController;
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

##面向模型开发获取信息转为模型
优点：提高开发效率，解决易错



```objectivec
@property(nonatomic,copy)NSString *access_token;
@property(nonatomic,copy)NSString *expires_in;
@property(nonatomic,copy)NSString *uid;
@property(nonatomic,copy)NSDate *create_time;

+(instancetype)accountWithDict:(NSDictionary*)dict;
//--------------_-------FXAccount.h------end


+(instancetype)accountWithDict:(NSDictionary*)dict
{
    FXAccount *account=[[self alloc] init];
    
    account.access_token=dict[@"access_token"];
    account.expires_in=dict[@"expires_in"];
    account.uid=dict[@"uid"];
    
    
    return account;
}
//--------------_-------FXAccount.m------end

```




##新建处理账号有关的所有操作
 >1.使用归档
 >2.过期处理

```obejectiveC
@class FXAccount;
//处理账号有关的所有操作(存，读取，验证账号是否过期)
@interface FXAccountTool : NSObject
//存放账号
+(void)saveAccount:(FXAccount *)account;
//读取
+(FXAccount *)account;

//------------------------FXAccountTool.h----end


//宏定义归档目录
#define FXPath [[NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject] stringByAppendingPathComponent:@"HFX.archive"]

@implementation FXAccountTool

//存放账号(归档)
+(void)saveAccount:(FXAccount *)account
{
    
    //获取沙盒路径 并把账号存入沙盒
    //        NSString *homePath=NSHomeDirectory();

    //归档 是按照一定的数据格式存放数据
    [NSKeyedArchiver archiveRootObject:account toFile:FXPath];


}
//读取(解档)
+(FXAccount *)account
{
    NSLog(@"----->%@",FXPath);
  FXAccount *account=  [NSKeyedUnarchiver unarchiveObjectWithFile:FXPath];
  return account;
    
}
//------------------FXAccountTool.m----end

```
在FXAccount文件增加解档和归档类型



```

```









