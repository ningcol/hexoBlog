title: 快速pop返回指定控制器

date: 2016-02-27 21:50:07

categories: iOS

tags:

- popToRootViewController

------

iOS中ViewController的跳转，比如我们从主页跳转到一级界面，又从一级界面跳转到二级界面...当我们需要一级一级返回的时候是没有问题的，可以直接使用navigationController popoViewControllerAnimated就行了，但是有些情况下我们需要马上返回到主页，而不是一级一级的返回(一级一级的跳转太累了)。具体实现请看下面代码

```
//第一种实现方式
[self.navigationController popToViewController:[self.navigationController.viewControllers objectAtIndex:1] animated:YES];

//第二种方法
 for (UIViewController *vc in self.navigationController.viewControllers) {
 //这里的JGHomeController是你需要返回的控制器
        if ([vc isKindOfClass:[JGHomeController class]]) {
            [self.navigationController popToViewController:vc animated:YES];
        }
    }
```