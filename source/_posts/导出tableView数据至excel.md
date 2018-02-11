title: 导出tableView数据至excel

date: 2015-08-27 13:41:08

categories: iOS

tags:

- UItableView
- excel
- 导出数据

------

0.开始的时候因为没有找到导出excel格式的文章，倒是找到了导出csv格式的文件的方法，但是由于中文乱码中文问题，问题一直没有找到好的解决方法。这里也稍微介绍一下：

```
- (void)exportCSV:(NSString *)fileName {  


    NSOutputStream *output = [[NSOutputStream alloc] initToFileAtPath:fileName append:YES];  
    [output open];  


    if (![output hasSpaceAvailable]) {  
        NSLog(@"没有足够可用空间");  
    } else {  

        NSString *header = @"学号,姓名\n";   //这里是文件第一行的头（逗号是换列，\n是换行）  

        const uint8_t *headerString = (const uint8_t *)[header cStringUsingEncoding:NSUTF8StringEncoding];  
        NSInteger headerLength = [header lengthOfBytesUsingEncoding:NSUTF8StringEncoding];  
        NSInteger result = [output write:headerString maxLength:headerLength];  
        if (result <= 0) {  
            NSLog(@"写入错误");  
        }  


        NSArray *students = [self queryStudents];  
        for (Student *stu in students) {  

            NSString *row = [NSString stringWithFormat:@"%@,%@\n", stu.num, stu.name];   //循环插入数据<span style="font-family: Arial, Helvetica, sans-serif;">（逗号是换列，\n是换行）</span>  

            const uint8_t *rowString = (const uint8_t *)[row cStringUsingEncoding:NSUTF8StringEncoding];  
            NSInteger rowLength = [row lengthOfBytesUsingEncoding:NSUTF8StringEncoding];  
            result = [output write:rowString maxLength:rowLength];  
            if (result <= 0) {  
                NSLog(@"无法写入内容");  
            }  

        }  

        [output close];  
    }  
}  
```

生产csv文件：

```
NSArray *documents = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDirectory, YES);  
   NSString *docementDir = [documents objectAtIndex:0];  
   self.filePath = [docementDir stringByAppendingPathComponent:@"Student.csv"];  
   NSLog(@"filePath = %@", self.filePath);  


   [self createFile:self.filePath];  
   [self exportCSV:self.filePath];
```

1.现在开始进入正题，要先下载微软的excel库文件[https://www.libxl.com/download.html](https://www.libxl.com/download.html)，解压

2.里面有example，可以参考一下、把LibXL.framework导入自己的项目当中，设置bitcode为no，以及linker也要改为**-lstdc++**

3.上代码，导入头文件，设置代理

```
#include "LibXL/libxl.h" 
```

```
@interface JGdetailController ()<UITableViewDataSource,UITableViewDelegate,JGPeopleViewDelegate,UIDocumentInteractionControllerDelegate>
```

```
-(void)clickBarButton{  
    NSLog(@"createExcel");  

    BookHandle book = xlCreateBook(); // use xlCreateXMLBook() for working with xlsx files  

    SheetHandle sheet = xlBookAddSheet(book, "Sheet1", NULL);  
    //第一个参数代表插入哪个表，第二个是第几行（默认从0开始），第三个是第几列（默认从0开始）  
    xlSheetWriteStr(sheet, 1, 0, "姓名", 0);  
    xlSheetWriteStr(sheet, 1, 1, "性别", 0);  
    xlSheetWriteStr(sheet, 1, 2, "学校", 0);  
    xlSheetWriteStr(sheet, 1, 3, "电话", 0);  


    for (int i = 0; i < self.nameArray.count; i++) {  
        const charchar *name_c = [self.nameArray[i] cStringUsingEncoding:NSUTF8StringEncoding];  //这里是将NSString字符串转为C语言字符串  
        xlSheetWriteStr(sheet, i+2, 0,name_c, 0);  

    }  
    for (int i = 0; i < self.sexArray.count; i++) {  
        const charchar *sex_c = [self.sexArray[i] cStringUsingEncoding:NSUTF8StringEncoding];  
        xlSheetWriteStr(sheet, i+2, 1,sex_c, 0);  

    }  
    for (int i = 0; i < self.schoolArray.count; i++) {  
        const charchar *school_c = [self.schoolArray[i] cStringUsingEncoding:NSUTF8StringEncoding];  
        xlSheetWriteStr(sheet, i+2, 2,school_c, 0);  

    }  
    for (int i = 0; i < self.phoneArray.count; i++) {  
        const charchar *phone_c = [self.phoneArray[i] cStringUsingEncoding:NSUTF8StringEncoding];  
        xlSheetWriteStr(sheet, i+2, 3,phone_c, 0);  

    }  


    NSString *documentPath =  
    [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory,NSUserDomainMask, YES) objectAtIndex:0];  
    NSString *filename = [documentPath stringByAppendingPathComponent:@"out.xls"];  
    NSLog(@"filepath--%@",filename);  

    xlBookSave(book, [filename UTF8String]);  

    xlBookRelease(book);  

    //导出xls文件  
    UIDocumentInteractionController *docu = [UIDocumentInteractionController interactionControllerWithURL:[NSURL fileURLWithPath:filename]];  

    docu.delegate = self;  
    CGRect rect = CGRectMake(0, 0, 320, 300);  //这里感觉没什么用  

    [docu presentOpenInMenuFromRect:rect inView:self.view animated:YES];  //不写可以直接预览  

    [docu presentPreviewAnimated:YES];  //这句比较坑爹。如果不写这句，只写上面那句会弹出选择支持xls文件的APP。  

}  
```

```
//doucumentDelegate方法（必须实现，还有几个展示选择系统自带print还是啥的就不说了，可以自己研究下）  
- ( UIViewController *)documentInteractionControllerViewControllerForPreview:( UIDocumentInteractionController *)interactionController{  

    return self;  

}  
```

4.把模型tableView里面的数据取出来放在数组里：

```
        self.nameArray = [NSMutableArray array];
        self.sexArray = [NSMutableArray array];
        self.schoolArray = [NSMutableArray array];
        self.phoneArray = [NSMutableArray array];
        for (JGdetail *de in self.people) {
            [self.nameArray addObject:de.name];
            if(de.sex == 0){
                [self.sexArray addObject:@"男"];
            }else if(de.sex == 1){
                [self.sexArray addObject:@"女"];
            }
             [self.schoolArray addObject:de.school];
             [self.phoneArray addObject:de.tel];
        }
        NSLog(@"nameArray---%@",self.sexArray);
```

5.这样就可以选择QQ打开生成的xls文件。效果如下，因为这个是要收费的，所以忽略第一行：

![](https://s1.ax1x.com/2018/02/11/9Gyjgg.jpg)