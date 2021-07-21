# 1.使用命令行将现有项目添加到 GitHub

[官方教程](https://docs.github.com/en/github/importing-your-projects-to-github/importing-source-code-to-github/adding-an-existing-project-to-github-using-the-command-line)

1. 在github上新建一个和本地项目一样名称的仓库

2. 打开终端，进入本地项目
	初始化：
   
   ```java
   git init
	```
	添加文件
	```java
	git add .
	git commit -m "First commit"
	```
	链接远程仓库,输入用户名密码
	```java
	git remote add origin https://github.com/willian-li/pathfollowingwithobstacleavoidance.git
	```
	上传到master分支
	```java
	git push origin master
	```
	
3. 设置主分支为master
	[https://www.cnblogs.com/dongyaotou/p/14130089.html](https://www.cnblogs.com/dongyaotou/p/14130089.html)

4. 删除main分支
	[https://blog.csdn.net/weixin_30646315/article/details/97449384](https://blog.csdn.net/weixin_30646315/article/details/97449384)

# 2.github本地切换分支

[https://www.cnblogs.com/u-1596086/p/11328374.html](https://www.cnblogs.com/u-1596086/p/11328374.html)

1. 查看分支
	```java
	git branch -a
	```
2. 切换分支
	```java
	git checkout master
	```

# 3.提交文件到仓库

[官方教程](https://docs.github.com/en/github/managing-files-in-a-repository/managing-files-using-the-command-line/adding-a-file-to-a-repository-using-the-command-line)
1. 添加
	```java
	git add .   
	git commit -m “first commit”    
	```
2. 更新本地仓库（远程做了文件修改要执行这步）
	```java
	git pull origin master
	```
3. 再上传
	```java
	git push -u origin master   
	```



### **Quick setup** — if you’ve done this kind of thing before

**or**

HTTPSSSH



Get started by [creating a new file](https://github.com/willian-li/md/new/main) or [uploading an existing file](https://github.com/willian-li/md/upload). We recommend every repository include a [README](https://github.com/willian-li/md/new/main?readme=1), [LICENSE](https://github.com/willian-li/md/new/main?filename=LICENSE.md), and [.gitignore](https://github.com/willian-li/md/new/main?filename=.gitignore).

### …or create a new repository on the command line



```
echo "# md" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/willian-li/md.git
git push -u origin main
```

### …or push an existing repository from the command line



```
git remote add origin https://github.com/willian-li/md.git
git branch -M main
git push -u origin main
```
