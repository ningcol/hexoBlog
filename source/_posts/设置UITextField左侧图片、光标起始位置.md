title: 设置UITextField左侧图片、光标起始位置

date: 2016-03-15 20:31:02

categories: iOS

tags:

- UITextField
- 开发技巧

------

一、我们通常在设计登录界面的时候会用到UITextField，如下图所示

 ![](http://odqosxg6n.bkt.clouddn.com/UITextField%E8%AE%BE%E7%BD%AE%E5%9B%BE%E7%89%87%E3%80%81%E5%85%89%E6%A0%87%E4%BD%8D%E7%BD%AE-1.png)

1.左边显示图片

2.Textfield中间添加提示文字，并且希望能与UITextField左右有一些间距

二、左边显示图片可以把ImageView加到TextField里，其实可以有更简单的方法：

```
 UIImageView *passwordImage = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"passwordIcon"]];
 password.leftView = passwordImage;
 password.leftViewMode = UITextFieldViewModeAlways;
```

三、设置占位符的位置和编辑状态时的位置试了很多方法感觉都不好用，最终找到了一个方法，自定义一个TextField继承至UITextField，然后重写下面的几个方法：

```
- (CGRect)borderRectForBounds:(CGRect)bounds;
- (CGRect)textRectForBounds:(CGRect)bounds;
- (CGRect)placeholderRectForBounds:(CGRect)bounds;
- (CGRect)editingRectForBounds:(CGRect)bounds;
- (CGRect)clearButtonRectForBounds:(CGRect)bounds;
- (CGRect)leftViewRectForBounds:(CGRect)bounds;
- (CGRect)rightViewRectForBounds:(CGRect)bounds;
```

四、下面是具体的代码实现：

```
#import <UIKit/UIKit.h>

@interface ZLoginTextField : UITextField

@end
```

```
#import "ZLoginTextField.h"
#import <objc/runtime.h>


@implementation ZLoginTextField


- (instancetype)init
{
    self = [super init];
    if (self) {

        self.backgroundColor = [UIColor whiteColor];
        self.tintColor = [UIColor lightGrayColor];
        self.layer.cornerRadius = 2;
        self.layer.borderWidth = 0.6;
        self.layer.masksToBounds = YES;
        self.layer.borderColor = [UIColor lightGrayColor].CGColor;
        self.font = [UIFont systemFontOfSize:12];

    }
    return self;
}

//leftView偏移量
- (CGRect)leftViewRectForBounds:(CGRect)bounds{
    CGRect iconRect = [super leftViewRectForBounds:bounds];
    iconRect.origin.x += 10;
    return iconRect;
}

//  重写占位符的x值
- (CGRect)placeholderRectForBounds:(CGRect)bounds{
    CGRect placeholderRect = [super placeholderRectForBounds:bounds];
    placeholderRect.origin.x += 1;
    return placeholderRect;
}

//  重写文字输入时的X值
- (CGRect)editingRectForBounds:(CGRect)bounds{
    CGRect editingRect = [super editingRectForBounds:bounds];
    editingRect.origin.x += 10;
    return editingRect;
}

//  重写文字显示时的X值
- (CGRect)textRectForBounds:(CGRect)bounds{
    CGRect textRect = [super editingRectForBounds:bounds];
    textRect.origin.x += 10;
    return textRect;
}

//rightView偏移量
-(CGRect)rightViewRectForBounds:(CGRect)bounds{
    CGRect textRect = [super rightViewRectForBounds:bounds];
    textRect.origin.x -= 10;
    return textRect;
}

@end

```

具体的间隔，偏移量自己多设置几个值，调调试试就知道是个怎么回事了