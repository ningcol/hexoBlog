title: 在OC项目中使用Swift(Swift-OC混编)

date: 2016-08-02 10:11:20

categories: iOS

tags:

- 混合开发
- OC和Swift

------

好几个月没有更新博客了，因为毕业答辩把成都的工作给丢了，回到家找到工作的同时又把婚也结了，算是成家立业了。这身份转换得太快，博主现在都还在懵逼之中。

现在这个项目是Swift和OC混合开发的，所以就涉及到如何在Swift项目中使用OC，以及如何在OC项目中使用Swift。

一、直接在OC工程里创建swift文件，XCode会自动提示是否创建这个桥接文件，这里点击确定（这个桥接文件是必须的）

![](https://s1.ax1x.com/2018/02/11/9GcPLd.png)

二、可以看到刚刚创建的swift文件以及桥接文件，我们在Test.swfit写个方法，如图

![](https://s1.ax1x.com/2018/02/11/9GcFeA.png)

三、最后我们在需要使用swift的OC文件里引入头文件`#import "OCandSwift-Swift.h"`，**OCandSwift**是项目的名称，**-Swift.h**是固定的。最后我们在viewDidLoad中调用Test.swift中的方法

```
 Test *test = [[Test alloc] init];
  NSLog(@"%@",[test run]);
```

![](https://s1.ax1x.com/2018/02/11/9GckdI.png)

四、如果我们要在swift中调用OC，就在刚刚的桥接文件里引入OC的头文件再调用其中的方法就好了

![](https://s1.ax1x.com/2018/02/11/9GcVFP.png)