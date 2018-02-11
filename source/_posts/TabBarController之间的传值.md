title: TabBarController之间的传值

date: 2015-11-4 16:41:08

categories: iOS

tags:

- TabBarController

------

1.下图(JGHomeController)为第一个控制器，他的tabbarController的index为0，将要传值给JGJobListController为第二个控制器，传递的值为abc数组。 ![](https://s1.ax1x.com/2018/02/11/9G6SDs.png)

2.JGJobListController控制器的头文件，需要声明abc属性。接下来就可以在m文件里使用abc了。

![](https://s1.ax1x.com/2018/02/11/9G6pbn.png)