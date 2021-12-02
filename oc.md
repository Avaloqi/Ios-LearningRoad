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
            
![image](https://user-images.githubusercontent.com/51845254/144391336-21aedafb-49dd-4a15-9939-07c89c1aa524.png)

            
解决方法很简单，（在 @interface 文件中继承它的代理即可）如下:
            
![image](https://user-images.githubusercontent.com/51845254/144391295-1b7228a9-957c-4bbd-89ff-d9032178086d.png)

这种情况可见于在.h中使用@protocol ***delegate类似的方式导入代理
            
或者导入了相关框架，如#import "CoreBluetooth/CoreBluetooth.h" ，但没有在@interface ***ViewController () <>的<>里加入代理
            
            
