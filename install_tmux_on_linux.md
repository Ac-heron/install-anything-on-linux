# 在linux上安装tmux

## 源码安装
### libevent安装:

tmux要依赖libevent，在安装tmux之前，要先安装libevent：

  - libevent官网：http://libevent.org/
  - libevent下载: sudo wget https://github.com/libevent/libevent/releases/download/release-2.0.22-stable/libevent-2.0.22-stable.tar.gz
  - 解压libevent：sudo tar -zxvf libevent-2.0.22-stable.tar.gz
  - 进入解压的目录：cd libevent-2.0.22-stable
  - 运行libevent：sudo ./configure
  - 编译：
    - sudo make
    - sudo make install

 ### tmux安装:
  - 官网：https://tmux.github.io/
  - 下载：sudo wget https://github.com/tmux/tmux/releases/download/2.2/tmux-2.2.tar.gz
  - 解压：sudo tar -zxvf tmux-2.2.tar.gz
  - 进入解压目录：cd tmux-2.2
  - 运行configure：./configure
  - 编译：

    - sudo make
    - sudo make install

  - 启动：tmux

## apt-get 方式安装：

  - 安装：sudo apt-get instal tmux

## tmux的基本使用：

  - http://www.jianshu.com/p/9e489824a627
  - http://mingxinglai.com/cn/2012/09/tmux/
  - http://cenalulu.github.io/linux/tmux/
  - https://tmuxcheatsheet.com/

