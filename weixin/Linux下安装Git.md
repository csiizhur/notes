# Linux下安装Git
查看当前操作系统发行版本信息
```shell
[root@iZ28j6jspynZ git-2.3.0]# cat /etc/redhat-release
CentOS release 5.11 (Final)
```
CentOS5的时代，由于yum源中没有git，所以需要预先安装一系列的依赖包。但在CentOS6的yum源中已经有git的版本了，可以直接使用yum源进行安装。
```shell
[root@iZ28j6jspynZ git-2.3.0]# yum install git
```

## 下载git源码编译安装
1.更新系统
```shell
[root@iZ28j6jspynZ mnt]# yum update
```
2.安装依赖包
```shell
[root@iZ28j6jspynZ mnt]# sudo yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker
```
3.下载git源码包并解压缩
```shell
[root@iZ28j6jspynZ mnt]# wget https://github.com/git/git/archive/v2.3.0.zip
[root@iZ28j6jspynZ mnt]# unzip v2.3.0.zip
[root@iZ28j6jspynZ mnt]# cd git-2.3.0
```
4.编译安装
```shell
[root@iZ28j6jspynZ mnt]# make prefix=/usr/local/git all
[root@iZ28j6jspynZ mnt]# sudo make prefix=/usr/local/git install
```
5.查看git安装目录
```shell
[root@iZ28j6jspynZ mnt]# whereis git
```
6.git环境变量配置
```shell
[root@iZ28j6jspynZ mnt]# sudo vim /etc/profile
export PATH=/usr/local/git/bin:$PATH
```
7.使用source命令应用修改
```shell
[root@iZ28j6jspynZ mnt]# source /etc/profile
```
8.查看安装git的版本
```shell
[root@iZ28j6jspynZ mnt]# git --version
```
## 使用git bash删除远程仓库分支
```shell
git push origin :HEAD  //推送一个空分支到远程分支，其实就相当于删除远程分支
```
