# Objective-c 


## 基本方法记录

### - UIIAlertController的自动消失

原文链接：https://blog.csdn.net/xiaoliu_ios/article/details/50563869

<details>
            <summary>code</summary>

        UIAlertController *alert = [UIAlertController alertControllerWithTitle:@"提示" message:@"没有上一部了" preferredStyle:UIAlertControllerStyleAlert];
        [self presentViewController:alert animated:NO completion:nil];
        [NSTimer scheduledTimerWithTimeInterval:0.5 target:self selector:@selector(creatAlert:) userInfo:alert repeats:NO];

        - (void)creatAlert:(NSTimer *)timer{
            UIAlertController *alert = [timer userInfo];
            [alert dismissViewControllerAnimated:YES completion:nil];
            alert = nil;
        }
</details>


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


### - iOS禁止右滑返回页面

需要禁止右滑的controller里面引入代理

            @interface ***controller ()<UIGestureRecognizerDelegate>
            @end
            @implementation ***controller

            - (void)viewDidAppear:(BOOL)animated {
                        [super viewDidAppear:animated];
                        if ([self.navigationController respondsToSelector:@selector(interactivePopGestureRecognizer)]) {
                                    self.navigationController.interactivePopGestureRecognizer.delegate = self;
                        }
            }
            - (void)viewWillDisappear:(BOOL)animated {
            	[super viewWillDisappear:animated];
            	if ([self.navigationController respondsToSelector:@selector(interactivePopGestureRecognizer)]) {
                        	self.navigationController.interactivePopGestureRecognizer.delegate = nil;
                        }
            }
            - (BOOL)gestureRecognizerShouldBegin:(UIGestureRecognizer *)gestureRecognizer {
                        return NO;
            }
            @end

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

## tableViewCell添加button并获取点击事件
参考来自：https://www.jianshu.com/p/b4b4b1ab0e13

在**cell.h中添加button属性，连接故事版中cell原型里的按钮
在cellForRowAt里给按钮指定事件

    - (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
         BarrierCell *cell = [tableView dequeueReusableCellWithIdentifier:@"BarrierCell" forIndexPath: indexPath];
         
        [cell.choice addTarget:self action:@selector(cellBtnClicked:event:) forControlEvents:UIControlEventTouchUpInside];
        return cell;
      }
   
   
    - (void)cellBtnClicked:(id)sender event:(id)event {
            NSSet *touches =[event allTouches];
            UITouch *touch =[touches anyObject];
            CGPoint currentTouchPosition = [touch locationInView:_tableView];
            NSIndexPath *indexPath= [_tableView indexPathForRowAtPoint:currentTouchPosition];
            if (indexPath!= nil) {
            //do staff
            NSLog(@"%zd",indexPath.row);
            }
        }

## 需要注意的ios版本问题

### 导航栏相关属性设置时，ios15的写法与过去有所区别

<details>
<summary>参考资料</summary>

参考资料：https://developer.apple.com/forums/thread/682420

参考来源：https://www.jianshu.com/p/f7dc127ecda9

导航栏标题自定义参考：https://www.jianshu.com/p/609077d85bc8
</details>

升上iOS15后，发现使用系统提供的导航栏滑动时会变透明，navigationBar的barTintColor设置无效。在有UIScrollView的情况下，上划后barTintColor生效，返回时正常。

解决方案：
在 iOS 15 中，UIKit 将 的使用扩展scrollEdgeAppearance到所有导航栏，默认情况下会产生透明背景。背景由滚动视图何时滚动导航栏后面的内容来控制。您的屏幕截图表明您已滚动到顶部，因此导航栏在滚动时选择scrollEdgeAppearance了standardAppearance它，并且在以前版本的 iOS 上。
要恢复老样子，你必须采用新UINavigationBar外观的API， UINavigationBarAppearance。删除您现有的自定义并执行如下操作：

<details>
<summary>Swift写法: </summary>

            let appearance = UINavigationBarAppearance()
            appearance.configureWithOpaqueBackground()
            appearance.backgroundColor = <your tint color>
            navigationBar.standardAppearance = appearance;
            navigationBar.scrollEdgeAppearance = navigationBar.standardAppearance
            
</details>
            
<details>
<summary>Objective-C写法：</summary>
            
        if (@available(iOS 13.0, *)) {
            self.navigationController.navigationBarHidden = NO;
            self.navigationController.navigationBar.tintColor = [UIColor whiteColor];
            UINavigationBarAppearance *appearance = [[UINavigationBarAppearance alloc]init];
            [appearance configureWithOpaqueBackground];
            //导航栏背景颜色（直接显示的颜色）
            appearance.backgroundColor = [UIColor colorWithHexString:@"#3a8bff" alpha:1];
            //导航栏标题颜色（可以自定义标题字号和颜色）
            [appearance setTitleTextAttributes:@{NSForegroundColorAttributeName:[UIColor whiteColor]}];
            self.navigationController.navigationBar.standardAppearance = appearance;
            self.navigationController.navigationBar.scrollEdgeAppearance = self.navigationController.navigationBar.standardAppearance;
        } else {
            self.navigationController.navigationBarHidden = NO;
            self.navigationController.navigationBar.tintColor = [UIColor whiteColor];
            self.navigationController.navigationBar.translucent = NO;  //取消导航栏的毛玻璃效果，处理色差问题
            self.navigationController.navigationBar.barTintColor = [UIColor colorWithHexString:@"#3a8bff" alpha:1];
            }
            
</details>            


## 警告消除⚠️

### - Assigning to "id<CALayerDelegate> _Nullable" from incompatible type "ZXCapture *const __strong" 的警告提示信息
该警告提示信息，是说，设置了代理对象，但是并没有继承它的代理
            
![image](https://user-images.githubusercontent.com/51845254/144391336-21aedafb-49dd-4a15-9939-07c89c1aa524.png)

            
解决方法很简单，（在 @interface 文件中继承它的代理即可）如下:
            
![image](https://user-images.githubusercontent.com/51845254/144391295-1b7228a9-957c-4bbd-89ff-d9032178086d.png)

这种情况可见于在.h中使用@protocol ***delegate类似的方式导入代理
            
或者导入了相关框架，如#import "CoreBluetooth/CoreBluetooth.h" ，但没有在@interface ***ViewController () <>的<>里加入代理
            
            
