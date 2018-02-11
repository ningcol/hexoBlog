title: 代码加载storyboard中的控制器

date: 2015-11-20 10:34:23

categories: iOS

tags:

- storyboard
- push

------

我们大多用纯代码来写我们的项目，便于后期的更新和维护，但是有时候我们会遇到有些界面基本不会改变的情况，加之开发进度比较紧，我们就可以在storyboard创建控制器，加载storyboard并push里面的viewcontroller，具体实现如下

1.我们需要创建一个storyboard，名字为baseController。

2.然后拖动一个ViewController至我们创建的storyboard中，设置custom class里的Class为我们代码创建的ViewController，并设置storyboard ID为base，如图

![](https://s1.ax1x.com/2018/02/11/9G6A8U.jpg)

3.加载storyboard里面的viewcontroller并push

```
 //加载storyboard
    UIStoryboard* storyboard = [UIStoryboard storyboardWithName:@"baseController" bundle:[NSBundle mainBundle]];
    BaseViewController *baseVc = ( BaseViewController *)[storyboard instantiateViewControllerWithIdentifier:@"base"];
    [basic setHidesBottomBarWhenPushed:YES];
    [self.navigationController pushViewController:baseVc animated:YES];
```

注意：storyboardWithName:@"baseController"是代表加载"'baseController"这个storyboard里面的标示符为"base"的这个控制器，因为一个storyboard的里面会有很多的控制器，我们就是通过这个找到它的，需要强调的是注意class和storyboard ID的区别。