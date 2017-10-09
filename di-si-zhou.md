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
 
 

