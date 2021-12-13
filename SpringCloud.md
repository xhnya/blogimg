# SpringCloud

## 微服务概述

![image-20210603192158693](SpringCloud.assets/image-20210603192158693.png)

==SpringCloud是分布式微服务架构的一站式解决方案，是多种微服务架构池技术的集合俗称微服务全家桶==

## 版本选择

![image-20210607145752151](SpringCloud.assets/image-20210607145752151.png)

GA代表稳定版本

![image-20210607145815612](SpringCloud.assets/image-20210607145815612.png)

springCloud里面有对应的boot版本

可以通过https://start.spring.io/actuator/info来得到具体消息

把JSON格式化https://tool.lu/

![image-20210607145954277](SpringCloud.assets/image-20210607145954277.png)

就可以得到具体的消息

## 工程创建

### 父工程创建

创建一个maven工程

删除src文件

修改pom文件

```xml
<packaging>pom</packaging>

    <!--统一管理jar包和版本-->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <junit.version>4.12</junit.version>
        <log4j.version>1.2.17</log4j.version>
        <lombok.version>1.16.18</lombok.version>
        <mysql.version>5.1.47</mysql.version>
        <druid.verison>1.1.16</druid.verison>
        <mybatis.spring.boot.verison>1.3.0</mybatis.spring.boot.verison>
    </properties>

    <dependencyManagement>
        <dependencies>
            <!--spring boot 2.2.2-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.2.2.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud Hoxton.SR1-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud alibaba 2.1.0.RELEASE-->
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>2.1.0.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!-- MySql -->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>
            <!-- Druid -->
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid-spring-boot-starter</artifactId>
                <version>${druid.verison}</version>
            </dependency>
            <!-- mybatis-springboot整合 -->
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>${mybatis.spring.boot.verison}</version>
            </dependency>
            <!--lombok-->
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${lombok.version}</version>
            </dependency>
            <!--junit-->
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.version}</version>
            </dependency>
            <!-- log4j -->
            <dependency>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
                <version>${log4j.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>
                    <addResources>true</addResources>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

让父工程进行统一的项目管理

### 创建子模块

创建一个支付的模块

同样创建一个maven工程

可以分为5步

+ 创建Module
+ 修改pom文件
+ yml
+ 主启动类
+ 业务

在父工程下创建一个Module

他的类型是maven

修改他的pom文件

完整pom文件如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>cloud</artifactId>
        <groupId>com.xhn.springcloud</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-provider-payment8001</artifactId>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>


</project>
```

给模块添加yml文件在resource下创建一个application.yml的文件

里面添加

```yml
server:
  port: 8001
spring:
  application:
    name: cloud-payment-service
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource      #当前数据源操作类型
    driver-class-name: org.gjt.mm.mysql.Driver        #mysql驱动包
    url: jdbc:mysql://localhost:3306/db2019?useUnicode=true&characterEncoding-utr-8&useSSL=false
    username: root
    password: root

mybatis:
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.atguigu.springcloud.entities       #所有Entity别名类所在包
```

然后在给模块添加主启动类

在java下创建一个com.xhn.springcloud 的包

添加一个类

```java
@SpringBootApplication
public class PaymentMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8001.class,args);
    }
}
```

## 编写支付模块

已经完成了工程创建

编写具体代码

项目的目录结构

![image-20210609191847330](SpringCloud.assets/image-20210609191847330.png)



创建数据库

数据库脚本

SQL

```sql
CREATE TABLE `payment`(
	`id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'ID',
    `serial` varchar(200) DEFAULT '',
	PRIMARY KEY (id)
)ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4

```

创建实体类

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Payment implements Serializable {
    private Long id;
    private String serial;
}
```

