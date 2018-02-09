title:  Mac破解Wifi密码教程 

date: 2016-09-13 13:01:08

categories: Mac

tags:

- 破解Wifi
- Mac技巧

------

了解过渗透知识的童鞋们都应该知道 BT5 和 Kail ，它们都能够破解 Wifi 密码。目前无线网络加密形式常见的有两种，不对，应该只有一种了，最开始的 WEP 加密方式已经不常见了，这种加密方式非常容易破解，现在常见的是WPA/WPA2 加密方式。破解无线密码的方式有多种，包括 Pin 码破解、抓包破解、伪连接破解等等，Pin 码破解最简单成功率最高，抓包破解的特点就是抓包容易跑包难啊，伪连接破解基本不常见。

**强烈声明：本文只为学习探索使用，如发生任何违法事件，均与本人无关**

下面开始进入正文，首先要确保电脑安装 Xcode 和 MacPorts，Xcode 可以直接在 [App Store](App Store)下载，MacPorts 需要进入[它的官网](https://www.macports.org/)下载。MacPorts 是一个软件包管理系统，用来简化 Mac OS 系统上软件的安装，与Fink和BSD类ports套件的目标和功能类似。就像apt-get、yum一样，可以快速安装些软件。把这两个软件安装成功后就可以开始安装 Aircrack。打开终端输入命令：

```
sudo port install aircrack-ng
```

要求输入密码，输入后可能会提示 **Port aircrack-ng not found** ，没关系我们需要更新ports tree，在终端执行：

```
sudo port -v selfupdate
```

现在可以开始安装 Aircrack 了，**如果还提示 not found，那么再次执行更新ports tree 命令**。安装 Aircrack 会比较慢，等安装好后 ，我们需要将 Aircrack 命令建立一个链接，类似Windows下的超级链接，在终端输入：

```
sudo ln -s /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport /usr/local/bin/airport
```

然后在终端内输入：

```
airport -s
```

就能查看到附近的 Wifi 信息了，如下图

![附近的Wifi](http://odqosxg6n.bkt.clouddn.com/%E9%99%84%E8%BF%91%E7%9A%84Wifi.png)

 SSID 是 wifi 名称，RSSI 是信号强度，CHANNEL 是信道。挑一个信号强的信道进行监听抓包（比如信道 1 ），在终端输入：

```
sudo airport en0 sniff 1
```

命令中的 en0 是自己电脑的网卡地址，根据自己的实际情况进行输入，查看电脑网卡在终端输入 **ifconfig** 就可以了。 抓包过程如下图所示：

![抓包](http://odqosxg6n.bkt.clouddn.com/%E6%8A%93%E5%8C%85.png)

过几分钟后就可以按「 control  ＋ c 」退出抓包，接着进入 tmp 文件就可以查看我们抓到的数据包了，进入 tmp 文件我们可以使用 Finder 的前往文件夹功能 ，如图

![前往tmp文件夹](http://odqosxg6n.bkt.clouddn.com/%E5%89%8D%E5%BE%80tmp%E6%96%87%E4%BB%B6%E5%A4%B9.png)

 进入以后这就刚刚抓到的数据包，我们可以将 cap 文件移到我们想保存的地方，也可以不移，tmp 文件夹在重启电脑后就会清空。![数据包](http://odqosxg6n.bkt.clouddn.com/%E6%95%B0%E6%8D%AE%E5%8C%85.png)

这里我们将文件夹移到桌面。在桌面上创建一个名为 wifiCrack 的文件夹，把刚刚刚的cap文件，直接拖进来重命名为 01。然后把我们的密码字典也拖进来也命名为 01。这里的后缀名是不一样的。 ![数据包和字典](http://odqosxg6n.bkt.clouddn.com/%E6%95%B0%E6%8D%AE%E5%8C%85%E5%92%8C%E5%AD%97%E5%85%B8.png)

接下来在终端中输入：

```
cd Desktop/wifiCrack/
```

进入wifiCrack后，接着输入：

```
aircrack-ng -w 01.txt 01.cap
```

就可以看到cap文件内的抓包情况，Encryption 中（0 handshake）是抓包失败，（1 handshake）则是抓包成功。点击终端，按下「command ＋ f」进行搜索1 handshake，图中看到第2行抓包成功，则在「Index number of target network ?」这里输入2后敲回车：

![读取结果](http://odqosxg6n.bkt.clouddn.com/%E8%AF%BB%E5%8F%96%E7%BB%93%E6%9E%9C.png)

如果cap文件内全是（0 handshake），就按「control  ＋ c 」键退出。重新回到「sudo airport en0 sniff 1」这步进行监听抓包。抓包成功率受到 wifi 信号强弱、电脑与路由器距离远近、路由器是否正处在收发数据状态的影响。总之多试几次、监听时间适当延长些，可以大大提高成功率。进入到破解过程如图 ![破解过程](http://odqosxg6n.bkt.clouddn.com/%E7%A0%B4%E8%A7%A3%E8%BF%87%E7%A8%8B.png)

接着就等着破解结果就行了，中断破解过程可以直接按「control  ＋ c 」键退出。破解过程所需时间长短受电脑硬件配置、字典体积大小的影响。如果 01.txt 字典破解失败，则可以换其它字典进行破解，直到破解成功。便于演示，我在 01.txt 中加入了正确的 wifi 密码，破解成功如下图![破解成功](http://odqosxg6n.bkt.clouddn.com/%E7%A0%B4%E8%A7%A3%E6%88%90%E5%8A%9F.png)

使用一个好的字典是很重要的，一个9位的纯数字字典大概1G多，结果经过几个小时的破解，如果密码是987654321就很令人郁闷了，所以最好准备几个常用的wifi密码字典，可以大大提高成功率和节省时间。常用字典可以直接百度Google搜索下载。只要字典够强大，破解密码只是时间问题，不过跑包的过程中非常伤机器。





