##请求微博首页数据

请求到的json数据数据格式如下
![](/assets/Snip20171031_1.png)
>定义一个loadStatus方法用于请求微博数据
>定义一个模型嵌套
>TABLEVIEW显示
>第三方框架加载图片
```objectivec

//获取微博
-(void)loadStatus
{
  //https://api.weibo.com/2/statuses/home_timeline.json
    
    //创建一个账号模型
    FXAccount *account=[FXAccountTool account];
    
    //创建一个管理对象
    AFHTTPRequestOperationManager *man=[AFHTTPRequestOperationManager manager];
    //从模型中取出请求信息
    NSMutableDictionary *params = [NSMutableDictionary dictionary];
    
    //存到字典
    params[@"access_token"]=account.access_token;
//    params[@"count"]=@1;
//    params[@"uid"]=account.uid;
    
    //发送get请求
    [man GET:@"https://api.weibo.com/2/statuses/home_timeline.json" parameters:params success:^(AFHTTPRequestOperation *operation, id responseObject) {
        
        //获取json数组
       NSArray *statusArry =responseObject[@"statuses"];
        
        //把数据嵌套到模型
        for(NSDictionary *dict in statusArry )
        {
            
            FXStatus *status = [FXStatus statusWithDict:dict];
            
            [self.status addObject:status];
        }
        //更新cell数据
        [self.tableView reloadData];
        
        NSLog(@"responseObject=%@",responseObject);
    } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
        NSLog(@"请求失败");
    }
     
    ];
}
```


##两个嵌套模型



```objectivec

@class FXUser;
@interface FXStatus : NSObject

@property(nonatomic,copy)NSString *idstr;


@property(nonatomic,copy)NSString *text;
//微博user属性
@property(nonatomic,strong)FXUser *user;

+(instancetype)statusWithDict:(NSDictionary *)dict;

----------------------------.h-----------

+(instancetype)statusWithDict:(NSDictionary *)dict
{
    FXStatus *status=[[self alloc ] init];
    
    status.idstr=dict[@"idstr"];
    status.text=dict[@"text"];
    
    status.user = [FXUser userWithDict:dict[@"user"]];
    return status;
}
----------------------------.m-------------
//---------------------------------**Status **


@interface FXUser : NSObject

//idstr;
//name
//
//profile_image_url
@property(nonatomic,copy)NSString *idstr;

@property(nonatomic,copy)NSString *name;

@property(nonatomic,copy)NSString *profile_image_url;

+(instancetype)userWithDict:(NSDictionary *)dict;

------------------.h------------------------------
+(instancetype)userWithDict:(NSDictionary *)dict
{
    FXUser *user=[[self alloc] init];
    
    user.name=dict[@"name"];
    user.profile_image_url=dict[@"profile_image_url"];
    user.idstr=dict[@"idstr"];
    
    return user;

}
--------------------.m-----------------------------------
//---------------------------------**FXUser **







```





##设置tableView


```objectivec


#pragma --设置行数
-(NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    
    return self.status.count;

}

//设置单元格数据
-(UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
  static NSString *ID=@"cell";
    
    UITableViewCell *cell=[tableView dequeueReusableCellWithIdentifier:ID];
    
    if(cell == nil){
    
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:ID];
    }
    
    FXStatus *status =self.status[indexPath.row];
    FXUser *user=status.user;
    //用户名
    cell.textLabel.text=user.name;
    //文本详情
    cell.detailTextLabel.text=status.text;
    //图片url地址
    NSString *imageUrl=user.profile_image_url;
    
    //加载图片
    [cell.imageView sd_setImageWithURL:[NSURL URLWithString:imageUrl] placeholderImage:[UIImage imageNamed:@"avatar_default"]];
    
    return cell;
}

```






