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

## String s = “123”;这个语句有几个对象产生

如果常量池已存在"123"，则直接s指向"123"即可，无需新生成字符串；如果常量池没有，则需要生成一个字符串"123"放到常量池。

# 异常处理

## Error、Exception和RuntimeException的区别和作用

1.Error和Exception区别：

Error类一般是指与虚拟机相关的问题，如系统崩溃，虚拟机错误，内存空间不足，方法调用栈溢等。对于这类错误的导致的应用程序中断，仅靠程序本身无法恢复和和预防，遇到这样的错误，建议让程序终止。Exception类表示程序可以处理的异常，可以捕获且可能恢复。遇到这类异常，应该尽可能处理异常，使程序恢复运行，而不应该随意终止异常。

2.Unchecked Exception (RuntimeException) 和Checked Exception (普通Exception) 区别：

是否需要显示catch

# IO

## Reader和Inputstream区别

Reader用于读取字符流，InputStream用于读取字节流。Reader读取出来的是char数组或者String字符串，InputStream读取出来的是byte数组。

# 集合类

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

# 多线程

## 线程实现方式

1.继承Thread

2.实现Runable

3.实现Callable

## synchronized关键字作用

1.锁类对象：类的所有实例

2.锁对象：该对象

3.修饰静态方法：类的所有实例

4.修饰非静态方法：该对象

## synchronized锁膨胀
- 偏向锁:顾名思义偏向某个线程的锁，适用于某个线程能长期获取到该锁（如果A线程第一次获得锁，那么锁就进入偏向模式，MarkWord的结构也变成偏向锁结构，如果没有其他线程和A线程竞争，A线程再次请求该锁时，无需任何同步操作。也就是说当一个线程访问同步块并且获取锁的时候，会通过CAS操作在对象头的偏向锁结构里记录线程的ID，如果记录成功，线程在进入和退出同步块时，不需要进行CAS操作来加锁和解锁，从而提高程序的性能。）
- 轻量级锁：如果偏向锁获取失败，那么会使用CAS自旋来完成，轻量级锁适用于线程交替进入临界区
- 重量级锁：自旋失败之后，会采取重量级锁策略我们线程会阻塞挂起

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/java/synchronized%E5%8E%9F%E7%90%86.png)

https://juejin.im/post/5ccd84dee51d456e3428c1af

## notify()和wait()为什么必须要放在同步块内
这是Java设计者为了避免使用者出现lost wake up问题。因为wait方法是运行在等待线程里的，notify或者notifyAll是运行在通知线程里的。而执行wait方法前需要判断一下某个条件是否满足，如果不满足才会执行wait方法，这是一个先检查后执行的操作，不是一个原子性操作，多线程环境会出现问题。

https://www.jianshu.com/p/b8073a6ce1c0

## wait/notify机制
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

## wait和sleep区别
1.wait是Object的成员方法，而sleep是Thread的静态方法。

2.调用wait方法需要先获得锁，而调用sleep方法是不需要的。

3.调用wait方法的线程需要用notify来唤醒，而sleep必须设置超时值。

4.线程在调用wait方法之后会先释放锁，而sleep不会释放锁。

# 线程生命周期(状态)
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

**5.线程死亡（DEAD）**

正常结束、异常结束、调用 stop（不推荐）。

# CAS机制
CAS (Compare And Set)：一个当前内存值V、旧的预期值A、即将更新的值B，当且仅当预期值A和内存值V相同时，将内存值修改为B并返回true，否则什么都不做，并返回false。

一种乐观锁的实现，可以称为”无锁”(lock-free)，CAS 由于要保证原子性无法由 JVM 本身实现，需要调用对应 OS 的指令。

Java实现：调用Unsafe包的Native方法 compareAndSwapObject/compareAndSwapInt/compareAndSwapLong。Unsafe 的cas 依赖了的是 jvm 针对不同的操作系统实现的 Atomic::cmpxchg，Atomic::cmpxchg 的实现使用了汇编的 cas 操作，并使用 cpu 硬件提供的 lock信号保证其原子性

CAS存在一个很明显的问题，即ABA问题：如果变量V初次读取的时候是A，并且在准备赋值的时候检查到它仍然是A，那能说明它的值没有被其他线程修改过了吗？

针对这种情况，java并发包中提供了一个带有标记的原子引用类AtomicStampedReference，它可以通过控制变量值的版本来保证CAS的正确性。