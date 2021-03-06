## JAVA基础

### 1.HashMap?

#### 1.1 说说它的底层结构？

> 链表，数组，红黑树

#### 1.2 什么时候链表变成红黑树？

> 链表长度大于等于8的时候，并且数组容量大于64的时候，如果链表大于等于8，但是容量小于等于64，那么会执行扩容。

#### 1.3 什么时候红黑树变成链表？

> 当元素小于等于6的时候。

#### 1.4 为什么每次都是扩容2倍？

> 因为元素计算hash下标的时候是(n-1)&hash,这样可以让结果充分散列，并且当扩容后做rehash的时候，就是老数组数据向新数据数据转移的时候，当节点是链表的时候，会将链表的元素分成两个数组，当元素的hash值和老数组容量与运算后为0，则f放到低位数组中，不为0的放到高位数组中，最后将低位数据放入老的下标值中，高位数据放到老数据长度加老的下标中，这样以来不但对原来的数据重新分配更简单，而且也不会新增hash碰撞。

#### 1.5 HashMap为什么不是线程安全的？

> 1.7：
>
> 数据丢失和死循环的问题
>
> 数据丢失好理解，当两个或者多个线程hash到一个下标是，这是值都是null，线程会将自己的元素放到这个下标上面，那么最后一个放的会将前面线程放的覆盖掉，这样就造成了数据丢失。
>
> 因为链表采用的是头插法，当扩容的时候，如果两个线程同时执行transfer，就有可能会出现环状的链表，这个get的时候，如果hash到这个链表，key又对不上，则会发生死循环，CPU飙升的情况。
>
> 1.8：
>
> 数据丢失
>
> 由于1.8使用了尾插法，所以不再又死循环的问题了，但是put的时候依然又数据丢失的问题。

### 2.ConcurrentHashMap?

> 数据结构：
>
> 数组+链表+红黑树
>
> 如何保证并发：
>
> 主要是CAS+synchronzied
>
> 如何计算元素个数：
>
> 照搬了LongAddr的代码，先用CAS给baseCount赋值，如果CAS失败，则hash到CountCell数组，在进行CAS操作为CountCell对象的value赋值。这样就把原来对一个资源进程变成了对多个资源进行竞争。

#### 2.1 CAS是什么？

> 全称是Compare-And-Swap,他是一条CPU原语，跟它的名字一样，比较并交换，先去读取主内存中的值，然后修改，再然后就是比较期望的值和内存中的值是不是一致的，如果是，就修改，如果不是就再读取新值，然后再进行比较交换。
>
> 原语的执行必须是连续的，再执行过程中不能被中断，就是说CAS是一条CPU的原子命令，不会造成数据不一致的问题。
>
> 他的功能是判断内存中某个位置的值是不是预期值，如果是就修改为新的值，这个过程是原子的。
>
> 硬件级别 是一条cmpxchg,如果判断是你多个CPU的话，那么会执行汇编指令 lock cmpxchg,实际上就是锁住一个北桥信号。
>
> 缺点：会出现ABA的问题，还有当线程比较多的时候，很多线程都自旋不成功，会导致性能下降。

#### 2.2 sizeCtl表示什么？

> 0：默认值
>
> -1：正在初始化
>
> 0+：扩容阈值
>
> -n：高16为表示扩容戳，低16位表示参与扩容的线程+1
>
> Node中的hash为负数时
>
> -1 表示已经迁移到新表
>
> -2 表示为迁移的新表为红黑树

#### 2.3 如何保证写数据安全？

> 如果计算出来下标再数组中是null的话，那么是通过CAS为这个下标赋值，如果不是null，则是synchronzied锁头节点。

#### 2.4 详细说说扩容？

> 第一个触发扩容的线程，会修改sizeCtl为-n，然后新建一个比之前大一倍的table,并设置nextTable指向新表，最后将老表的长度赋值给transferIndex(转移下标)
>
> 迁移完的数据变成一个ForwardingNode节点，其中有一个字段为nextTable,就是告诉你从新表中查询，另外还有一个find方法，会直接从新表中进行查询数据。
>
> 最后一个推出扩容的线程会做收尾工作，检查老表，看有没有漏迁移的，然后将table字段指向新表，将sizeCtl设置成下次要扩容的阈值。

#### 2.5 当map正在扩容的时候，此时来了一个线程想写数据，怎么办？

