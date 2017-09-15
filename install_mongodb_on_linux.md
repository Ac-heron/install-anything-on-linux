# 安装MongoDB
- 官网：https://www.mongodb.com/
- 下载地址：https://www.mongodb.com/download-center?jmp=nav#community
- 下载：curl -O https://fastdl.mongodb.org/osx/mongodb-osx-x86_64-3.5.10.tgz
- 解压：tar -zxvf mongodb-osx-x86_64-3.5.10.tgz
- 可以配置环境变量: export PATH=<mongodb-install-directory>/bin:$PATH
- 用sudo创建默认的数据目录：sudo mkdir -p /data/db
   这里的数据储存目录也可以在其它目录，只不过在启动时要加参数dbpath指向你创建的目录 :mongod --dbpath <path to data directory>
- 给/data/db赋权：sudo chown `id -u` /data/db
- 启动：./bin/mongod
  加dbpath的：./bin/mongod --dbpath /Users/acheron/Tmp/mongodb-data
- 命令连接：./bin/mongo
- MongoDb web 用户界面
MongoDB 提供了简单的 HTTP 用户界面。 如果你想启用该功能，需要在启动的时候指定参数 --rest 。
$ ./mongod --dbpath=/data/db --rest
MongoDB 的 Web 界面访问端口比服务的端口多1000。
如果你的MongoDB运行端口使用默认的27017，你可以在端口号为28017访问web用户界面，即地址为：http://localhost:28017。






参考：
- `https://docs.mongodb.com/master/tutorial/install-mongodb-on-os-x/?_ga=2.34471713.1096956177.1505378205-316093734.1505378205`
- `http://www.runoob.com/mongodb/mongodb-linux-install.html`


