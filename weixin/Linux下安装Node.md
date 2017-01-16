# Linux下安装Node
## 通过下载源码编译安装
1. 下载Node源码包
```shell
[root@iZ28j6jspynZ mnt]# wget http://nodejs.org/dist/v0.11.7/node-v0.11.7.tar.gz
--2017-01-16 16:07:10--  http://nodejs.org/dist/v0.11.7/node-v0.11.7.tar.gz
Resolving nodejs.org... 104.20.22.46, 104.20.23.46, 2400:cb00:2048:1::6814:172e, ...
Connecting to nodejs.org|104.20.22.46|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14885051 (14M) [application/gzip]
Saving to: `node-v0.11.7.tar.gz'

100%[====================================================================================================================================================================================================================================>] 14,885,051   219K/s   in 86s     

2017-01-16 16:08:41 (168 KB/s) - `node-v0.11.7.tar.gz' saved [14885051/14885051]

[root@iZ28j6jspynZ mnt]# 
```
2. 解压缩
```shell
[root@iZ28j6jspynZ mnt]# tar -xzf node-v0.11.7.tar.gz
```
3. 编译安装
```shell
[root@iZ28j6jspynZ node-v6.9.4]# ./configure
[root@iZ28j6jspynZ node-v6.9.4]# make && make install
[root@iZ28j6jspynZ node-v6.9.4]# cp /usr/local/bin/node /usr/sbin/
```

这种通过源码编译安装发现安装过程中会出现各种错误。下面采用编译好的文件安装

## 通过编译好的文件安装

1. 下载nodejs安装包
```shell
[root@iZ28j6jspynZ ~]# wget https://nodejs.org/dist/v6.9.4/node-v6.9.4-linux-x64.tar.xz
```
2. 解压并安装
xz文件格式文件需先解压成tar（yun源安装xz）
```shell
[root@iZ28j6jspynZ ~]# yum search xz
[root@iZ28j6jspynZ ~]# yum install xz.x86_64
```

这样就可以使用xz命令来解压tar.xz格式文件了
```shell
[root@iZ28j6jspynZ mnt]# xz -d node-v6.9.4-linux-x64.tar.xz
[root@iZ28j6jspynZ mnt]#tar -xvf node-v6.9.4-linux-x64.tar
```
3. 解压完毕直接通过./node -v查看是否成功
```shell
[root@iZ28j6jspynZ mnt]# cd node-v6.9.4-linux-x64/bin/
[root@iZ28j6jspynZ bin]# ls
node  npm
[root@iZ28j6jspynZ bin]# ./node -v
v6.9.4
[root@iZ28j6jspynZ bin]# 
```
4. node全局设置

```shell
[root@iZ28j6jspynZ bin]# ln -s /mnt/node-v6.9.4-linux-x64/bin/node /usr/local/bin/node
[root@iZ28j6jspynZ bin]# ln -s /mnt/node-v6.9.4-linux-x64/bin/npm /usr/local/bin/npm
[root@iZ28j6jspynZ bin]# cd /root
[root@iZ28j6jspynZ ~]# node -v
v6.9.4
```
