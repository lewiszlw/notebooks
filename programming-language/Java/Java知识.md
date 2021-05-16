# 接口和抽象类的区别

1.设计层次：抽象层次不同，抽象类是对类抽象，而接口是对行为的抽象。

2.跨域不同：抽象类所体现的是一种继承关系，接口是抽象方法的集合，就像契约模式。

3.默认的方法实现：抽象类可以有方法实现，而接口是完全抽象的，不能有方法的实现。

4.构造器：抽象类可以有构造器，但不能实例化，接口不能有构造器。

5.访问修饰符：抽象类方法可以有public,protected,default,private，接口方法只能是public，不写默认为是public static方法。抽象类可以有main方法并且可以运行它，而接口没有

6.多继承：抽象类单继承，接口可以实现多个

# 字符和字符串

## char型变量存贮一个中文汉字
Java中char类型是Unicode编码而非ASCII编码，一个char类型占2个字节（16位），所以可以存放汉子。而在C语言中则不行。

## String、StringBuffer和StringBuilder的区别
1.是否重新equals

String重写了equals方法，StringBuffer和StringBuilder没有重写equals方法。

2.是否可变

String类中使用字符数组保存字符串，private final char value[\]，所以string对象是不可变的。StringBuilder与StringBuffer都继承自AbstractStringBuilder类，在AbstractStringBuilder中也是使用字符数组保存字符串，char[\] value，这两种对象都是可变的。

3.线程安全性

String中的对象是不可变的，也就可以理解为常量，线程安全。AbstractStringBuilder是StringBuilder与StringBuffer的公共父类，定义了一些字符串的基本操作，如expandCapacity、append、insert、indexOf等公共方法。StringBuffer对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。StringBuilder并没有对方法进行加同步锁，所以是非线程安全的。

4.性能

每次对String 类型进行改变的时候，都会生成一个新的String 对象，然后将指针指向新的String 对象，性能较差。StringBuffer每次都会对StringBuffer 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用StirngBuilder 相比使用StringBuffer 能获得10%~15% 左右的性能提升。

# equals方法实现
- 覆写equals方法的时候，也必须覆写hashcode方法（主要用于Map之类的接口，利用hashcode方法来计算存储位置，所以当两个对象equals比较相同，即为同一对象，如果hashcode不一样，会判定为不同对象，产生矛盾）；
- 编写equals方法后，检查是否符合：对称性、传递性、一致性、自反性和非空性

TODO

# 异常处理

## Error、Exception和RuntimeException的区别和作用

1.Error和Exception区别：

Error类一般是指与虚拟机相关的问题，如系统崩溃，虚拟机错误，内存空间不足，方法调用栈溢等。对于这类错误的导致的应用程序中断，仅靠程序本身无法恢复和和预防，遇到这样的错误，建议让程序终止。Exception类表示程序可以处理的异常，可以捕获且可能恢复。遇到这类异常，应该尽可能处理异常，使程序恢复运行，而不应该随意终止异常。

2.Unchecked Exception (RuntimeException) 和Checked Exception (普通Exception) 区别：

是否需要显示catch

# 注解
**作用**
1. 生成文档：@see、@param
2. 替代配置文件
3. 编译检查：@Override

元注解
- @Target，表明该注解可以应用的java元素类型，如 TYPE、METHOD、FIELD等
- @Retention，表明该注解的生命周期，如 SOURCE、CLASS 和 RUNTIME
- @Document，表明该注解标记的元素可以被Javadoc 或类似的工具文档化
- @Inherited，表明使用了@Inherited注解的注解，所标记的类的子类也会拥有这个注解

# Reader和Inputstream区别

Reader用于读取字符流，InputStream用于读取字节流。Reader读取出来的是char数组或者String字符串，InputStream读取出来的是byte数组。

# BIO、NIO和AIO区别
**BIO**

同步并阻塞，服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，当然可以通过线程池机制改善。

BIO方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4以前的唯一选择，但程序直观简单易理解。

**NIO**

同步非阻塞，服务器实现模式为一个请求一个线程，即客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有I/O请求时才启动一个线程进行处理。原来的 I/O 以流的方式处理数据，而 NIO 以块的方式处理数据。NIO 面向缓冲区(Buffer oriented)。

NIO方式适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，JDK1.4开始支持。

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/java/JDK-NIO.png)


**AIO**

异步非阻塞，服务器实现模式为一个有效请求一个线程，客户端的I/O请求都是由OS先完成了再通知服务器应用去启动线程进行处理。

