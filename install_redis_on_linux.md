# 在linux上安装redis


   - redis(REmote DIctionary Server)官网：http://www.redis.io
   - 下载： wget http://labfile.oss.aliyuncs.com/files0422/redis-2.8.9.tar.gz
   - 解压：tar xvfz redis-2.8.9.tar.gz

   - 进入解压之后的目录,进行编译

    ```
    cd redis-2.8.9
    make
    make install
    ```
   - 编译成功后，可以运行测试：make test
   - 在Redis安装完成后，注意一些重要的文件：
    
    ```
    服务端：src/redis-server
    客户端：src/redis-cls
    默认配置文件：redis.conf
    ```
   - 然后将可执行文件放置在$PATH环境目录下，便于以后执行程序时可以不用输入完整的路径
     
     ```
     cp redis-server /usr/local/bin/
     cp redis-cli /usr/local/bin/
     ```
  
  - 启动Redis-server:redis-server
  - 可以重开一个终端，查看redis进程：ps aux | grep redis
  - 或者检查redis服务器状态： netstat -nlt | grep 6379
  - 启动Redis-client：redis-cli

## redis高级使用
