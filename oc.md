# Objective-c 


## - 基本方法记录

### Objective-C中字典的使用方法总结 
https://www.cnblogs.com/xiaomanon/p/5700195.html


### - 按钮（等控件）配置圆角边框
            self.clearButton.layer.cornerRadius = 5;
            self.clearButton.layer.borderColor = [[UIColor blackColor] CGColor];
            self.clearButton.layer.borderWidth = 0.5;
            self.clearButton.layer.masksToBounds = YES;
  
### - 按钮（等控件）配置圆角
            self.setButton.layer.cornerRadius = 5;
            self.setButton.clipsToBounds = YES;

### - 监听键盘

创建通知中心监听相应的键盘动作， 配置selector来进行后续操作

            NSNotificationCenter *nCenter = NSNotificationCenter.defaultCenter;

            [nCenter addObserver:self selector:@selector(adjustForKeyBoardHide) name:UIKeyboardWillHideNotification object:nil];

            [nCenter addObserver:self selector:@selector(adjustForKeyBoard) name:UIKeyboardWillChangeFrameNotification object:nil];
            
### - 获取键盘高度
https://www.jianshu.com/p/dd761d0b4966


### - 自定义模式的UIBarbuttonItem
            UIBarButtonItem *ScanQrButton = [[UIBarButtonItem alloc] initWithImage:[UIImage systemImageNamed: @"qrcode.viewfinder"] style:UIBarButtonItemStylePlain target:self action:@selector(StartScan)];

### - 延迟1s之后执行

            dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
                        // do stuff
            });
    
    
### - 唤起一个UIAlertController窗口
            UIAlertController *ac = [UIAlertController alertControllerWithTitle:@"Connected" message:nil preferredStyle:UIAlertControllerStyleAlert];

            UIAlertAction *defalutAction = [UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler: ^(UIAlertAction *action) {
            [self performSegueWithIdentifier:@"showBlueT" sender:nil];}
            ];
    
            [ac addAction: defalutAction];
            [self presentViewController:ac animated:YES completion:nil];
            
            
### - 根据条件调用segue的行动

![image](https://user-images.githubusercontent.com/51845254/144395790-b55c8513-ac82-43cd-af9f-f2610f312f2e.png)

使用故事版时，从这个位置拉出segue连接到其他页面，设置segue的identifier，在需要时用下面的命令调用

            [self performSegueWithIdentifier:@"showBlueT" sender:nil];
            

### - UITextField shouldChangeCharactersInRange 获取真实数据

https://www.jianshu.com/p/0999a59eaef6

在这个方法UITextField shouldChangeCharactersInRange里面，调用：

      NSString * str = [textField.text stringByReplacingCharactersInRange:range withString:string];
      

### - iOS开发之将字典、数组转为JSON字符串方法
https://www.cnblogs.com/hecanlin/p/10752986.html


### - 关于cell中textfield值回传的问题
参考 https://www.jianshu.com/p/bfd1df44af46

使用场景在collection cell里有textfield的情况， 传一个字典和要对应修改的键key进去， 在cell里修改的位置进行setValue即可

            - (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath {
                        parameterCoCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"ParameterCoCell" forIndexPath:indexPath];
                        cell.data = self.lastDic;
                        cell.paraName = paramName;
在cell文件里增加接受位

            @property (nonatomic, strong) NSMutableDictionary *data;
            @property (nonatomic, strong) NSString *paraName;

 在修改的位置更改字典对应键的值
 
            - (void)textFieldDidEndEditing:(UITextField *)textField {
           
                        if ([textField.text isEqualToString:self.lastTF]) {
                                    self.layer.borderColor = nil;
                        } else {
                                    self.layer.borderColor = UIColor.redColor.CGColor;
                                    [self.data setValue:textField.text forKey:self.paraName];
                        }
    
            }




## 警告消除⚠️

### Assigning to "id<CALayerDelegate> _Nullable" from incompatible type "ZXCapture *const __strong" 的警告提示信息
该警告提示信息，是说，设置了代理对象，但是并没有继承它的代理
            
![image](https://user-images.githubusercontent.com/51845254/144391336-21aedafb-49dd-4a15-9939-07c89c1aa524.png)

            
解决方法很简单，（在 @interface 文件中继承它的代理即可）如下:
            
![image](https://user-images.githubusercontent.com/51845254/144391295-1b7228a9-957c-4bbd-89ff-d9032178086d.png)

这种情况可见于在.h中使用@protocol ***delegate类似的方式导入代理
            
或者导入了相关框架，如#import "CoreBluetooth/CoreBluetooth.h" ，但没有在@interface ***ViewController () <>的<>里加入代理
            
            