AIO方式适用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂，JDK7开始支持。

# HashMap
## HashMap和Hashtable的区别

1.安全性

HashMap是线程不安全的，Hashtable是线程安全的

2.底层实现

HashMap底层是数组加链表和红黑树，Hashtable底层// TODO

3.继承

Hashtable继承自Dictionary，HashMap继承自AbstractMap。

4.Hashtable的key和value都不允许为null；HashMap最多允许1个null值的key，n多个null的value，当get()方法返回null值时，既可以表示HashMap中没有该键，也可以表示该键所对应的值为null。因此，在HashMap中不能由get()方法来判断HashMap中是否存在某个键，而应该用containsKey()方法来判断。

4.HashMap把Hashtable的contains()方法去掉了，改成containsValue()和containsKey()。因为contains()方法容易让人引起误解。

5.HashMap的Iterator迭代器是fail-fast迭代器，而Hashtable的Enumeration迭代器不是fail-fast的。所以当有其它地方改变了HashMap的结构（增加或者移除元素），将会抛出ConcurrentModificationException，但迭代器本身的remove()方法移除元素则不会抛出ConcurrentModificationException异常。但这并不是一个一定发生的行为，要看JVM。这条同样也是Enumeration和Iterator的区别。

6.HashTable中hash数组默认大小是11，增加的方式是old*2+1；HashMap中hash数组的默认大小是16，而且一定是2的指数。

7.哈希值的使用不同，HashTable直接使用对象的hashCode； HashMap重新计算hash值，而且用与代替求模。

## HashMap自定义对象做为key
1.不可变对象

2.重写hashcode和equals。需要用到hashcode值计算索引位置

## HashMap实现原理

1.原理

HashMap实际上是一个“链表散列”的数据结构，即数组和链表的结合体。HashMap底层就是一个数组结构，数组中的每一项又是一个链表。当新建一个HashMap的时候，就会初始化一个数组。

当我们往HashMap中put元素的时候，先根据key的hashCode重新计算hash值，根据hash值得到这个元素在数组中的位置（即下标），如果数组该位置上已经存放有其他元素了，那么在这个位置上的元素将以链表的形式存放，新加入的放在链头，最先加入的放在链尾。如果数组该位置上没有元素，就直接将该元素放到此数组中的该位置上。Java8后，如果链表长度大于8，则会转为红黑树。当 size 超过 threshold (capacity * load factor) 时扩容，重新hash计算索引。

2.HashMap解决hash冲突

链地址法

3.其他方法解决hash冲突

开放定址法/线性探测法：产生冲突向后移

再哈希：多个hash函数

建立公共溢出区：一个基本表，一个冲突表

## HashMap并发不安全原因
链地址法解决哈希冲突，插入链表时不安全，并发操作可能导致另一个插入操作失效。

## hashcode作用
1.获取哈希值，用于计算索引等，如HashMap使用hashcode计算key的位置

2.判断对象是否相同

## HashMap中链表和红黑树转化
```
    /**
     * The bin count threshold for using a tree rather than list for a
     * bin.  Bins are converted to trees when adding an element to a
     * bin with at least this many nodes. The value must be greater
     * than 2 and should be at least 8 to mesh with assumptions in
     * tree removal about conversion back to plain bins upon
     * shrinkage.
     */
    static final int TREEIFY_THRESHOLD = 8;

    /**
     * The bin count threshold for untreeifying a (split) bin during a
     * resize operation. Should be less than TREEIFY_THRESHOLD, and at
     * most 6 to mesh with shrinkage detection under removal.
     */
    static final int UNTREEIFY_THRESHOLD = 6;

    /**
     * The smallest table capacity for which bins may be treeified.
     * (Otherwise the table is resized if too many nodes in a bin.)
     * Should be at least 4 * TREEIFY_THRESHOLD to avoid conflicts
     * between resizing and treeification thresholds.
     */
    static final int MIN_TREEIFY_CAPACITY = 64;
```
当链表长度大于等于8但table长度小于64时，会进行数组扩容；当链表长度大于等于8并且table长度大于等于64时，会将该链表转化成红黑树；在resize扩容时，如果链表长度小于等于6，会退化成链表。

**为什么链表长度大于等于8时转化成红黑树**
利用泊松分布分析，达到8个元素的时候，概率已经很低了，此时树化，性价比会很高。

红黑树特点：自平衡，高度低，查询速度快。

