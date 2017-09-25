## 实现单元格的点击方法

* 用压栈弹栈的方式

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

* 添加+号按钮并且触发事件

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

* 自定义导航条内容

  1. 添加自定义按钮\(左边\) 右边也一样

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

```
效果图：
```

![](/assets/Snip20170918_2.png)

* 实现导航按钮的抽取

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

* 以上都是在相应的类里viewController\_里实现，代码重复。
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

## 创建相应的类文件夹

* 目录结构分析

![](/assets/Snip20170919_6.png)

* 效果图示

  ![](/assets/Snip20170919_8.png)

## 在Home标签添加导航

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

代码复用了，实现代码的封装

* 两种方法：

  1.继承：

  > 在iOS中继承是单继承\(同类型间的继承\)，只能有一个父类。在继承中，子类可以使用父类的方法和变量，当子类想对本类或者父类的变量进行初始化，那么需要重写init（）方法 。父类也可以访问子类的方法和成员变量。

  2.分类：

  > 1.可以拓展这个类.添加额外的方法.  
  > 2.使得在不修改该类原先代码的情况下.拓展或者修改现有类的定义.并且是向下有效的.既会影响到该类所有子类.

  3.重写一个类的方式用继承还是分类?

  > 取决于具体情况.假如目标类有许多的子类.我们需要拓展这个类又不希望影响到原有的代码.继承后比较好.  
  > 如果仅仅是拓展方法.分类更好.（不需要涉及到原先的代码）

* 使用分类的方式

  1.先创建继承UIBarButtonItem的类  
  2.把复用的方法和放到这个类里，并改成类方法  
  3.使用时直接包含分类，并使用类方法调用

* 解决分类中调用点击事件报错

  应该添加Target参数，明确button是什么类调用的

  ```objectivec
  +(UIBarButtonItem *)addItemTarget:(id)target Image:(NSString *)image selectImage:(NSString *)selImage action:(SEL)action
  {
  ...
  //添加点击事件
   [Btn addTarget:target action:action forControlEvents:UIControlEventTouchUpInside];
  }
  ```

  总结：易读，对原有类进行扩充，并没有改变原有类。

- pch的使用
![](/assets/Snip20170919_11.png)

作用：整个工程项目\(头文件或者宏定义，系统框架，库文件）
好处：预编译，提高程序的运行效率

* 案例
使用自定义xcode打印方法
```
#define FXLog(...) NSLog(__VA_ARGS__)
```

* 在profile中添加设置，并改变颜色
1.不能直接使用字典的方式改变颜色，会找不到键值
2.用以下方式

```
//返回一个Item对象 并可以对改对象的样式（颜色和形状等）进行设置
//拿到局部的item
UIBarButtonItem *item= [UIBarButtonItem appearance];
//新建字典
NSMutableDictionary *testDic=[NSMutableDictionary dictionary];
//根据键值更改颜色
testDic[NSForegroundColorAttributeName] = [UIColor orangeColor];
//设置item
[item setTitleTextAttributes:testDic forState:UIControlStateSelected];
```

效果图：

![](/assets/Snip20170919_14.png)

-添加不能点击状态

![](/assets/Snip20170925_1.png)

```
- (void)viewDidLoad {
[super viewDidLoad];
...

//不可点击时
NSMutableDictionary *disableDic=[NSMutableDictionary dictionary];
//根据键值更改颜色
disableDic[NSForegroundColorAttributeName] = [UIColor grayColor];

//没有选中状态的颜色
[item setTitleTextAttributes:disableDic forState:UIControlStateDisabled];
}
```

并且在相应的类添加

```
//关闭导航控制器的选中状态
-(void)viewWillAppear:(BOOL)animated
{
[super viewWillAppear:animated];
self.navigationItem.rightBarButtonItem.enabled=NO;
}
```





