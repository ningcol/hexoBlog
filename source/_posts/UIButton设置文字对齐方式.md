title: UIButton设置文字对齐方式

date: 2016-03-21 17:01:33

categories: iOS

tags:

- UIButton
- 实用技巧

------

我们开发的时候，用下面的代码设置按钮文字右对齐发现没有卵用

```
//获取验证码按钮
    UIButton *codeBtn = [UIButton buttonWithType:UIButtonTypeCustom];
    codeBtn.width = 100;
    codeBtn.height = 60;
    [codeBtn setTitle:@"获取验证码" forState:UIControlStateNormal];
   //设置对齐方式
    codeBtn.titleLabel.textAlignment = NSTextAlignmentRight;
```
其实上面的最后一行代码是没有效果的，这只是让标签文本对齐，但是有没有改变标签在按钮中的对齐方式，这里我们应该用

```
codeBtn.contentHorizontalAlignment = UIControlContentHorizontalAlignmentRight;
```