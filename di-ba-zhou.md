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
        [self.status insertObject:newStatus atIndex:set];
        
    
        
        [self.control endRefreshing];
        
        //更新cell数据
        [self.tableView reloadData];
        
        NSLog(@"responseObject=%@",responseObject);
    }
  
  ....

}

```



