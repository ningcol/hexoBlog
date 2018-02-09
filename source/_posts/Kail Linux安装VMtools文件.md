title: Kail Linux安装VMtools文件

date: 2014-11-03 13:53:51

categories: Kail 

tags:

- kail linux
- VMware 

---

昨天在网上搜了半天如何在kail下安装tools工具，结果都没有成功，今天就来说说我是怎么安装成功的。

按照网上的办法只能到这一步，一直提示找不到路径，点了回车也会照常出现这样的情况。

```
Searching for a valid kernel header path...
   The path "" is not a valid path to the 3.7-trunk-686-pae kernel headers
   Would you like to change it? [yes]
```

这时你就会觉得是不是没有更新，其实你apt-get install linux-headers-3.7-trunk-686-pae也是没用的。

 去查看[kail官方文档](http://cn.docs.kali.org/general-use-cn/kali%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%AE%89%E8%A3%85vmware-tools)，发现apt-get install open-vm-tools也没用，即使是用它下面提供的方法安装一些VMware Tools安装器需要的包也没用。

#### 现在我们进入正题：####

   1.修改修改sources.list文件：
    **leafpad /etc/apt/sources.list**

   2.添加源文件，下面的源我加进去了中科大和阿里云的源，你们也可以选择一下，就是不知道安装tools的时候能不能成功。

```
#官方源
deb http://http.kali.org/kali kali main non-free contrib
deb-src http://http.kali.org/kali kali main non-free contrib
deb http://security.kali.org/kali-security kali/updates main contrib non-free

#激进源，新手不推荐使用这个软件源
deb http://repo.kali.org/kali kali-bleeding-edge main
deb-src http://repo.kali.org/kali kali-bleeding-edge main

#中科大kali源
deb http://mirrors.ustc.edu.cn/kali kali main non-free contrib
deb-src http://mirrors.ustc.edu.cn/kali kali main non-free contrib
deb http://mirrors.ustc.edu.cn/kali-security kali/updates main contrib non-free

#阿里云kali源
deb http://mirrors.aliyun.com/kali kali main non-free contrib
deb-src http://mirrors.aliyun.com/kali kali main non-free contrib
deb http://mirrors.aliyun.com/kali-security kali/updates main contrib non-free
```

3.安装tools

```
  #apt-get update                              
  #apt-get install open-vm-tools 
```

到此， 然后你就会发现你的tools工具已经安装完毕了。

*备注*：如果到现在还是不能把主机的文件拖到kail下的话，执行

     #**apt-get remove open-vm-tools**     

然后再去安装虚拟机提供的vmtools

```
   #cp VMwareTools-5.5.1-19175.tar.gz /tmp 
   #cd /tmp
   #tar zxpf VMwareTools-5.5.1-19175.tar.gz
   #cd vmware-tools-distrib 
   #./vmware-install.pl
```

 4. 重启虚拟机就可以了，就可以把文件拖到虚拟机了




