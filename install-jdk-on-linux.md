# 在linux上安装JDK

- 查看是否安装： java -version
- 官网地址： http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
- 下载JDK: wget http://download.oracle.com/otn-pub/java/jdk/8u101-b13/jdk-8u101-linux-i586.tar.gz
- 解压： tar -zxvf jdk-8u60-linux-i586.tar.gz
- 配置环境变量： vim /etc/profile

  ```
  JAVA_HOME=/usr/local/program/jdk1.8.0_72
  JRE_HOME=$JAVA_HOME/jre
  PATH=$PATH:$JAVA_HOME/bin
  CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
  export JAVA_HOME
  export JRE_HOME
  export PATH
  export CLASSPATH
  ```
- 生效： source /etc/profile
- 注意：

如果切换su root时提示Authentication failure：
 ```
  $ su  root
  Password:
  su: Authentication failure
  Sorry.
  ```
则如下操作那可:
  ```
  $ sudo passwd root
  Enter new UNIX password:
  Retype new UNIX password:
  passwd: password updated successfully
  ```