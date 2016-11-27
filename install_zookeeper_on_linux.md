
在linux上安装zookeeper

## 安装
- zookeeper官网：http://zookeeper.apache.org/
- 下载地址：http://www.apache.org/dyn/closer.cgi/zookeeper/
- 下载：wget http://mirrors.cnnic.cn/apache/zookeeper/zookeeper-3.4.9/zookeeper-3.4.9.tar.gz
- 移动到安装目录：mv zookeeper-3.4.9.tar.gz /usr/local/program
- 解压：tar -zxvf zookeeper-3.4.9.tar.gz
- 进入配置文件目录：cd zookeeper-3.4.9/conf

可见此目录下有一个`zoo_sample.cfg`文件，zookeeper启动时会默认找`zoo.cfg`作为配置文件,所以将其复制一份，重命名为`zoo.cfg`:
- cp zoo_sample.cfg zoo.cfg
- 打开zoo.cfg：vim zoo.cfg

zookeeper可以单机模式和集群模式两种方式来配置：

## 单机模式

```
tickTime=2000
dataDir=/tmp/zookeeper
clientPort=2181
```

  - tickTime:这个时间是作为`Zookeeper`服务器之间或客户端与服务器之间维持心跳的时间间隔，也就是每个`tickTime`时间就会发送一个心跳。`ZooKeeper`中其他的时间定于都是基于这个时间单元的。例如：最小的session失效时间是tickTime的2倍。
  - dataDir: Zookeeper 保存数据的目录，默认情况下，Zookeeper 将写数据的日志文件也保存在这个目录里。
  - clientPort:这个端口就是客户端连接 Zookeeper 服务器的端口，Zookeeper 会监听这个端口，接受客户端的访问请求。

- 启动服务：bin/zkServer.sh start

```
start
Keeper JMX enabled by default
-cp zoo_sample.cfg zoo.cfg                                            │Using config: /usr/local/program/zookeeper-3.4.9/bin/../conf/zoo.cfg
-打开zoo.cfg：vim zoo.cfg                                             │Starting zookeeper ... STARTED 
```

`zooKeeper`使用log4j来记录消息日志。根据log4j的配置，日志信息会显示在控制台和/或日志文件中。
单机版的ZooKeeper服务器通常用于开发和测试环境。由于没有复制，因此如果ZooKeeper进程失效了，整个服务就会失效。这在生产环境中通常无法满足可用性的要求。在生产环境中，我们通常使用复制的方式创建ZooKeeper服务器。


- 客户端连接: bin/zkCli.sh -server 127.0.0.1:2181
- 登录之后，输入help：
  ```
      [zkshell: 0] help
    ZooKeeper host:port cmd args
            get path [watch]
            ls path [watch]
            set path data [version]
            delquota [-n|-b] path
            quit
            printwatches on|off
            create path data acl
            stat path [watch]
            listquota path
            history
            setAcl path acl
            getAcl path
            sync path
            redo cmdno
            addauth scheme auth
            delete path [version]
            deleteall path
            setquota -n|-b val path
  ```

- 查看节点：

  ```
  [zk: 127.0.0.1:2181(CONNECTED) 0] ls /
  [zookeeper]
  ```

- 创建新节点，并关联到字符串`my_data`:

  ```
  [zk: 127.0.0.1:2181(CONNECTED) 1] create /zk_test my_data
  Created /zk_test
  ```

- 运行查看节点命令查看效果: ls /

  ```
  [zk: 127.0.0.1:2181(CONNECTED) 0] ls /
  [zookeeper, zk_test]
  ```

- 查看节点数据：get /zk_test

  ```
    [zk: 127.0.0.1:2181(CONNECTED) 1]  get /zk_test
    my_data
    cZxid = 0x4
    ctime = Wed Oct 07 21:14:51 CST 2015
    mZxid = 0x4
    mtime = Wed Oct 07 21:14:51 CST 2015
    pZxid = 0x4
    cversion = 0
    dataVersion = 0
    aclVersion = 0
    ephemeralOwner = 0x0
    dataLength = 7
    numChildren = 0
  ```

