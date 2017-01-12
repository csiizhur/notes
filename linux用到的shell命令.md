#linux会用到的操作
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