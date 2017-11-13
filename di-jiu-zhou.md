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

 - 进行拦截UITableViewCellStyleSubtitle方法
 