## HashMap代码分析示例
```
HashMap<String,Integer> map = new HashMap(5);  // 堆里面创建一个对象，Node<K,V>[] table初始容量为第一个大于等于initialCapacity的2的幂，如果不传initialCapacity则默认16
map.put("a",1);   // table[1]=Node("a"->1)
map.put("b",2);   // table[2]=Node("b"->2)
map.put("c",3);   // table[3]=Node("c"->3)
map.put("d",4);   // table[4]=Node("d"->4)
map.put("e",5);   // table[5]=Node("e"->5)
map.put("a",5);   // table[1]=Node("a"->5)，更改key为"a"的Node的val（==或者equals）
map.put("f",10);  // table[6]=Node("f"->10)
map.put("g",11);  // table扩容至16，table[7]=Node("g"->11)，其他索引位置未变（Hashmap中的元素个数超过table数组大小*loadFactor时，就会进行2倍数组扩容）
map.put("h",12);  // table[8]=Node("h"->12)
map.put("aa",13); // table[0]=Node("aa"->13)
map.put("bb",14); // table[0].next=Node("bb",14)，链表
```

# ConcurrentHashMap
**JDK 1.7**

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/java/JDK1.7ConcurrentHashMap%E7%BB%93%E6%9E%84.png)

由 Segment 数组、HashEntry 组成
```
/**
 * Segment 数组，存放数据时首先需要定位到具体的 Segment 中。
 */
final Segment<K,V>[] segments;

static final class Segment<K,V> extends ReentrantLock implements Serializable {
       private static final long serialVersionUID = 2249069246763182397L;
       
       // 和 HashMap 中的 HashEntry 作用一样，真正存放数据的桶
       transient volatile HashEntry<K,V>[] table;
       transient int count;
       transient int modCount;
       transient int threshold;
       final float loadFactor;
       
}
```

put方法流程：
1. 将当前 Segment 中的 table 通过 key 的 hashcode 定位到 HashEntry。
2. 遍历该 HashEntry，如果不为空则判断传入的 key 和当前遍历的 key 是否相等，相等则覆盖旧的 value。
3. 不为空则需要新建一个 HashEntry 并加入到 Segment 中，同时会先判断是否需要扩容。
4. 最后会解除在 1 中所获取当前 Segment 的锁。

get方法流程：
- 只需要将 Key 通过 Hash 之后定位到具体的 Segment ，再通过一次 Hash 定位到具体的元素上。

**JDK 1.8**

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/java/JDK1.8ConcurrentHashMap%E7%BB%93%E6%9E%84.png)

抛弃了原有的 Segment 分段锁，而采用了 CAS + synchronized 来保证并发安全性。底层由“数组”+链表+红黑树来实现的。

Segment虽保留，但已经简化属性，仅仅是为了兼容旧版本。

```
/**
 * The array of bins. Lazily initialized upon first insertion.
 * Size is always a power of two. Accessed directly by iterators.
 */
transient volatile Node<K,V>[] table;
```

put方法流程：
1. 检查key/value是否为空，如果为空，则抛异常，否则进行2
2. 进入for死循环，进行3
3. 检查table是否初始化了，如果没有，则调用initTable()进行初始化然后进行 2，否则进行4
4. 根据key的hash值计算出其应该在table中储存的位置i，取出table[i]的节点用f表示。
    - 如果table[i]==null(即该位置的节点为空，没有发生碰撞)，则利用CAS操作直接存储在该位置，如果CAS操作成功则退出死循环。
    - 如果table[i]!=null(即该位置已经有其它节点，发生碰撞)，碰撞处理也有两种情况
        - 检查table[i]的节点的hash是否等于MOVED，如果等于，则检测到正在扩容，则帮助其扩容
        - 说明table[i]的节点的hash值不等于MOVED，如果table[i]为链表节点，则将此节点插入链表中即可，如果table[i]为树节点，则将此节点插入树中即可。插入成功后，进行 5
5. 如果table[i]的节点是链表节点，则检查table的第i个位置的链表是否需要转化为数，如果需要则调用treeifyBin函数进行转化

get方法：
- 根据key的hash值找到其在table所对应的位置i,然后在table[i]位置所存储的链表(或者是树)进行查找是否有键为key的节点，如果有，则返回节点对应的value，否则返回null。

