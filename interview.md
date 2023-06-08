<details> 
    <summary></summary>
    
</details>

# Mysql
## 数据类型
<details> 
    <summary></summary>
    char int date
</details>

## char和varchar的区别
<details> 
    <summary></summary>
   char是定长字符串，varchar是变长字符串<br/>
   char(10) varchar(10)后面的参数定义了最大长度，只是char的内容不到10字符还是会占用10字符，而varchar用多少字符就占用多少字符。 
</details>

## 索引的最左前缀原则
<details> 
    <summary></summary>
    0. 创建索引时，会有顺序
    1. sql语句中字段顺序无关
    2. sql语句中必须包含第一个，否则全部失效
    3. sql语句中缺少中间的，那么缺少的后面的失效
    4. >、< 后面的失效（使用<、>本身的字段不失效），应使用>=

</details>

## 怎么知道sql语句好不好
<details> 
    <summary></summary>
    <b>explain（执行计划）</b>  id:大的先执行，相同的从上到下。 type：const主键或唯一索引字段，ref普通索引，index索引，all全表扫描。 key：实际用到的索引。  key_len：实际索引的最大长度<br/><br/>
    show profiles 显示sql语句执行的时间
</details>


## 索引类型
<details> 
    <summary></summary>
    主键索引、唯一索引、普通索引
</details>

## 数据库事物隔离级别有哪些，分别讲一下
<details> 
    <summary></summary>
    未提交读：未提交的事务所作出的修改会被其他事务读取到<i>脏读</i><br/>
    提交读：解决脏读，但<i>不可重复读</i><br/>
    可重复读：解决不可重复读，但会<i>幻读</i><br/>
    串行化：将事务串行执行
</details>

## 数据库索引有什么优缺点
<details> 
    <summary></summary>
    在数据量大的时候能提高数据查询效率<br/>
    但创建索引的时候会有一定开销，并且在对数据增加、修改、删除的时候会动态维护索引也会造成开销<br/>
    索引占用一定的磁盘空间
</details>

## mysql一共有哪些锁
<details> 
    <summary></summary>
    表锁,读写锁; 行锁, 记录锁, 间隙锁, 临键锁
</details>

## 行锁
<details> 
    <summary></summary>
    对于数据库进行读写操作时，只会对操作涉及的行加锁<br/>
    记录锁：该记录上锁<br/>
    间隙锁：该记录范围上锁，不包含该记录<br/>
    临键锁：该记录范围上锁，包含该记录<br/>
</details>

## 常见事务隔离级别，生产环境使用哪个，隔离级别的影响
## 聚簇索引，非聚簇索引，如何避免回表查找，二级索引是什么（就是非聚簇索引）
<details> 
    <summary></summary>
    聚簇索引的叶子节点存放真实数据<br/>
    非聚簇索引的叶子节点存放的是主键，再根据主键查询真实数据,所以需要两次查询（回表查询）<br/>
    覆盖索引可以避免回表查询：要查询的字段正好是索引建立的字段或主键，就只用一次查询就行了。记住：叶子节点存的是主键和建立索引的字段
</details>

## 怎样优化sql(例举几个Mysql优化手段)
<details> 
    <summary></summary>
    使用explain分析执行计划<br/>
    不使用select *而是查询所需要的特定的字段。<br/>
    使用limit获取特定的条数<br/>
    设置合适的字段数据类型<br/>
    避免索引失效<br/>
</details>

## B+树相比B树的优点（MySQL为什么用B+树）
<details> 
    <summary></summary>
    1.与二叉树相比：对每一个节点读取就是一次磁盘IO，B+树一个节点有多个key，节点比二叉树小，因此读取效率高<br/>
    2.与B树相比：叶子节点连成链表，可以范围查询，非叶子节点不存储数据，可以存储更多key，减少节点数量，减少磁盘IO
</details>

## #{}和${}的区别
<details> 
    <summary></summary>
    向sql语句传递参数的占位符<br/>
    ${}相当于字符拼接，会sql注入<br/>
    #{}使用预编译，防止sql注入
</details>

## 数据库死锁定义，怎样避免死锁
<details> 
    <summary></summary>
    多个事务各自锁定了一些记录，同时又相互请求对方锁定的记录，互相等待对方释放锁<br/>
    超时释放锁；回滚事务

</details>

