### 一起动手玩Demo - 实践步骤

* ##### 运行环境

  * windows

  * maven 3.3.1 + 

  * jdk 1.8

    

* ##### 启动Apache ServiceComb服务中心ServiceCenter

  * 下载地址： https://apache.org/dyn/closer.cgi/servicecomb/servicecomb-service-center/1.2.0/

  * 启动：

    ```
    start-service-center.bat    // 监听端口 30100
    ```

    还可以同时启动ServiceCenter UI管理服务

    ```
    start-frontend.bat          // 监听端口 30103
    ```

    

* ##### 启动Eureka服务中心

  已提供一个集成好的Eureka服务中心 ./registry/eureka-server.jar

  ```
  java -jar ./registry/eureka-server.jar  // 监听端口 8761
  ```

  

* ##### 下载Apache ServiceComb Syncer 异构通信工具
  在当前项目中已提供一个./syncer/sycener.exe

* ##### 生成基于ServiceComb框架的服务提供者servicecomb-provider

  ```
  cd HelloService 
  mvn toolkit:generate@provider
  ```

* ##### 生成基于SpringCloud框架的服务消费者springcloud-consumer

  ```
  mvn toolkit:generate@consumer
  ```



* ##### 启动 ServiceCenter 侧的syncer

  ```
  cd syncer
  start-syncer-sc.bat
  ```

* ##### 启动 Eureka 侧的syncer

  ```
  start-syncer-eureka.bat
  ```

* ##### 为生成的服务提供者添加业务代码

  服务提供者默认生成目录 output/servicecomb-provider/project/servicecomb-provider

  编辑其中的 domain.orgnization.project.sample.api.HelloController 类

  添加自定义的业务逻辑

  ```
  return new ResponseEntity<String>(String.format("Hello %s", name), HttpStatus.OK);
  ```

  

* ##### 使用服务消费者调用服务提供者

  * 修改消费者springcloud-consumer的启动类Application.java，添加如下代码

    ```
    
      @Autowired
      HelloController helloController;
    
      @Bean
      ApplicationRunner applicationRunner() {
        return args -> {
          String serviceComb = helloController.sayHello("ServiceComb");
          System.out.println(serviceComb);
        };
      }
    ```

  * 调用

    ```
    mvn spring-boot:run
    ```

    在控制台输出的日志最后有如下信息，则调用成功

    ```
    "Hello ServiceComb"
    ```