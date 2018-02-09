title: 自定义navigation左边返回按钮

date: 2015-12-27 15:11:33

categories: iOS

tags:

- UIBarButtonItem
- 返回按钮

------

我们在开发的时候，会遇到自定义navigation左边返回按钮的情况，系统自带的button图片看上去比较大，或者有时候我们会换种图片上去，具体实现细节请看下面代码

```
//自定义返回按钮
-(void)configureLeftBtn{
    UIButton *leftBtn = [UIButton buttonWithType:UIButtonTypeCustom];
    [leftBtn setImage:[UIImage imageNamed:@"btn_icon_cancel_normal"] forState:UIControlStateNormal];
    leftBtn.frame = CGRectMake(0, 0, 55/2, 38/2);
    UIBarButtonItem *leftBtnItem = [[UIBarButtonItem alloc] initWithCustomView:leftBtn];
    [leftBtn addTarget:self action:@selector(clickLeftBtn) forControlEvents:UIControlEventTouchUpInside];

    self.navigationItem.leftBarButtonItem = leftBtnItem;

}

//设置按钮点击返回事件
-(void)clickLeftBtn{
    [self.navigationController popToRootViewControllerAnimated:YES];
}
```