> 有两种情况
>
> 一：当hash到的那个桶此时还没有被迁移或者是null，那么就用CAS或者synchronzied直接写数据
>
> 二：已经迁移的数据，此时写数据的线程会参与到扩容当中，具体实现就是会更具transferIndex的值给自己分配任务区间，然后负责迁移这写区间的数据。迁移完之后，再根据全局的transferIndex分配下一次的任务区间，直到不能再分配任务的时候，扩容协助扩容的操作就完毕了。然后继续自己的写操作，将数据写到新表里面。

#### 2.6 对于红黑树如果正在读的时候能否写，写的时候能否读？

> 读的时候是不能写的，因为会造成红黑树的失衡，会触发红黑树的平衡操作，所以treeBin的处理方式是用一个lockState字段
>
> 当写的时候会将state加4，读的时候会加1，如果此时是读状态，那么写线程会将state加2，然后将waiting设置成自己的线程，最后把自己的线程挂起，等读线程结束的时候，state会减4,然后看是否等于2，如果等于2，则代表一个线程正在等这写呢，会将wating线程释放掉就是unpark。如果一个线程正在写，那么此时的状态应该是1，为了提高性能，当节点是红黑树时，其实也会建一个链表，这个时候读就直接读链表就好了。

#### 2.7 为什么要用红黑树，我能不能用二叉树？

> 不行，因为二叉树再一些极端的情况下会变成单侧，也就是链表，这样链表转链表就没什么意思了，红黑树是一个自平衡的二叉树，他不会成为链表，查询性能肯定高于链表。

#### 2.8 加载因子为什么是0.75？

> 1.如果太大，那么发生扩容的频率就比较低，浪费的空间比较少，但是发生hash碰撞的概率就会比较大，就会造成有些链表过长，或者形成比较多的红黑树，从而影响性能。
>
> 2.如果太小，那么发生扩容的频率就比较高，浪费的空间比较多，虽然发生hash碰撞的概率小了，但是频繁的扩容，也不太好。
>
> 0.75是跟泊松分布有关系。当负载因子=0.75，代入泊松公式中，计算出来长度为8时，概率为0.00000006，这个概率已经很小了，完全满足使用。



### 3.ArrayList和LinkedList?

> 数据结构：
>
> array是数组，内存上连续，查找比较快，增删慢，因为牵扯到数组移动。
>
> linked是链表，内存上不联系，增删比较快，查询慢。

### 4.JUC下的类?

#### 4.1 ReentrantLock 可重入锁

> java层面的可重入锁
>
> 与synchronized相比，有三个特性
>
> 1.等待可中断
>
> 2.可是实现公平锁
>
> 3.可以绑定多个条件。
>
> 重入锁设计的目的是为了防止死锁的发生

#### 4.2 stampedLock 

#### AQS主要使用了什么设计模式？

#### AQS的尾分叉？

### 5.谈谈synchronized?

> synchronized是一个互斥同步锁，也可以叫做对象监视器，依赖JVM实现。

#### 5.1 synchronized的作用域？

> 1.用在非静态方法上，锁的是当前对象。
>
> 2.用在代码块，如果传入的是this，锁主的还是当前对象，否者就是其他传入的对象。
>
> 3.如果用在静态方法上，则锁住的是当前类。
>
> 4.如果用在静态方法的代码块中传如的是当前类的class,那么锁住的还是当前类。

> ps:synchronized不能被继承，子类如果覆盖了父类的synchronized修饰的方法，默认并非同步的，除非子类方法也显示的加上此关键字。

#### 5.2 synchronized的工作原理

> synchronized被编译之后，会在同步块的前后形成两个指令monitorenter和monitorexit。这两个指令都需要一个reference参数来指明加锁和解锁的对象。
>
> 1.执行monitorenter的时候，线程需要先尝试获取对象的锁，如果对象没有锁定，或者是被当前线程锁定的，就把这个锁的计数器加1，执行monitorexit的时候，就把计数器减1。减到0就随之把对象的锁释放掉，如果在执行monitorenter的时候没有获取到锁，则当前线程就应该被阻塞等待，直到对象的锁被其它线程释放为止。
>
> 2.对于同一个线程来说，它是可以重入的。
>
> 3.JDK6之前，synchronized是重量级锁，java线程是映射到操作系统的原生内核线程之上的。如果要阻塞或者唤醒一条线程，都需要操作系统来帮忙，这就不可避免的进行用户态到内核态的转换，这种状态转换时比较消耗cpu的时间的，所以之前的synchronized是没有ReentrantLock的效率高的。
>
> 4.JDK6开始，虚拟机对synchronized做了很好的优化，锁升级。

