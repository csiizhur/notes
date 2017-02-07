#linux会用到的操作

* ssh登陆切换会话

```shell
bogon:~ zhurun$ ssh root@121.42.8.84
root@121.42.8.84's password: 
Last login: Tue Feb  7 09:48:29 2017 from 112.80.58.198

Welcome to aliyun Elastic Compute Service!
[root@iZ283ujwpe6Z ~]# exit
logout

Connection to 121.42.8.84 closed.
bogon:~ zhurun$
```

* Linux查看版本当前操作系统内核信息

```shell
[root@iZ28j6jspynZ git-2.3.0]# uname -a
Linux iZ28j6jspynZ 2.6.18-371.11.1.el5 #1 SMP Wed Jul 23 15:12:55 EDT 2014 x86_64 x86_64 x86_64 GNU/Linux
```

* Linux查看当前操作系统版本信息

```shell
[root@iZ28j6jspynZ git-2.3.0]# cat /proc/version
Linux version 2.6.18-371.11.1.el5 (mockbuild@builder17.centos.org) (gcc version 4.1.2 20080704 (Red Hat 4.1.2-54)) #1 SMP Wed Jul 23 15:12:55 EDT 2014
```

* Linux查看版本当前操作系统发行版信息

```shell
[root@iZ28j6jspynZ git-2.3.0]# cat /etc/issue
Aliyun Linux release 5.7
Kernel \r on an \m
[root@iZ28j6jspynZ git-2.3.0]# cat /etc/redhat-release
CentOS release 5.11 (Final)
[root@iZ28j6jspynZ git-2.3.0]# 
```

* yum源相关命令
	1. 使用YUM查找软件包
	命令：yum search 

	2. 列出所有可安装的软件包 
	命令：yum list 

	3. 列出所有可更新的软件包 
	命令：yum list updates 

	4. 列出所有已安装的软件包 
	命令：yum list installed
 
	5. 列出所有已安装但不在 Yum Repository 内的软件包 
	命令：yum list extras
 
	6. 列出所指定的软件包 
	命令：yum list
 
	7. 使用YUM获取软件包信息 
	命令：yum info
 
	8. 列出所有软件包的信息 
	命令：yum info
 
	9. 列出所有可更新的软件包信息 
	命令：yum info updates
 
	10. 列出所有已安装的软件包信息 
	命令：yum info installed
 
	11. 列出所有已安装但不在 Yum Repository 内的软件包信息 
	命令：yum info extras
 
	12. 列出软件包提供哪些文件 
	命令：yum provides
	13. 列出所有已安装的软件包
	[root@iZ28j6jspynZ node-v6.9.4]# yum list installed

* 一个文件过大，我们想清空文件内容而不删掉该文件

```shell
[root@iZ283ujwpe6Z logs]# cat /dev/null > catalina.out
```

* 查看磁盘空间大小

显示格式：文件系统 容量 已用 可用 已用% 挂载点
```shell
[root@iZ283ujwpe6Z logs]# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/hda1              20G   16G  4.4G  78% /
tmpfs                 7.9G     0  7.9G   0% /dev/shm
/dev/xvdb1            985G  6.0G  929G   1% /mnt/data
```
* 列出目录结构(以树状图列出目录的内容)

```shell
[root@iZ28j6jspynZ ~]# yum search tree
[root@iZ28j6jspynZ ~]# yum install tree      //安装tree命令
```
```shell
[root@iZ28j6jspynZ vue2.x-douban]# tree -L 1  //只显示第一层目录
.
|-- README.md
|-- build
|-- config
|-- docs
|-- git.sh
|-- index.html
|-- node_modules
|-- package.json
|-- src
|-- static
`-- tree.md
```
```shell
[root@iZ28j6jspynZ vue2.x-douban]# tree -d   //只显示目录
```

* 查看服务监听端口，PID状态。

	1. -t (tcp)仅显示tcp相关选项
	2. -n 拒绝显示别名，能显示数字的全部转化成数字。就是以IP地址显示出来
	3. -l 仅列出有在 Listen (监听) 的服务状态
	4. -p 显示建立相关链接的程序名  就是pid

```shell
[root@iZ283ujwpe6Z logs]# netstat -ntlp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address               Foreign Address             State       PID/Program name   
tcp        0      0 127.0.0.1:8100              0.0.0.0:*                   LISTEN      2513/java           
tcp        0      0 127.0.0.1:8101              0.0.0.0:*                   LISTEN      2422/java           
tcp        0      0 127.0.0.1:9000              0.0.0.0:*                   LISTEN      2031/php-fpm        
tcp        0      0 0.0.0.0:8110                0.0.0.0:*                   LISTEN      2513/java           
tcp        0      0 0.0.0.0:8911                0.0.0.0:*                   LISTEN      2422/java           
tcp        0      0 0.0.0.0:80                  0.0.0.0:*                   LISTEN      1999/nginx          
tcp        0      0 0.0.0.0:8080                0.0.0.0:*                   LISTEN      1901/java           
tcp        0      0 0.0.0.0:22                  0.0.0.0:*                   LISTEN      1793/sshd           
tcp        0      0 0.0.0.0:8088                0.0.0.0:*                   LISTEN      1999/nginx          
tcp        0      0 0.0.0.0:8089                0.0.0.0:*                   LISTEN      1901/java           
tcp        0      0 0.0.0.0:443                 0.0.0.0:*                   LISTEN      1999/nginx          
tcp        0      0 0.0.0.0:8094                0.0.0.0:*                   LISTEN      2513/java           
tcp        0      0 0.0.0.0:8895                0.0.0.0:*                   LISTEN      2422/java           
tcp        0      0 127.0.0.1:8095              0.0.0.0:*                   LISTEN      1901/java
```