# spring
## spring中bean对象是单例还是多例
<details> 
    <summary></summary>
    默认是单例的
</details>

## 怎么设置多例
<details> 
    <summary></summary>
    bean标签中设置scope属性
    <pre><code>
    &ltbean id="..." class="..." scope="singleton">&lt/bean>
    </pre></code>
    使用scope注解设置
    <pre><code>@Bean
@Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE)
public Person personPrototype() {
    return new Person();
}</pre></code>

</details>

## 单例的bean对象线程安全么
<details> 
    <summary></summary>
    无状态bean是安全的：工具类bean、dao、service，创建一个就能一直用的<br/>
    有状态bean是不安全的：成员变量存储数据的bean，可能会对bean内成员变量操作的
</details>

## 如何保证bean对象线程安全
<details> 
    <summary></summary>
    <b>Threadlocal</b><br/>
    使用多例模式（prototype）<br/>

</details>

## spring中用到了哪些设计模式
<details> 
    <summary></summary>
    
</details>

## spring的aop是什么？
<details> 
    <summary></summary>
    创建AOP类（@Compent、@Aspect），定义切入点（@Cutpoint），定义通知类型（@Around、@Before、@After）。<br/>
    抽取公用代码来简化代码。<br/>
    多个切面使用@Order(1)定义优先级，值越小越优先。
</details>

## 如何实现的
<details> 
    <summary></summary>
    代理
</details>



# 操作系统
## 操作系统中线程与进程关系/区别
<details> 
    <summary></summary>
    进程是系统资源分配的单位，线程是cpu分配的单位。<br/>
    一个进程包含了一个或多个线程，同一个进程中的线程共享它们所在进程的资源。<br/>
    线程提高了系统的并发性，同一个进程中的线程切换开销较小。
</details>

## 进程通信方式
<details> 
    <summary></summary>
    信号量、套接字、消息队列、管道
</details>

## 线程的生命周期
<details> 
    <summary></summary>
    创建态、结束态、就绪态、执行态、阻塞态<br/>
    在java中：创建态、结束态、就绪态、阻塞态、等待态、超时等待态
</details>

## 怎样会进入阻塞状态
<details> 
    <summary></summary>
    缺乏某一系统资源而导致即使获得cpu也无法继续执行。例如等待io
</details>

## 同步、异步 一句话介绍一下
<details> 
    <summary></summary>
    同步是进程在请求资源时会阻塞，获得资源后才继续执行下面的操作<br/>
    异步是进程在请求资源时等待的同时继续执行下面的操作。
</details>


# Java
## Java GC操作底层发生了什么
## Java中创建线程有几种方式，分别是什么
<details> 
    <summary></summary>
    自定义类继承Thread，重写run方法，创建实例后调用start方法启动
    <pre><code>Thread myThread=new MyThread();
myThread.start();</code></pre>
    自定义类实现Runnable接口，重写run方法
    <pre><code>MyThread myThread=new MyThread();
Thread t1=new Thread(myThread);
t1.start();</code></pre>
自定义类实现Callable接口，重写call方法
    <pre><code>MyCall myCall=new MyCall();//自定义类
FutureTask<> ft=new FutureTask<>(myCall);
Thread t=new Thread(ft);
t.start();
ft.get();//可以获取线程执行返回值。</code></pre>
</details>

## Java线程池创建需要哪些核心参数
<details> 
    <summary></summary>
    核心线程数量、最大线程数量、空闲线程存活时间、时间单位、任务队列、线程工厂、拒绝策略
</details>

## 线程安全的map
<details> 
    <summary></summary>
    Hashtable
</details>

## 说说java集合
<details> 
    <summary></summary>
    1.collection接口存放单一元素，map接口存放键值对<br/>
    2.List:<br/>
    ArrayList有序可重复<br/>
    3.Set:<br/>
    HashSet无序唯一元素，基于HashMap实现<br/>
    LinkedHashSet双链表<br/>
    TreeSet红黑树<br/>
    4.map:<br/>
    HashMap，哈希数组和链表存储，链表大于8时，若数组大于64会转换为红黑树，否则先进行数组扩容<br/>
    LinkedHashMap双链表<br/>
    TreeMap红黑树<br/>

</details>

## HashMap和Hashtable区别
<details> 
    <summary></summary>
    线程安全、效率、是否支持null、会不会转换为红黑树、扩容机制
</details>

