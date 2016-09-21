# 在linux上安装maven


   - 官网：http://maven.apache.org/download.cgi

   - 下载： wget http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz

   - 解压：tar -zxvf apache-maven-3.3.9-bin.tar.gz

   - 环境变量设置：vim /etc/profile

   - 在文件最尾巴添加下面内容：
      
      ```
      # Maven
      MAVEN_HOME=/usr/local/program/apache-maven-3.3.9
      PATH=$PATH:$MAVEN_HOME/bin
      MAVEN_OPTS="-Xms256m -Xmx356m"
      export MAVEN_HOME
      export PATH
      export MAVEN_OPTS
      ```
   - 刷新配置文件：source /etc/profile

   - 测试是否安装成功：mvn -v

