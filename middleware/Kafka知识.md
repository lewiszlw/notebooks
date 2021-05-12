# Kafka概念
![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/middleware/Kafka%E6%A6%82%E5%BF%B5.png)

1. broker：Kafka 服务器，负责消息存储和转发
2. topic：消息类别，Kafka 按照 topic 来分类消息
3. partition：topic 的分区，一个 topic 可以包含多个 partition，topic 消息保存在各个partition 上
4. offset：消息在日志中的位置，可以理解是消息在 partition 上的偏移量，也是代表该消息的唯一序号
5. Producer：消息生产者
6. Consumer：消息消费者
7. Consumer Group：消费者分组，每个 Consumer 必须属于一个 group
8. Zookeeper：保存着集群 broker、topic、partition 等 meta 数据；另外，还负责 broker 故障发现，partition leader 选举，负载均衡等功能

# Kafka数据存储设计
**partition的数据文件(offset，MessageSize，data)**

partition 中的每条 Message 包含了以下三个属性: offset，MessageSize，data，其中 offset 表 示 Message 在这个 partition 中的偏移量，offset 不是该 Message 在 partition 数据文件中的实际存储位置，而是逻辑上一个值，它唯一确定了 partition 中的一条 Message，可以认为 offset 是 partition 中 Message 的 id;MessageSize 表示消息内容 data 的大小;data 为 Message 的具体内容。

**数据文件分段segment(顺序读写、分段命令、二分查找)**

partition 物理上由多个 segment 文件组成，每个 segment 大小相等，顺序读写。每个 segment 数据文件以该段中最小的 offset 命名，文件扩展名为.log。这样在查找指定 offset 的 Message 的 时候，用二分查找就可以定位到该 Message 在哪个 segment 数据文件中。

**数据文件索引(分段索引、稀疏存储)**

Kafka 为每个分段后的数据文件建立了索引文件，文件名与数据文件的名字是一样的，只是文件扩展名为.index。index 文件中并没有为数据文件中的每条 Message 建立索引，而是采用了稀疏存储的方式，每隔一定字节的数据建立一条索引。这样避免了索引文件占用过多的空间，从而可以将索引文件保留在内存中。

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/middleware/Kafka%E6%95%B0%E6%8D%AE%E6%96%87%E4%BB%B6%E7%B4%A2%E5%BC%95.png)

# Kafka生产者设计
**负载均衡(partition会均衡分布到不同broker上)**

由于消息 topic 由多个 partition 组成，且 partition 会均衡分布到不同 broker 上，因此，为了有效利用 broker 集群的性能，提高消息的吞吐量，producer 可以通过随机或者 hash 等方式，将消 息平均发送到多个 partition 上，以实现负载均衡。

**批量发送**

是提高消息吞吐量重要的方式，Producer 端可以在内存中合并多条消息后，以一次请求的方式发送了批量的消息给 broker，从而大大减少 broker 存储消息的 IO 操作次数。但也一定程度上影响了消息的实时性，相当于以时延代价，换取更好的吞吐量。

**压缩(GZIP或Snappy)**

Producer 端可以通过 GZIP 或 Snappy 格式对消息集合进行压缩。Producer 端进行压缩之后，在 Consumer 端需进行解压。压缩的好处就是减少传输的数据量，减轻对网络传输的压力，在对大 数据处理上，瓶颈往往体现在网络上而不是 CPU(压缩和解压会耗掉部分 CPU 资源)。

# Kafka消费者设计
同一 Consumer Group 中的多个 Consumer 实例，不同时消费同一个 partition，等效于队列模 式。partition 内消息是有序的，Consumer 通过 pull 方式消费消息。Kafka 不删除已消费的消息。对于 partition，顺序读写磁盘数据，以时间复杂度 O(1)方式提供消息持久化能力。

# Kafka高吞吐量原因
1. 顺序读写（相较于随机读写，读写速度更快）
2. 文件分段和文件索引（为每个分段后的数据文件建立了索引文件。稀疏存储，避免了索引文件占用过多的空间，从而可以将索引文件保留在内存中）
3. 生产者消息批量发送（Producer 端可以在内存中合并多条消息后，以一次请求的方式发送了批量的消息给 broker）
4. 消息压缩（Producer 端消息集合进行压缩，在 Consumer 端需进行解压）
5. 负载均衡（partition 会均衡分布到不同 broker 上，因此，有效利用 broker 集群的性能）
6. 零拷贝Sendfile：（就是跳过“用户缓冲区”的拷贝，建立一个磁盘空间和内存的直接映射，数据不再复制到“用户态缓冲区”）

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/middleware/Kafka%E9%9B%B6%E6%8B%B7%E8%B4%9D1.png)
![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/middleware/Kafka%E9%9B%B6%E6%8B%B7%E8%B4%9D2.png)

# Kafka再平衡机制
再平衡是指在kafka consumer所订阅的topic发生变化时发生的一种分区重分配机制。

哪些情况触发再平衡
- consumer group中的新增或删除某个consumer，导致其所消费的分区需要分配到组内其他的consumer上；
- consumer订阅的topic发生变化，比如订阅的topic采用的是正则表达式的形式，如test-*此时如果有一个新建了一个topic test-user，那么这个topic的所有分区也是会自动分配给当前的consumer的，此时就会发生再平衡；
- consumer所订阅的topic发生了新增分区的行为，那么新增的分区就会分配给当前的consumer，此时就会触发再平衡。

再平衡策略主要有三种：Round Robin，Range和Sticky，默认使用的是Range。这三种分配策略的主要区别在于：
- Round Robin：会采用轮询的方式将当前所有的分区依次分配给所有的consumer；
- Range：首先会计算每个consumer可以消费的分区个数，然后按照顺序将指定个数范围的分区分配给各个consumer；
- Sticky：这种分区策略是最新版本中新增的一种策略，其主要实现了两个目的：将现有的分区尽可能均衡的分配给各个consumer，存在此目的的原因在于Round Robin和Range分配策略实际上都会导致某几个consumer承载过多的分区，从而导致消费压力不均衡；如果发生再平衡，那么重新分配之后在前一点的基础上会尽力保证当前未宕机的consumer所消费的分区不会被分配给其他的consumer上；
