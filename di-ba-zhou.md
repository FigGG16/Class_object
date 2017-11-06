##下拉刷新
 - 扩展
 >实现控件监听的4中方法：连线，代理,addTarget,通知机制
 
在FXHomeViewController新增刷新方法


```
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



