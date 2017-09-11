##第一节课
 - 介绍了程序开发流程
![](/assets/Snip20170911_3.png)# First Chapter
其中第三点开发周期较长
 - 程序启动界面设置和程序图标<BR>
 1.程序启动<br>
 ![](/assets/Snip20170911_4.png)
 
 2.图标<br>
 ![](/assets/Snip20170911_5.png)
 
 
##第二节课
 - 微博框架的基本模板
  - 例：点击底部的不同位置就会出现不同的颜色
  
```objectivec
//    步骤：
//    1.初始化控制器
//    2.给窗口设置根控制器
//    3.显示窗口
    
//    初始化控制器
    self.window=[[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    
    //创建三个ViewControl
    UIViewController *viewC1=[[UIViewController alloc] init];
    viewC1.view.backgroundColor=[UIColor redColor];
    
    UIViewController *viewC2=[[UIViewController alloc] init];
    viewC2.view.backgroundColor=[UIColor blueColor];
    
    UIViewController *viewC3=[[UIViewController alloc] init];
    viewC3.view.backgroundColor=[UIColor yellowColor];
    
    
    //创建TabBarController，它是程序底部的标签页,是一个容器
    UITabBarController *taberC=[[UITabBarController alloc] init];
    
    //添加ViewController到 taberC 容器
    [taberC addChildViewController:viewC1];
    [taberC addChildViewController:viewC2];
    [taberC addChildViewController:viewC3];
    
    //再把 taberC 容器 赋值给 window的根控制器
    self.window.rootViewController=taberC;
    //显示
    [self.window makeKeyAndVisible];

```
##第三节课
 - 把TabBarController,UIViewController换成文件形式初始化
 
 ![](/assets/Snip20170911_8.png)
 
 - 实现标签如图所示
 
 ![](/assets/Snip20170911_7.png)
 
 - 创建addChildWithTitle: image方法，并实例化相应的类，实现抽取重复的代码



```objectivec
     //实例化四个标签
    FXHomeViewController *homeviewC=(FXHomeViewController *)[self addChildWithTitle:@"首页" image:@"tabbar_home"];
    FXMessageViewController *messageViewC=(FXMessageViewController*)[self addChildWithTitle:@"消息" image:@"tabbar_message_center_selected"];
    FXProfileViewController *profileviewC=(FXProfileViewController *)[self addChildWithTitle:@"我" image:@"tabbar_profile_selected"];
    FXDiscoverViewController *discoverViewC=(FXDiscoverViewController *)[self addChildWithTitle:@"发现" image:@"tabbar_discover_selected"];
    
    
/**
 <#Description#>
 @param title 标题
 @param image 图片名称
 @return return childVC (UIViewController)
 */
-(UIViewController *)addChildWithTitle:(NSString *)title image:(NSString*)image
{
    //实例化
    UIViewController *childVC=[[UIViewController alloc] init];
    //指定背景颜色
    childVC.view.backgroundColor=[UIColor blueColor];
    //设置Item标题
    childVC.tabBarItem.title=title;
    //设置Item图片
    childVC.tabBarItem.image=[UIImage imageNamed:image];
    return childVC;
}
```
 
