# 面向对象
## 面对对象和面向过程的区别
1.面向过程

优点：性能比面向对象高，因为类调用时需要实例化，开销比较大，比较消耗资源;比如单片机、嵌入式开发、Linux/Unix等一般采用面向过程开发，性能是最重要的因素。

缺点：没有面向对象易维护、易复用、易扩展

2.面向对象

优点：易维护、易复用、易扩展，由于面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统更加灵活、更加易于维护

## 接口和抽象类的区别

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

# 五种IO模型
**1.阻塞 IO 模型**

典型的阻塞 IO 模型的例子为:data = socket.read();如果数据没有就 绪，就会一直阻塞在 read 方法。

**2.非阻塞 IO 模型**

当用户线程发起一个 read 操作后，并不需要等待，而是马上就得到了一个结果。如果结果是一个 error 时，它就知道数据还没有准备好，于是它可以再次发送 read 操作。

用户线程需要不断地询问内核数据是否就绪，也就说非阻塞 IO 不会交出 CPU，而会一直占用 CPU。

**3.多路复用 IO 模型**

有一个线程不断去轮询多个 socket 的状态，只有当 socket 真正有读写事件时，才真正调用实际的 IO 读写操作。

另外多路复用 IO 为何比非阻塞 IO 模型的效率高是因为在非阻塞 IO 中，不断地询问 socket 状态
时通过用户线程去进行的，而在多路复用 IO 中，轮询每个 socket 状态是内核在进行的，这个效 率要比用户线程要高的多。

**4.信号驱动 IO 模型**

在信号驱动 IO 模型中，当用户线程发起一个 IO 请求操作，会给对应的 socket 注册一个信号函 数，然后用户线程会继续执行，当内核数据就绪时会发送一个信号给用户线程，用户线程接收到 信号之后，便在信号函数中调用 IO 读写操作来进行实际的 IO 请求操作。

**5.异步 IO 模型**

异步 IO 模型才是最理想的 IO 模型，在异步 IO 模型中，当用户线程发起 read 操作之后，立刻就 可以开始去做其它的事。而另一方面，从内核的角度，当它受到一个 asynchronous read 之后， 它会立刻返回，说明 read 请求已经成功发起了，因此不会对用户线程产生任何 block。然后，内 核会等待数据准备完成，然后将数据拷贝到用户线程，当这一切都完成之后，内核会给用户线程 发送一个信号，告诉它 read 操作完成了。也就说用户线程完全不需要实际的整个 IO 操作是如何进行的，只需要先发起一个请求，当接收内核返回的成功信号时表示 IO 操作已经完成，可以直接 去使用数据了。