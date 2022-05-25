# 答题技巧:

尽量使用分总结构回答问题

总: 当前问题有什么具体的点

分: 1234  突出技术名词(核心概念,类,接口,核心的方法,)模糊的地方直接跳过去方法的具体执行流程不用说

# 八股文

## 1.String为什么设计成不可变的？

​	①为了实现String pool java中有大量的字符串的使用，重复的字符串就直接指向对象

​	如果是可变的当被修改后其他的指向就不正确了，所以设计成不可变的

​	②为了多线程的安全，在并发场景下，多个线程同时读一个资源，是安全的，不会引发竞争，

​	但对资源进行写操作时是不安全的，不可变对象不能被写，所以保证了多线程的安全

​	③为了加快字符串处理速度，由于String是不可变的，保证了hashcode的唯一性。

​	创建对象时其hashcode就可以放心的缓存了，不需要重新计算。这也就是Map喜欢将String

​	作为Key的原因，处理速度要快过其它的键对象。

## 2.Spring中监听程序启动的三个方法？

​	①tomcat创建的Listener，但是不是spring创建的，许多对象属性无法注入

​	②spring本身有一个Listener，在内部创建对象，我们知道对象有init-method方法(postconstruct注解)，

​	 在这里就能知道方法初始化缓存，对象被创建了

​	③spring程序启动完成会自动发布一个事件，名称是ContextRefreshedEvent,通过监听它就可以知道程序启动。

## 3.CopyOnWriteArrayList集合

​	①写有锁，读无锁，读写之间不阻塞，优于读写锁。

​	②先复制一个数组，读写操作在复制的数组上进行，最后替换引用。

 ## 4.为什么new一个 string会创建两个对象?

​	一个对象是:new关键字在堆空间创建的

​	一个是:字符串常量池中的对象	

 ## 5.switch 是否能作用在 byte 上，是否能作用在 long 上， 是否能作用在 String 上？

​	在 Java 5 以前，switch(expr)中，expr 只能是byte、short、char、int。Java 5 开始，Java 中引入了枚举类型，expr 也可以是 enum 类型，从 Java 7 开始， expr 还可以是字符串（String），但是长整型（long）在目前所有的版本中都是 不可以的。

 ## 6.两个对象值相同(x.equals(y) == true)，但却可有不同的 hash code，这句话对不对？

​	不对，如果两个对象 x 和 y 满足 x.equals(y) == true，它们的哈希码（hash code） 应当相同。Java 对于 eqauls 方法和 hashCode 方法是这样规定的：(1)如果两个对象相同（equals 方法返回 true），那么它们的 hashCode 值一定要相同；(2) 如果两个对象的 hashCode 相同，它们并不一定相同。

> equals 方法:
>
> 1.自反性（x.equals(x)必须返回 true）
>
> 2.对称性（x.equals(y)返回 true 时，y.equals(x)也必须返回 true）
>
> 3.传递性 （x.equals(y)和 y.equals(z)都返回 true 时，x.equals(z)也必须返回 true）
>
> 4.一 致性（当 x 和 y 引用的对象信息没有被修改时，多次调用 x.equals(y)应该得到同 样的返回值），而且对于任何非 null 值的引用 x，x.equals(null)必须返回 false。

## 7.char 型变量中能不能存贮一个中文汉字，为什么?

​	char 类型可以存储一个中文汉字，因为 Java 中使用的编码是 Unicode（不选择 任何特定的编码，直接使用字符在字符集中的编号，这是统一的唯一方法），一 个 char 类型占 2 个字节（16 比特），所以放一个中文是没问题的。

## 8.Stream流跳出forEach的方法

stream.forEach()循环

1. 处理集合时不能使用`break`和`continue`中止循环；
2. 可以使用关键字`return`跳出本次循环，并执行下一次遍历。
3. 抛异常跳出stream

## 9.抽象的（abstract）方法是否可同时是静态的（static）, 是否可同时是本地方法（native），是否可同时被 synchronized 修饰？

都不能。

1. 抽象方法需要子类重写，而静态的方法是无法被重写的，因此二者是矛盾的。

