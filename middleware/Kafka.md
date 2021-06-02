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
6. 零拷贝Sendfile（就是跳过“用户缓冲区”的拷贝，建立一个磁盘空间和内存的直接映射，数据不再复制到“用户态缓冲区”）

![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/middleware/Kafka%E9%9B%B6%E6%8B%B7%E8%B4%9D1.png)
![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/middleware/Kafka%E9%9B%B6%E6%8B%B7%E8%B4%9D2.png)

# Rebalance再平衡机制
Rebalance 本质上是一种协议，规定了一个 Consumer Group 下的所有 Consumer 如何达成一致，来分配订阅 Topic 的每个分区。比如某个 Group 下有 20 个 Consumer 实例，它订阅了一个具有 100 个分区的 Topic。正常情况下，Kafka 平均会为每个 Consumer 分配 5 个分区。这个分配的过程就叫 Rebalance。

Rebalance 过程对 Consumer Group 消费过程有极 大的影响。会stop the world，简称 STW。我们知道在 STW 期间，所有应用线程都会停止工作，表现为 整个应用程序僵在那边一动不动。Rebalance 过程也和这个 类似，在 Rebalance 过程中，所有 Consumer 实例都会停 止消费，等待 Rebalance 完成。这是 Rebalance 为人诟病 的一个方面。

**触发再平衡条件**
- consumer group中的新增或删除某个consumer，导致其所消费的分区需要分配到组内其他的consumer上；
- consumer订阅的topic发生变化，比如订阅的topic采用的是正则表达式的形式，如test-*此时如果有一个新建了一个topic test-user，那么这个topic的所有分区也是会自动分配给当前的consumer的，此时就会发生再平衡；
- consumer所订阅的topic发生了新增分区的行为，那么新增的分区就会分配给当前的consumer，此时就会触发再平衡。

**再平衡策略**
主要有三种：Round Robin，Range和Sticky，默认使用的是Range。这三种分配策略的主要区别在于：
- Round Robin：会采用轮询的方式将当前所有的分区依次分配给所有的consumer；
- Range：首先会计算每个consumer可以消费的分区个数，然后按照顺序将指定个数范围的分区分配给各个consumer；
- Sticky：这种分区策略是最新版本中新增的一种策略，其主要实现了两个目的：将现有的分区尽可能均衡的分配给各个consumer，存在此目的的原因在于Round Robin和Range分配策略实际上都会导致某几个consumer承载过多的分区，从而导致消费压力不均衡；如果发生再平衡，那么重新分配之后在前一点的基础上会尽力保证当前未宕机的consumer所消费的分区不会被分配给其他的consumer上；

# Kafka怎么保证消息不丢失
TODO

# Kafka怎么保证可用性
### 备份机制

Kafka允许同一个Partition存在多个消息副本，每个Partition的副本通常由1个Leader及0个以上的Follower组成，生产者将消息直接发往对应Partition的Leader，Follower会周期地向Leader发送同步请求。

Kafka会尽量将所有的Partition以及各Partition的副本均匀地分配到整个集群的各个Broker上。

### ISR机制

**ISR 副本集合**

ISR 中的副本都是与 Leader 同步的副本，相反，不在 ISR 中的追随者副本就被认为是与 Leader 不同步的（这里的保持同步不是指与Leader数据保持完全一致，只需在replica.lag.time.max.ms时间内与Leader保持有效连接）。

Follower周期性地向Leader发送FetchRequest请求，发送时间间隔配置在replica.fetch.wait.max.ms中，默认值为500。

各Partition的Leader负责维护ISR列表并将ISR的变更同步至ZooKeeper，被移出ISR的Follower会继续向Leader发FetchRequest请求，试图再次跟上Leader重新进入ISR。

ISR中所有副本都跟上了Leader，通常只有ISR里的成员才可能被选为Leader。

**Unclean领导者选举**

当Kafka中unclean.leader.election.enable配置为true(默认值为false)且ISR中所有副本均宕机的情况下，才允许ISR外的副本被选为Leader，此时会丢失部分已应答的数据。

开启 Unclean 领导者选举可能会造成数据丢失，但好处是，它使得分区 Leader 副本一直存在，不至于停止对外提供服务，因此提升了高可用性，反之，禁止 Unclean 领导者选举的好处在于维护了数据的一致性，避免了消息丢失，但牺牲了高可用性。

### ACK机制
生产者发送消息中包含acks字段，该字段代表Leader应答生产者前Leader收到的应答数
1. acks=0
生产者无需等待服务端的任何确认，消息被添加到生产者套接字缓冲区后就视为已发送，因此acks=0不能保证服务端已收到消息
2. acks=1
只要 Partition Leader 接收到消息而且写入本地磁盘了，就认为成功了，不管它其他的 Follower 有没有同步过去这条消息了
3. acks=all
Leader将等待ISR中的所有副本确认后再做出应答，因此只要ISR中任何一个副本还存活着，这条应答过的消息就不会丢失

acks=all是可用性最高的选择，但等待Follower应答引入了额外的响应时间。Leader需要等待ISR中所有副本做出应答，此时响应时间取决于ISR中最慢的那台机器。

如果说 Partition Leader 刚接收到了消息，但是结果 Follower 没有收到消息，此时 Leader 宕机了，那么客户端会感知到这个消息没发送成功，他会重试再次发送消息过去。

### 故障恢复机制
首先需要在集群所有Broker中选出一个Controller，负责各Partition的Leader选举以及Replica的重新分配。

当出现Leader故障后，Controller会将Leader/Follower的变动通知到需为此作出响应的Broker。

**Broker**
当Broker发生故障后，由Controller负责选举受影响Partition的新Leader并通知到相关Broker。
- 当Broker出现故障与ZooKeeper断开连接后，该Broker在ZooKeeper对应的znode会自动被删除，ZooKeeper会触发Controller注册在该节点的Watcher；
- Controller从ZooKeeper的/brokers/ids节点上获取宕机Broker上的所有Partition；
- Controller再从ZooKeeper的/brokers/topics获取所有Partition当前的ISR；
- 对于宕机Broker是Leader的Partition，Controller从ISR中选择幸存的Broker作为新Leader；
- 最后Controller向Broker发送LeaderAndISRRequest请求。

**Controller**
集群中的Controller也会出现故障，因此Kafka让所有Broker都在ZooKeeper的Controller节点上注册一个Watcher。

Controller发生故障时对应的Controller临时节点会自动删除，此时注册在其上的Watcher会被触发，所有活着的Broker都会去竞选成为新的Controller(即创建新的Controller节点，由ZooKeeper保证只会有一个创建成功)。竞选成功者即为新的Controller。
