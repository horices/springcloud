### spring cloud config  服务端配置(配置文件放置在远程服务器,git 仓库)
> #### 1.配置 pom.xml 
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.horice</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>demo</name>
    <description>Demo project for Spring Boot</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.6.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
        <spring-cloud.version>Finchley.SR1</spring-cloud.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>


</project>
```
> #### 2.添加注解 @EnableConfigServer
```
@EnableConfigServer
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

> #### 3.配置 application.yml
```
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/horices/springCloudConfig #git 仓库地址
          username: 864742615@qq.com    # 仓库用户名
          password: xxxxxxxxxxxxxxxxx   # 仓库密码
  application:
    name: configServer  #应用名，用于访问使用
server:
  port: 8001    #配置端口号 ，因为本地可以有多个服务 config 只是其中一个，
```

> #### 4.配置 git 仓库，新建一个 远程配置文件 application.yml 的文件      https://github.com/horices/springCloudConfig/tree/master

服务器端配置已经完成，可浏览器进行如下方式进行访问

    /{application}/{profile}[/{label}]
    /{application}-{profile}.yml
    /{label}/{application}-{profile}.yml
    /{application}-{profile}.properties
    /{label}/{application}-{profile}.properties
http://localhost:8001/configServer/test
HTTP服务具有以下格式的资源：

----------


> > 至此服务端已经配置完成，
> 通过HTTP进行访问配置文件 
测试后发现浏览器访问随意一个二个路径都可以访问
http://localhost:8001/a/b 可以正常访问刚才服务器的远程配置文件