## ConcurrentHashMap 线程安全
JDK 1.8，基于CAS + synchronized。
- CAS：在判断数组中当前位置为null的时候，使用CAS来把这个新的Node写入数组中对应的位置
- synchronized ：当数组中的指定位置不为空时，通过加锁来添加这个节点进入数组(链表<8)或者是红黑树（链表>=8）

ConcurrentHashMap会判断tabAt(tab, i = (n - 1) & hash)是不是 null，是的话就直接采用CAS进行插入，而如果不为空的话，则是synchronized锁住当前Node的首节点，这是因为当该Node不为空的时候，证明了此时出现了Hash碰撞，就会涉及到链表的尾节点新增或者红黑树的节点新增以及红黑树的平衡，这些操作自然都是非原子性的。从而导致无法使用CAS，当Node的当前下标为null的时候，由于只是涉及数组的新增，所以用CAS即可。

# HashSet
HashSet实际上就是HashMap，采用HashMap作为容器，key为元素，value为PRESENT对象。
```
    /**
     * Constructs a new, empty set; the backing <tt>HashMap</tt> instance has
     * default initial capacity (16) and load factor (0.75).
     */
    public HashSet() {
        map = new HashMap<>();
    }
    // Dummy value to associate with an Object in the backing Map
    private static final Object PRESENT = new Object();
    
```

# volatile

**1.内存可见性**

当对非volatile变量进行读写的时候，每个线程先从主内存拷贝变量到CPU缓存中，如果计算机有多个CPU，每个线程可能在不同的CPU上被处理，这意味着每个线程可以拷贝到不同的CPU cache中。 

volatile变量不会被缓存在寄存器或者对其他处理器不可见的地方，保证了每次读写变量都从主内存中读，跳过CPU cache这一步。当一个线程修改了这个变量的值，新值对于其他线程是立即得知的。 

**2.禁止指令重排序**

指令重排序是JVM为了优化指令、提高程序运行效率，在不影响单线程程序执行结果的前提下，尽可能地提高并行度。指令重排序包括编译器重排序和运行时重排序。

volatile关键字提供内存屏障的方式来防止指令被重排，编译器在生成字节码文件时，会在指令序列中插入内存屏障来禁止特定类型的处理器重排序。JVM内存屏障插入策略：
1. 在每个volatile写操作的前面插入一个StoreStore屏障；
2. 在每个volatile写操作的后面插入一个StoreLoad屏障；
3. 在每个volatile读操作的前面插入一个LoadLoad屏障；
4. 在每个volatile读操作的后面插入一个LoadStore屏障。

# synchronized

**synchronized关键字作用**
1. 锁类对象：类的所有实例
2. 锁对象：该对象
3. 修饰静态方法：类的所有实例
4. 修饰非静态方法：该对象

**synchronized锁膨胀**
- 偏向锁：顾名思义偏向某个线程的锁，适用于某个线程能长期获取到该锁（如果A线程第一次获得锁，那么锁就进入偏向模式，MarkWord的结构也变成偏向锁结构，如果没有其他线程和A线程竞争，A线程再次请求该锁时，无需任何同步操作。也就是说当一个线程访问同步块并且获取锁的时候，会通过CAS操作在对象头的偏向锁结构里记录线程的ID，如果记录成功，线程在进入和退出同步块时，不需要进行CAS操作来加锁和解锁，从而提高程序的性能）
- 轻量级锁：如果偏向锁获取失败，那么会使用CAS自旋来完成，轻量级锁适用于线程交替进入临界区
- 重量级锁：自旋失败之后，会采取重量级锁策略我们线程会阻塞挂起

引入偏向锁是为了在无多线程竞争的情况下尽量减少不必要的轻量级锁执行路径，因为轻量级锁的获取及释放依赖多次CAS原子指令，而偏向锁`只需要在置换ThreadID的时候依赖一次CAS原子指令即可`。偏向锁只有遇到其他线程尝试竞争偏向锁时，持有偏向锁的线程才会释放锁，线程不会主动释放偏向锁。偏向锁的撤销，需要`等待全局安全点（在这个时间点上没有字节码正在执行）`，它会首先暂停拥有偏向锁的线程，判断锁对象是否处于被锁定状态。撤销偏向锁后恢复到无锁（标志位为“01”）或轻量级锁（标志位为“00”）的状态。

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/java/synchronized%E5%8E%9F%E7%90%86.png)

https://juejin.im/post/5ccd84dee51d456e3428c1af

**synchronized重入性**

