# 搭建prometheus+grafana监控系统



## 基本概念

grafana可视化数据，数据从哪里来，从prometheus取，prometheus怎么取，prometheus 从各个端点取



## 搭建grafana

1.下载地址：https://grafana.com/grafana/download?pg=get&plcmt=selfmanaged-box1-cta1

官网有各种下载方式，推荐下载独立包

```
wget https://dl.grafana.com/enterprise/release/grafana-enterprise-8.5.4.linux-amd64.tar.gz
tar -zxvf grafana-enterprise-8.5.4.linux-amd64.tar.gz
```

2.解压

```bash
tar -zxvf grafana-enterprise-8.5.4.linux-amd64.tar.gz
```

3.启动

```bash
./bin/grafana-server web
```

4.启动成功，http://localhost:3000/，登录admin/admin





## 搭建prometheus 

文档：https://prometheus.io/docs/introduction/first_steps/

1.下载：https://prometheus.io/download/

2.解压：tar zxvf prometheus-2.36.0.linux-amd64.tar.gz

3.启动：./prometheus --config.file=prometheus.yml

4.访问:http://localhost:9090,等30秒收集自己的信息，看端点：http://localhost:9090/targets

5.真正使用：监控服务器：在服务器上安装node-exporter：

​	5.1安装node-exporter:https://prometheus.io/docs/guides/node-exporter/

```
wget https://github.com/prometheus/node_exporter/releases/download/v*/node_exporter-*.*-amd64.tar.gz
tar xvfz node_exporter-*.*-amd64.tar.gz
cd node_exporter-*.*-amd64
./node_exporter
```

node_exporter端口默认9100

5.2怎么把数据传到prometheus，修改prometheus配置文件prometheus.yml，新增配置：

```
- job_name: node
  static_configs:
  - targets: ['192.168.1.1:9100']
```

5.3.重启prometheus，看http://localhost:9090/targets?search=，能看到数据收集到了，怎么接入grafana可视化

6.grafana 里新增数据源：prometheus

7.grafana导入dashboard，可以在这里搜索，https://grafana.com/grafana/dashboards/，比如node_exporter中文版本：https://grafana.com/grafana/dashboards/8919，输入8919，导入保存成功即可看到监控面板。





## 安装其他监控，步骤同上

### 安装redis监控

**3.3.1 下载redis_exporter**：https://github.com/oliver006/redis_exporter

官网上没有redis_exporter, 可以从github上获取，另外redis插件无需放在redis机器上也可以

```
/**  下载  */
wget https://github.com/oliver006/redis_exporter/releases/download/v0.30.0/redis_exporter-v0.30.0.linux-amd64.tar.gz
/**  解压  */
tar -zxvf  redis_exporter-v0.30.0.linux-amd64.tar.gz
```

**3.3.2 启动redis_exporter**



```
/**  redis无密码 */
nohup  ./redis_exporter -redis.addr=192.168.56.118:6379 -web.listen-address 0.0.0.0:9121  &

/**  redis有密码  */
nohup  ./redis_exporter -redis.addr=192.168.56.118:6479 -redis.password 123456   -web.listen-address 0.0.0.0:9122 & 

/**   
 -web.listen-address  可以自定义监控端口
*/
```

redis_exporter启动成功后，其他步骤同上面，现在prometheus.yml配置job，然后grafana里搜索一个好用的redis仪表盘显示即可：

比如：https://grafana.com/grafana/dashboards/11835





### 安装postGresql监控

参考：https://www.cnblogs.com/ilifeilong/p/10543876.html

1.下载postgres_exporter：https://github.com/wrouesnel/postgres_exporter/releases

2.启动

```
export DATA_SOURCE_NAME="postgresql://user:password@192.168.2.2:27981/database?sslmode=disable"
./postgres_exporter &
```

3.其他同上。dashboard可以用https://grafana.com/grafana/dashboards/9628





## 用Prometheus+Grafana监控Springboot应用

参考：

https://zhuanlan.zhihu.com/p/106036485

https://cloud.tencent.com/developer/article/1863250

1.pom.xml

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>

<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
    <version>1.1.4</version>
</dependency>
```



2.application.properties

```
spring.application.name=springboot2demo
# 打开所有 Actuator 服务
management.endpoints.web.exposure.include=*
# 将应用名称添加到计量器的 tag 中去
# 以便 Prometheus 根据应用名区分不同服务
management.metrics.tags.application=${spring.application.name}
```

3.启动类application.java,在启动类中添加Bean，用于监控JVM性能指标：

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Springboot2demoApplication.class, args);
    }

    @Bean
    MeterRegistryCustomizer<MeterRegistry> configurer(
            @Value("${spring.application.name}") String applicationName) {
        return (registry) -> registry.config().commonTags("application", applicationName);
    }
}
```

4.启动成功，查看监控端点：localhost:8080/actuator/prometheus

5.配置prometheus.yml

```yaml
- job_name: 'springboot_app'
    scrape_interval: 5s
    metrics_path: '/actuator/prometheus'
    static_configs:
      - targets: ['192.168.31.6:8080']
  
```

6.整合到grafana，原理同上





参考资料

https://www.prometheus.wang/quickstart/why-monitor.html

https://cloud.tencent.com/developer/article/1595811