2. 本地方法是由本地代码（如 C 代码）实现的方法，而抽象方法是没有实现的，也是矛盾的。

3. synchronized 和方法的实现细节有关，抽象方法不涉及实现细节，因此也是相互矛盾的。

## 10.为什么 char 数组比 Java 中的 String 更适合存储密码

将密码存储为纯文本，它将在内存中可用，直到垃圾收集器清除它，并且为了可重用性，会存在 [String](http://mp.weixin.qq.com/s?__biz=MzI3ODcxMzQzMw==&mid=2247492247&idx=2&sn=261897add1b2b8d8014fd13cc6d8c0eb&chksm=eb5067a1dc27eeb7bc71fa85609fcce42222c89808152785b481057193e5eeae847693e815da&scene=21#wechat_redirect) 在字符串池中, 它很可能会保留在内存中持续很长时间，从而构成安全威胁。**在字符数组中存储密码可以明显降低窃取密码的安全风险。**

## 11.强引用、软引用、弱引用、幻象引用有什么区别

1. 所谓强引用（"Strong" Reference），就是我们最常见的普通对象引用,只要还有强引用指向一个对象，就能表明对象还“活着”，垃圾收集器不会碰这种对象。
2. 软引用（SoftReference），是一种相对强引用弱化一些的引用，可以让对象豁免一些垃圾收集，只有当 JVM 认为内存不足时，才会去试图回收软引用指向的对象。JVM 会确保在抛出OutOfMemoryError 之前，清理软引用指向的对象。软引用通常用来实现内存敏感的缓存，如果还有空闲内存，就可以暂时保留缓存，当内存不足时清理掉，这样就保证了使用缓存的同时，不会耗尽内存。
3. 弱引用和软引用的区别在于：弱引用拥有更短暂的生命周期，不管内存够不够，都会回收，都会回收它的内存。
4. 虚引用，**形同虚设** ，虚引用不会决定对象的生命周期，如果一个对象仅持有虚引用，其实就和没有任何引用一样。在任何时候都可能被垃圾回收器回收。 虚引用和软引用的一个区别是，虚引用必须和引用队列（ReferenceQueue）联合使用。当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之关联的引用队列中。



## 12. 为什么HashMap中数组的长度始终是2 << n？

## 散列算法？

## HashMap如何尽量避免Hash碰撞？

​	将key的hashcode的高低16位进行^运算，将结果对数组长度 - 1,进行&运算。



## 13.ORM和JPA分别什么意思?有什么关系?

ORM: 是一种思想是对象关系映射,是以对象的方式去操作数据库

JPA: 是Java的规范,是基于ORM思想实现的 其常用的注解有

```
@Entity   加在类上,代表当前类是实体类
@Table    加载类上,代表实体类映射的哪张表
@Id		  加在属性上,代表属性映射表中的主键
@GeneratedValue   加在@Id的属性上,指定主键的生成策略'
@Column   加在普通的属性上,代表指定映射的字段名
@Transient  加在普通属性上,代表忽略当前的属性
```



## 14. MyBatis一级缓存和二级缓存的区别？

| 区别     | 一级缓存             | 二级缓存                                                     |
| -------- | -------------------- | ------------------------------------------------------------ |
| 作用域   | SqlSession           | SqlSessionFactory                                            |
| 默认开关 | 默认开启             | 默认关闭(全局开关默认开启,映射文件开关默认关闭,select语句开关默认开启) |
| 存储内容 | 存储对象             | 存储对象的byte[]                                             |
| 存储过程 | 直接找PerpetualCache | 经过一道Cache链，再查询PerpetualCache                        |
| Executor | BaseExecutor         | CachingExecutor                                              |
| 优先级   | 低                   | 高                                                           |



## 15. ReentrantLock的实现原理？

ReentrantLock是基于AQS实现,AQS是AbstractQueuedSynchronizer，是JUC包下的一个基类, AQS中主要维护了一个双向链表的队列，以及一个state线程需要获取锁资源时，以CAS尝试将state从0~1，如果修改成功，代表获取锁资源成功，如果修改失败，将其放入队列排队



## 16. 事务的四大特性?

事务的四大特性
	原子性：事务是一个最小的执行单位，一次事务操作要么都成功，要么都失败。
	一致性：
		1.一次事务前后，数据总量不变。
		2.事务操作后，必须遵循表的约束。
		3.事务提交后，预期结果和真实结果是一致的。
	隔离性：每个事务是独立存在的互不干扰。
	持久性：一旦提交事务，数据会持久化到磁盘中。

## 17. 关于包装类的缓存区?

jdk1.5后，当需要进行**自动装箱**（Integer.valueOf()），如果在数字在**[-128,127）**之间，会直接使用缓存（**cache[]**）中的对象，而不是重新创建一个对象。**享元设计模式**。

String常量池、数据库连接池、缓冲池都是享元设计模式，享元模式，运用共享技术有效地支持大量细粒度的对象

```
Byte [-128~127]
Short [-128~127]
Integer [-128~127]
Long  [-128~127]
Boolean  true false
Character [0~127]
Float  new对象  不带缓存
Double new对象  不带缓存
```

除了Character和Float Double之外其他的包装类的缓存区都是-128到127之间

## 18. 在java中，匿名内部类对象使用外部方法中的局部变量需要加final关键字，为什么?

生命周期不同，因为局部变量直接存储在栈中，当方法执行结束，非final的局部变量就被销毁，而局部内部类对局部变量的引用依然存在，当局部内部类要调用局部变量时，就会出错，出现非法引用。



## 19. 简述常见的http响应码及含义。

2xx 成功
		200 成功
		201 创建成功
		202 接收成功
	3xx 重定向
		301 永久移动至url
		302 找到了但是不在原位置
		303 查看其它内容
		304 305 使用代理
	4xx 客户端错误
		400 请求错误
		401 授权问题
		402 需要购买
		403 禁止请求
		404 未找到资源
	5xx 服务器错误
		500 服务器内部错误
		504 网关超时
		505 http版本不匹配

## 20. Base64编码规则?一共有多少种字符？

1、每6位拿出来组成一个部分。
2、每个部分前面添加两个00组成一个字节。
3、每个字节对应的数字取查表找到对应的字符。
4、把所有部分的字符按顺序组合起来。
源数据n个字节
当n%3\==0，就是正好的情况。
当n%3\==1，最后补两个=号。
当n%3\==2，最后补一个=号。

包涵65种

## 21.Get方式请求服务器，传输base64类型的数据可以吗？

Base64编码中包含了/和=，这两个字符在url有其他含义，因此不适合在get方式下传输。如果非要用get传输，可以把base64数据再用urlencode处理一下，接到以后，再urldecode就可以了。



## 22. JDK底层的排序?

一种是双轴快排,另一种是三项快排

三项切分快速排序:就是把数组分割成小于pivot,等于pivot大于pivot的三组,当要排列的数组中包涵很多相同的元素时相同元素就不需要排序

双轴快速排序: 双轴快排是为了减小出现基准数pivot出现选择的是最大最小的概率,如果基准数选的是最大或最小值快排的时间复杂度是最坏的情况O(n^2^)

![JDK快速排序](https://alibabapicbed.oss-cn-beijing.aliyuncs.com/img/JDK快速排序.png?Expires=1653484707&OSSAccessKeyId=TMP.3Kj1y464J5mHMm9jtNuzuDkhafPEGDgFFcxHNj3Tpf6TDYuWrnjcrckDs1PPhCgXJupVgiEuNv2Qwoe9XLDjXFDeG86PyF&Signature=A4852rRMLw3MmHlvT0ft3D1ofGs%3D)



## 23 .SpringMVC中方法返回一个View，用一个AJAX调用这个方法，页面会跳转吗？为什么？

不会发生跳转。SpringMVC返回的View本质上就是jsp，jsp本质上就是打印html格式的数据。AJAX里的XMLHttpRequest对象的响应数据里，出现的都是html数据。如果是浏览器的请求对象，那么就会发生跳转。也就是说，对于跳转这件事情而言，不仅仅是后端需要提供数据，前端也应该使用对应的请求对象。

## 24. SpringBoot有什么好处，为什么现在推荐使用SpringBoot？

1、简化配置，使得工程开发更加简单。
2、内置Tomcat 无需打包成war  而是打包成jar，在任意JVM环境下都可以运行，使得运维方便。贴合容器技术。
3、使用xxxx-starter管理依赖，并统一进行版本控制。
4、大量的自动配置，方便集成第三方。
5、对于spring而言无需xml配置，减少代码冗余，开箱即用。
6、向后兼容SpringCloud，为以后项目升级与重构提供便捷。





































































# Redis

## 1. Redis是单线程为什么还这么快?

1. 命令的执行是基于内存的,速度比较快
2. 命令的执行是单线程的,没有线程切换的开销
3. 最重要的原因是基于多路复用的NIO模型提升Redis的I/O利用率
4. 高效的数据存储结构 例如:跳表,压缩列表,等等



## 2. Redis的底层数据结构

![db-redis-object-2-2](https://cdn.jsdelivr.net/gh/jjtheshy/picgo-bed/img/db-redis-object-2-2.png)

- 简单动态字符串 - sds
- 压缩列表 - ZipList
- 快表 - QuickList
- 字典/哈希表 - Dict
- 整数集 - IntSet
- 跳表 - ZSkipList

## 简单动态字符串 - sds

> `len` 保存了SDS保存字符串的长度
>
> `buf[]` 数组用来保存字符串的每个元素
>
> `alloc`分别以uint8, uint16, uint32, uint64表示整个SDS, 除过头部与末尾的\0, 剩余的字节数.
>
> `flags` 始终为一字节, 以低三位标示着头部的类型, 高5位未使用.
>
> 这是一种用于存储二进制数据的一种结构, 具有动态扩容的特点.

为什么使用SDS

![image-20220503193710440](https://cdn.jsdelivr.net/gh/jjtheshy/picgo-bed/img/image-20220503193710440.png)

- **常数复杂度获取字符串长度**

- **杜绝缓冲区溢出**,对于 SDS 数据类型，在进行字符修改的时候，**会首先根据记录的 len 属性检查内存空间是否满足需求**，如果不满足，会进行相应的空间扩展，然后在进行修改操作，所以不会出现缓冲区溢出。

- **二进制安全**

## 压缩列表 - ZipList

> ziplist是为了提高存储效率而设计的一种特殊编码的双向链表。它可以存储字符串或者整数，存储整数时是采用整数的二进制而不是字符串形式存储。他能在O(1)的时间复杂度下完成list两端的push和pop操作。但是因为每次操作都需要重新分配ziplist的内存，所以实际复杂度和ziplist的内存使用量相关。
>
> 简单的说:列表元素较少的情况下使用一块连续的内存存储,这个结构就是ziplist



## 快表 - QuickList

> 它是一种以ziplist为结点的双端链表结构. 宏观上, quicklist是一个链表, 微观上, 链表中的每个结点都是一个ziplist。

![db-redis-ds-x-4](https://cdn.jsdelivr.net/gh/jjtheshy/picgo-bed/img/db-redis-ds-x-4.png)



## 字典/哈希表 - Dict

如何解决哈希冲突，有开放地址法和链地址法。Redis采用的便是链地址法，通过next这个指针可以将多个哈希值相同的键值对连接在一起，用来**解决哈希冲突**。

## 跳表 - ZSkipList

![db-redis-ds-x-10](https://cdn.jsdelivr.net/gh/jjtheshy/picgo-bed/img/db-redis-ds-x-10.png)



 ## 为什么不用平衡树或者哈希表而是用跳表

skiplist和各种平衡树（如AVL、红黑树等）的元素是有序排列的，而哈希表不是有序的。因此，在哈希表上只能做单个key的查找，不适宜做范围查找。所谓范围查找，指的是查找那些大小在指定的两个值之间的所有节点。

在做范围查找的时候，平衡树比skiplist操作要复杂。在平衡树上，我们找到指定范围的小值之后，还需要以中序遍历的顺序继续寻找其它不超过大值的节点。如果不对平衡树进行一定的改造，这里的中序遍历并不容易实现。而在skiplist上进行范围查找就非常简单，只需要在找到小值之后，对第1层链表进行若干步的遍历就可以实现。

平衡树的插入和删除操作可能引发子树的调整，逻辑复杂，而skiplist的插入和删除只需要修改相邻节点的指针，操作简单又快速。

从内存占用上来说，skiplist比平衡树更灵活一些。一般来说，平衡树每个节点包含2个指针（分别指向左右子树），而skiplist每个节点包含的指针数目平均为1/(1-p)，具体取决于参数p的大小。如果像Redis里的实现一样，取p=1/4，那么平均每个节点包含1.33个指针，比平衡树更有优势。

查找单个key，skiplist和平衡树的时间复杂度都为O(log n)，大体相当；而哈希表在保持较低的哈希值冲突概率的前提下，查找时间复杂度接近O(1)，性能更高一些。所以我们平常使用的各种Map或dictionary结构，大都是基于哈希表实现的



## 3. Redis持久化策略?

RDB(Redis DataBase): 默认开启

> 每隔一段时间,将数据集快照写入硬盘,使用了`写时复制技术`Reds会单独创建(fork)一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件昔换上次持久化好的文件dump.rdb 。整个过程
> 中，主进程是不进行任何IO操的，这就确保了极高的性能如果需要进行大规模数据的恢复，速度快,且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失。

AOF(Append Of File): 默认不开启

> 以日志的形式来记录每个写操作（增量保存），将Rdis执行过的所有写指令记录下来（读操作不记录），只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作;备份机制更加稳健,数据丢失概率更低,但是占用空间大,恢复备份速度慢,有一定的性能压力
>
> ReWrite压缩: AOF文件的大小超过所设定的阈值64MB时，Redis就会启动AOF文件的内容压缩，只保留可以恢复数据的最小指令集
>
> 备份恢复过程:
>
> (1)客户端的请求写命令会被append追加到AOF缓冲区内；
> (2)AOF缓冲区根据AOF持久化策略[always,everysec.,no]将操作sync同步(也是`写时复制技术`)到磁盘的AOF文件中；
> (3)AOF文件大小超过重写策略或手动重写时，会对AOF文件rewrite重写，压缩AOF文件容量；
> (4)Redis重启后,会重新加在AOF文件的写操作达到数据恢复的目的

## 4. Redis实现限流的三种方式

### 第一种：基于Redis的setnx的操作

> 依靠了setnx的指令，在CAS（Compare and  swap）的操作的时候，同时给指定的key设置了过期实践（expire），我们在限流的主要目的就是为了在单位时间内，有且仅有N数量的请求能够访问我的代码程序。所以依靠setnx可以很轻松的做到这方面的功能。比如我们需要在10秒内限定20个请求，那么我们在setnx的时候可以设置过期时间10，当请求的setnx数量达到20时候即达到了限流效果。

### 第二种：基于Redis的数据结构zset

> 我们可以将请求打造成一个zset数组，当每一次请求进来的时候，value保持唯一，可以用UUID生成，而score可以用当前时间戳表示，因为score我们可以用来计算当前时间戳之内有多少的请求数量。而zset数据结构也提供了range方法让我们可以很轻易的获取到2个时间戳内有多少请求

```java
public Response limitFlow(){
        Long currentTime = new Date().getTime();
        System.out.println(currentTime);
        if(redisTemplate.hasKey("limit")) {
            Integer count = redisTemplate.opsForZSet().rangeByScore("limit", currentTime -  intervalTime, currentTime).size();        // intervalTime是限流的时间 
            System.out.println(count);
            if (count != null && count > 5) {
                return Response.ok("每分钟最多只能访问5次");
            }
        }
        redisTemplate.opsForZSet().add("limit",UUID.randomUUID().toString(),currentTime);
        return Response.ok("访问成功");
    }

```

### 第三种：基于Redis的令牌桶算法

> 令牌桶算法提及到输入速率和输出速率，当输出速率大于输入速率，那么就是超出流量限制了。也就是说我们每访问一次请求的时候，可以从Redis中获取一个令牌，如果拿到令牌了，那就说明没超出限制，而如果拿不到，则结果相反。依靠上述的思想，我们可以结合Redis的List数据结构很轻易的做到这样的代码

只是简单实现依靠List的leftPop来获取令牌

```
public Response limitFlow2(Long id){
        Object result = redisTemplate.opsForList().leftPop("limit_list");
        if(result == null){
            return Response.ok("当前令牌桶中无令牌");
        }
        return Response.ok(articleDescription2);
    }
```

再依靠Java的定时任务，定时往List中rightPush令牌，当然令牌也需要唯一性，所以我这里还是用UUID进行了生成

```java
// 10S的速率往令牌桶中添加UUID，只为保证唯一性
    @Scheduled(fixedDelay = 10_000,initialDelay = 0)
    public void setIntervalTimeTask(){
        redisTemplate.opsForList().rightPush("limit_list",UUID.randomUUID().toString());
    }

```





















# Mysql



## 1. InnoDB引擎和MyISAM引擎的区别?

| 对比项 | **MyISAM**                                               | **InnoDB**                                                   |
| ------ | -------------------------------------------------------- | ------------------------------------------------------------ |
| 外键   | 不支持                                                   | 支持                                                         |
| 事务   | 不支持                                                   | 支持                                                         |
| 行表锁 | 表锁，即使操作一条记录也会锁住整个表，不适合高并发的操作 | 行锁，操作时只锁某一行，不对其它行有影响，适合高并发的操作   |
| 缓存   | 只缓存索引，不缓存真实数据                               | 不仅缓存索引还要缓存真实数据，对内存要求较高，而且内存大小对性能有决定性的影响 |
| 关注点 | 性能：节省资源、消耗少、简单业务                         | 事务：并发写、事务、更大资源                                 |



## 2. B+树一般不会超过4层,为什么? 好处?

树的高度低就会有更少的IO提升性能

一个数据页大小是16KB 估算一下的3层的b+树大约能存1亿条数据,4层的树能存一千亿条数据













































------

# 数据结构和算法

## 1.单向链表找到倒数第k个元素？

快慢指针,快指针走到k-1的位置,然后慢指针和快指针一起走,当快指针指向null时说明链表走到头了,这个时候慢指针指向的位置就是倒数第k个元素

```java
public class Node{
	int val;
	Node next;
}
public Node findLastButK(Node head,int k){
	if(head == null || k <= 0){
		return null;
	}
	Node fast = head;
	Node slow = head;
	int j = k - 1;
	while(fast.next != null){
		fast = fast.next;
		j--;
		if(j == 0){
			break;
		}
	}
	while(fast.next != null){
		fast = fast.next;
		slow = slow.next;
	}
	return slow;
}
```

## 2.冒泡和快排

```java
private void bubbleSort(int[] arr) {
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = 0; j < arr.length - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    int t = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = t;
                }
            }
        }
    }
```

```java
    private void quickSort(int[] arr, int left, int right) {
        if (left >= right) {
            return;
        }
        int i = left;
        int j = right;
        while (left < right) {
            //从后往前
            for (; left < right; right--) {
                if (arr[right] < arr[left]) {
                    int temp = arr[left];
                    arr[left] = arr[right];
                    arr[right] = temp;
                    left++;
                    break;
                }
            }
            //从前往后
            for (; left < right; left++) {
                if (arr[left] > arr[right]) {
                    int temp = arr[left];
                    arr[left] = arr[right];
                    arr[right] = temp;
                    right--;
                    break;
                }
            }
        }
        quickSort(arr, i, left - 1);
        quickSort(arr, left + 1, j);
    }
```



## 3.翻转单向链表

```java
public Node reverseNode(Node head){
	if(head == null || head.next == null) return head;
	//1. 声明另外两个指针
	Node pre = null;
	Node next = null;

	//2. 循环动~~~
	while(head.next != null){
		next = head.next;
		head.next = pre;
		pre = head;
		head = next;
	}

	//3. 最后再动动~~
	head.next = pre;

	//4. 返回
	return head;
}
链表类：
public class Node(){
	int val;
	Node next;
}
```



## 4.给出两个字符串，求出两个字符串的最长公共子集序列的长度

例子： 
	abcdefg，adfg  --> adfg  返回4
	abcdefg，adgf  --> adf，adg   返回3
	people，apply  --> ppl  返回3

![image-20220505084208782](https://cdn.jsdelivr.net/gh/jjtheshy/picgo-bed/img/image-20220505084208782.png)

```java
public int longestCommonSubsequence(String text1,String text2){
	if(text1 == null || text2 == null) return 0;
	//1. 获取到字符串长度
	int m = text1.length();
	int n = text2.length();

	//2. 声明二位数组
	int[][] lcs = new int[m+1][n+1];

	//3. 遍历字符串
	for(int i = 1;i <= m;i++){
		char c1 = text1.charAt(i - 1);
		for(int j = 1;j <= n;j++){
			char c2 = text2.charAt(j - 1);
			if(c1 == c2){
				lcs[i][j] = lcs[i-1][j-1] + 1;
			}else{
				lcs[i][j] = Math.max(lcs[i-1][j],lcs[i][j-1]);
			}
		}
	}

	//4. 返回最长公共子序列长度
	return lcs[m][n];
}
```

## 5. 使用两个栈结构实现一个队列结构？

栈：先进后出
队列：先进先出

放数据就是往第一个栈里面放,弹数据如果2栈里面没有就把1里面的数据转到2栈里面,从2栈里面弹出来

```java
public class Test(){
	Stack<Integer> stack1 = new Stack();
	Stack<Integer> stack2 = new Stack();

	public void push(int num){
		stack1.push(num);
	}

	public int pop(){
		if(stack2.empty()){
			while(!stack1.empty()){
				stack2.push(stack1.pop());
			}
		}
		return stack2.pop();
	}
}
```



## 6. 判断一个单向链表是否有环，并且找到环的触发点

快慢指针,相遇时将一个指针回到起始位置,再一个走一步再相遇的时候就是触发点

```java
public Node hasRing(Node head){
	if(head == null || head.next == null){
		return null;
	}
	// 声明快慢指针
	Node slow = head;
	Node fast = head;
	// 移动
	while(fast != null && fast.next != null){
		fast = fast.next.next;
		slow = slow.next;
		if(fast == slow){
			// 有环，找环的触发点
			fast = head；
			while(fast != slow){
				fast = fast.next;
				slow = slow.next;
			}
			// 返回触发点
			return slow;
		}
	}
	// 没有环
	return null;
}
```

## 7. 翻转单项链表

```java
    //递归
    private Node fanzhuan1(Node top) {
        Node p = top;
        if (p==null||p.next == null) {
            return p;
        }
        Node r = fanzhuan1(p.next);
        p.next.next = p;
        p.next = null;
        return r;
    }

    //三指针
    private Node fanzhuan2(Node top) {
        if (top.next == null) return top;
        Node p1 = top;
        Node p2 = top.next;
        Node p3 = top.next.next;
        p1.next = null;
        while (p3 != null) {
            p2.next = p1;
            p1 = p2;
            p2 = p3;
            p3 = p3.next;
        }
        //反转最后一个节点
        p2.next = p1;
        return p2;
    }
```

## 8.判断连个链表是否相交

1. 两个链表都不带环：
   把第一个链表的尾部与第二个链表的头部连接起来。判断第一个链表是否带环。带环就是有交点，不带环就是没有交点。
2. 两个链表一个带环，一个不带环：
   一定没有交点。
3. 两个链表都带环：
   - 缓存表方式，把第一个链表的元素都存到缓存表中，然后遍历第二个链表的所有
     元素，比对是否有重复节点。如果没有重复节点则没有交点，反之则有交点。
   - 把第一个链表的环拆掉，判断第二个链表是否带环。如果带环，则没有交点，如
     果不带环则有交点。

> 这两个链表分这三种情况进行分类讨论,代码实现如下:

```java
private boolean xiangjiaobu(Node top1, Node top2) {
        boolean result1 = youhuanbu(top1);
        boolean result2 = youhuanbu(top2);
        if (!result1&&!result2) {//1.都没环
            //找到第一个链表的尾
            Node p = top1;
            while (p.next != null) {
                p = p.next;
            }
            //将第一个链表的尾和第二个链表的头连接
            p.next = top2;
            //然后判断链表1是否带环
            boolean r = youhuanbu(top1);
            return r;
        } else if (result1 && result2) {//2.都有环
            Node p = chufadian(top1);
            //把一个链表的环断开,如果另一个链表没环了说明二者相交,否者就是不想交
            p.next = null;
            boolean r = youhuanbu(top2);
            return !r;
        } else { //3.一个有环一个没环,说明两个一定不想交.如果相交那个环肯定是共同都存在的
            return false;
        }
    }
```

## 9.约瑟夫环

41个人依次报数123-123-123,将数到3的人killed,站多少位置是最后killed的?  (16,31)

```JAVA
   public void test() {
        Node top = new Node();
        Node p = top;
        for (int i = 1; i <=41; i++) {
            p.value = i;
            if (i != 41) {
                p.next = new Node();
                p.next.prev = p;
                p = p.next;
            }
        }
        //将收尾相连
        p.next = top;
        top.prev = p;

        p = top;
        for (int i = 1; ; i++) {
            if (p.next == p) {
                System.out.println(p.value + " ");
                break;
            }
            if (i % 3 == 0) { //标号是3的人
                System.out.println(p.value + " ");
                //删除标号3的人
                p.prev.next = p.next;
                p.next.prev = p.prev;
            }
            p = p.next;
        }
    }
```

## 10. 走阶梯，有n级台阶一次可以走一级、两级或者三级问一共有多少种上法？

```java
public class Test11 {

    /**
     * 五十级台阶的结果是：10562230626642，缓存表优化后，耗时2毫秒。
     *                  10562230626642，使用循环优化后，耗时0毫秒。
     * @param args
     */
    public static void main(String[] args) {
        /**
         * 1 1 1 1 1 1
         * 2 1 1 1 1
         * 1 2 1 1 1
         * 1 1 2 1 1
         * 1 1 1 2 1
         * 1 1 1 1 2
         * 3 1 1 1
         * 1 3 1 1
         * 1 1 3 1
         * 1 1 1 3
         * 2 2 1 1
         * 2 1 2 1
         * 2 1 1 2
         * 1 2 2 1
         * 1 2 1 2
         * 1 1 2 2
         * 3 3
         * 1 2 3
         * 1 3 2
         * 2 1 3
         * 2 3 1
         * 3 2 1
         * 3 1 2
         * 2 2 2
         */
        long startTime=System.currentTimeMillis();
        System.out.println(f3(50));
        System.out.println("总耗时为："+(System.currentTimeMillis()-startTime)+"毫秒");
    }


    /**
     * 1 2 4 7 13
     * a b c d
     *   a b c d
     * 循环改造递归。
     */
    private static long f3(long n){
        if(n==1){
            return 1;
        }
        if(n==2){
            return 2;
        }
        if(n==3){
            return 4;
        }
        long a=1;
        long b=2;
        long c=4;
        long d=0;
        for (int i = 0; i < n-3; i++) {
            d=a+b+c;
            a=b;
            b=c;
            c=d;
        }
        return d;
    }

    /**
     * 使用缓存表优化方案优化裂项式递归
     *
     */
    private static HashMap<Long,Long> cache=new HashMap<>();
    private static long f2(long n){
        if(n==1){
            return 1;
        }
        if(n==2){
            return 2;
        }
        if(n==3){
            return 4;
        }
        //从缓存中取出结果
        Long cacheResult = cache.get(n);
        if(cacheResult!=null){
            //结果不为空就直接返回
            return cacheResult;
        }
        //如果上面没返回就说明这个n没有计算过，那就计算一下
        cacheResult=f2(n-1)+f2(n-2)+f2(n-3);
        //把计算的结果放进缓存表中
        cache.put(n,cacheResult);
        //返回结果
        return cacheResult;
    }
    /**
     * f1(n)
     * n<=3  f1(1)=1  f1(2)=2  f1(3)=4
     * n>3   f1(n)=f1(n-1)+f1(n-2)+f1(n-3)
     * @param n
     * @return
     */
    private static long f1(long n){
        if(n==1){
            return 1;
        }
        if(n==2){
            return 2;
        }
        if(n==3){
            return 4;
        }
        return f1(n-1)+f1(n-2)+f1(n-3);
    }
}
```