## HashMap扩容为什么是扩为两倍？
<details> 
    <summary></summary>
    1 减少元素位置的移动。<br/>
    2 使元素均匀的散布hashmap中，减少hash碰撞。<br/>
    https://zhuanlan.zhihu.com/p/541944778
</details>

## CAS，优缺点，缺点的解决
<details> 
    <summary></summary>
    https://javaguide.cn/java/concurrent/java-concurrent-questions-02.html#%E4%BB%80%E4%B9%88%E6%98%AF%E4%B9%90%E8%A7%82%E9%94%81<br/>
    属于乐观锁。只在修改值的时候进行比较，不会加锁，所以不会阻塞线程，不会死锁。<br/>
    要更新的值与预期值相等时才进行更新。CAS操作本身是原子性的，由操作系统提供。<br/>
    ABA问题：虽然验证数据时是一样的，但是也可能是改过的，用版本号解决。<br/>
    自旋：验证失败时会一直循环重试，造成cpu开销<br/>
    只能对一个共享变量进行原子操作
</details>

## syn锁 lock锁 可重入锁 公平锁和非公平锁
<details> 
    <summary></summary>
    公平锁：先申请的先获得<br/>
    非公平锁：随机或其他优先级获得锁，性能好，但可能有的线程永远不会获得锁。<br/>
    可重入锁：线程可以再次获得内部锁。synchronized、ReentrantLock都是<br/>

</details>

## volatile
<details> 
    <summary></summary>
    1.防止编译器优化，让共享变量可以被不同线程可见，每次读取该变量都去主存中读取。<br/>
    2.防止指令重排序，jvm可能会先执行后面的语句再执行前面的语句
</details>

## ThreadLocal
<details> 
    <summary></summary>
    多个线程可以操作ThreadLocal类对象（全局变量），可以进行set和get，每个线程只能获取到自己线程set的值。做到了线程拥有本地变量。原理是ThreadLocal为每个线程（通过Thread.currentThread()获取具体的线程）创建ThreadLocalMap用来存数据。
</details>



有哪些锁（volatile vs synchronized vs ReentrantLock）
synchronized底层实现


# redis
## redis常用数据结构，排行榜使用什么
<details> 
    <summary></summary>
    String、哈希表、集合、列表、有序集合
</details>

## 缓存穿透，雪崩，击穿，如何解决
<details> 
    <summary></summary>
    穿透：对缓存和数据库都不存在的数据提出大量无理的请求，由于缓存中并没有所要的数据，导致大量请求直接发送到数据库，对数据库造成巨大压力。缓存无效key、布隆过滤器<br/>
    雪崩：缓存数据在同一时间大面积失效，导致大量请求直接发送到数据库。（大面积失效）。设置随机失效时间。<br/>
    击穿：热点数据在缓存中不存在或失效，导致大量请求直接发送到数据库。（热点数据失效）设置合理的过期时间、设置互斥锁，第一个请求负责查询数据和更新缓存，其余可以返回空数据。<br/>

</details>

## redis做缓存的时候，怎么保证mysql中的数据和redis同步（双写一致性）
<details> 
    <summary></summary>
    为什么会不一致：先删除缓存再修改数据库（删除之后，修改之前，旧数据被其他线程读到缓存），先修改数据库再删除缓存（缓存失效，a线程读到旧数据，b线程这时修改数据和删缓存，a线程存到缓存），因此先改数据库好一些。<br/>
    可以用读写锁。降低并发性。
</details>

## redis缓存怎么数据持久化
<details> 
    <summary></summary>
    1.RDB（redis database backup file），将内存中的数据写到磁盘中。开启子进程（共享主进程内存）后台写入磁盘。 恢复快，根据备份间隔可能会有不同的丢失情况<br/>
    2.AOF（append only file），记录每一条操作命令到该文件中。 恢复慢，一般间隔一秒记录，最多只丢一秒的数据
</details>

## 分布式锁
<details> 
    <summary></summary>
    因为业务会分配到不同的服务器，所以几台服务器需要从外部获取同一个锁。<br/>
    set key value nx ex 10。key添加成功会返回1，失败返回0，所以可以看成是否获取锁成功。必须写在一句才是原子性的，不能先设置key value再对这个key设置过期时间。因为只设置了锁没设置过期时间的时候宕机（业务服务器宕机，不是redis服务器宕机），这个key就会一直存在，造成死锁。<br/>
    redission实现了监控业务自动给锁续期。
