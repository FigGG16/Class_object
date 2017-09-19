 - 代码复用了，实现代码的封装
 
 两种方法：
 
      1.继承：
   >在iOS中继承是单继承(同类型间的继承)，只能有一个父类。在继承中，子类可以使用父类的方法和变量，当子类想对本类或者父类的变量进行初始化，那么需要重写init（）方法 。父类也可以访问子类的方法和成员变量。   
      
      2.分类：
   >1.可以拓展这个类.添加额外的方法.
2.使得在不修改该类原先代码的情况下.拓展或者修改现有类的定义.并且是向下有效的.既会影响到该类所有子类.
      
      3.重写一个类的方式用继承还是分类?
 > 取决于具体情况.假如目标类有许多的子类.我们需要拓展这个类又不希望影响到原有的代码.继承后比较好.
如果仅仅是拓展方法.分类更好.（不需要涉及到原先的代码）
      
      
      
      
      
     
     
     


 - 使用类的方式
   
 1.先创建继承object的类
 2.#import <UIKit/UIKit.h>
 3.把复用的方法和放到这个类里，并改成类方法
 4.使用时直接包含分类，并使用类方法调用
 
 
 - 解决分类中调用点击事件报错
 
 应该添加Target参数，明确button是什么类调用的
 ```
 +(UIBarButtonItem *)addItemTarget:(id)target Image:(NSString *)image selectImage:(NSString *)selImage action:(SEL)action
 {
 ...
  //添加点击事件
    [Btn addTarget:target action:action forControlEvents:UIControlEventTouchUpInside];
 }
 ```
 总结：代码不易读。乱
 
 
 - 使用分类的方式
 
 把以上的代码复制到分类里。
 总结：易读，对原有类进行扩充，并没有改变原有类
             