# 在linux上安装nexus

> 以前记录过在windows下安装使用nexus的步骤，放在这里，linux安装相差无几

## 安装Nexus

### 下载Nexus
下载地址：http://www.sonatype.org/nexus/archived/
有两种压缩方式可供下载，`tar.gz`和`zip`,我下载的是`nexus-2.8.0-05-bundle.zip`

### 启动Nexus
cmd管理员身份进入Nexus\bin目录，运行如下命令，请确保JDK版本1.7以上。
``` 
cd d:\javatools\nexus-2.8.0-05\bin
nexus install
nexus start
```
启动成功，访问地址：http://localhost:8081/nexus，进入nexus登录页面，默认帐户是`admin/admin123`。

### 端口更改
Nexus的默认端口是8081，可在`nexus-2.8.0-05\conf\nexus.properties`文件里修改`application-port`属性。

### 将nexus安装成windows服务
运行\nexus-2.8.0-05\bin\jsw\windows-x86-64下的`install-nexus.bat`即可。

## 使用Nexus

### 添加代理仓库
点击菜单栏上的Add按钮后选择Proxy Repository，主要填写`Group ID`，`Group Name` 、`remote storage location`
以oschina为例，如图：

！[ppp](http://www.iacheron.com/xxx.png)

> 
maven:http://www.mvnrepository.com/
jboss:https://repository.jboss.org/
oschina: http://maven.oschina.net/content/groups/public/

### 仓库组
一般来说，maven中使用的都是都是仓库组的路径，在`Public Respositories`的配置界面，可以选择nexus中的仓库，将其添加到仓库组中，仓库组所包含的仓库顺序决定了仓库组遍历其所包含的次序，因为一般将常用的仓库放在前面。

### 手动部署第三方构件
某些文件如oracle的jdbc文件，需要手动部署，选择`3rd party`，然后点击Artifact Upload，如果该构件是maven构建的，则在`GAV Definition`下拉列表中选择`FROM POM`，否则选`GAV Parameters`，点击`select Artifacts to Upload`上传构件。

### 开启远程索引
 nexus默认是关闭远程索引下载功能的。开启的方式：将以下三个仓库`Apache Snapshots`，`Codehaus Snapshots`，`Central`的`Download Remote` Indexes修改为true。然后在这三个仓库上分别右键，选择Re-index，这样Nexus就会去下载远程的索引文件。

### 常用maven命令
```
clean compile -U               更新
clean deploy                   部署
clean package                  打包

mvn install -D maven.test.skip=true 跳过测试 
```


