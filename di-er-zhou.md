##实现单元格的点击方法
 - 用压栈弹栈的方式
 ```
 //实现单元格的点击方法
-(void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    //实例化控制器
    FXTest1ViewController *test1=[[FXTest1ViewController alloc] init];
    //设置背景颜色
    test1.view.backgroundColor=[UIColor redColor];
    //加入栈顶
    [self.navigationController pushViewController:test1 animated:YES];
}
 ```
 
 - 添加+号按钮并且触发事件
 
 
```
//添加btn
    UIButton *btn=[UIButton buttonWithType:UIButtonTypeContactAdd];
    
    btn.frame=CGRectMake(100, 100, 30, 30);
    
    [btn addTarget:self action:@selector(clickBtn)   forControlEvents:UIControlEventTouchUpInside];

    [test1.view addSubview:btn];

//响应点击
-(void)clickBtn{
        //实例化test12
        FXTest2ViewController *test2=[[FXTest2ViewController alloc] init];
        //设置背景颜色
        test2.view.backgroundColor=[UIColor yellowColor];
        //把test2压入栈顶
        [self.navigationController pushViewController:test2 animated:YES];
}

```

 - 自定义导航条内容
 
  1. 添加自定义按钮
 
 ![](/assets/Snip20170918_1.png)
 
```
   //实例化Btn
    UIButton *leftBtn =[[UIButton alloc] init];
    
    //设置普通按钮
    [leftBtn setImage:[UIImage imageNamed:@"navigationbar_back_withtext"] forState:UIControlStateNormal];
    //设置按钮尺寸
    leftBtn.frame=CGRectMake(2, 2, 40, 40);
    //设置高亮按钮
    [leftBtn setImage:[UIImage imageNamed:@"navigationbar_back_withtext_highlighted"] forState:UIControlStateHighlighted];
    //自定义系统返回按钮
    self.navigationItem.leftBarButtonItem=[[UIBarButtonItem alloc] initWithCustomView:leftBtn];
``` 
 
    效果图：
 ![](/assets/Snip20170918_2.png)
 
 
 
 
 
 