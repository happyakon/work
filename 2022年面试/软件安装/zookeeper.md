## ZOOKEEPER



### 1.安装zookeeper

#### 1.1. 启动命令

```sh
# 启动命令
./zkServer.sh 
# 重启
./zkServer.sh restart
# 连接客户端
./zkCli.sh -server 127.0.0.1:2181
```



### 2.融合SpringBoot

#### 2.1 引入依赖

```java
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
    <version>3.1.1</version>
</dependency>
```

#### 2.2 配置yaml

```yaml
spring:
  application:
    name: zooker-spring-mysql
  cloud:
    zookeeper:
      connect-string: 192.168.2.109:2181
```

