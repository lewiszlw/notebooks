# NoSQL和传统SQL区别
1. 传统SQL 是 为存储进行优化，存储范式化的数据，尽量减少了存储空间的使用，同时构建了非常强大的查询能力。NoSQL 对计算资源进行了优化，以存储空间换计算时间。数据表中直接存储了非范式化的数据，可以直接拿出来使用。
2. 扩展
传统SQL 是垂直扩展（加机器CPU内存），DDB是水平扩展（加机器数量）。
3. 传统SQL 是强一致性，但DDB是最终一致性
4. 传统SQL 支持join功能，而NoSQL不支持join功能。
5. 传统SQL 是基于schema，而NoSQL是schema-less。

P.S.这里NoSQL以DynamoDB为例。

# DynamoDB
DDB一张表包含一个partion key（可以理解为主键） 和 sort key，表里的每一条数据就是一个文档，采用json格式。

Partition Key 的作用是将数据分散到不同的分区(Partition)里，构建无序的哈希索引，Sort Key的作用是将同一个分区中的数据按照一定的顺序排列起来，便于查找。

全局二级索引GSI
几乎相当于一个全新的表，除了与原表共用同一个表名之外，GSI 使用了新的 Partition Key 和 Sort Key, 有自己独立的 Partition, 计算读写数量时也与原表相互独立。GSI 会在原表发生改变的时候，通过流(Stream)将更新同步到 GSI 中。

DDB Stream
DynamoDB 中的数据在发生变化时，均会发送相应的事件流。

DDB是AP数据库
通常主表会有三个备份，默认情况下写入到两个副本中即认为写入成功，因此在高频读取时也有一定的概率读到未完成写入的数据。GSI 读写会产生一致性的原因是 GSI 和主表实际是 两个不同的存储，写入到主表的数据会通过流同步到GSI，这个过程会存在一定的延时（10ms级别）。

DDD 事务
DynamoDB通过TransactWriteItems和TransactGetItems API调用支持事务。

Hot partition问题
partition key设计不合理，某一partition key下有很多条数据，在极端情况下，如果单个分区接收的流量超出 3000 RCU 或 1000 WCU，就会出现限制。