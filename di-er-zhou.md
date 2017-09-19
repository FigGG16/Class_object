##实现单元格的点击方法
 - 用压栈弹栈的方式
 ```objectivec

 //实现单元格的点击方法
-(void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    //实例化控制器
    FXTest1ViewController *test1=[[FXTest1ViewController alloc] init];
    //设置背景颜色
    test1.view.backgroundColor=[UIColor redColor];
    //加入栈顶
    [self.navigationController pushViewController:test1 animated:YES];
}
 ```
 
 - 添加+号按钮并且触发事件
 
 
```objectivec

//添加btn
    UIButton *btn=[UIButton buttonWithType:UIButtonTypeContactAdd];
    
    btn.frame=CGRectMake(100, 100, 30, 30);
    
    [btn addTarget:self action:@selector(clickBtn)   forControlEvents:UIControlEventTouchUpInside];

    [test1.view addSubview:btn];

//响应点击
-(void)clickBtn{
        //实例化test12
        FXTest2ViewController *test2=[[FXTest2ViewController alloc] init];
        //设置背景颜色
        test2.view.backgroundColor=[UIColor yellowColor];
        //把test2压入栈顶
        [self.navigationController pushViewController:test2 animated:YES];
}

```

 - 自定义导航条内容
 
  1. 添加自定义按钮(左边) 右边也一样
 
 ![](/assets/Snip20170918_1.png)
 
```objectivec

   //实例化Btn
    UIButton *leftBtn =[[UIButton alloc] init];
    
    //设置普通按钮
    [leftBtn setImage:[UIImage imageNamed:@"navigationbar_back_withtext"] forState:UIControlStateNormal];
    //设置按钮尺寸
    leftBtn.frame=CGRectMake(2, 2, 40, 40);
    //设置高亮按钮
    [leftBtn setImage:[UIImage imageNamed:@"navigationbar_back_withtext_highlighted"] forState:UIControlStateHighlighted];
    //自定义系统返回按钮
    self.navigationItem.leftBarButtonItem=[[UIBarButtonItem alloc] initWithCustomView:leftBtn];
``` 
 
    效果图：
 ![](/assets/Snip20170918_2.png)
 
 - 实现导航按钮的抽取
 
```objectivec
 - (void)viewDidLoad {
    [super viewDidLoad];
//自定义左边按钮
    self.navigationItem.leftBarButtonItem=[self addItemImage:@"navigationbar_back_withtext" selectImage:@"navigationbar_back_withtext_highlighted" action:@selector(back)];

//自定义右边按钮
    self.navigationItem.rightBarButtonItem=[self addItemImage:@"navigationbar_more" selectImage:@"navigationbar_more_highlighted" action:@selector(more)];

//点击隐藏标签页
    self.hidesBottomBarWhenPushed=YES;
}

/**
  导航Item的抽取
 @param image 图片
 @param selImage 选中图片
 @param action 点击事件
 @return   UIBarButtonItem
 */
-(UIBarButtonItem *)addItemImage:(NSString *)image selectImage:(NSString *)selImage action:(SEL)action
{
    UIButton *Btn =[[UIButton alloc] init];
    //设置普通按钮
    [Btn setImage:[UIImage imageNamed:image] forState:UIControlStateNormal];
    //设置按钮尺寸
    Btn.frame=CGRectMake(2, 2, 40, 40);
    //设置高亮按钮
    [Btn setImage:[UIImage imageNamed:selImage] forState:UIControlStateHighlighted];
    //自定义UIBarButtonItem按钮
    UIBarButtonItem *item=[[UIBarButtonItem alloc] initWithCustomView:Btn];
    //添加点击事件
    [Btn addTarget:self action:action forControlEvents:UIControlEventTouchUpInside];
    
    return item;
}
```


 - 以上都是在相应的类里viewController_里实现，代码重复。
  在每次push时调用初始化导航栏按钮，
  1. 在NavigationController文件重写push方法，并把之前的方法移到导航
  2. 去掉首页的导航按钮


```
-(void)pushViewController:(UIViewController *)viewController animated:(BOOL)animated
{
    //初始化父类方法
    [super pushViewController:viewController animated:animated];
    
    //过滤掉首页的导航按钮
    if(self.viewControllers.count>1)
    {

    //自定义左边按钮
    viewController.navigationItem.leftBarButtonItem=[self addItemImage:@"navigationbar_back_withtext" selectImage:@"navigationbar_back_withtext_highlighted" action:@selector(back)];
    
    //自定义右边按钮
    viewController.navigationItem.rightBarButtonItem=[self addItemImage:@"navigationbar_more" selectImage:@"navigationbar_more_highlighted" action:@selector(more)];
    
    }

}

```
##创建相应的类文件夹
 - 目录结构分析
 
![](/assets/Snip20170919_6.png)
 
 - 效果图示
 
 ![](/assets/Snip20170919_8.png)
 
##在Home标签添加导航
![](/assets/Snip20170919_9.png)

代码：把之前的copy上来 
```
- (void)viewDidLoad {
    [super viewDidLoad];
//    //自定义左边按钮
    self.navigationItem.leftBarButtonItem=[self addItemImage:@"navigationbar_friendsearch" selectImage:@"navigationbar_friendsearch_highlighted" action:@selector(friendSearch)];
    
    //自定义右边按钮
    self.navigationItem.rightBarButtonItem=[self addItemImage:@"navigationbar_pop" selectImage:@"navigationbar_pop_highlighted" action:@selector(pop)];
    
}
```
