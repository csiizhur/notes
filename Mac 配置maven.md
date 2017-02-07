# 国际惯例，在apache官网下载对应的maven版本解压缩
# 配置maven的环境变量

* mac一般使用bash作为默认shell

Mac系统的环境变量，加载顺序为：
/etc/profile /etc/paths ~/.bash_profile ~/.bash_login ~/.profile ~/.bashrc
当然/etc/profile和/etc/paths是系统级别的，系统启动就会加载，后面几个是当前用户级的环境变量。
后面3个按照从前往后的顺序读取，如果~/.bash_profile文件存在，则后面的几个文件就会被忽略不读了，
如果~/.bash_profile文件不存在，才会以此类推读取后面的文件。~/.bashrc没有上述规则，它是bash shell打开的时候载入的。

* 单个用户设置

1. ~/.bash_profile （任意一个文件中添加用户级环境变量）（注：Linux 里面是 .bashrc 而 Mac 是 .bash_profile）

若bash shell是以login方式执行时，才会读取此文件。该文件仅仅执行一次!默认情况下,他设置一些环境变量
设置命令别名alias ll=’ls -la’
设置环境变量：
export PATH=/opt/local/bin:/opt/local/sbin:$PATH

2. ~/.bashrc 同上

如果想立刻生效，则可执行下面的语句：
$ source 相应的文件
一般环境变量更改后，重启后生效。

* 环境变量配置（按 i 就进入插入模式 按 esc就进入命令模式）

```shell
bogon:~ zhurun$ touch ~/.bash_profile
```

```shell
bogon:~ zhurun$ vim ~/.bash_profile
```

```shell
export MAVEN_HOME=/Users/zhurun/Library/apache-maven-3.3.9
export PATH=$PATH:$MAVEN_HOME/bin
```

```shell
bogon:~ zhurun$ source ~/.bash_profile
```

```shell
bogon:~ zhurun$ mvn -v
Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-11T00:41:47+08:00)
Maven home: /Users/zhurun/Library/apache-maven-3.3.9
Java version: 1.8.0_111, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
Default locale: zh_CN, platform encoding: UTF-8
OS name: "mac os x", version: "10.12.3", arch: "x86_64", family: "mac"
```
