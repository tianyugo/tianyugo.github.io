---
layout: post
title:  "Java关键知识记录"
date:   2020/04/08 10点33分        
categories: me 
tags: java
excerpt: 学习过程中记录下好的学习资源，方便自己复习
mathjax: true
author: Sky
---

* content
{:toc}

## 前言 ##
学习过程中记录好的学习资源，方便自己复习。

## 记录

### 以往整理

#### [自定义注解详细介绍](https://blog.csdn.net/xsp_happyboy/article/details/80987484)

#### [**Java hashCode()** **和** **equals()****的若干问题解答**](https://www.cnblogs.com/skywang12345/p/3324958.html)

### 2020年4月18日

#### [漫画：什么是 CAS 机制？](https://mp.weixin.qq.com/s?__biz=MzIxMjE5MTE1Nw==&mid=2653192625&idx=1&sn=cbabbd806e4874e8793332724ca9d454&chksm=8c99f36bbbee7a7d169581dedbe09658d0b0edb62d2cbc9ba4c40f706cb678c7d8c768afb666&scene=21#wechat_redirect)

#### [漫画：什么是CAS机制？（进阶篇）](https://mp.weixin.qq.com/s/nRnQKhiSUrDKu3mz3vItWg)

#### [Java并发编程：volatile关键字解析](https://www.cnblogs.com/dolphin0520/p/3920373.html)—含并发基础

### 2020年4月26日

#### [可能是最漂亮的Spring事务管理详解](https://juejin.im/post/5b00c52ef265da0b95276091)—含事务基础



- 事务：

事务是逻辑上的一组操作，要么都执行，要么都不执行.

- 事物的特性（ACID）：

**原子性：** 事务是最小的执行单位，不允许分割。事务的原子性确保动作要么全部完成，要么完全不起作用；

**一致性：** 执行事务前后，数据保持一致；

**隔离性：** 并发访问数据库时，一个用户的事物不被其他事物所干扰，各并发事务之间数据库是独立的；

**持久性:**  一个事务被提交之后。它对数据库中数据的改变是持久的，即使数据库发生故障也不应该对其有任何影响。

- 其他待深入学习+补充





#### [**一千行MySQL命令**]([https://github.com/Snailclimb/JavaGuide/blob/master/docs/database/%E4%B8%80%E5%8D%83%E8%A1%8CMySQL%E5%91%BD%E4%BB%A4.md](https://github.com/Snailclimb/JavaGuide/blob/master/docs/database/一千行MySQL命令.md))

捡到了大宝贝







#### ArraryList 扩容



ArrayList实现了List接口，继承了AbstractList，底层是数组实现的，

