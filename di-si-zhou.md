##在标签导航栏中添加+号标签
 - 分析
 >添加一个buttom控件
 >使用继承UITabBar的方式添加控件
 >获取UITabBarButton这个类，然后将标签进行移位，布局
 
 - 图片分析
 
 ![](/assets/Snip20171009_4.png)
 
 
 - 代码
 
 
 
```objectivec


//--------------------.h------------文件
#import <UIKit/UIKit.h>

@interface FXTabar : UITabBar

@property(nonatomic,strong)UIButton *plusBtn;

@end

//---------------------.m-----------------图片
#import "FXTabar.h"

@implementation FXTabar


-(instancetype)initWithFrame:(CGRect)frame
{
   self= [super initWithFrame:frame];
   
    //添加+号按钮
    UIButton *plusBtn=[[UIButton alloc] init];
    //设置button的frame
    plusBtn.frame=CGRectMake(self.width*0.5-20, 1, 40, 40);
    //普通按钮背景图和图片
    [plusBtn setImage:[UIImage imageNamed:@"tabbar_compose_icon_add"] forState:UIControlStateNormal];
    [plusBtn setBackgroundImage:[UIImage imageNamed:@"tabbar_compose_button"] forState:UIControlStateNormal];
    
    //高亮按钮背景图和图片
    [plusBtn setImage:[UIImage imageNamed:@"tabbar_compose_icon_add_highlighted"] forState:UIControlStateHighlighted];
    [plusBtn setBackgroundImage:[UIImage imageNamed:@"tabbar_compose_button_highlighted"] forState:UIControlStateHighlighted];
    //添加子控件
    [self addSubview:plusBtn];

    _plusBtn=plusBtn;
    
    return self;
}

-(void)layoutSubviews{

    [super layoutSubviews];
    
    //设置button的中间坐标
    _plusBtn.centerX=self.width*0.5;
    _plusBtn.centerY=self.height*0.5;
    
    //获取这个类对象
    Class class = NSClassFromString(@"UITabBarButton");
    //获取屏幕的宽度，并分成五份
    CGFloat tabBarButtonW=self.width/5;
    NSLog(@"tabBarButtonW=%lf",tabBarButtonW);
    
    //临时索引变量
    CGFloat tabBarButtonIndex=0;
    NSInteger count=self.subviews.count;
    
    for(int i=0;i<count;i++)
    {
        //遍历子控件
        UIView *child=self.subviews[i];
        //是否是想要的控件
        if([child isKindOfClass:class]){
          
            //设置控件宽度
            child.width=tabBarButtonW;
            //改变x轴坐标
            child.x=tabBarButtonIndex*tabBarButtonW;
            //
            tabBarButtonIndex++;
            //为+号控件留出空格
            if(tabBarButtonIndex==2)
            {
                tabBarButtonIndex++;
            }
        }
    }
}

```

 - 使用代理的方式实现监听按钮的点击
 
 前提是使用addTarget的方式通知代理
 


```objectivec


....
@class FXTabar;
//声明协议
@protocol FXTabBarDelegate <NSObject>

- (void)clickPlusBtn:(FXTabar *)tabbar;

@end

....
@property(nonatomic,weak)id<FXTabBarDelegate> delegate;
....
//-----------------FXTabsr.h文件END--------------



...
//遵守协议
@interface FXTaBarController ()<FXTabBarDelegate>

@end

...

//实现协议方法
-(void)clickPlusBtn:(FXTabar *)tabbar
{
UIViewController *VC=[[UIViewController alloc] init];
VC.view.backgroundColor=[UIColor redColor];
// [self.navigationController pushViewController:VC animated:YES];
[self presentViewController:VC animated:YES completion:nil];
}


- (void)viewDidLoad 
{
 ...
     //设置KVC,自定义tabBar
    [self setValue:[[FXTabar alloc] init] forKey:@"tabBar"];
    
    //重新初始化(如果不初始化，KVC将FXTabar变成标签控制器管理的对象，报错)
    FXTabar *plusBtn=[[FXTabar alloc]init];
    
   //设置代理
    plusBtn.delegate=self;
 ...
}

//-----------------FXTaBarController.m-------end




//添加监听点击事件
-(instancetype)initWithFrame:(CGRect)frame
{
...
    [plusBtn addTarget:self action:@selector(plusBtnClick) forControlEvents:UIControlEventTouchUpInside];
...
}

...
//响应button的点击
-(void)plusBtnClick{
    //判断代理方法是否实现
   if([self.delegate respondsToSelector:@selector(clickPlusBtn:)])
   {
       //传递self
       [self.delegate clickPlusBtn:self];
   }
}
//-----------------FXTabsr.m文件END--------------

```

