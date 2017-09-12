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
##第四节课(优化)
 - 类型强制转换耗费内存，效率低，
 用传递控制器的方式配置其他参数
 
```objectivec
/**
 <#Description#>

 @param VC UIViewController
 @param title 标题
 @param image 图片名称
 */
-(void)addChildWithVC:(UIViewController *)VC Title:(NSString *)title image:(NSString*)image
{
    //指定背景颜色
    VC.view.backgroundColor=[UIColor blueColor];
    //设置Item标题
    VC.tabBarItem.title=title;
    //设置Item图片
    VC.tabBarItem.image=[UIImage imageNamed:image];
}
```

- 设置选中图片和颜色
当我们想改变标签的图片颜色和文本颜色

```objectivec
  //渲染选中图片
 VC.tabBarItem.selectedImage=[[UIImage imageNamed:seletedImag] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
 
/**文本颜色的设置*/
    //创建一个字典
    NSMutableDictionary *titleAttri = [NSMutableDictionary dictionary];
    //KVO修改颜色
    titleAttri[NSForegroundColorAttributeName] = [UIColor orangeColor];
    //设置文本颜色
    [VC.tabBarItem setTitleTextAttributes:titleAttri forState:UIControlStateSelected];
```
##第五节课(优化并添加导航控制器)
  - 分析框架结构：
  
 ![](/assets/Snip20170912_1.png)


 - 优化
 首先在AppDelegate.h文件添加属性
```
@property(strong,nonatomic) FXTaBarController *taBer;
```
实例化，并更新addChildWithVC:方法

```objectivec
-(void)addChildWithVC:(UIViewController *)VC Title:(NSString *)title image:(NSString*)image seletedImage:(NSString *)seletedImag
{
    UINavigationController *nav=[[UINavigationController alloc]initWithRootViewController:VC];
    //指定背景颜色
    VC.view.backgroundColor=[UIColor yellowColor];
    
    //设置navigationItem标题
    VC.navigationItem.title=title;
    //设置Item标题
    VC.tabBarItem.title=title;
    //设置Item图片
    VC.tabBarItem.image=[UIImage imageNamed:image];
    //创建一个字典
    NSMutableDictionary *titleAttri = [NSMutableDictionary dictionary];
    //KVO修改颜色
    titleAttri[NSForegroundColorAttributeName] = [UIColor orangeColor];
    //设置文本颜色
    [VC.tabBarItem setTitleTextAttributes:titleAttri forState:UIControlStateSelected];
    //渲染选中图片
    VC.tabBarItem.selectedImage=[[UIImage imageNamed:seletedImag] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
    
    [self.taBer addChildViewController:nav];
}
```

 - 最后在Tabar的实现文件里面添加ViewController
 
 - 继承UINavigationControllers，替换成自己的类
 





