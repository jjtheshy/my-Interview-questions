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



## 14. Redis是单线程为什么还这么快?

1. 命令的执行是基于内存的,速度比较快
2. 命令的执行是单线程的,没有线程切换的开销
3. 最重要的原因是基于多路复用的NIO模型提升Redis的I/O利用率
4. 高效的数据存储结构 例如:跳表,压缩列表,等等





























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







## 3.翻转单项链表

```
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

