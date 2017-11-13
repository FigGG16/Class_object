##自定义单元格
分析:
![](/assets/Snip20171113_1.png)

 - 新建继承UITableViewCell文件并添加


```objectivec
//将所有可能显示的控件，放到contentView
@interface FXTableViewCell : UITableViewCell

//原创微博的整体
@property(nonatomic,weak)UIView *originalView;
//头像
@property(nonatomic,weak)UIImageView *icon;
//用户昵称
@property(nonatomic,weak)UILabel *nameLabel;
//时间
@property(nonatomic,weak)UILabel *timeLabel;
//数据来源
@property(nonatomic,weak)UILabel *sourceLabel;
//内容
@property(nonatomic,weak)UILabel *contentLabel;
//vip
@property(nonatomic,weak)UIImageView *vipView;

//parms: 解决单元格重用
+(instancetype )cellWithTableView:(UITableView *)tableView;

//------------------------------------------------FXTableViewCell.h----



+(instancetype )cellWithTableView:(UITableView *)tableView
{
   static NSString *ID=@"FX";
    
    //创建方法，并实现封装
    FXTableViewCell *cell=[tableView dequeueReusableCellWithIdentifier:ID];
   
    if(cell == nil)
    {   //不能让系统创建UITableViewCellStyleSubtitle 方法，要进行拦截
        //重写这个方法
        cell=[[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:ID];
    }
      
    return cell;
}




```


创建步骤:
![](/assets/Snip20171113_2.png)

 - 进行拦截UITableViewCellStyleSubtitle方法并实例化控件
 
```objectivec

、-(instancetype) initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier
{
    self =[super initWithStyle:style reuseIdentifier:reuseIdentifier];
   
    if(self){
    //原创微博的整体
       _originalView=[[UIView alloc] init];
       [self.contentView addSubview:_originalView];
                      
        //头像   //添加以下控件到原创微博整体上
       _icon=[[UIImageView alloc] init];
        [_originalView addSubview:_icon];
                             
        //用户昵称
       _nameLabel=[[UILabel alloc] init];
        [_originalView addSubview:_nameLabel];
                             
        //时间
       _timeLabel=[[UILabel alloc] init];
        [_originalView addSubview:_timeLabel];
        
        //数据来源
       _sourceLabel=[[UILabel alloc] init];
        [_originalView addSubview:_sourceLabel];
        
        //内容
       _contentLabel=[[UILabel alloc] init];
        [_originalView addSubview:_contentLabel];
        
        //vip
       _vipView=[[UIImageView alloc] init];
        [_originalView addSubview:_vipView];
    
    }
}   
 
```

 - 在model新建一个继承NSObject的文件存储cell控件的位置信息
 
```objectivec

//微博数据
@property(nonatomic,strong)FXStatus *status;
//原创微博的整体
@property(nonatomic,assign)CGRect originalViewF;
//头像
@property(nonatomic,assign)CGRect iconF;
//用户昵称
@property(nonatomic,assign)CGRect nameLabelF;
//时间
@property(nonatomic,assign)CGRect timeLabelF;
//数据来源
@property(nonatomic,assign)CGRect sourceLabelF;
//内容
@property(nonatomic,assign)CGRect contentLabelF;
//vip
@property(nonatomic,assign)CGRect vipView;
//单元格的高度
@property(nonatomic,assign)CGFloat cellHeight;
//----------------------------------------FXStatusFrame.h---------------


//重写status的set方法，接收微博数据 根据微博
-(void)setStatus:(FXStatus *)status
{

}
//----------------------------------------FXStatusFrame.m---------------




//----------------------------------------FXTableViewCell.h---------------\\
//添加： 设置frame模型
@property(nonatomic,strong)FXStatusFrame *statusframe;


//重写frame 的set方法 给每个控件设置frame
//----------------------------------------FXTableViewCell.m---------------
//重写frame 的set方法 给每个控件设置frame
-(void)setStatusframe:(FXStatusFrame *)statusframe
{
    _statusframe=statusframe;
    
}
```
因为已经有了各个控件的frame信息，
在FXHomeViewController.h文件新定义升级版的模型

```objectivec
@property(nonatomic,strong)NSMutableArray *statusesFrame;
```

 - 在微博的下拉刷新的方法中部分修改


```objectivec
...
  //将微博模型转化为frame模型
        NSMutableArray *frames=[NSMutableArray array];
        for(FXStatus *status in newStatus){
            
            //frame模型
            FXStatusFrame *f=[[FXStatusFrame alloc] init];
            
            //把微博模型转化为frame模型
            f.status = status;
            
            [frames addObject:f];
        }
        self.statusesFrame=frames;
...
            //在前方插入
        [self.statusesFrame insertObjects:frames atIndexes:set];
...      
```




 
