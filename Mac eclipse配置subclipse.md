# mac环境下需要JavaHL
* 安装homebrew在终端输入
```shell
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)";
```
* 安装javahl在终端输入
```shell
brew install --universal --java subversion
```
* 安装完成后，执行下边的两个sudo命令
```shell
sudo mkdir -p /Library/Java/Extensions
sudo ln -s /usr/local/homebrew/lib/libsvnjavahl-1.dylib /Library/Java/Extensions/libsvnjavahl-1.dylib
```
安装完成后会提示安装的javahl的版本是1.9.4
* 安装subclipse。根据javahl的wiki文档http://subclipse.tigris.org/wiki/JavaHL获取对应的版本
1.Eclipse-Help-Install New Software...
2.点击add按钮，name随便输入，Location输入http://subclipse.tigris.org/update_1.12.x
* 设置Eclipse的Preferencess
svn接口：client JavaHL(JNI)1.9.4(r174.329)
