##下拉刷新
 - 补充：实现控件的四种监听方法
 >1.连线，代理，addTarget,通知机制

ios有一个 UIRefreshControl下拉刷新的方法，先在FXHomeViewController



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

//下拉刷新的监听方法
-(void)refershStatusChange
{


}
```

