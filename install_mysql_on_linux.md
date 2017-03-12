# 在linux上安装mysql


## apt-get 安装

-  参考资料：http://wiki.ubuntu.org.cn/MySQL

-  安装 ：mysql数据库分服务器和客户端，服务器用于管理和维护数据库，客户端用于连接和访问数据库，可以用下列命令安装服务器和客户端。安装期间将会提示输入数据库管理员root的密码:

     ```
     sudo apt-get install mysql-server
     ```
   - mysql的配置文件是/etc/mysql/my.cnf,主要用于配置数据库文件的存储位置，日志文件等，修改my.cnf必须重启mysql

   - 安装完成后，mysql应该已经启动，可以用如下命令检查mysql是否运行：sudo netstat -tap | grep mysql

   - 如果没有启动，则可以用命令启动： sudo /etc/init.d/mysql restart

   - 使用mysql：进入mysql命令环境，使用如下命令，-u后跟用户名，-p后跟密码，没有则为空：mysql -uroot -proot

## mysql常用命令

- 查看版本信息： mysql -V
- 启动mysqld服务: sudo /etc/init.d/mysql start
   - 停止mysqld服务: sudo /etc/init.d/mysql stop
   - 启动mysqld服务: sudo /etc/init.d/mysql restart
   - SELECT VERSION(); MYSQL 版本
   - SELECT NOW(); : 显示当前时间
   - SELECT USER(); : 查看当前用户

   - mysql数据库安装后的根目录在：/etc/mysql/

   - 查看警告信息：show WARNINGS;
   - 创建数据库： CREATE DATABASE IF NOT EXISTS TEST1 CHARACTER SET UTF8;
   - 查看数据库详情： SHOW CREATE DATABASE TEST1;
   - 修改数据库编码：ALTER DATABASE TEST1 CHARACTER SET UTF8;
   - 删除数据库： DROP DATABASE IF EXISTS TEST1;
   - 显示数据库：show databases;
   - 使用：use test;

## 设置用户及访问权限

- mysql的用户和密码都存在一个专用的数据库mysql中，管理员root可以在其中添加用户及赋权，命令如下：

```
GRANT ALL PRIVILEGES ON database TO username@"servername" IDENTIFIED  BY 'password';
```

- 如果flush配置变量没有设为ON，或者启动mysql进程时没有使用“--flush”选项，需要使用flush命令，才能使添加的帐户的生效。

```
flush privileges
```

## SQL脚本与批处理
- 可以将多条命令写入一个文件，以I/O的方式运行mysql，批量执行命令，命令如下 ：

```
mysql -u username -p [password] < scriptfile
```

- 例如新建一个文件：test.sql,内容如下：

```
show databases;
use test;
show tables;
select * from user;
```

- 利用命令执行,则将会显示执行结果：

```
mysql -u root -p < test.sql;
```

- 如果是在mysql交互环境下，则可用“source” 或 “ \ . ” 命令运行sql脚本：

```
source test.sql;

或

\. test.sql
```

## 安装navicat客户端

- 下载：https://www.navicat.com.cn/download/navicat-for-mysql
- 解压：sudo tar -zxvf navicat112_mysql_cs_x86.tar.gz
   - 进入解压目录 ：cd navicat112_mysql_cs_x86
   - 运行：./start_navicat**

## 卸载mysql

-   删除mysql：

    - sudo apt-get remove mysql-server
    - sudo apt-get autoremove mysql-server
    - sudo apt-get remove mysql-common

-   清理残留数据：

      ```
      dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
      ```

## 源码安装（centos为例）

- 官网下载：http://dev.mysql.com/downloads/mysql/
- Mysql 5.6 下载：wget http://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.29.tar.gz （大小：31 M）

   - Mysql 5.7 下载：wget http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.11.tar.gz （大小：47 M）
   - 解压：tar -zxvf mysql-5.6.29.tar.gz
   - 进入解压目录 ：cd mysql-5.6.29
   - 生成安装目录 ：mkdir -p /usr/local/mysql/data
   - 安装依赖包、编译包（centos）：`yum install -y make gcc-c++ cmake bison-devel ncurses-devel`
   - 在编译时，需要用到cmake，如果没有安装，要先安装cmake命令及cmake依赖包
   - 运行cmake命令，生成配置：

 ```
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DMYSQL_DATADIR=/usr/local/mysql/data \
-DSYSCONFDIR=/etc \
-DMYSQL_USER=mysql \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_READLINE=1 \
-DMYSQL_UNIX_ADDR=/usr/local/mysql/data/mysql.sock \
-DMYSQL_TCP_PORT=3306 \
-DENABLED_LOCAL_INFILE=1 \
-DENABLED_DOWNLOADS=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DEXTRA_CHARSETS=all \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_DEBUG=0 \
-DMYSQL_MAINTAINER_MODE=1 \
-DWITH_SSL:STRING=bundled \
-DWITH_ZLIB:STRING=bundled \
-DDOWNLOAD_BOOST=1 \
-DWITH_BOOST=/usr/share/doc/boost-doc-1.41.0/
 ```



- make命令编译（此过程较长）：make

- 安装：make install

- 配置开机启动：

  `cp /mysql-5.6.19/support-files/mysql.server /etc/init.d/mysql`

  `chmod 755 /etc/init.d/mysql`

  `chkconfig mysql on`

- 复制一份配置文件： `cp /support-files/my-default.cnf /etc/my.cnf`

- 添加组和用户及安装目录权限

  添加组：groupadd mysql

  创建用户mysql并加入到mysql组，不允许mysql用户直接登录系统：useradd -g mysql mysql -s /bin/false

  设置MySQL数据库目录权限：chown -R mysql:mysql /usr/program/mysql/data

- 初始化数据库：`/usr/local/mysql/scripts/mysql_install_db --basedir=/usr/mysql --datadir=/usr/mysql/data --skip-name-resolve --user=mysql`

- 禁用 selinux编辑配置文件：`vim /etc/selinux/config`把 `SELINUX=enforcing` 改为 `SELINUX=disabled`

- 常用命令软连接，才可以在终端直接使用：mysql 和 mysqladmin 命令

  ln -s /usr/mysql/bin/mysql /usr/bin 

  ln -s /usr/mysql/bin/mysqladmin /usr/bin

  ln -s /usr/mysql/bin/mysqldump /usr/bin

  ln -s /usr/mysql/bin/mysqlslap /usr/bin





# 在Mac上安装mysql

- 下载：https://dev.mysql.com/downloads/mysql/

   我下载的是**Mac OS X 10.12 (x86, 64-bit), DMG Archive**

- 双击运行安装

- 安装过程中，mysql会给root设置一个随机的密码，记住它。

- 安装完成后，启动

- 用上面的密码登录：/usr/local/mysql/bin mysql -uroot -p

- 登录后，雪行任何命令都会报如下错误：

  ```
  You must reset your password using ALTER USER statement before executing this statement.
  ```

- 修改密码：mysql> SET PASSWORD = PASSWORD("新密码");

### mac卸载mysql：

```
mysql 终端卸载：
1 sudo rm /usr/local/mysql
2 sudo rm -rf /usr/local/mysql*
3 sudo rm -rf /Library/StartupItems/MySQLCOM
4 sudo rm -rf /Library/PreferencePanes/My*
5 vim /etc/hostconfig  (and removed the line MYSQLCOM=-YES-)
6 rm -rf ~/Library/PreferencePanes/My*
7 sudo rm -rf /Library/Receipts/mysql*
8 sudo rm -rf /Library/Receipts/MySQL*
9 sudo rm -rf /var/db/receipts/com.mysql.*
```

