# Svn项目转Gitlab使用记录

## 参考资料
参考：https://www.cnblogs.com/goodwell21/p/10044818.html  
参考：https://stackoverflow.com/questions/11037166/author-not-defined-when-importing-svn-repository-into-git

## 准备
svn项目(svnPro)、地址(https://xxx.xxx.xx.xxx/svn/svnPro)和svn账号(svnName/svnPassword)  
电脑上已经装好git客户端并进行了配置  
电脑上安装git客户端，客户端下载地址：https://www.git-scm.com/download/  
下载后安装客户端，安装及配置参考：https://blog.csdn.net/u013295518/article/details/78746007  
git项目位置(新建一个gitPro项目)和gitlab账号密码  
gitlab新建项目注意：  
登录后可以在menu-projects新建一个个人项目，或者在menu-groups新建一个群组，在群组内新建项目，看情况使用  
地址：https://xxx.xxx.xx.xxx/gitPro  
账号：gitName 密码： gitPassword  

svn项目的用户清单 users.txt  
users.txt文件内容为:  
user1 = user1 <user1@refromer.com>  
user2 = user2 <user2@refromer.com>  
user3 = user3 <user3@refromer.com>  
user4 = user4 <user4@refromer.com>  

注意:  
= 左边为svn用户名字，右边为git用户名字  
清单内要包括svn项目所有提交过的用户名称  
文件编码格式为utf-8，可以使用nodepad++打开，选择编码->utf8，或者使用vscode打开txt文件，点击右下角的编码格式选择为utf-8  

## 数据迁移
在users.txt目录下新建文件夹gitPro  
右键文件夹空白处，点击Git Bash Here  
bash界面输入命令  
git svn clone https://xxx.xxx.xx.xxx/svn/svnPro/ --no-metadata --authors-file=users.txt gitPro  
成功后会看到gitPro文件夹内已经有了相应svn项目的文件  

此处可能会报错误Author: xxx not defined in users.txt file  
可能的问题和解决办法为  
1. users.txt文件中没有xxx --- 直接打开文件添加这个 xxx = xxx <xxx@refromer.com>
2. users.txt文件编码格式不对 --- 使用nodepad++或vscode调整文件格式
错误后需要删除gitPro文件夹并重新创建一个  

svn库clone成功后cd gitPro  
执行：  
git remote add origin git@xxx.xxx.xx.xxx/gitPro.git   
关联远程git仓库  

使用 git remote -v 查看关联情况  
使用 git remote rm origin 删除关联  

关联成功后提交记录到git  
git push -u origin master  
成功后登录gitlab网站相应库就可以看到代码上传成功  

此处可能碰到问题：remote: The project you were looking for could not be found.  
1. git项目的地址写错了
2. 账号密码不对，这里需要使用能登录到git项目所在服务器的gitlab的账号
3. 电脑上存储有之前的登录凭证，命令输入完后没有提示输入账号密码就直接走下去了，可以在push之前使用命令
   git remote set-url origin http://gitName@xxx.xxx.xx.xxx/gitPro.git
   这里可以在push时指定使用账号gitName访问