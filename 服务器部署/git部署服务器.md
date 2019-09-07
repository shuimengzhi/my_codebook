# git_server_book #
git部署服务器，git来提交代码

# 服务端 #
## 1.服务器先创建用户和用户组.这个用户组和用户名是专门git用的 ##
## 2.到git用户下创建公私钥 ##
```
ssh-keygen
```
创建authorized_keys文件并把要链接的电脑的公钥复制到里面
~/.ssh/authorized_keys

 创建一个裸的git仓库里面不浏览文件只是用来存放git记录用的。再创建一个文件仓库 

   创建裸仓
	 
  ```
mkdir **.git
cd **.git
git init --bare
  ```
到另一个放文件的地方
``` 
mkdir ***
cd ***
git clone git仓库地址 比如git clone git@www.github.com:/home/.git/
```
 进入裸仓

```
cd hook
vim post-receive
```
文件内容如下
 ```
 #!/bin/sh

# 打印输出
echo '======上传代码到服务器======'
# 打开线上项目文件夹
cd /web/www/gd168
# 这个很重要，如果不取消的话将不能在cd的路径上进行git操作
unset GIT_DIR
git pull origin master
# 自动编译vue项目
# npm run build
echo $(date) >> hook.log
echo '======代码更新完成======'
 ```
``` 
chmod 777 -R 一个裸仓 一个文件的地址
```
## 客户端
```
git clone git@www.***.com:/home/.git
```