##版本新特性功能开发
 >新建文件夹newFeature
 >原理和轮播器类似
 



```objectivec

#import "FXNewFeatureViewController.h"

@interface FXNewFeatureViewController ()

@property(nonatomic,strong)UIScrollView *scrollView;
@property(nonatomic,strong)UIPageControl *pageControl;

@end

@implementation FXNewFeatureViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    UIScrollView *scroll=[[UIScrollView alloc] init];
    
    //图片数量
    NSInteger imageCount=4;
    //设置大小
    scroll.frame=self.view.bounds;
    
    //循环添加image到滚动控制器
    for(int i=0;i<imageCount;i++)
    {
        
        UIImageView *imageV=[[UIImageView alloc] init];
        
        imageV.frame=CGRectMake(i*self.view.width, 0, self.view.width, self.view.height);
        
        NSString *imageName=[NSString stringWithFormat:@"new_feature_%d",i+1];
        
        imageV.image=[UIImage imageNamed:imageName];
        [scroll addSubview:imageV];     
    }
    //设置滚动范围
    scroll.contentSize=CGSizeMake(imageCount*self.view.width, self.view.height);
    //开启分页
    scroll.pagingEnabled=true;
    
    //去除水平线
    scroll.showsHorizontalScrollIndicator=NO;
    
    //取消弹簧效果
    scroll.bounces=NO;
        [self.view addSubview:scroll];
    
    //实例UIPageControl
    UIPageControl *pageControl=[[UIPageControl alloc] init];
    
    pageControl.frame=CGRectMake(150, 600, 100, 30);
    //分页数量
    pageControl.numberOfPages=imageCount;
    //add pageContol to the view
    [self.view addSubview:pageControl];
    
    //当前颜色
    pageControl.currentPageIndicatorTintColor=[UIColor redColor];
    
    //其余颜色
    pageControl.pageIndicatorTintColor=[UIColor blueColor];
    //关闭交互
    pageControl.userInteractionEnabled=NO;
    
    //强引用控制器
    _scrollView=scroll;
    _pageControl=pageControl;
    
    //make control begcame delagage!
    scroll.delegate=self;
}

#pragma mark 实现代理方法
-(void)scrollViewDidScroll:(UIScrollView *)scrollView
{
    //根据偏移量计算
    NSInteger page=(scrollView.contentOffset.x/_scrollView.width+0.5);
    _pageControl.currentPage=page;

}


```

控件不显示的原因
![](/assets/Snip20171010_1.png)



 - 对最后一页进行处理
 

![](/assets/Snip20171010_3.png)




```objectivec
...
- (void)viewDidLoad {
    for(int i=0;i<imageCount;i++)
    {
        ...
               //对最后一页进行处理
        if(i==3){
           [self setUpLastImage:imageV];
        }
        ...
  }
  ...
}


-(void)changeState:(UIButton *)shareBtn{
   //改变状态
   shareBtn.selected=!shareBtn.isSelected;
}

-(void)setUpLastImage:(UIImageView*)imag
{
   //开启交互
   imag.userInteractionEnabled=TRUE;
   //实例控件
   UIButton *shareBtn=[[UIButton alloc] init];
   shareBtn.frame=CGRectMake(100, 400, 200, 40);
   
   //设置图片
   [shareBtn setImage:[UIImage imageNamed:@"new_feature_share_false"] forState:UIControlStateNormal];
   
   [shareBtn setImage:[UIImage imageNamed:@"new_feature_share_true"] forState:UIControlStateSelected];
   
   //设置标题
   [shareBtn setTitle:@"分享微博" forState:UIControlStateNormal];
   //标题颜色
   [shareBtn setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
   //监控点击
   [shareBtn addTarget:self action:@selector(changeState:) forControlEvents:UIControlEventTouchUpInside];
   //添加到image
   [imag addSubview:shareBtn];
   
   //实例控件
   UIButton *startBtn=[[UIButton alloc] init];
   startBtn.frame=CGRectMake(150, 500, 100, 40);
   
   //设置背景图
   [startBtn setBackgroundImage:[UIImage imageNamed:@"new_feature_finish_button_highlighted"] forState:UIControlStateNormal];
   [startBtn setBackgroundImage:[UIImage imageNamed:@"new_feature_finish_button_highlighted"] forState:UIControlStateHighlighted];
   
   //设置标题
   [startBtn setTitle:@"开始微博" forState:UIControlStateNormal];
   //标题颜色
   [startBtn setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
   

   [imag addSubview:startBtn];

}
//-----------------FXNewFeatureViewController.m----end
```




 
 