</details>



# 计算机网络
## http状态码含义
<details> 
    <summary></summary>
    200成功<br/>
    300重定向<br/>
    400客户端错误<br/>
    500服务端错误<br/>
</details>

## https用的是对称加密还是非对称加密
<details> 
    <summary></summary>
    //客户端从CA机构获取服务器公钥进行加密，服务器用私钥解密。<br/>
    第一次使用非对称加密传输对称加密的密钥，之后使用该对称加密的密钥进行对称加密。
</details>

## cookie和session
<details> 
    <summary></summary>
    0.http协议是无状态的，因此使用cookie和session记录状态
    1.cookie存储在客户端，session存储在服务端，安全性高
    2.将sessionID存储在cookie中，就可以通过cookie确定session。就不需要将敏感信息存储在cookie中。
</details>


# 盲区
1、cpu三级缓存，每层干什么
3、http长连接如何实现
4、get post区别，get请求参数过长如何解决
5、用户态，内核态区别，java线程属于哪一个状态原因，如何实现读写并发，读写内存
7、锁的实现方式，公平非公平锁优缺点

10、jvm类加载过程
17、订单多次创建，幂等性问题



（2）进程和线程，启动jvm是一个什么？进程or线程
（3）jvm实现读取windows文件流的整个过程是什么样


面经1
简单介绍项目
知道哪些数据结构以及他们的特点
链表增删快，那如何提高其查询效率，有没有什么想法？
B+树了解吗？B+树如何范围查询？B+树退化的极端情况是什么？
跳表了解吗？
大顶堆、小顶堆了解吗？
实现长地址请求到服务端，然后服务端重定向短地址给客户端，如何实现长短地址的互相映射？
那我现在有10份数据，有1000个线程来争抢，你要怎么处理？
分布式是什么？为什么要分布式？分布式又会有哪些问题？分布式系统是如何实现事物的？
Redis集群了解吗？如何处理宕机的情况？Redis的同步策略？
LRU算法了解吗？你会如何实现它？这个算法可以应用在哪些场景下？
TCP为什么是三次握手？两次行不行？多次行不行？
TCP的安全性是如何实现的？两台服务器之间可以同时建立多条TCP链接吗？怎么实现的？
客服端输入一个网址后，是如何拿到客服想要的数据的，是怎样在网络中传输的？

java有哪些锁？共享锁是什么？CAS？乐观锁和悲观锁？synchronied的底层原理？锁升级？死锁怎么形成的？如何破解死锁？
面经2

Map的遍历方式

Java线程同步机制（信号量，闭锁，栅栏）
对volatile的理解：常用于状态标记
八种基本数据类型的大小以及他们的封装类（顺带了解自动拆箱与装箱）
线程阻塞几种情况？如何自己实现阻塞队列？
Java垃圾回收。可达性分析->引用级别->二次标记（finalize方法）->垃圾收集 算法（4个）->回收策略（3个）->垃圾收集器（GMS、G1）。
java内存模型
TCP/IP的理解
ThreadLocal（线程本地变量），如何实现一个本地缓存
JVM内存区哪里会出现溢出？
双亲委派模型的理解，怎样将两个全路径相同的类加载到内存中？
CMS收集器和G1收集器
TCP流量控制和拥塞控制
服务器处理一个http请求的过程



面向对象的设计原则
策略模式的实现
操作系统的内存管理的页面淘汰 算法 ，介绍下LRU（最近最少使用算法 ）
B+树的特点与优势
面经3
自我介绍，说简历里没有的东西
说几个你最近在看的技术（MySQL，多线程）
口述了一个统计数据的场景题
如果这个统计数据场景不用MySQL，而是用Java来实现，怎么做
如果数据量过大，内存放不下呢
用面向对象的思想解决上面提出的问题，创建出父类，子类，方法，说一下思路
下一个场景，口述了一个登录场景，同学用线程池做登录校验，会有什么问题
如何解决这些问题
你给出的方案弊端在哪里，还有哪些方案
面经4
谈谈类加载机制。
hashmap和concurenthashmap
16g机器，让你分配jvm内存怎么分配。
机器慢了怎么排查。
谈谈consul和zookeeper，还有服务发现机制。
详细说明raft协议。
谈谈consul和zookeeper区别。
服务注册的时候发现没有注册成功会是什么原因。
讲讲你认为的rpc和service mesh之间的关系。


