##难点一
 - 处理plist文件中的嵌套
 
 
 ![](/assets/Snip20170928_1.png)
 >我的解决思路是：
 >定义两个模型文件(继承NSObject),分别处理两个array!
 
- 处理第一层array
创建文件FXCarTitle


```objectivec

//车的类型
@property(nonatomic,copy) NSString *title;
//车的数量
@property(nonatomic,copy) NSMutableArray *cars;

//类方法
+(instancetype)carTitleWithDict:(NSDictionary *)dict;

//实例化字典
-(instancetype)initWithDict:(NSDictionary *)dict;

//----------------.h文件END------------------。


#import "FXCarTitle.h"
#import "FXCarModel.h"
@implementation FXCarTitle

//类方法
+(instancetype)carTitleWithDict:(NSDictionary *)dict
{
    //调用实例方法
    return [[self alloc] initWithDict:dict];
}

-(instancetype)initWithDict:(NSDictionary *)dict
{
    if(self=[super init])
    {
        //标题
        _title=dict[@"title"];
        
        //KVC 快速进行模型转换，要求：属性变量名要与字典名一致，不能使用在嵌套的内容。
        //[self setValuesForKeysWithDictionary:dict];
    }
    //分配内存空间
    _cars =[[NSMutableArray alloc] init];
    
/**二层模型转化*/
    
    //判断类型汽车是否存在
    if(dict[@"cars"]!=nil)
    {
        //遍历添加model
        for(NSDictionary *temDict in dict[@"cars"])
        {
            //实例化模型
            FXCarModel *car=[FXCarModel carsWithDict:temDict];
            
            //添加模型
            [_cars addObject:car]; 
        }
    }
    return self;
}

//----------------.M文件END------------------。
```



 - 处理第二层模型
新建文件FXCarModel

```objectivec
@interface FXCarModel : NSObject
//名称
@property(nonatomic,copy) NSString *name;
//图片
@property(nonatomic,copy) NSString *icon;
//转模型
-(instancetype)initWithDict:(NSDictionary *)dict;
+(instancetype)carsWithDict:(NSDictionary *)dict;

//----------------.h文件END------------------。
//类方法
+(instancetype)carsWithDict:(NSDictionary *)dict
{
    //调用实例方法
    return [[self alloc] initWithDict:dict];
}
/**
 @param dict 字典(汽车的名字和图片)
 @return 对象
 */
-(instancetype)initWithDict:(NSDictionary *)dict
{
    
  if(self=[super init])
  {
      //KVC
      [self setValuesForKeysWithDictionary:dict];

  }
    return self;
}
//----------------.M文件END------------------。

```
##难点二
 自定义cell的布局
 ![](/assets/Snip20170928_2.png)

 >思路：
 >1.上图中分析有四个子控件，声明四个子空间
 >2.重写或覆盖初始化方法 -(instancetype)initWithStyle:(UITableViewCellStyle)style 
 >3.在 -(void)layoutSubviews 方法给添加的子控件进行布局


 - 属性

```objectivec
//图片
@property(strong,nonatomic)UIImageView *imageViewFX;

//标题
@property(strong,nonatomic)UILabel *titleLabelFX;

//内容详情
@property(strong,nonatomic)UILabel *textFX;

//评论
@property(strong,nonatomic)UILabel *commentFX;
```


 - 重写方法cell的初始化方法

```objectivec
//重写或覆盖初始化方法
-(instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier
{
self=[super initWithStyle:style reuseIdentifier:reuseIdentifier];
if(self)
{
//实例化控件，图片
_imageViewFX=[[UIImageView alloc] init];
//实例化控件，标题
_titleLabelFX=[[UILabel alloc] init];
//实例化控件，内容详情
_textFX=[[UILabel alloc] init];
//实例化控件，评论
_commentFX=[[UILabel alloc] init];
//添加子类
[self.contentView addSubview:_imageViewFX];
[self.contentView addSubview:_titleLabelFX];
[self.contentView addSubview:_textFX];
[self.contentView addSubview:_commentFX];
}
return self;
}
```
 - 进行布局


```objectivec
//重写或布局子控件
-(void)layoutSubviews
{
//获取屏幕的frame
CGRect mainScreen=[UIScreen mainScreen].bounds;
//设置图片的宽高
_imageViewFX.frame=CGRectMake(mainScreen.size.width-80, 0, 80, 80);
/**---------------------------这也是一种方法，不用设置行数，比较麻烦-------------------------*/
// //设置字体的键值
// NSDictionary *attribute = @{NSFontAttributeName: [UIFont systemFontOfSize:14]};
//
// //声明一个CGSize结构体，并设置宽度长度，换行，并传入参数字典参数 attribute
// CGSize labelSize = [_textFX.text boundingRectWithSize:CGSizeMake(260, 60) options: NSStringDrawingTruncatesLastVisibleLine | NSStringDrawingUsesLineFragmentOrigin | NSStringDrawingUsesFontLeading attributes:attribute context:nil].size;
/**--------------------------- END-------------------------*/
//设置textFX的frame
_textFX.frame=CGRectMake(40, 5, 260, 45);
//设置字体大小
_textFX.font = [UIFont systemFontOfSize:16];
//指定行数
_textFX.numberOfLines=2;
//titleLabelFX布局
_titleLabelFX.frame=CGRectMake(40, self.contentView.frame.size.height-30, 150, 20);
//_commentFX布局
_commentFX.frame=CGRectMake(_textFX.frame.size.width-20, self.contentView.frame.size.height-30, 50, 20);
}
```







