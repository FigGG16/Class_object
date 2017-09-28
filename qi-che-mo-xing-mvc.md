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
 >1.上图中分析有四个子控件，声明四个子控件
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
##难点三
 设置分层索引<BR/>
 ![](/assets/Snip20170928_4.png)
 
 >思路
 >plist文件中必须给出对应的组数字符，如难点一图片所示
 >有对应的数据源方法设置
 

```objectivec
//侧栏检索
-(NSArray<NSString *> *)sectionIndexTitlesForTableView:(UITableView *)tableView
{            //title与plist文件中一致
            return [self.model_arr valueForKey:@"title"];
}
```

##难点四
纯代码添加搜索栏<br>
![](/assets/Snip20170928_5.png)<BR>
![](/assets/Snip20170928_6.png)<BR>

>思路：
>1.使用的是UISearchBar,遵守代理协议，设置代理
>2.使用tableHeaderView 摆放搜索栏的位置
>3.定义数组属性searchArray，存放搜索到的模型对象
>4.实现搜索协议-(void)searchBar:(UISearchBar *)searchBar textDidChange:(NSString *)searchText 监控搜索框中的值
>5.得到数据及时更新
>6.根据 searchBar.text.length==0 语句判断是否为搜索状态，返回相应的行数，组数，cell,和侧边栏


- 代码


```objectivec
- (void)viewDidLoad {
    [super viewDidLoad];
    
    //背景颜色
    self.view.backgroundColor=[UIColor redColor];
    //指定cell的高度
    self.tableView.rowHeight=80;
    //实例化searchBar
    UISearchBar *searchBar=[[UISearchBar alloc] init];
    //设置尺寸
    searchBar.frame=CGRectMake(0, 0, 375, 40);
    //遵守代理
    searchBar.delegate=self;
    //赋值给tableHeader
    self.tableView.tableHeaderView=searchBar;
    //属性调用
    _searchBar=searchBar;
}

//实现协议(searchBar)
-(void)searchBar:(UISearchBar *)searchBar textDidChange:(NSString *)searchText
{
    //临时搜索数组
    NSMutableArray  *tempCarSearch=[NSMutableArray array];
    //遍历取出要搜索的汽车
    for(FXCarTitle *carTitle in self.model_arr)
    {
        for(FXCarModel *model in carTitle.cars)
        {
           //把文本转成大写，进行比较是否包含搜索的字符  ，Unix区分大小写
           if([   [model.name uppercaseString]   containsString:   [searchText uppercaseString]   ])
           {
               //加入数组
               [tempCarSearch addObject:model];
           }
        }
    }
    //属性引用
    _searchArray=tempCarSearch;
    //更新数据
    [self.tableView reloadData];
}

```









