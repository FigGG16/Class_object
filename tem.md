##添加搜索框
- 给搜索框添加UITextFile
 - UITextView和UITextFile的区别？
 >结构：UITextView继承UISrollView。 作用：可以输入多行，可以滚动。
 >结构：UITextFile继承UIControl。    作用：只能输入一行，不可滚动。
 >区别：与Label相比  TextView和TextFile多了一个键盘设置，各自有一个委托协议。
 
-自定义搜索框
封装一个继承UITextFile的类 

![](/assets/Snip20170925_3.png)

代码：

```objectivec
+(UITextField *)addSearchBarImage:(NSString *)image plachHolder:(NSString *)placeHoder frame:(CGRect)frame
{
    //实例化imageView
    UIImageView *searchIcon=[[UIImageView alloc] init];
    //添加图片
    searchIcon.image=[UIImage imageNamed:@"tabbar_discover"];
    //设置尺寸
    searchIcon.frame=CGRectMake(0, 0, frame.size.height, frame.size.height);
    
    //实例化TextFile
    UITextField *textField=[[UITextField alloc] init];
    //设置尺寸
    textField.frame=frame;
    //设置背景颜色
    textField.backgroundColor=[UIColor redColor];
    //设置占位文字
    textField.placeholder=placeHoder;
    
    //设置边框样式
    textField.borderStyle=UITextBorderStyleRoundedRect;
    //文本编辑时添加清除按钮
    textField.clearsOnBeginEditing=YES;
    textField.clearButtonMode=UITextFieldViewModeWhileEditing;
    
    //设置图片
    textField.leftView=searchIcon;
    textField.leftViewMode=UITextFieldViewModeUnlessEditing;
    
    return textField;
}
```

##添加下拉菜单
 - 首页下拉菜单





