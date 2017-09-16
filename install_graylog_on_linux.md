
# 在linux上安装Graylog

   - 官网： https://www.graylog.org/

   - System requirements
            The Graylog server application has the following prerequisites:

            - Some modern Linux distribution (Debian Linux, Ubuntu Linux, or CentOS recommended)
            - Elasticsearch 2.3.5 or later
            - MongoDB 2.4 or later (latest stable version is recommended)
            - Oracle Java SE 8 or later (OpenJDK 8 also works; latest stable update is recommended)


   - 下载：https://www.graylog.org/releases

   - 解压：

      ```
        ~$ tar xvfz graylog-2.3.1.tgz
        ~$ cd graylog-2.3.1
      ```
    - 拷贝配置文件：cp graylog.conf.example /etc/graylog/server/server.conf

### 必须要修改的配置:
vim /etc/graylog/server/graylog.conf

    - password_secret
    You must set a secret that is used for password encryption and salting here. The server will refuse to start if it’s not set. Generate a secret with for example pwgen -N 1 -s 96. If you run multiple graylog-server nodes, make sure you use the same password_secret for all of them!



### 启动
- 启动：./bin/graylogctl run

- 启动日志见：log/graylog-server.log


http://www.imooc.com/article/13481

http://docs.graylog.org/en/2.3/pages/installation/manual_setup.html