每个对象有一个监视器锁（monitor）。当monitor被占用时就会处于锁定状态，线程执行monitorenter指令时尝试获取monitor的所有权，过程如下：
1. 如果monitor的进入数为0，则该线程进入monitor，然后将进入数设置为1，该线程即为monitor的所有者。
2. 如果线程已经占有该monitor，只是重新进入，则进入monitor的进入数加1.
3. 如果其他线程已经占用了monitor，则该线程进入阻塞状态，直到monitor的进入数为0，再重新尝试获取monitor的所有权。

执行monitorexit的线程必须是objectref所对应的monitor的所有者。指令执行时，monitor的进入数减1，如果减1后进入数为0，那线程退出monitor，不再是这个monitor的所有者。其他被这个monitor阻塞的线程可以尝试去获取这个 monitor 的所有权。

**synchronized原理**
代码块同步，同步代码块的起始位置插入了moniterenter指令，在同步代码块结束的位置插入了monitorexit指令。

方法级的同步是隐式，即无需通过字节码指令来控制的，它实现在方法调用和返回操作之中。JVM可以从方法常量池中的方法表结构(method_info Structure) 中的 ACC_SYNCHRONIZED 访问标志区分一个方法是否同步方法。当方法调用时，调用指令将会 检查方法的 ACC_SYNCHRONIZED 访问标志是否被设置，如果设置了，执行线程将先持有monitor（虚拟机规范中用的是管程一词）(重量锁)， 然后再执行方法，最后再方法完成(无论是正常完成还是非正常完成)时释放monitor。在方法执行期间，执行线程持有了monitor，其他任何线程都无法再获得同一个monitor。如果一个同步方法执行期间抛 出了异常，并且在方法内部无法处理此异常，那这个同步方法所持有的monitor将在异常抛到同步方法之外时自动释放。

# 对象结构
对象分为三块区域: 对象头、实例数据和对齐填充。

**对象头**
第一部分是Mark Word，用于存储对象自身的运行时数据，如哈希码（HashCode）、GC分代年龄、锁状态标志、线程持有的锁、偏向线程 ID、偏向时间戳等等，这一部分占一个字节。
![MarkWord](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/java/MarkWord.png)

第二部分是Klass Pointer（类型指针），是对象指向它的类元数据的指针，虚拟机通过这个指针来确定这个对象是哪个类的实例，这部分也占一个字节。(如果对象是数组类型的，则需要3个字节来存储对象头，因为还需要一个字节存储数组的长度)

**实例数据**
实例数据存放的是类属性数据信息，包括父类的属性信息，如果是数组的实例部分还包括数组的长度，这部分内存按4字节对齐。

**填充数据**
填充数据是因为虚拟机要求对象起始地址必须是8字节的整数倍。填充数据不是必须存在的，仅仅是为了字节对齐。

# Monitor对象
可以把它理解为一个同步工具，也可以描述为一种同步机制，它通常被描述为一个对象。
在Java虚拟机(HotSpot)中，monitor是由ObjectMonitor实现的，其主要数据结构如下（位于HotSpot虚拟机源码ObjectMonitor.hpp文件，C++实现的）

ObjectMonitor中有几个关键属性
- _count用来记录该线程获取锁的次数
- _WaitSet存放处于wait状态的线程队列
- _EntryList存放处于等待获取锁block状态的线程队列，即被阻塞的线程
- _owner指向持有ObjectMonitor对象的线程

监视器锁（monitor）是依赖于底层的操作系统的Mutex Lock。

Synchronized当其从轻量级锁膨胀为重量级锁时，锁标识位为10，其中指针指向的是monitor对象(也称为管程或监视器锁)的起始地址。

# happens-before规则
**定义**：

1）如果一个操作happens-before另一个操作，那么第一个操作的执行结果将对第二个操作可见，而且第一个操作的执行顺序排在第二个操作之前。

2）两个操作之间存在happens-before关系，并不意味着Java平台的具体实现必须要按照happens-before关系指定的顺序来执行。如果重排序之后的执行结果，与按happens-before关系来执行的结果一致，那么这种重排序并不非法（也就是说，JMM（Java内存模型）允许这种重排序）。

