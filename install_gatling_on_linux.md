# install gatling 

- download:http://gatling.io/download/
- unzip: unzip gatling-charts-highcharts-bundle-2.3.0.zip

###  创建测试脚本
可以用bin/recorder.sh来创建，也可以直接在/user-files/simulations下手动创建


两个例子：
- 1. 测试baidu
```
import io.gatling.core.Predef._
import io.gatling.http.Predef._
import scala.concurrent.duration._

class BaiduSimulation extends Simulation {
  //设置请求的根路径
  val httpConf = http.baseURL("https://www.baidu.com")
  /*
    运行100秒 during 默认单位秒,如果要用微秒 during(100 millisecond)
   */
  val scn = scenario("BaiduSimulation").during(100){
    exec(http("baidu_home").get("/"))
  }
  //设置线程数
  //setUp(scn.inject(rampUsers(500) over(10 seconds)).protocols(httpConf))
  setUp(
    scn.inject(atOnceUsers(10)).protocols(httpConf))
}
```
- 2. DoctorAppSimulation
```
import io.gatling.core.Predef._
import io.gatling.http.Predef._
import scala.concurrent.duration._

class DoctorAppSimulation extends Simulation {
  //设置请求http路径
  val httpConf = http.baseURL("http://192.168.1.201:8484")


  // 设置header值
  val headers = Map(
        "Authorization" -> "Acheron:ZG9jdG9yX2tleV9hbmRyb2lk:57IejgyCSkcv3v6Ncwm8IGQ7BSs=",
        "X-Acheron-Date" -> "1867836000000",
        "X-Acheron-UserId" -> "910700000023052360",
        "X-Acheron-SessionId" -> "GJX=JDKd23ksHD",
        "X-Acheron-Version" -> "Android/1.9.0"
    )


  // 定义Scenario
  val scn = scenario("DoctorAppSimulation").during(100){
    // 生成二维码接口
    exec(http("qrcode")
        .get("/qrcode/generate?zhigongbh=1213")
    )
    .pause(5)
    // 今日医声列表接口
    .exec(http("doctor_sound/records")
        .get("/doctor_sound/records?groupId=910700000050545061")
        .headers(headers)
    )
  }
  //设置线程数
  setUp(scn.inject(atOnceUsers(10)).protocols(httpConf))
}
```

### 运行测试
./bin/gatling.sh

### 查看结果
/results/**/index.html

参考：
- http://gatling.io/docs/current/quickstart/
- https://testerhome.com/topics/3633
