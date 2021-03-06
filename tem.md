##添加搜索框
- 给搜索框添加UITextFile
 - UITextView和UITextFile的区别？
 >结构：1. UITextView继承UISrollView。 作用：可以输入多行，可以滚动。
 >　　　2. UITextFile继承UIControl。    作用：只能输入一行，不可滚动。
 >区别：与Label相比  TextView和TextFile多了一个键盘设置，各自有一个委托协议。
 
-自定义搜索框
封装一个继承UITextFile的类 

![](/assets/Snip20170925_3.png)

代码：

```objectivec
+(UITextField *)addSearchBarImage:(NSString *)image plachHolder:(NSString *)placeHoder frame:(CGRect)frame
{
    //实例化imageView
    UIImageView *searchIcon=[[UIImageView alloc] init];
    //添加图片
    searchIcon.image=[UIImage imageNamed:@"tabbar_discover"];
    //设置尺寸
    searchIcon.frame=CGRectMake(0, 0, frame.size.height, frame.size.height);
    
    //实例化TextFile
    UITextField *textField=[[UITextField alloc] init];
    //设置尺寸
    textField.frame=frame;
    //设置背景颜色
    textField.backgroundColor=[UIColor redColor];
    //设置占位文字
    textField.placeholder=placeHoder;
    
    //设置边框样式
    textField.borderStyle=UITextBorderStyleRoundedRect;
    //文本编辑时添加清除按钮
    textField.clearsOnBeginEditing=YES;
    textField.clearButtonMode=UITextFieldViewModeWhileEditing;
    
    //设置图片
    textField.leftView=searchIcon;
    textField.leftViewMode=UITextFieldViewModeUnlessEditing;
    
    return textField;
}
```

##添加下拉菜单
 - 首页下拉菜单
 >注意：self.navigationItem.titleView的优先级 大于 self.navigationItem.title

![](/assets/Snip20170926_1.png)


```objectivec
    //实例化按钮
    UIButton *titleButton=[[UIButton alloc]init];
    //设置尺寸
    titleButton.frame=CGRectMake(1, 1, 100, 40);
    //设置标题
    [titleButton setTitle:@"首页" forState:UIControlStateNormal];
    //设置颜色
    [titleButton setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
    //设置图片
    [titleButton setImage:[UIImage imageNamed:@"navigationbar_arrow_down"] forState:UIControlStateNormal];
    [titleButton setImage:[UIImage imageNamed:@"navigationbar_arrow_up"] forState:UIControlStateHighlighted];
    
    //设置边距 标题和图片的位置<#CGFloat top#>, <#CGFloat left#>, <#CGFloat bottom#>, <#CGFloat right#>
    titleButton.titleEdgeInsets=UIEdgeInsetsMake(0, 0, 0, 15);
    titleButton.imageEdgeInsets=UIEdgeInsetsMake(0, 70, 0, 0);
    
    
    //应用到导航栏
    self.navigationItem.titleView=titleButton;

```
 - 添加Button的监听事件
  >注意：因为弹出的是图片，添加按钮后要开启用户交互，否则按钮不能点击
  
![](/assets/Snip20170926_3.png)


```objectivec
- (void)viewDidLoad {
   ...
    //设置button监听
    [titleButton addTarget:self action:@selector(titleButtonClick) forControlEvents:UIControlEventTouchUpInside];
}
-(void)titleButtonClick
{
    //实例化ImageView
    UIImageView *dropdownView=[[UIImageView alloc] init];
    //设置背景图片
    dropdownView.image=[UIImage imageNamed:@"popover_background"];
    //尺寸
    dropdownView.frame=CGRectMake(0, 0, 217, 217);
    //实例化button
    UIButton *addButton=[UIButton buttonWithType:UIButtonTypeContactAdd];
    //开启用户交互
    dropdownView.userInteractionEnabled=YES;
    //设置Frame
    addButton.frame=CGRectMake(30, 40, addButton.frame.size.width, addButton.frame.size.height);
    //往图片添加Button
    [dropdownView addSubview:addButton];
        //添加到View
    [self.view addSubview:dropdownView];

}
```
 - 解决dropdownView随cell滚动而滚动


```objectivec
-(void)titleButtonClick
{
 ...
     //获取window窗口
    UIWindow *window=[UIApplication sharedApplication].keyWindow;
    //添加到Window
    [window addSubview:dropdownView];
}
```
- 解决键盘覆盖下拉框


```objectivec 
-(void)titleButtonClick
{
...
    //获取window数组，再取出最后一个数组对象(window)
    UIWindow *window=[[UIApplication sharedApplication].windows lastObject];
...
}
```
- 添加蒙版

```objectivec 
-(void)titleButtonClick
{
...
    //添加蒙版
    UIView *cover=[[UIView alloc]initWithFrame:window.bounds];
    //背景透明
    cover.backgroundColor=[UIColor clearColor];
    //添加子控件
    [cover addSubview:dropdownView];
    //添加到Window
    [window addSubview:cover];
}
```

- 把dropdownView类型替换成UIView,并实现封装
新建继承UIView文件,


```objectivec
//FXDropdownView----------.h文件------------

//下拉菜单属性
@property(nonatomic,strong)UIView * dropView;
////下拉内容属性
@property(nonatomic,strong)UIView * content;

+(instancetype)menu;

-(void)show;


//FXDropdownView----------.m文件------------
-(instancetype)initWithFrame:(CGRect)frame
{
    self = [super initWithFrame:frame];
    if(self){
        //实例化View
        UIView *dropdownView=[[UIView alloc] init];
        
        //背景颜色
        dropdownView.backgroundColor=[UIColor whiteColor];
        
        //尺寸
        dropdownView.frame=CGRectMake(0, 64, 375, 300);
        
        //强引用
        _dropView=dropdownView;
        
        [self addSubview:dropdownView];
    }
    return self;
}

//返回一个UIView对象
+(instancetype)menu
{
    return [[self alloc] init];
}

//显示下拉菜单
-(void)show
{
    //获取window数组，再取出最后一个数组对象(window)
    UIWindow *window=[[UIApplication sharedApplication].windows lastObject];
    
    [window addSubview:self];
    
    //设置尺寸
    self.frame=window.bounds;
}
//重写setContent的Set方法，接受外界传入的控件，并显示到下拉菜单
-(void)setContent:(UIView *)content{
   
    _content=content;
    [self.dropView addSubview:_content];

}
//重写dropView的get方法，用来加载外界传入的控件
-(UIView *)dropView{
  
    if(_dropView == nil){
    
        _dropView = [[UIView alloc] init];
    }
    return _dropView;
}




//FXHomeViewController-------.m

-(void)titleButtonClick
{
    //实例化button
    UIButton *addButton=[UIButton buttonWithType:UIButtonTypeContactAdd];
    
    //设置Frame
    addButton.frame=CGRectMake(30, 40, addButton.frame.size.width, addButton.frame.size.height);
    //实例化
    FXDropdownView *dropView=[FXDropdownView menu];
    
    //添加控件
    dropView.content=addButton;

    //显示
    [dropView show];
}
```

 - 点击cell移除下拉菜单


```
//当触摸控制器view时调用
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event
{
    [self removeFromSuperview];
}
```




 




