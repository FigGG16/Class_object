##步骤
1.新建继承文件继承UITableViewCell
2.声明需要自定义的属性

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


3.重写方法cell的初始化方法

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
4.进行布局


```objectivec
//重写或布局子控件
-(void)layoutSubviews
{
    //获取屏幕的frame
    CGRect mainScreen=[UIScreen mainScreen].bounds;
    
    //设置图片的宽高
    _imageViewFX.frame=CGRectMake(mainScreen.size.width-80, 0, 80, 80);
    
    /**---------------------------这也是一种方法，不用设置行数，比较麻烦-------------------------*/
//    //设置字体的键值
//    NSDictionary *attribute = @{NSFontAttributeName: [UIFont systemFontOfSize:14]};
//    
//    //声明一个CGSize结构体，并设置宽度长度，换行，并传入参数字典参数 attribute
//    CGSize labelSize = [_textFX.text boundingRectWithSize:CGSizeMake(260, 60) options: NSStringDrawingTruncatesLastVisibleLine | NSStringDrawingUsesLineFragmentOrigin | NSStringDrawingUsesFontLeading attributes:attribute context:nil].size;
    
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


