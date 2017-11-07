##下拉刷新
 - 扩展
 >实现控件监听的4中方法：连线，代理,addTarget,通知机制
 
在FXHomeViewController新增刷新方法


```objectivec
//下拉刷新方法
-(void)setUpRefershStatus
{
//刷新控件
    UIRefreshControl *control=[[UIRefreshControl alloc] init];
   
    [self.tableView addSubview:control];
    
    //实现监听方法
    [control addTarget:self action:@selector(refershStatusChange) forControlEvents:UIControlEventValueChanged];

}

```
实现下拉刷新的监听方法


```objectivec
//下拉刷新的监听方法
-(void)refershStatusChange
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
>分析：以上方法直接复制loadStatus方法并不是很好， 另一种更好的方式
>A=[@"1",@"2",@"3"];
>现在要刷新A,结果：
>A1=[@"5",@"6"];
>使用[A addobject:A1], 那么A的值为A=[[@"5",@"6"]@"1",@"2",@"3"]; 不符合要求！
>我们理想的的值应该为：[@"5",@"6",@"1",@"2",@"3"]；
>使用：[self.status insertObjects:... atIndexes:...];


首先使用第三方控件快速转模型

```
#import "MJExtension.h"
//下拉刷新的监听方法
-(void)refershStatusChange
{
  ....
  
  [man GET:@"https://api.weibo.com/2/statuses/home_timeline.json" parameters:params success:^(AFHTTPRequestOperation *operation, id responseObject) {
        
        //第三方框架快速转模型
        NSArray *newStatus= [FXStatus objectArrayWithKeyValuesArray:responseObject[@"statuses"]];
     
        //根据数组内容的个数，获取范围
        NSRange range=NSMakeRange(0, newStatus.count);
        
        // 设置位置
        NSIndexSet *set= [NSIndexSet indexSetWithIndexesInRange:range];
        
        //在前方插入
       [self.status insertObjects:newStatus atIndexes:set];
        
    
        
        [self.control endRefreshing];
        
        //更新cell数据
        [self.tableView reloadData];
        
        NSLog(@"responseObject=%@",responseObject);
    }
  
  ....

}

```
以上插入最新数据之后一直重复，再添加如下代码



```objectivec

//下拉刷新的监听方法
-(void)refershStatusChange
{
....
    //加载最新微博(解决重复包含)
    FXStatus *first   = [self.status firstObject];

    if(first!=nil)
    {
          params[@"since_id"]=first.idstr;
    }

....

}

```

重复请求，以下方法进行合并
删除loadStatus

```objectivec
    //获取微博
//    [self loadStatus];
    //下拉刷新
    [self setUpRefershStatus];
```

在setUpRefershStatus方法添加


```objectivec
//下拉刷新方法
-(void)setUpRefershStatus
{
...
    //程序一启动就进行刷新
    [self refershStatusChange:control];
}
```

##显示更新数据


```objectivec
//显示更新数据
-(void)setUpNewStatusCount:(NSInteger)count{
    UILabel *label=[[UILabel alloc] initWithFrame:CGRectMake(0, 0, self.tableView.width, 50)];
    
        //文本居中
    label.textAlignment=NSTextAlignmentCenter;
    //根据图片设置背景色
    label.backgroundColor=[UIColor colorWithPatternImage:[UIImage imageNamed:@"timeline_new_status_background"]];
      
    
    if(count)
    {
//        NSLog(@"更新了%ld条数据",newStatus.count);
        NSString *str=[NSString stringWithFormat:@"更新了%ld条微博数据",count];
        label.text=str;
    }
    else
    {
        label.text=@"没有最新数据";
    }
    //在....控件的下方插入数据
    [self.navigationController.view insertSubview:label belowSubview:self.navigationController.navigationBar];
   //添加动画
    
    CGFloat dura=1.0;
    [UIView animateWithDuration:dura animations:^{
        //执行动画的操作
        label.y+=50;
        
    } completion:^(BOOL finished) {
        
        CGFloat delay =1.0;
        
        //完成操作后进入该代码块,options: 匀速运动
        [UIView animateWithDuration:dura delay:delay options:UIViewAnimationOptionCurveLinear animations:^{
           
            label.y-=50;
        } completion:^(BOOL finished) {
            
            //从内存中 清除
            [label removeFromSuperview];
            
        }];
        
    }];
}

```
##上拉刷新
思路：
>新建继承文件UIView文件，并关联xib
>赋值给tableView的tableFooteView,上拉时会显示
>更新数据时与下拉刷新相同，只需改一下数据向数组后插入，取出时也取出数组最后的数据
>更改since_id时间晚的



```objectivec
//产生一个xib对象
+(instancetype)footer;
//-------FXLoadFooter.h------->

+(instancetype)footer
{
    return [[NSBundle mainBundle] loadNibNamed:@"FXLoadFooter" owner:nil options:nil].lastObject;

}
//-------FXLoadFooter.m------->


- (void)viewDidLoad {
    [super viewDidLoad]
    ...
        //上拉刷新
    [self setUpFooter];
    }
//上拉刷新的实现方法
-(void)setUpFooter
{
    FXLoadFooter *foot=[FXLoadFooter footer]; 
    self.tableView.tableFooterView=foot;
}
//-----------FXHomeViewController.m-------

```

##显示微博未读数














