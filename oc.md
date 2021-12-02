# Objective-c 
## 监听键盘
            NSNotificationCenter *nCenter = NSNotificationCenter.defaultCenter;

            [nCenter addObserver:self selector:@selector(adjustForKeyBoardHide) name:UIKeyboardWillHideNotification object:nil];

            [nCenter addObserver:self selector:@selector(adjustForKeyBoard) name:UIKeyboardWillChangeFrameNotification object:nil];

## 自定义模式的UIBarbuttonItem
            UIBarButtonItem *ScanQrButton = [[UIBarButtonItem alloc] initWithImage:[UIImage systemImageNamed: @"qrcode.viewfinder"] style:UIBarButtonItemStylePlain target:self action:@selector(StartScan)];

## 延迟1s之后执行


            dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
                        // do stuff
            });
    
## 唤起一个UIAlertController窗口
            UIAlertController *ac = [UIAlertController alertControllerWithTitle:@"Connected" message:nil preferredStyle:UIAlertControllerStyleAlert];

            UIAlertAction *defalutAction = [UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler: ^(UIAlertAction *action) {
            [self performSegueWithIdentifier:@"showBlueT" sender:nil];}
            ];
    
            [ac addAction: defalutAction];
            [self presentViewController:ac animated:YES completion:nil];
