## ROCKETMQ



### 1.安装单机RocketMQ

#### 1.1. 启动命令

```sh
#1.修改内存，预防虚拟机内存不足
vim runserver.sh
vim runbroker.sh
vim tools.sh
#2.后台启动
nohup ./bin/mqnamesrv > /home/akon/dev/rocketmq/logs/mqnamesrv.log 2>&1 &
nohup sh bin/mqbroker -c /home/akon/dev/rocketmq/conf/broker.conf > /home/akon/dev/rocketmq/logs/mqbroker.log 2>&1 &
#3.关闭namesrv服务：
sh bin/mqshutdown namesrv
#4.关闭broker服务 ：
sh bin/mqshutdown broker

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

