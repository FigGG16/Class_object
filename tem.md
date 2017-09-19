 - pch的使用
 ![](/assets/Snip20170919_11.png)
 
 
 作用：整个工程项目(头文件或者宏定义，系统框架，库文件）
 好处：预编译，提高程序的运行效率

 - 案例
 使用自定义xcode打印方法
 ```
 #define FXLog(...) NSLog(__VA_ARGS__)
 ```
 
 
 - 在profile中添加设置，并改变颜色
 1.不能直接使用字典的方式改变颜色，会找不到键值
 2.用以下方式
 ```
     //返回一个Item对象 并可以对改对象的样式（颜色和形状等）进行设置
    //拿到局部的item
    UIBarButtonItem *item= [UIBarButtonItem appearance];
    //新建字典
    NSMutableDictionary *testDic=[NSMutableDictionary dictionary];
    //根据键值更改颜色
    testDic[NSForegroundColorAttributeName] = [UIColor orangeColor];
    //设置item
    [item setTitleTextAttributes:testDic forState:UIControlStateSelected];
 ```
 效果图：
 
 ![](/assets/Snip20170919_14.png)