# Thrift架构
![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/middleware/Thrift%E6%9E%B6%E6%9E%84.png)

**协议层**
- TBinaryProtocol —— 二进制编码格式进行数据传输
- TJSONProtocol —— 使用 JSON 的数据编码协议进行数据传输

**传输层**
- TSocket —— 使用阻塞式 I/O 进行传输，是最常见的模式
- TFramedTransport —— 使用非阻塞方式，按块的大小进行传输，类似于 Java 中的 NIO
- TNonblockingTransport —— 使用非阻塞方式，用于构建异步客户端

# Thrift服务器端工作模式
- TSimpleServer：只有一个工作线程，循环监听新请求的到来并完成对请求的处理
- TNonblockingServer：也是单线程工作，但是该模式采用NIO的方式，所有的socket都被注册到selector中，在一个线程中通过seletor循环监控所有的socket，每次selector结束时，处理所有的处于就绪状态的socket，对于有数据到来的socket进行数据读取操作，对于有数据发送的socket则进行数据发送，对于监听socket则产生一个新业务socket并将其注册到selector中
- TThreadPoolServer：阻塞socket方式工作，主线程负责阻塞式监听“监听socket”中是否有新socket到来，业务处理交由一个线程池来处理
- TThreadedSelectorServer：一个AcceptThread线程对象，专门用于处理监听socket上的新连接；若干个SelectorThread对象专门用于处理业务socket的网络I/O操作，所有网络数据的读写均是有这些线程来完成；一个负载均衡器SelectorThreadLoadBalancer对象，主要用于AcceptThread线程接收到一个新socket连接请求时，决定将这个新连接请求分配给哪个SelectorThread线程；一个ExecutorService类型的工作线程池，在SelectorThread线程中，监听到有业务socket中有调用请求过来，则将请求读取之后，交个ExecutorService线程池中的线程完成此次调用的具体执行
![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/middleware/Thrift%E7%9A%84TThreadedSelectorServer%E6%A8%A1%E5%BC%8F.png)

# Thrift踩坑
1.入参Integer传null，服务端接收到参数值变成0

答：基本类型的入参，传null会变成默认值。