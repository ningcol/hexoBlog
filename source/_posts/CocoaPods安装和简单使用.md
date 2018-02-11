title: CocoaPods安装和简单使用

date: 2016-02-20 20:20:20

categories: Xcode

tags:

- CocoaPods

---

**1.CocoaPods是什么？**

当你开发iOS应用时，会经常使用到很多第三方开源类库，比如JSONKit，AFNetWorking等等。可能某个类库又用到其他类库，所以要 使用它，必须得另外下载其他类库，而其他类库又用到其他类库，“子子孙孙无穷尽也”，这也许是比较特殊的情况。总之小编的意思就是，手动一个个去下载所需 类库十分麻烦。另外一种常见情况是，你项目中用到的类库有更新，你必须得重新下载新版本，重新加入到项目中，十分麻烦。如果能有什么工具能解决这些恼人的 问题，那将“善莫大焉”。所以，你需要 CocoaPods。

CocoaPods应该是iOS最常用最有名的类库管理工具了，上述两个烦人的问题，通过cocoaPods，只需要一行命令就可以完全解决，当然 前提是你必须正确设置它。重要的是，绝大部分有名的开源类库，都支持CocoaPods。所以，作为iOS程序员的我们，掌握CocoaPods的使用是必不可少的基本技能了。

**2.如何下载和安装CocoaPods？**

在安装CocoaPods之前，首先要在本地安装好Ruby环境。至于如何在Mac中安装好Ruby环境，默认Mac系统中已经安装好。然后在终端中输入以下命令：

```
sudo gem install cocoapods
```

如果你在天朝，在终端中敲入这个命令之后，会发现半天没有任何反应。原因无他，因为那堵墙阻挡了cocoapods.org。我们可以用淘宝的Ruby镜像来访问cocoapods。按照下面的顺序在终端中敲入依次敲入命令：

```
$ gem sources --remove https://rubygems.org/
//等有反应之后再敲入以下命令
$ gem sources -a http://ruby.taobao.org/
```

为了验证你的Ruby镜像是并且仅是taobao，可以用以下命令查看：

```
$ gem sources -l
```

只有在终端中出现下面文字才表明你上面的命令是成功的：

```
*** CURRENT SOURCES ***
http://ruby.taobao.org/
```

上面所有的命令完成之时，终端应该是这个的样子：

 ![](https://s1.ax1x.com/2018/02/11/9G6KV1.jpg)

这时候，你再次在终端中运行：

```
$ sudo gem install cocoapods
```

等上十几秒钟，CocoaPods就可以在你本地下载并且安装好了，不再需要其他设置。敲入以上命令时，等待完成。

最后执行以下命令：

```
$ pod install
```

出现Setting up CocoaPods master repo，说明Cocoapods在将它的信息下载到 ~/.cocoapods里。使用cd命令切换终端到该目录里，用du -sh *命令来查看文件大小，每隔几分钟查看一次，这个目录最终大小是100多M，就是完成了。

**3.如何使用CocoaPods?**

**利用CocoaPods，在项目中导入AFNetworking类库**

AFNetworking类库在GitHub地址是：https://github.com/AFNetworking/AFNetworking

为了确定AFNetworking是否支持CocoaPods，可以用CocoaPods的搜索功能验证一下。在终端中输入：

```
$ pod search AFNetworking
```

过几秒钟之后，你会在终端中看到关于AFNetworking类库的一些信息。比如：

![](https://s1.ax1x.com/2018/02/11/9G6Q56.jpg)

​    这说明，AFNetworking是支持CocoaPods，所以我们可以利用CocoaPods将AFNetworking导入你的项目中。

​      首先，我们需要在我们的项目中加入CocoaPods的支持。先利用Xcode创建一个名字CocoaPodsDemo的项 目，用于以下的教程。创建好之后，在继续下一步之前，先看看以下截图，看看项目没有支持CocoaPods时的项目Xcode目录结构：

![](https://s1.ax1x.com/2018/02/11/9G61PK.jpg)

上图等一下要跟项目支持CocoaPods之后的项目Xcode目录结构做对比。

你看到这里也许会问，CocoaPods为什么能下载AFNetworking呢，而不是下载其他类库呢？这个问题的答案是，有个文件来控制 CocoaPods该下载什么。这个文件就叫做“Podfile”（注意，一定得是这个文件名，而且没有后缀）。你创建一个Podfile文件，然后在里 面添加你需要下载的类库，也就是告诉CocoaPods，“某某和某某和某某某，快到碗里来！”。每个项目只需要一个Podfile文件。

我们先创建PodFile，在终端中进入（cd命令）你项目所在目录，然后在当前目录下，利用vim创建Podfile，运行：

```
$ vim Podfile
```

按i键，终端左下角出现Inset就可以输入：

```
platform :ios, '7.0'
pod "AFNetworking", "~> 2.0"
```

注意，这段文字不是凭空生成的，可以在AFNetworking的github页面找到。这两句文字的意思是，当前AFNetworking支持的iOS最高版本是iOS 7.0, 要下载的AFNetworking版本是2.0。然后保存退出。

这时候，你会发现你的项目目录中，出现一个名字为Podfile的文件，而且文件内容就是你刚刚输入的内容。注意，Podfile文件应该和你的工程文件.xcodeproj在同一个目录下。

这时候，你就可以利用CocoPods下载AFNetworking类库了。还是在终端中的当前项目目录下，运行以下命令：

```
$ pod install
```

因为是在你的项目中导入AFNetworking，这就是为什么这个命令需要你进入你的项目所在目录中运行。

运行上述命令之后，终端出现以下信息：

![](https://s1.ax1x.com/2018/02/11/9G682D.jpg)

注意最后一句话，意思是：以后打开项目就用 CocoaPodsDemo.xcworkspace打开，而不是之前的.xcodeproj文件。