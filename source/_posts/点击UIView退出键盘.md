title: 点击UIView退出键盘

date: 2016-03-14 10:41:02

categories: iOS

tags:

- ios键盘
- 手势

------

一个简单的手势退出键盘，当我们有时候使用UITextField的时候，键盘可能会把输入框遮住，具体看实现代码：

```
 _/**
 *  设置手势隐藏键盘
 */
-(void)setUpkeyboardHide{
    //退出键盘
    UITapGestureRecognizer *tapGestureRecognizer = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(keyboardHide:)];
    //设置成NO表示当前控件响应后会传播到其他控件上，默认为YES。
    tapGestureRecognizer.cancelsTouchesInView = NO;
    //将触摸事件添加到当前view
    [self.view addGestureRecognizer:tapGestureRecognizer];
    
}

-(void)keyboardHide:(UITapGestureRecognizer*)tap{
    
    [self.phoneField resignFirstResponder];
    [self.codeField resignFirstResponder];
    [self.pwdField resignFirstResponder];
    
}
```

注意这里的cancelsTouchesInView，大家有可以去网上查查相关资料。