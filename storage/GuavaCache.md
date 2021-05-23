# Guava cache
**数据结构**
类似 1.7 ConcurrentHashMap，结构是 Segement 数组 + HashEntry数组 + 链表。

**关键参数**
- expireAfterWrite 当缓存项在指定的时间内没有更新就会被回收
- expireAfterAccess 当缓存项在指定的时间内没有被读或写就会被回收
- refreshAfterWrite 当缓存项上一次更新操作之后的多久会被刷新
- concurrencyLevel 并发级别，使得缓存支持并发的写入和读取

refreshAfterWrite
特点是，在refresh的过程中，严格限制只有1个重新加载操作，而其他查询先返回旧值，这样有效地可以减少等待和锁争用。但不能严格保证所有的查询都获取到新值。

**回收机制**
- 基于容量回收：在缓存项的数目达到限定值之前，采用LRU的回收方式
- 定时回收：expireAfterAccess，缓存项在给定时间内没有被读/写访问，则回收，回收顺序和基于大小回收一样（LRU）；expireAfterWrite，缓存项在给定时间内没有被写访问（创建或覆盖），则回收
- 基于引用回收：通过使用弱引用的键、或弱引用的值、或软引用的值，Guava Cache可以垃圾回收

每个entry会有个long accessTime 字段存储上一次访问时间，expireAfterAccess、expireAfterWrite 会在访问时查看是否过期，如果过期则重新load一次到本地缓存并返回。