- 更新数据：set /zk_test junk

  ```
    [zk: 127.0.0.1:2181(CONNECTED) 2] set /zk_test junk
    cZxid = 0x4
    ctime = Wed Oct 07 21:14:51 CST 2015
    mZxid = 0x7
    mtime = Wed Oct 07 21:17:42 CST 2015
    pZxid = 0x4
    cversion = 0
    dataVersion = 1
    aclVersion = 0
    ephemeralOwner = 0x0
    dataLength = 4
    numChildren = 0
    [zk: 127.0.0.1:2181(CONNECTED) 3]  get /zk_test   
    junk
    cZxid = 0x4
    ctime = Wed Oct 07 21:14:51 CST 2015
    mZxid = 0x7
    mtime = Wed Oct 07 21:17:42 CST 2015
    pZxid = 0x4
    cversion = 0
    dataVersion = 1
    aclVersion = 0
    ephemeralOwner = 0x0
    dataLength = 4
    numChildren = 0
  ```

- 删除节点：delete /zk_test

  ```
  [zk: 127.0.0.1:2181(CONNECTED) 4] delete /zk_test
  [zk: 127.0.0.1:2181(CONNECTED) 5] ls /
  [zookeeper]
  ```


## 集群模式

zookeeper不仅可以单机提供服务，同时也支持多机组成集群来提供服务。实际上zookeeper还支持另外一种伪集群的方式，也就是可以在一台物理机上运行多个 Zookeeper 实例，zookeeper的伪分布式搭建，就是在同一台机器上的不同端口，启动多个zookeeper Server实例，并通过多个配置文件来实现Server之间的通信。


keeper 的集群模式的安装和配置也不是很复杂，所要做的就是增加几个配置项。集群模式除了上面的三个配置项还要增加下面几个配置项：

```
 initLimit=5 
 syncLimit=2 
 server.1=192.168.211.1:2888:3888 
 server.2=192.168.211.2:2888:3888
```

   - initLimit：这个配置项是用来配置 Zookeeper 接受客户端（这里所说的客户端不是用户连接 Zookeeper 服务器的客户端，而是 Zookeeper 服务器集群中连接到 Leader 的 Follower 服务器）初始化连接时最长能忍受多少个心跳时间间隔数。当已经超过 10 个心跳的时间（也就是 tickTime）长度后 Zookeeper 服务器还没有收到客户端的返回信息，那么表明这个客户端连接失败。总的时间长度就是 5*2000=10 秒
   - syncLimit：这个配置项标识 Leader 与 Follower 之间发送消息，请求和应答时间长度，最长不能超过多少个 tickTime 的时间长度，总的时间长度就是 2*2000=4 秒
   - server.A=B：C：D：其中 A 是一个数字，表示这个是第几号服务器；B 是这个服务器的 ip 地址；C 表示的是这个服务器与集群中的 Leader 服务器交换信息的端口；D 表示的是万一集群中的 Leader 服务器挂了，需要一个端口来重新进行选举，选出一个新的 Leader，而这个端口就是用来执行选举时服务器相互通信的端口。如果是伪集群的配置方式，由于 B 都是一样，所以不同的 Zookeeper 实例通信端口号不能一样，所以要给它们分配不同的端口号。

除了修改 zoo.cfg 配置文件，集群模式下还要配置一个文件 myid，这个文件在 dataDir 目录下，这个文件里面就有一个数据就是 A 的值，Zookeeper 启动时会读取这个文件，拿到里面的数据与 zoo.cfg 里面的配置信息比较从而判断到底是那个 server。



### 集群环境搭建

- 在zookeeper目录下新建`pseudo_conf`目录:mkdir $ZOOKEEPER_HOME/pseudo_conf
- 在pseudo_conf下建如下目录和文件,注意myid为文件，其中只填入数字1/2/3,见下面创建脚本：

  ```
  ├── zk1
│   ├── data
│   │   └── myid
│   └── zk1.cfg
├── zk2
│   ├── data
│   │   └── myid
│   └── zk2.cfg
└── zk3
    ├── data
    │   └── myid
    └── zk3.cfg
  ```