非线程安全的，一般多用于单线程环境下(与Vector最大的区别就是，Vector是线程安全的

Serializable接口，因此它支持序列化：但是：**private** **transient** Object[] elementData; 

注：假如elementData的长度为10，而其中只有5个元素，那么在序列化的时候只需要存储5个元素，而数组中后面5个元素是不需要存储的。于是将elementData定义为transient，避免了Java自带的序列化机制，并定义了两个方法，实现了自己可控制的序列化操作。（声明为static和transient类型的成员数据不能被序列化。因为static代表类的状态，transient代表对象的临时数据。）

实现了RandomAccess接口，支持快速随机访问(只是个标注接口，并没有实际的方法),这里主要表现为可以通过下标直接访问(底层是数组实现的，所以直接用数组下标来索引)

Cloneable接口，能被克隆



**无参构造方法**：public ArrayList()  默认提供容量为10的数组（add实现

自定义大小：public ArrayList(int initialCapacity)；

**add方法**：

超过当前集合的容量elementData.length，如果超过，则调用**grow**方法进行扩容：int newCapacity = oldCapacity + (oldCapacity >> 1), 扩容1.5倍









### 2020年4月27日

#### [Arrays.binarySearch()方法详解](https://www.cnblogs.com/TomHe789/p/12589056.html)

- 作用

  在**已排好序的数组**中，使用二分法查找指定的元素，返回元素下标，不存在也会返回一个下标。

  排序使用：`Arrays.sort(array);`

- 使用方法

  方法原型为：`public static int binarySearch(Object[] a, Object key)`.

  - 头包：`import java.util.Arrays;`
  - 使用：`Arrays.binarySearch(Object[] a, Object key)`
  - 参数：数组、查找的元素
  - 返回值类型：`int`

- 返回值详解

  - 如果数组中存在该元素，则会返回该元素在数组中的**下标**

    ~~~java
    int[] scores = {1, 20, 30, 40, 50};        //在数组scores中查找元素20        
    int res = Arrays.binarySearch(scores, 20);
    ~~~

    返回：`res = 1`

  - 如果数组中不存在该元素，则会返回  **-(插入点 + 1)**，

    插入点指：如果存在，该元素在数组中的位置

    ~~~java
    int[] scores = {1, 20, 30, 40, 50};
    //1.在数组scores中查找元素25
    int res1 = Arrays.binarySearch(scores, 25);
            //从数组中可以看出 25位于20与30之间，
            //由于20的下标为1，所以25的下标为2，
            //最后的返回值就为：-(2 + 1) = -3
    
    //2.同理在该数组中查找-2
    int res2 = Arrays.binarySearch(scores, -2);
            //可以看出-2比数组中的任何一个元素都要小
            //所以它应该在数组的第一个，所以-2的下标就应该为0
            //最后的返回值就为：-(0 + 1) 
    
    //3.又例如在该数组中查找55
    int res3 = Arrays.binarySearch(scores, 55);
            //由于55比数组中的任何一个元素都要大
            //所以他应该位于数组的最后一个，它的下标就为5
            //最后的返回值就为：-(5 + 1) = -6
    
    ~~~








#### [Redis简介](https://github.com/Snailclimb/JavaGuide/blob/master/docs/database/Redis/Redis.md)

- **简介**

  简单来说 redis 就是一个**数据库**，不过与传统数据库不同的是 redis 的数据是**存在内存中的**，所以读写速度非常快，因此 **redis 被广泛应用于缓存方向**。另外，redis 也经常用来做分布式锁。redis 提供了多种数据类型来支持不同的业务场景。除此之外，redis 支持事务 、持久化、LUA脚本、LRU驱动事件、多种集群方案。

- **为什么要用 redis/为什么要用缓存**

  主要从“高性能”和“高并发”这两点来看待这个问题。

  - **高性能：**

    假如用户第一次访问数据库中的某些数据。这个过程会比较慢，因为是从硬盘上读取的。将该用户访问的数据存在缓存中，这样下一次再访问这些数据的时候就可以直接从缓存中获取了。操作缓存就是直接操作内存，所以速度相当快。如果数据库中的对应数据改变的之后，同步改变缓存中相应的数据即可！

    ![](https://camo.githubusercontent.com/ee35a76c4adb3f46b82e46bd9b9f6698ff82f763/687474703a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f31382d392d32342f35343331363539362e6a7067)

  - **高并发：**

    直接操作缓存能够承受的请求是远远大于直接访问数据库的，所以我们可以考虑把数据库中的部分数据转移到缓存中去，这样用户的一部分请求会直接到缓存这里而不用经过数据库。

    ![](https://camo.githubusercontent.com/5905a206838e5837821793c84c0db28f90a4766e/687474703a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f31382d392d32342f38353134363736302e6a7067)

- **为什么要用 redis 而不用 map/guava 做缓存?**

  本地缓存在多实例情况下，缓存不具有一致性，redis属于分布式缓存，多实例具有一致性。

  - 使用自带的 map 或者 guava 实现的是***本地缓存***，最主要的特点是轻量以及快速，生命周期随着 jvm 的销毁而结束，并且在多实例的情况下，每个实例都需要各自保存一份缓存，缓存不具有一致性。
  - 使用 redis 或 memcached 之类的称为***分布式缓存***，在多实例的情况下，各实例共用一份缓存数据，缓存具有一致性。缺点是需要保持 redis 或 memcached服务的高可用，整个程序架构上较为复杂。

- **redis 和 memcached 的区别**

  ![](https://camo.githubusercontent.com/f51fdf878ceaedb4c6d6df7d568ca718d2e327f6/687474703a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f31382d392d32342f36313630333137392e6a7067)





- **redis 常见数据结构以及使用场景分析**

**1.String**

> **常用命令:** set,get,decr,incr,mget 等。

String数据结构是简单的key-value类型，value其实不仅可以是String，也可以是数字。 常规key-value缓存应用； 常规计数：微博数，粉丝数等。



**2.Hash**

> **常用命令：** hget,hset,hgetall 等。

hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象，后续操作的时候，你可以直接仅仅修改这个对象中的某个字段的值。 比如我们可以 hash 数据结构来存储用户信息，商品信息等等。比如下面我就用 hash 类型存放了我本人的一些信息：

```
key=JavaUser293847
value={
  “id”: 1,
  “name”: “SnailClimb”,
  “age”: 22,
  “location”: “Wuhan, Hubei”
}
```



**3.List**

> **常用命令:** lpush,rpush,lpop,rpop,lrange等

list 就是链表，Redis list 的应用场景非常多，也是Redis最重要的数据结构之一，比如微博的关注列表，粉丝列表，消息列表等功能都可以用Redis的 list 结构来实现。

Redis list 的实现为一个双向链表，即可以支持反向查找和遍历，更方便操作，不过带来了部分额外的内存开销。

另外可以通过 lrange 命令，就是从某个元素开始读取多少个元素，可以基于 list 实现分页查询，这个很棒的一个功能，基于 redis 实现简单的高性能分页，可以做类似微博那种下拉不断分页的东西（一页一页的往下走），性能高。



**4.Set**

> **常用命令：** sadd,spop,smembers,sunion 等

set 对外提供的功能与list类似是一个列表的功能，特殊之处在于 set 是可以自动排重的。

当你需要存储一个列表数据，又不希望出现重复数据时，set是一个很好的选择，并且set提供了判断某个成员是否在一个set集合内的重要接口，这个也是list所不能提供的。可以基于 set 轻易实现交集、并集、差集的操作。

比如：在微博应用中，可以将一个用户所有的关注人存在一个集合中，将其所有粉丝存在一个集合。Redis可以非常方便的实现如共同关注、共同粉丝、共同喜好等功能。这个过程也就是求交集的过程，具体命令如下：

```
sinterstore key1 key2 key3     将交集存在key1内
```



**5.Sorted Set**

> **常用命令：** zadd,zrange,zrem,zcard等

和set相比，sorted set增加了一个权重参数score，使得集合中的元素能够按score进行有序排列。

**举例：** 在直播系统中，实时排行信息包含直播间在线用户列表，各种礼物排行榜，弹幕消息（可以理解为按消息维度的消息排行榜）等信息，适合使用 Redis 中的 Sorted Set 结构进行存储。



- 布隆过滤器解决缓存穿透问题





#### [Spring中都用到了那些设计模式?](https://github.com/Snailclimb/JavaGuide/blob/master/docs/system-design/framework/spring/Spring-Design-Patterns.md)



#### 基本数据类型和包装类

###### [基本数据类型有什么好处](https://hollischuang.github.io/toBeTopJavaer/#/basics/java-basic/boxing-unboxing?id=基本数据类型有什么好处)

我们都知道在Java语言中，`new`一个对象是存储在堆里的，我们通过栈中的引用来使用这些对象；所以，对象本身来说是比较消耗资源的。

对于经常用到的类型，如int等，如果我们每次使用这种变量的时候都需要new一个Java对象的话，就会比较笨重。所以，和C++一样，Java提供了基本数据类型，这种数据的变量不需要使用new创建，他们不会在堆上创建，而是直接在栈内存中存储，因此会更加高效。



###### [为什么需要包装类](https://hollischuang.github.io/toBeTopJavaer/#/basics/java-basic/boxing-unboxing?id=为什么需要包装类)

很多人会有疑问，既然Java中为了提高效率，提供了八种基本数据类型，为什么还要提供包装类呢？

这个问题，其实前面已经有了答案，因为Java是一种面向对象语言，很多地方都需要使用对象而不是基本数据类型。比如，在集合类中，我们是无法将int 、double等类型放进去的。因为集合的容器要求元素是Object类型。

为了让基本类型也具有对象的特征，就出现了包装类型，它相当于将基本类型“包装起来”，使得它具有了对象的性质，并且为其添加了属性和方法，丰富了基本类型的操作。

~~~java
Integer integer=1; //装箱
int i=integer; //拆箱
//反编译结果
Integer integer=Integer.valueOf(1);       
int i=integer.intValue(); 
~~~

从上面反编译后的代码可以看出，int的自动装箱都是通过`Integer.valueOf()`方法来实现的，Integer的自动拆箱都是通过`integer.intValue`来实现的。如果读者感兴趣，可以试着将八种类型都反编译一遍 ，你会发现以下规律：

> 自动装箱都是通过包装类的`valueOf()`方法来实现的.自动拆箱都是通过包装类对象的`xxxValue()`来实现的。











#### JVM知识串联（需要总结）



#### Linux的常用命令（需要复习）

- find、grep、ps、cp、move、tar、head、tail、netstat、lsof、tree、wget、curl、ping、ssh、echo、free、top



会使用常用设计模式

为什么推荐使用枚举实现单例?

避免

单例的七种写法



简单数据结构
栈
队列
链表
数组
哈希表
栈和队列的相同和不同之处
栈通常采用的两种存储结构
两个栈实现队列，和两个队列实现栈

树

- 二叉树
- 字典树
- 平衡树
- 排序树
- B树
- B+树
- R树
- 多路树
- 红黑树





- 深度优先和广度优先搜索
- 全排列
- 贪心算法
- KMP算法
- hash算法
- 海量数据处理









### https://gitee.com/SnailClimb/JavaGuide/blob/master/docs/java/Multithread/synchronized.md



B+树索引





2020年4月8日 10点38分

联系我：shentianyu18@163.com









  


