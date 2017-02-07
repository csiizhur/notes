# mac环境下需要JavaHL

* 安装homebrew在终端输入(打开HomeBrew的主页：http://brew.sh)
```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
或者      ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
* 安装javahl在终端输入(提示有个包下不到，原因你懂得，被墙了)
```shell
brew install --universal --java subversion
```

```shell
==> make
==> make install
==> make tools
==> make install-tools
==> make javahl
==> make install-javahl
==> Caveats
svntools have been installed to:
  /usr/local/opt/subversion/libexec

You may need to link the Java bindings into the Java Extensions folder:
  sudo mkdir -p /Library/Java/Extensions
  sudo ln -s /usr/local/lib/libsvnjavahl-1.dylib /Library/Java/Extensions/libsvnjavahl-1.dylib

Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/subversion/1.9.5_1: 151 files, 23.4M, built in 11 minutes 10 seconds
```

* 安装完成后，执行下边的两个sudo命令
```shell
sudo mkdir -p /Library/Java/Extensions
sudo ln -s /usr/local/homebrew/lib/libsvnjavahl-1.dylib /Library/Java/Extensions/libsvnjavahl-1.dylib
```
安装完成后会提示安装的javahl的版本是1.9.5
* 安装subclipse。根据javahl的wiki文档http://subclipse.tigris.org/wiki/JavaHL获取对应的版本
1.Eclipse-Help-Install New Software...
2.点击add按钮，name随便输入，Location输入http://subclipse.tigris.org/update_1.12.x
* 设置Eclipse的Preferencess
svn接口：client JavaHL(JNI)1.9.5(r174.329)

# 建立软连接不成功会导致eclipse中找不到JavaHL(http://blog.csdn.net/zhuo212/article/details/51278018)
```shell
   sudo mkdir -p /Library/Java/Extensions
   sudo ln -s /usr/local/lib/libsvnjavahl-1.dylib /Library/Java/Extensions/libsvnjavahl-1.dylib
```
    注意：在这个目录下有这个文件时才能创建软连接成功/usr/local/lib/libsvnjavahl-1.dylib
     (如果没有，可取/usr/local/目录下寻找，一般都在此目录下) 
       建立软链接之后，先去/Library/Java/Extensions目录下找到建立的软连接，右键显示原生，
     如果能够显示成功说明建立软连接成功，才能保证到eclipse中找到javaHL。
    说明：在解决错误，重新运行安装命令时，可能会看到警告(Waring)，不用担心，可以直接忽略。
