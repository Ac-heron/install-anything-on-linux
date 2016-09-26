# 在linux上安装svn

- 官网：http://subversion.apache.org/


## apt-get 安装
- 安装命令：sudo apt-get install subversion

    svn的安装要依赖其它包，如果没有安装，运行上面命令后会提示没有安装的包：`libjpeg62`,`libapr1`,`libsvn1`,依次用apt-get命令安装这些包后，再运行上面的安装命令安装。

- 安装成功后查看安装版本：svn --version
- 查看svn的帮助：svn --help


- 
- - 下载：sudo wget http://mirrors.cnnic.cn/apache/subversion/subversion-1.9.4.tar.gz
- 解压：sudo tar -zxvf subversion-1.9.4.tar.gz
- 进入目录：cd subversion-1.9.4
- 运行配置：./configure



