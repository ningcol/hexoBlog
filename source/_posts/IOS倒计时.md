title: ios倒计时

date: 2015-08-01 14:41:02

categories: iOS

tags:

- UIButton
- 实用技巧

------

就是一个简单的倒计时，重要的是注意倒计时按钮的类型一定要设置成custom，不然按钮上面的文字会闪！！！

```
 __block int timeout= 60; //倒计时时间  

   dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);  
   dispatch_source_t _timer = dispatch_source_create(DISPATCH_SOURCE_TYPE_TIMER, 0, 0,queue);  
   dispatch_source_set_timer(_timer,dispatch_walltime(NULL, 0),1.0*NSEC_PER_SEC, 0); //每秒执行  
   dispatch_source_set_event_handler(_timer, ^{  
       if(timeout<=0){ //倒计时结束，关闭  
           dispatch_source_cancel(_timer);  
           dispatch_async(dispatch_get_main_queue(), ^{  
               //设置界面的按钮显示 根据自己需求设置  
               [self.codeBtn setTitle:@"获取验证码" forState:UIControlStateNormal];  
               [self.codeBtn setTitleColor:RGBColor(53, 154, 243, 1) forState:UIControlStateNormal];  
               self.codeBtn.userInteractionEnabled = YES;  

           });  
       }else{  
           int seconds = timeout % 60;  
           if (seconds ==00) {  
               seconds = 60;  
           }  
           NSString *strTime = [NSString stringWithFormat:@"%.2d", seconds];  
           dispatch_async(dispatch_get_main_queue(), ^{  
               //设置界面的按钮显示 根据自己需求设置  
               [UIView beginAnimations:nil context:nil];  
               [UIView setAnimationDuration:1];  

               [self.codeBtn setTitle:[NSString stringWithFormat:@"%@s后重新获取",strTime] forState:UIControlStateNormal];  
               [self.codeBtn setTitleColor:[UIColor grayColor] forState:UIControlStateNormal];  
               [UIView commitAnimations];  
               self.codeBtn.userInteractionEnabled = NO;  

           });  

           timeout--;  
       }  
   });  
   dispatch_resume(_timer);  
```