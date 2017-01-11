#notes
##git修改 提交 上传
1. 加载（Stage）文件 $ git add .
2. 提交文件 $ git commit -m 'update'
3. 推送到远程仓库 $ git push  

##另一种git提交
如果不是git pull获取远程仓库，而使用了上面的命令，是不能将代码提交到远程服务器上去，原因是git在本地也是有代码仓库的！！！
所以，如果我们需要将代码提交到远程服务器上面去，需要先配置部分信息，如下：
```shell
git remote add origin https://github.com/csiizhur/notes.git
```
一般config配置信息如下：
```config
[core]
	repositoryformatversion = 0
	filemode = false
	bare = false
	logallrefupdates = true
	symlinks = false
	ignorecase = true
	hideDotFiles = dotGitOnly
[remote "origin"]
	url = https://github.com/csiizhur/notes.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
```
然后，你想要推送你的本地代码库的主干分支到你的远程代码库，可以使用如下命令：
```shell
git push origin master
```
master是你的分支名称，你也可以修改到其它分支

##Markdown入门指南
http://www.jianshu.com/p/1e402922ee32/
##Git简单操作
http://blog.jobbole.com/53573/