自我介绍
hashmap
他的线程安全类
hashmap是会死锁的, 你知道吗(头插法会死锁)
动态代理和静态代理(jdk原生或者cglib, 答得不好)
jvm的理解(数据区,回收器,对象内存分布,回收算法)
常见的7个GC回收器
mysql的数据引擎有哪些, 区别
索引失效的情况

事务隔离级别, 默认级别
说说你对redis的理解(答做缓存,5个基础数据结构,感觉答的不是很好)
说说你对rabbitmq的理解(生产者,消费者,队列,交换机, 消息生产消费的工作流程, 工作模式, 死信队列)
如何保证幂等性(rabbitmq中要保持交换机,队列,消费者,三者一对一对一; kafka的话是通过offset,说白了这个问题就是问如何保证消息不重复消费,我可能答混了)
还了解哪些消息队列,(kafka,rocketmq)
什么是雪花算法(这个不熟, 只知道是推特出的,分布式ID用的,然后面试官做了一些补充)
场景问题:
高可用如何保证(首先机器要24小时运行, 然后还要保证数据一致性, 持久化, 集群之类的, 这种题目我是没了解过, 全凭感觉回答, 感觉也答得不好)

●tcp和udp区别

●tcp可靠性是怎么达到的

●tcp的拥塞控制

●Linux机器路径下的文件打开计算机干了什么，操作系统干了什么

●磁盘结构

●为什么区分用户态和内核态
●进程切换和线程切换都会干什么

●Java中都有哪里用到了红黑树 

●红黑树相比avl树优点在哪里

●手写单例模式

●volatile的用处，除了保证可见性还能够干什么

●JVM中哪里会发生OOM

●写个方法让方法区或元空间OOM，看实在不会，问给个思路就可以了

●什么情况会导致栈的OOM 说了个递归调用（傻子）

●JVM什么时候会将对象从新生代移动到老年代

●数据库底层的索引结构是什么



●为什么递增字段的作为主键会更好

●mysql中的undolog和redolog，binlog

分布式锁怎么实现的？setnx有啥缺陷
Springboot如何开发一个http接口
Springmvc处理请求的流程

BeanFactory和FactoryBean有啥区别
Mybatis动态sql

Redis在项目中的作用
缓存用啥结构
Redis存String转map和直接存map的区别
Redis持久化方式
Ngnix怎么配置不同接口映射到不同服务器
Kafka用过吗？用来做啥，原理，为啥顺序写
面向对象的特性
多态的特点
Java是多继承吗？为啥不能多继承
抽象类和接口的区别
Java集合？hashmap的原理（老八股了）
红黑树是啥样的（不会）
Hashmap线程安全吗？ConcurrentHashMap如何实现线程安全的。1.7对比1.8
Synchronize和ReentrantLock的区别，原理。
锁升级
Synchronize的作用域。实际用过吗
run和start的区别
线程池核心参数。自己补充了执行流程等
线程池核心线程数量怎么设置，为啥
上下文切换有啥操作
虚拟机栈的结构。执行方法的流程
AQS了解吗？原理是啥
JVM的运行时数据区，方法区里有啥
Mysql的索引结构？优点啥啥的
一页存2000w怎么算出来的（自己挖的坑）
索引失效的情况。
事务和日志的原理。
脏读。怎么解决。原理
归并排序
Url解析到显示
Http1.0和2.0的区别
Get Post的区别
浏览器对url有什么限制
有什么你擅长的我没问到的？（不知道说啥，说了算法）
回形打印（口述思路）
写一个http接口（主要是看代码规范）

6.5一面 大量常规八股

1.hashmap linkedhashmap treemap

2.线程池





5.mysql事务 隔离级别

6.索引结构

7.索引什么时候会失效

8.查询优化

9.mysql行锁

10. redolog redolog binlog mvcc

11.主从原理


13.持久化技术

14.分片集群

15.哨兵机制

16.淘汰策略

算法：相交链表

十分钟后电话通知过了，直接约了当晚二面

二面30%技术 70%聊天

1.聊到了redis底层数据结构

2.分布式锁

3.项目遇到了什么问题 怎么解决

其他问了很多软的  比如怎么学习的  怎么迅速融入公司  怎么应对复杂需求这种

等结果中