- 上述创建脚本：
  ```
    mkdir $ZOOKEEPER_HOME/pseudo_conf
    mkdir $ZOOKEEPER_HOME/pseudo_conf/zk1
    mkdir $ZOOKEEPER_HOME/pseudo_conf/zk2
    mkdir $ZOOKEEPER_HOME/pseudo_conf/zk3
    mkdir $ZOOKEEPER_HOME/pseudo_conf/zk1/data
    mkdir $ZOOKEEPER_HOME/pseudo_conf/zk2/data
    mkdir $ZOOKEEPER_HOME/pseudo_conf/zk3/data
    touch $ZOOKEEPER_HOME/pseudo_conf/zk1/data/myid
    touch $ZOOKEEPER_HOME/pseudo_conf/zk2/data/myid
    touch $ZOOKEEPER_HOME/pseudo_conf/zk3/data/myid
    echo 1 > $ZOOKEEPER_HOME/pseudo_conf/zk1/data/myid
    echo 2 > $ZOOKEEPER_HOME/pseudo_conf/zk2/data/myid
    echo 3 > $ZOOKEEPER_HOME/pseudo_conf/zk3/data/myid
    touch $ZOOKEEPER_HOME/pseudo_conf/zk1/zk1.cfg
    touch $ZOOKEEPER_HOME/pseudo_conf/zk2/zk2.cfg
    touch $ZOOKEEPER_HOME/pseudo_conf/zk3/zk3.cfg
  ```

- zk1.cfg:
  ```
    Time=2000
    initLimit=10
    syncLimit=5
    dataDir=./data
    clientPort=2181
    server.1=127.0.0.1:2222:2223
    server.2=127.0.0.1:3333:3334
    server.3=127.0.0.1:4444:4445
```

- zk2.cfg:
  ```
    tickTime=2000
    initLimit=10
    syncLimit=5
    dataDir=./data
    clientPort=2183
    server.1=127.0.0.1:2222:2223
    server.2=127.0.0.1:3333:3334
    server.3=127.0.0.1:4444:4445
  ```

- 启动zk1：
  ```
    cd KEEPER_HOME/pseudo_conf/zk1
    #第一个参数值可以使start/stop  第二个参数指的是配置文件的配置
    /usr/local/zookeeper-3.4.9/bin/zkServer.sh start ./zk1.cfg
  ```

因为设置了集群中有3台机器，现在只启动了其中一台，所以Zookeeper目前还是不可以用的，
查看当前目录下的zookeeper.out文件，可以看到zk1尝试连接其他的zookeeper server，但是由于其他的都没启动，所以连接失败。


- 启动zk2：
  ```
   cd ZOOKEEPER_HOME/pseudo_conf/zk2
   #第一个参数值可以使start/stop  第二个参数指的是配置文件的配置
   /usr/local/zookeeper-3.4.9/bin/zkServer.sh start ./zk2.cfg
  ```

查看zk2的启动日志，说明zk1是follower，虽然我们只启动了2个zk实例，但是由于已经超过集群总数的一半(3个当中启动了2个)，所以此时服务已经可以用使用了。

此时我们要使用zkCli.sh去连接ZK服务。虽然zk3还没有启动，但是服务现在已经可以使用的，所以我们可以去连接，在zk3没有启动的情况下，去连接ZK服务，可以帮助我们更好的了解zookeeper的一些特点。
  ```
 ${ZOOKEEPER_HOME}/bin/zkCli.sh -server 127.0.0.1:2181,127.0.0.1:2182,127.0.0.1:2183
  ```

启动zkCli时，控制台打印出类似以下内容：
```
Socket connection established, initiating session, client: /127.0.0.1:42939, server: localhost/127.0.0.1:2182
```

说明当前连接的是zk2，如果停止zkCli，再启动zkCli，重复几次这样的操作。我们会看到客户端连接连接的端口号在zk1(2181)和zk2(2182)之间切换，如果连接了zk3(2183)，就会提示连接失败信息，并自动尝试去连接其他的server。




