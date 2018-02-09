title:  Markdown语法简介 

date: 2015-08-3 14:41:08

categories: Markdown

tags:

- Markdown

------

Markdown 是一种用来写作的轻量级「标记语言」，它用简洁的语法代替排版，而不像一般我们用的字处理软件 *Word* 或 *Pages* 有大量的排版、字体设置。它使我们专心于码字，用「标记」语法，来代替常见的排版格式。其不追求大而全，简洁至上，正所谓不求最贵，只求最好！

本文介绍Markdown基本语法，内容很少，一行语法一行示例，学会后可轻松写出高大上的文档，再也不需要各种编辑器去调文章格式。另外，网上有各平台下的Markdown工具可用，比如mou，ulysses(收费)，还有我自己的用的Typora，这篇文章就是用Typora写出来的，我用的是Mac，介绍的这三款都是Mac平台的。

-----

```
星号和下划线
**这是mac**
__markdown简介__
*当你老了
回顾一生*
_就会发觉_
~~什么时候出国读书~~    
```

**这是mac**

__markdown简介__

*当你老了*

*回顾一生*

_就会发觉_

~~什么时候出国读书~~



```
三个或更多"-"，可以组成分隔线
---
```

---



```
引用
> 什么时候决定做第一份职业
```

> 什么时候决定做第一份职业



```
>何时选定对象而恋爱
 >>什么时候结婚
```

> 何时选定对象而恋爱  


>>什么时候结婚



还有一级标题，二级标题，三级标题

```
注意中级还有空格
# 其实都是命运的巨变
## 只是当时站在三岔路口
### 还以为是生命中普通的一天
```

# 其实都是命运的巨变

## 只是当时站在三岔路口

### 还以为是生命中普通的一天



```
无序列表
- 杀鹌鹑的少女
- 杀鹌鹑的少女
- 陶杰
- 陶杰
+ 哈哈
+ 嘿嘿
```

- 杀鹌鹑的少女
- 杀鹌鹑的少女
- 陶杰
- 陶杰
- 哈哈
- 嘿嘿


```
超链接
[百度](http://www.baidu.com)
```

[百度](http://www.baidu.com)

```
图片
![这是test图片](/img/test.png)
```

 ![这是test图片](http://odqosxg6n.bkt.clouddn.com/test.png)



```
代码框
`
UITextField *pwdLabel = [[UITextField alloc] initWithFrame:CGRectMake(90+i*40, 100, 30, 30)];
        pwdLabel.layer.borderColor = [UIColor blackColor].CGColor;
        pwdLabel.enabled = NO;
        pwdLabel.textAlignment = NSTextAlignmentCenter;//居中
        pwdLabel.secureTextEntry = YES;//设置密码模式
        pwdLabel.layer.borderWidth = 1;
        [self.view addSubview:pwdLabel];
`
```

`UITextField *pwdLabel = [[UITextField alloc] initWithFrame:CGRectMake(90+i*40, 100, 30, 30)];        pwdLabel.layer.borderColor = [UIColor blackColor].CGColor;        pwdLabel.enabled = NO;        pwdLabel.textAlignment = NSTextAlignmentCenter;//居中        pwdLabel.secureTextEntry = YES;//设置密码模式        pwdLabel.layer.borderWidth = 1;        [self.view addSubview:pwdLabel];`

