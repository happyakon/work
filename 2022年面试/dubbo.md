## Dubbo

### 1. dubbo是什么？

> dubbo是⼀个分布式服务[框架](https://so.csdn.net/so/search?q=框架&spm=1001.2101.3001.7020)，提供⾼性能和透明化的RPC远程服务调⽤⽅案，以及SOA服务治理方案。说白了其实dubbo就是一个远程调用的分布式框架。

### 2.dubbo有几种容错机制？

1. failsafe 失败安全，可以认为是把错误吞掉（记录日志）

2. failover(默认) 重试其他服务器；retries(2)重试的次数，默认为2次

3. failback 失败后自动恢复

4. forking forks. 设置并行数

5. Broadcast 广播，任意一台报错，则执行的方法报错，通过cluster方式，配置制定的容错方案

### 3.SPI (service provider interface服务提供者接口) 机制

#### 3.1 java spi的缺点：

1. JDK 标准的 SPI 会一次性加载实例化扩展点的所有实现，什么意思呢？就是如果你在 META-INF/service 下的文件里面加了 N 个实现类，那么 JDK 启动的时候都会一次性全部加载。那么如果有的扩展点实现初始化很耗时或者如果有些实现类并没有用到， 那么会很浪费资源。
2. 如果扩展点加载失败，会导致调用方报错，而且这个错误很难定位到是这个原因。
3. java spi没有AOP和IOC机制

#### 3.2 dubbo spi的优点：

1. 鉴于java以上的缺点，dubbo自己实现了一个spi,可以实现按需加载。