**具体规则**：
1. 程序顺序规则：一个线程中的每个操作，happens-before于该线程中的任意后续操作。
2. 监视器锁规则：对一个锁的解锁，happens-before于随后对这个锁的加锁。
3. volatile变量规则：对一个volatile域的写，happens-before于任意后续对这个volatile域的读。
4. 传递性：如果A happens-before B，且B happens-before C，那么A happens-before C。
5. start()规则：如果线程A执行操作ThreadB.start()（启动线程B），那么A线程的ThreadB.start()操作happens-before于线程B中的任意操作。
6. join()规则：如果线程A执行操作ThreadB.join()并成功返回，那么线程B中的任意操作happens-before于线程A从ThreadB.join()操作成功返回。
7. 程序中断规则：对线程interrupted()方法的调用先行于被中断线程的代码检测到中断时间的发生。
8. 对象finalize规则：一个对象的初始化完成（构造函数执行结束）先行于发生它的finalize()方法的开始。

# wait/notify
`等待队列`：每一个锁都对应了一个等待队列，Wait 会挂起自己让出 CPU 时间片，并将自身加入锁定对象的 Wait Set 中，释放对象的监视器锁（monitor）让其他线程可以获得，直到其他线程调用此对象的 notify( ) 方法或 notifyAll( ) 方法，自身才能被唤醒（这里有个特殊情况就是 Wait 可以增加等待时间）；Notify 方法则会释放监视器锁的同时，唤醒对象 Wait Set 中等待的线程，顺序是随机的不确定。

通用模式：
```java
// 等待线程
synchronized (对象) {
    while(条件不满足) {
        对象.wait();
    }
}

// 通知线程
synchronized (对象) {
    完成条件
    对象.notifyAll();
}
```

注意事项：
1. 必须在同步代码块中调用wait、 notify或者notifyAll方法。
2. 在同步代码块中，必须调用获取的锁对象的wait、 notify或者notifyAll方法。
3. 在等待线程判断条件是否满足时，应该使用while，而不是if。
4. 在调用完锁对象的notify或者notifyAll方法后，等待线程并不会立即从wait()方法返回，需要调用notify()或者notifyAll()的线程释放锁之后，等待线程才从wait()返回继续执行。
5. notify方法只会将等待队列中的一个线程移出，而notifyAll方法会将等待队列中的所有线程移出。

**wait和sleep区别**
1. wait是Object的成员方法，而sleep是Thread的静态方法。
2. 调用wait方法需要先获得锁，而调用sleep方法是不需要的。
3. 调用wait方法的线程需要用notify来唤醒，而sleep必须设置超时值。
4. 线程在调用wait方法之后会先释放锁，而sleep不会释放锁。

**notify()和wait()为什么必须要放在同步块内**

这是Java设计者为了避免使用者出现lost wake up问题。因为wait方法是运行在等待线程里的，notify或者notifyAll是运行在通知线程里的。而执行wait方法前需要判断一下某个条件是否满足，如果不满足才会执行wait方法，这是一个先检查后执行的操作，不是一个原子性操作，多线程环境会出现问题。

https://www.jianshu.com/p/b8073a6ce1c0

# 线程实现方式
1. 继承Thread
2. 实现Runable
3. 实现Callable

# 线程生命周期(状态)
![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/java/%E7%BA%BF%E7%A8%8B%E7%8A%B6%E6%80%81%E6%B5%81%E8%BD%AC.jpg)

**1.新建状态（NEW）**

当程序使用 new 关键字创建了一个线程之后，该线程就处于新建状态。

**2.就绪状态（RUNNABLE）**

当线程对象调用了 start()方法之后，该线程处于就绪状态。Java 虚拟机会为其创建方法调用栈和程序计数器，等待调度运行。

**3.运行状态（RUNNING）**

如果处于就绪状态的线程获得了 CPU，开始执行 run()方法的线程执行体，则该线程处于运行状态。

**4.阻塞状态（BLOCKED）**

阻塞状态是指线程因为某种原因放弃了 cpu 使用权，也即让出了 cpu timeslice，暂时停止运行。直到线程进入可运行(runnable)状态，才有机会再次获得 cpu timeslice 转到运行(running)状态。

1. 等待阻塞（o.wait->等待对列）：运行(running)的线程执行 o.wait()方法，JVM 会把该线程放入等待队列(waitting queue)中。
2. 同步阻塞(lock->锁池)：运行(running)的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则 JVM 会把该线程放入锁池(lock pool)中。
3. 其他阻塞(sleep/join)：运行(running)的线程执行Thread.sleep(long ms)或 t.join()方法，或者发出了 I/O 请求时，JVM 会把该线程置为阻塞状态。当 sleep()状态超时、join()等待线程终止或者超时、或者 I/O处理完毕时，线程重新转入可运行(runnable)状态。

