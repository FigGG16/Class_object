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

