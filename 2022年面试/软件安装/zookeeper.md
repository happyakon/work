## ZOOKEEPER



### 1.安装zookeeper

#### 1.1. 启动命令

```sh
# 启动命令
./zkServer.sh 
# 重启
./zkServer.sh restart
# 停止
./zkServer.sh stop
# 连接客户端
./zkCli.sh -server 127.0.0.1:2181
```

#### 1.2. 常用命令

| 命令                 | 说明                               |
| -------------------- | ---------------------------------- |
| ls path              | 查看节点                           |
| ls -s path           | 查看节点详细信息                   |
| ls -R path           | 查看该节点的所有子节点             |
| ls -w path           | 监听节点                           |
| create path data     | 创建永久节点                       |
| create -s path data  | 创建永久顺序节点                   |
| create -e path data  | 创建临时节点                       |
| create -es path data | 创建顺序临时节点                   |
| set path data        | 修改存在的节点的数据               |
| set -s path data     | 修改存在的节点的数据并返回详细信息 |
| get path             | 获取节点值                         |
| get -s path          | 获取节点值和详细数据               |
| get -w path          | 获取节点值，并监听此节点           |
| stat path            | 查看节点详细信息                   |
| stat -w path         | 查看节点详细信息，并监听此节点     |



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