- Waiting（等待）：Object.wait方法、Thread.join方法、java.util.concurrent库中的Lock或Condition时
- Timed waiting（计时等待）：Object.wait、Thread.join、Lock.tryLock和Condition.await等方法有超时参数，还有Thread.sleep方法、LockSupport.parkNanos方法和LockSupport.parkUntil方法

**5.线程死亡（DEAD）**

正常结束、异常结束、调用 stop（不推荐）。

**WAITING 和 BLOCKED 状态**

BLOCKED是指线程正在等待获取锁；WAITING是指线程正在等待其他线程发来通知（notify），收到通知后进入BLOCKED状态，获取锁之后才能恢复执行。

# 线程间通信方式
- JVM提供的API：如wait方法、notify方法和notifyAll方法

# CAS机制
CAS (Compare And Set)：一个当前内存值V、旧的预期值A、即将更新的值B，当且仅当预期值A和内存值V相同时，将内存值修改为B并返回true，否则什么都不做，并返回false。

一种乐观锁的实现，可以称为”无锁”(lock-free)，CAS 由于要保证原子性无法由 JVM 本身实现，需要调用对应 OS 的指令。

Java实现：调用Unsafe包的Native方法 compareAndSwapObject/compareAndSwapInt/compareAndSwapLong。Unsafe 的cas 依赖了的是 jvm 针对不同的操作系统实现的 Atomic::cmpxchg，Atomic::cmpxchg 的实现使用了汇编的 cas 操作，并使用 cpu 硬件提供的 lock信号保证其原子性

缺点
**ABA问题**，我认为 V的值为A，有其它线程在这期间修改了值为B，但它又修改成了A，那么CAS只是对比最终结果和预期值，就检测不出是否修改过。

改进
通过版本号的机制来解决，如AtomicStampedReference。

**存在问题**
- **ABA问题**：如果变量V初次读取的时候是A，并且在准备赋值的时候检查到它仍然是A，那能说明它的值没有被其他线程修改过了吗？
针对这种情况，java并发包中提供了一个带有标记的原子引用类AtomicStampedReference，它可以通过控制变量值的版本来保证CAS的正确性。
- **循环时间长开销大**。CAS操作如果长时间不成功，会导致其一直自旋，给CPU带来非常大的开销。
- **只能保证一个共享变量的原子操作**。对一个共享变量执行操作时，CAS能够保证原子操作，但是对多个共享变量操作时，CAS是无法保证操作的原子性的。
Java从1.5开始JDK提供了AtomicReference类来保证引用对象之间的原子性，可以把多个变量放在一个对象里来进行CAS操作。

# 主流锁分类
![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/java/Java%E4%B8%BB%E6%B5%81%E9%94%81.png)

# ReentrantLock
**重入性原理**

获取锁：如果该锁未被任何线程占有，该锁能被当前线程获取；若被占有，检查占有线程是否是当前线程，是的话再次获取，state计数加一。

释放锁：state同步状态减1；只有当同步状态为0时，锁成功被释放，返回true，否则返回false。

**加锁过程**
非公平锁：
1. 尝试CAS获取锁
2. 执行nonfairTryAcquire方法尝试CAS获取锁
3. 如果两次CAS未获取到锁，则进入阻塞队列

非公平锁
1. 执行tryAcquire方法，如果当前线程是属于同步队列的头节点的下一个节点(头节点是释放锁的节点)，则尝试CAS获取锁
2. 进入阻塞队列

若state>0则两者直接重入锁。

**释放锁过程**
1. 尝试释放锁，如果释放失败，直接返回 false
2. 释放成功，唤醒同步队列的头节点的下一个节点（即队列最前面的线程），让其去竞争锁

**公平锁 和 非公平锁**

区别和共性：
1. 非公平锁在调用 lock 后，首先就会调用 CAS 进行一次抢锁，如果这个时候恰巧锁没有被占用，那么直接就获取到锁返回了。
2. 非公平锁在 CAS 失败后，和公平锁一样都会进入到 tryAcquire 方法，在 tryAcquire 方法中，如果发现锁这个时候被释放了（state == 0），非公平锁会直接 CAS 抢锁，但是公平锁会判断等待队列是否有线程处于等待状态，如果有则不去抢锁。
3. 非公平锁如果两次 CAS 都不成功，那么后面非公平锁和公平锁是一样的，都要进入到阻塞队列等待唤醒。


