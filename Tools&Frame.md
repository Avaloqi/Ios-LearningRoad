## Xcode中的`Preprocessor Macros`的使用
可以使用配置来增加渠道、测试环境切换. 
参考： https://blog.csdn.net/zWbKingGo/article/details/106427562  
      https://www.jianshu.com/p/f00e2899c155
使用步骤：
1.在TARGETS位置右键项目进行Duplicate，新增了一个“projectname copy” target和projecname copy-info 文件  
2.修改”projectname copy“为“projectnameTest”，target和info都要同步修改，对应的projectnameTest->Build Settings->Packaging位置的Info.plist File也要修改  
![image](https://user-images.githubusercontent.com/51845254/173317366-35d41c16-a705-4985-a3b3-e7c7cd78a39d.png)  
3.在Build Settings里搜索Preprocessing，修改Apple Clang->Preprocessing->Preprocessing Macros->Debug，双击Debug，点击打开的框里的加号添加TESTMODE=1. 
![image](https://user-images.githubusercontent.com/51845254/173317996-32828931-8bc4-459b-8e06-fa6f11478bd2.png). 
4.在Build Settings->Swift Compiler->Custom Flags->Other Swift Flags 点击Debug，添加 -D TESTMODE. 
5.代码中进行预定义，在Scheme切换版本即可自动切换环境  
![image](https://user-images.githubusercontent.com/51845254/173318483-1e78094a-8ec2-40bb-8cd2-fdebab749589.png). 

## Cocodpod使用
参考：https://blog.csdn.net/qq_27597629/article/details/98743873

cd 到工程目录下，创建Podfile文件（vim Podfile）或者通过命令初始化一个：（创建 Podfile，只需键入以下命令：pod init）
按以下形式编写

    platform :ios, '15.0'
    target 'TestMoya' do
	      pod 'Moya', '~> 15.0'
    end
    
 安装第三方库
 
    pod install
