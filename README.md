# Ios-LearningRoad
ios/OC/Swift学习笔记

[Swift](https://github.com/Avaloqi/Ios-LearningRoad/blob/main/Swift.md)

[OC](https://github.com/Avaloqi/Ios-LearningRoad/blob/main/oc.md)

[Tools&Frame](https://github.com/Avaloqi/Ios-LearningRoad/blob/main/Tools%26Frame.md)

## Xcode

### 项目添加文件
  对项目添加已有文件时需要注意操作，直接将文件拉到xcode打开的项目某目录下，此时项目中可以读到，但项目文件夹下无该文件，此时是项目对文件建立了reference
  直接在访达中将文件拉到项目目录下，xcode无法读取到
  
  理想方案：在访达中 拷贝/复制的副本转移 到项目目录下，再打开项目，在xcode中将文件拉到指定目录下

## 版本控制

### SVN

参考：https://blog.csdn.net/leleyuan1130/article/details/56016561

参考：https://www.pianshen.com/article/77731478987/

### SVN 2 Git

git svn clone http://xxx.xxx.xx.xxx/project/ --no-metadata --authors-file=users.txt GitProject

cd GitProject

git remote add origin git@xxx.xxx.xx.xxx:Group/GitProject.git

git remote -v

关联出错时 git remote rm origin

git remote set-url origin http://name@xxx.xxx.xx.xxx:Group/GitProject.git

git push -u origin master