优缺点：
1. 公平锁每次获取到锁为同步队列中的第一个节点，保证请求资源时间上的绝对顺序，而非公平锁有可能刚释放锁的线程下次继续获取该锁，则有可能导致其他线程永远无法获取到锁，造成“饥饿”现象。
2. 公平锁为了保证时间上的绝对顺序，需要频繁的上下文切换，而非公平锁会降低一定的上下文切换，降低性能开销。因此，ReentrantLock默认选择的是非公平锁，则是为了减少一部分上下文切换，保证了系统更大的吞吐量。

# 可重入锁应用场景
1.递归调用
2.此线程调用同一对象其它synchronized或者有同步锁函数。
举例：用户名和密码保存在本地txt文件中，则登录验证方法和更新密码方法都应该被加synchronized，那么当更新密码的时候需要验证密码的合法性，所以需要调用验证方法，此时是可以调用的。

# 乐观锁和悲观锁实现
乐观锁：CAS、
悲观锁：传统数据库行锁，表锁等，读锁，写锁等，Synchronized 和 ReentrantLock等。

# 共享锁和排他锁（读锁和写锁）
**共享锁**
获取共享锁后，其它线程也可以获取共享锁完成读操作，但都不能修改删除数据

**排他锁**
获取锁后，既能读又能写，但是此时其它线程不能获取这个锁了，只能由当前线程修改数据独享锁

举例
ReentrantReadWriteLock，适合读多写少，提高并发效率。

# JUC总结
![JUC](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/java/JUC.png)


# 动态代理和静态代理
动态代理类的源码是在程序运行期间由JVM根据反射等机制动态的生成，所以不存在代理类的字节码文件。代理类和委托类的关系是在程序运行时确定。

由程序员创建或工具生成代理类的源码，再编译代理类。所谓静态也就是在程序运行前就已经存在代理类的字节码文件，代理类和委托类的关系在运行前就确定了。

# CGlib 和 JDK 动态代理
**CGlib 动态代理**
利用ASM开源包，对代理对象类的class文件加载进来，通过修改其字节码生成子类来处理。

**JDK 动态代理**
实现InvocationHandler接口来实现动态代理，然后使用Proxy将其初始化。

**CGlib 和 JDK 动态代理区别**
CGlib 不能代理 final 类，因为 final 类没有子类，JDK 动态代理需要实现指定接口。

# ThreadPoolExecutor线程池处理流程
![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/java/ThreadPoolExecutor%E7%BA%BF%E7%A8%8B%E6%B1%A0%E5%A4%84%E7%90%86%E6%B5%81%E7%A8%8B.png)

处理流程：
1. 当workerCount < corePoolSize，创建线程执行任务。
2. 当workerCount >= corePoolSize&&阻塞队列workQueue未满，把新的任务放入阻塞队列。
3. 当workQueue已满，并且workerCount >= corePoolSize，并且workerCount < maximumPoolSize，创建线程执行任务。
4. 当workQueue已满，workerCount >= maximumPoolSize，采取拒绝策略,默认拒绝策略是直接抛异常。

**拒绝策略**
1. DiscardPolicy：直接丢弃
2. DiscardOldestPolicy：丢弃队列中最老的任务
3. AbortPolicy：抛异常
4. CallerRunsPolicy：将任务分给调用线程来执行

**keepAliveTime**

keepAliveTime是指当线程池中线程数量大于corePollSize时，此时存在非核心线程，keepAliveTime指非核心线程空闲时间达到的阈值会被回收。
Java核心线程池的回收由allowCoreThreadTimeOut参数控制，默认为false，若开启为true，则此时线程池中不论核心线程还是非核心线程，只要其空闲时间达到keepAliveTime都会被回收。
（但如果这样就违背了线程池的初衷（减少线程创建和开销），所以默认该参数为false）

线程池关闭
- shutdown 通知有序停止，先前提交的任务务会执行
- shutdownNow 尝试立即停止，忽略队列里等待的任务，方法返回未执行过的tasks

# ScheduledThreadPoolExecutor实现
TODO
1.DelayQueue是无界队列
2.ScheduledFutureTask为具体任务，包含long型time（任务将要被执行的具体时间）
3.DelayQueue中维护一个优先队列，time小的在前
4.线程执行完后会修改time（下次执行时间）并放回DelayQueue
5.如果DelayQueue，线程会进入Condition(类似等待队列waitset)等待