创建统一返回结果集

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class CommonResult<T> {
    private Integer code;
    private String message;
    private T      data;

    public CommonResult(Integer code, String message) {
        this(code,message,null);
    }
}
```

创建Mapper接口

```java
@Mapper
public interface PaymentDao {
    public int create(Payment payment);
    public Payment getPaymentById(@Param("id") Long id);

}
```

编写操作数据库内容

```xml
<?xml version="1.0" encoding="UTF8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.xhn.springcloud.dao.PaymentDao">
    <insert id="create" parameterType="Payment" useGeneratedKeys="true" keyProperty="id">
        insert into payment(serial) values(#{serial});
    </insert>
    <select id="getPaymentById" parameterType="Long" resultMap="BaseResultMap">
        select * from payment where id=#{id};
    </select>
    <resultMap id="BaseResultMap" type="com.xhn.springcloud.entities.Payment">
        <id column="id" property="id" jdbcType="BIGINT"/>
        <id column="serial" property="serial" jdbcType="VARCHAR"/>
    </resultMap>
</mapper>
```

编写service层代码

```java
public interface PaymentService {
    public int create(Payment payment);
    public Payment getPaymentById(@Param("id") Long id);
}
```

```java
@Service
public class PaymentServiceImpl implements PaymentService {
    @Resource
    private PaymentDao paymentDao;

    @Override
    public int create(Payment payment) {
        return paymentDao.create(payment);
    }

    @Override
    public Payment getPaymentById(Long id) {
        return paymentDao.getPaymentById(id);
    }
}
```

controller

```java
@RestController
@Slf4j
public class PaymentController {
    @Resource
    private PaymentService paymentService;

    @PostMapping("/payment/create")
    public CommonResult crete(Payment payment){
        int result = paymentService.create(payment);
        log.info("*******插入结果********："+result);
        if (result>0) {
            return new CommonResult(200,"success",result);
        }else {
            return new CommonResult(444,"error",null);
        }
    }

    @GetMapping("/payment/get/{id}")
    public CommonResult crete(@PathVariable("id") Long id){
        Payment payment = paymentService.getPaymentById(id);
        log.info("*******查询结果********："+payment);
        if (payment!=null) {
            return new CommonResult(200,"success",payment);
        }else {
            return new CommonResult(444,"error---->"+id,null);
        }
    }



}
```

测试

发送http://localhost:8001/payment/create和http://localhost:8001/payment/get/{{id}}

## 开启自动热部署功能

添加依赖

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
</dependency>
```

添加插件在maven父工程

```xml
<plugins>
    <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <configuration>
            <fork>true</fork>
            <addResources>true</addResources>
        </configuration>
    </plugin>
</plugins>
```

设置IDEA

![image-20211114174052438](G:\JavaSource\SpringCloudStudy\SpringCloud.assets\image-20211114174052438.png)

## 建立订单模块

建立maven工程

cloud-consumer-order80

加入依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
   
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <scope>runtime</scope>
        <optional>true</optional>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

配置文件application.yml

```yml
server:
  port: 80
```

```java
/**
 * @author xhn
 * @created  2021 11 14
 */
@SpringBootApplication
public class OrderMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderMain80.class, args);
    }
}
```

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class CommonResult<T> {
    private Integer code;
    private String message;
    private T      data;

    public CommonResult(Integer code, String message) {
        this(code,message,null);
    }
}
```

```java
/**
 * @author xhn
 * @Date 2021/11/14 18:08
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Payment {
    private Long id;
    private String serial;
}
```

```java
@Configuration
public class ApplicationContextConfig {
    @Bean
    public  RestTemplate getRestTemplate() {
        return new RestTemplate();
    }
}
```

```java
@RestController
@Slf4j
public class OrderController {

    public static final String PAYMENT = "http://localhost:8001";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping("/consumer/payment/create")
    public CommonResult<Payment> create(Payment payment) {

        return  restTemplate.postForObject(PAYMENT+"/payment/create",payment,CommonResult.class);
    }
    @GetMapping("consumer/payment/get/{id}")
    public CommonResult<Payment> getPayment(@PathVariable("id") Long id) {
        return restTemplate.getForObject(PAYMENT+"/payment/get/"+id,CommonResult.class);

    }

}

```



![image-20211114182507271](G:\JavaSource\SpringCloudStudy\SpringCloud.assets\image-20211114182507271.png)

测试

## 工程重构

系统中有部分重复的内容



![image-20211208170345858](http://img.xhnya.top/img/image-20211208170345858.png)

## Eureka

Eureka主要是服务注册中心

采用了CS的设计架构，Eureka Server作为服务注册的服务器，是服务的注册中心，而系统的其他的微服务使用Eureka的客户端连接到Eureka Server并维持连接，这样可以通过Eureka Server来监控系统各个微服务是否正常运行

包含两个组件

Eureka Server提供服务注册服务

Eureka Client 

### 单机Eureka构建步骤

IDEA生成Eureka Server

建module

改pom

写yml

主启动

业务类

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```



```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
<dependency>
    <groupId>com.xhn.springcloud</groupId>
    <artifactId>cloud-api-commen</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-devtools</artifactId>
<scope>runtime</scope>
<optional>true</optional>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot</artifactId>
</dependency>
```

![image-20211209225515461](http://img.xhnya.top/img/image-20211209225515461.png)



### pyment使用Eureka

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

配置配置文件

```yml
eureka:
  client:
    register-with-eureka: true
    fatchRegistration: true
    service-url:
      defaultZone: http://localhost:7001/eureka
```

在启动类上使用注解

```java
@EnableEurekaClient
```

同样配置80端口

### Eureka集群的使用

**互相注册，相互守望**



新建一个服务，参考cloud-eureka-server7001

主要改yml

7001的配置文件，注意defaultZone

```yml
server:
  port: 7001


eureka:
  instance:
    hostname: eureka7001.com #eureka服务端的实例名称
  client:
    #false表示不向注册中心注册自己。
    register-with-eureka: false
    #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    fetch-registry: false
    service-url:
      #设置与Eureka server交互的地址查询服务和注册服务都需要依赖这个地址。
      defaultZone: http://eureka7002.com:7002/eureka/
```

7002

```yml
server:
  port: 7002


eureka:
  instance:
    hostname: eureka7002.com #eureka服务端的实例名称
  client:
    #false表示不向注册中心注册自己。
    register-with-eureka: false
    #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    fetch-registry: false
    service-url:
      #设置与Eureka server交互的地址查询服务和注册服务都需要依赖这个地址。
      defaultZone: http://eureka7001.com:7001/eureka
```

把8001发布到配置文件

修改配置文件即可

```yml
eureka:
  client:
    register-with-eureka: true
    fatchRegistration: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
```

### 负载均衡

调用地址不可以写死

![image-20211210233852059](http://img.xhnya.top/img/image-20211210233852059.png)



配置负载均衡

**@LoadBalanced注解赋予RestTemplate负载均衡的能力**

```java
@Configuration
public class ApplicationContextConfig {

    @Bean
    @LoadBalanced
    public  RestTemplate getRestTemplate() {
        return new RestTemplate();
    }
}
```

负载均衡默认轮询

### actuator微服务信息的完善

服务名称的修改

8001和8002的修改

![image-20211211200400914](http://img.xhnya.top/img/image-20211211200400914.png)



```yml
eureka:
  client:
    register-with-eureka: true
    fatchRegistration: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
  instance:
    instance-id: payment8002
```

```java
instance:
  instance-id: payment8002
  prefer-ip-address: true
```

### 服务发现Discovery

对于注册进的服务可以服务发现可以获取服务的信息

修改payment的controller

```java
@Resource
private DiscoveryClient discoveryClient;
```

```java
@GetMapping(value = "/payment/discovery")
public Object discovery(){
    List<String> services = discoveryClient.getServices();
    for (String service : services) {
        log.info("==========="+service+"==============");

    }
    List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
    for (ServiceInstance instance : instances) {
        log.info(instance.getServiceId()+"=============="+instance.getHost()+"\t"
        +instance.getPort()+"\t"+instance.getUri());
    }
    return this.discoveryClient;
}
```

![image-20211211202807823](http://img.xhnya.top/img/image-20211211202807823.png)

### Eureka自我保护

#### **EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE.**

这就是进入了保护状态

![image-20211211203127536](http://img.xhnya.top/img/image-20211211203127536.png)

某个时刻某一微服务不可用了，Eureka不会立即清理，依旧会对该微服务的信息进行保存

属于CAP的AP分支

禁止自我保护

```java
eureka:
  instance:
    hostname: eureka7001.com #eureka服务端的实例名称
  client:
    #false表示不向注册中心注册自己。
    register-with-eureka: false
    #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    fetch-registry: false
    service-url:
      #设置与Eureka server交互的地址查询服务和注册服务都需要依赖这个地址。
      # defaultZone: http://eureka7002.com:7002/eureka/
      defaultZone: http://eureka7001.com:7001/eureka/
  server:
    enable-self-preservation: false
    eviction-interval-timer-in-ms: 2000
```

payment修改

```java
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
  instance:
    instance-id: payment8001
    prefer-ip-address: true
    lease-expiration-duration-in-seconds: 2
    lease-renewal-interval-in-seconds: 1
```

关闭服务eureka立马删除

## Zookeeper

安装zookeeper

![image-20211212171515145](http://img.xhnya.top/img/image-20211212171515145.png)

遇到这个问题，可以通过重命名配置文件来解决

启动zookeeper 进入目录的bin下面

使用./zkServer.sh start

然后使用 ./zkCli连接

使用命令ls /

![image-20211212171739864](http://img.xhnya.top/img/image-20211212171739864.png)

启动payment8004

![image-20211212171835602](http://img.xhnya.top/img/image-20211212171835602.png)





![image-20211212171847288](http://img.xhnya.top/img/image-20211212171847288.png)



8004的配置

在确定类上使用

```java
@EnableDiscoveryClient
```

```yml
server:
  port: 8004
spring:
  application:
    name: cloud-payment-service
  cloud:
    zookeeper:
      connect-string: 192.168.43.128:2181
```

这个微服务注册中心节点临时的

cp原则



写一个消费者，模块基本和80相同

把Eureka换成zookeeper

```java
/**
 * @author xhn
 * @date 2021/11/14 18:09
 */
@RestController
@Slf4j
public class OrderController {

    public static final String PAYMENT = "http://CLOUD-PAYMENT-SERVICE";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping("/consumer/payment/create")
    public CommonResult<Payment> create(Payment payment) {

        return  restTemplate.postForObject(PAYMENT+"/payment/create",payment,CommonResult.class);
    }
    @GetMapping("consumer/payment/get/{id}")
    public CommonResult<Payment> getPayment(@PathVariable("id") Long id) {
        log.info("========");
        return restTemplate.getForObject(PAYMENT+"/payment/get/"+id,CommonResult.class);

    }

}
```

```yml
server:
  port: 80
eureka:
  client:
    register-with-eureka: true
    fatchRegistration: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
spring:
  application:
    name: cloud-consumer-order80
```

## Consul

换成这个即可

https://www.consul.io/

consul比较完善

提供了微服务的服务治理，配置中心，控制总线等功能，这些功能中每一个都可以根据需要单独使用

+ 服务发现，HTTP和DNS
+ 健康监测： HTTP，TCP，Docker，shell
+ KV存储
+ 多数据中心
+ 可视化web界面

https://www.consul.io/downloads下载地址

然后解压双击运行

查看版本

![image-20211212210917514](http://img.xhnya.top/img/image-20211212210917514.png)

开发者模式启动

consul agent -dev

![image-20211212211000425](http://img.xhnya.top/img/image-20211212211000425.png)

前端界面

http://localhost:8500/ui/dc1/services访问地址

![image-20211212211104452](http://img.xhnya.top/img/image-20211212211104452.png)

服务提供者注册

```yml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-consul-discovery</artifactId>
</dependency>
```

```yml
server:
  port: 8006
spring:
  application:
    name: cloud-payment-service
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```

![image-20211212215325942](http://img.xhnya.top/img/image-20211212215325942.png)





![image-20211212215351424](http://img.xhnya.top/img/image-20211212215351424.png)

![image-20211212215428281](http://img.xhnya.top/img/image-20211212215428281.png)

消费者

![image-20211212215942742](http://img.xhnya.top/img/image-20211212215942742.png)

同样建立一个

![image-20211212220206215](http://img.xhnya.top/img/image-20211212220206215.png)

![image-20211212220223941](http://img.xhnya.top/img/image-20211212220223941.png)

注册中心都一样

### 三个注册中心的异同点

## Ribbon负载均衡服务调用



## OpenFeign

