#### 5.3 锁升级

> 无锁->偏向锁->轻量级锁(CAS自旋锁)->重量级锁（互斥同步锁）在JVM中对这几种锁有详细说明，根据对象头实现。

### 6.volatile原理?

> 首先从功能上来说，java内存模型对volatile修饰的变量由两个特殊的规则：
>
> 1.保证了线程可见性，2.禁止指令重排序。
>
> 1.保证线程可见性：
>
> 其实底层就是缓存一致性协议，针对Intel处理器和HOTSPOT虚拟机来说就是MESI。
>
> CPU从内存中读取的数据是按块读的，这一块叫做cache line (缓存行)。缓存行中有四个状态，分别是修改、独占、共享、无效。
>
> 2.禁止指令从排序：
>
> volatile通过内存屏障来防止指令重排序，实现为：
>
> jvm层面：
>
> 在每个写操作前面插入StoreStore屏障，写后面插入StoreLoad屏障。
>
> 在每个读操作前面插入LoadLoad屏障，读后面插入LoadStore屏障。

#### 6.1 jvm内存屏障

| 屏障类型   | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| LoadLoad   | Load1;LoadLoad;Load2 该屏障确保Load1数据的装载先于Load2及其后所有装载指令的的操作 |
| StoreStore | Store1;StoreStore;Store2 该屏障确保Store1立刻刷新数据到内存(使其对其他处理器可见)的操作先于Store2及其后所有存储指令的操作 |
| LoadStore  | Load1;LoadStore;Store2 确保Load1的数据装载先于Store2及其后所有的存储指令刷新数据到内存的操作 |
| StoreLoad  | Store1;StoreLoad;Load1 该屏障确保Store1立刻刷新数据到内存的操作先于Load2及其后所有装载装载指令的操作.它会使该屏障之前的所有内存访问指令(存储指令和访问指令)完成之后,才执行该屏障之后的内存访问指令 |

#### 6.2 硬件层面内存屏障

| 屏障类型     | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| Load 读屏障  | 在指令前插入读屏障，可以让高速缓存的数据失效，强制从内存中重新加载数据 |
| Store 写屏障 | 在指令后插入写屏障，能让写入缓存的最新数据更新到主存，让其他线程可见。 |



### 7.java的集合为什么要使用迭代器？

### 8.对象的创建过程？

> 创建对象通常时new 关键字。
>
> 1.首先检查这个指令的参数能否再常量池中定位到一个类的符号引用。并且检查这个这个符号引用代表的类是否已被加载，解析和初始化。如果没有，就执行相应的类加载过程。
>
> 2.检查通过之后，为对象分配内存空间。
>
> 3.初始化变量为零值。
>
> 4.设置对象头信息（元对象信息，GC分代年龄）
>
> 5.执行构造函数初始化，就是赋值。
>
> 6.将对象的引用保存到本地变量表中。

###### 字节码文件如下：

```properties
1.new
2.dup
3.invokespecial<o.init>
3.astore_1
5.return
```

### 9.为什么会引入自适应自旋锁？

> jdk1.6之后引入了自适应自旋锁，他的自旋次数不是固定的，如果线程上次自旋某个锁成功了，那么这次自旋的次数会有所增加，虚拟机认为既然上次成功了，那么这次也极有可能成功，反之，如果某个锁很少有自旋成功的，那么以后自旋的次数会减少，甚至省略掉自旋的过程，以免浪费资源。

### 10.final finally fianlize分别是什么？

#### 1. final

> final是修饰关键字，用来修饰类，方法，变量
>
> 当修饰类的时候，该类不能被继承。
>
> 当修饰方法的时候，次方法不能被重写。
>
> 当修饰变量的时候，变量的值不能大声改变，也就是说该值是一个常量，对于final修饰的变量，必须在声明它的时候复制，或者在构造函数中赋值。

#### 2. finally

> 它是异常处理的关键字，通常是通过try...finally或者try...catch...finally,表示一定会被执行，除非宕机或者在try块里调用system.exit(0)又或者是进入try块之前，程序发生异常。

#### 3. finalize

> Object类中的一个方法，此方法是垃圾回收器在确定此对象没有引用，需要将对象清理的时候调用的。如果finalize被继承了，则此对象会被放在finalizer队列，有一个finalizeThread专门对它管理，因为这个线程的优先级不高，如果这样的对象太多，可能会出现OOM的异常，我们可以把这个方法忘掉。





