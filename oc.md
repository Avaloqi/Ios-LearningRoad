# Objective-c 


## 基本方法记录

### 监听键盘
            NSNotificationCenter *nCenter = NSNotificationCenter.defaultCenter;

            [nCenter addObserver:self selector:@selector(adjustForKeyBoardHide) name:UIKeyboardWillHideNotification object:nil];

            [nCenter addObserver:self selector:@selector(adjustForKeyBoard) name:UIKeyboardWillChangeFrameNotification object:nil];

### 自定义模式的UIBarbuttonItem
            UIBarButtonItem *ScanQrButton = [[UIBarButtonItem alloc] initWithImage:[UIImage systemImageNamed: @"qrcode.viewfinder"] style:UIBarButtonItemStylePlain target:self action:@selector(StartScan)];

### 延迟1s之后执行


            dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
                        // do stuff
            });
    
### 唤起一个UIAlertController窗口
            UIAlertController *ac = [UIAlertController alertControllerWithTitle:@"Connected" message:nil preferredStyle:UIAlertControllerStyleAlert];

            UIAlertAction *defalutAction = [UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler: ^(UIAlertAction *action) {
            [self performSegueWithIdentifier:@"showBlueT" sender:nil];}
            ];
    
            [ac addAction: defalutAction];
            [self presentViewController:ac animated:YES completion:nil];

## 警告消除⚠️

### Assigning to "id<CALayerDelegate> _Nullable" from incompatible type "ZXCapture *const __strong" 的警告提示信息
该警告提示信息，是说，设置了代理对象，但是并没有继承它的代理
            ![image](https://user-images.githubusercontent.com/51845254/144391069-6acb0e10-30d8-4be9-b380-4d5d94f8743e.png)
解决方法，很简单，（在 @interface 文件中继承它的代理即可）如下
            ![image](https://user-images.githubusercontent.com/51845254/144391125-914664ec-7f62-42b8-b943-c17881de2f66.png)

            
            
