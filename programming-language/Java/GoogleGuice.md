# GoogleGuice用法
1.通过继承AbstractModule，在configure方法里面定义接口和对应的实现，也就是说定义了从interface到implementation的绑定。然后再每个类的构造函数上加Inject注释，这样Guice就可以自动的帮助类去传递constructor的参数，并且知道如何把接口和实现对应起来。

```
public class BillingModule extends AbstractModule {
  @Override 
  protected void configure() {
    bind(TransactionLog.class).to(DatabaseTransactionLog.class);
    bind(CreditCardProcessor.class).to(PaypalCreditCardProcessor.class);
    bind(BillingService.class).to(RealBillingService.class);
  }
}

public class RealBillingService implements BillingService {
  private final CreditCardProcessor processor;
  private final TransactionLog transactionLog;

  @Inject
  public RealBillingService(CreditCardProcessor processor,
      TransactionLog transactionLog) {
    this.processor = processor;
    this.transactionLog = transactionLog;
  }
```

2.Linked Bindings：类型到实现的绑定（bind a type to its implementation）。
如果一个类型，对应着多个实现，比如说，一个interface，有3个子类的实现，那么就需要一个key来分辨不同的子类实现。这个key就是注释。

```
// 自定义一个PayPal注释
@BindingAnnotation @Target({ FIELD, PARAMETER, METHOD }) @Retention(RUNTIME)
public @interface PayPal {}

// 绑定注释对应的实现
bind(CreditCardProcessor.class)
        .annotatedWith(PayPal.class)
        .to(PayPalCreditCardProcessor.class);

// 使用注释
public class RealBillingService implements BillingService {

  @Inject
  public RealBillingService(@PayPal CreditCardProcessor processor,
      TransactionLog transactionLog) {
    ...
  }
```
然后使用这个注释，告诉Guice，如果你看到这个注释PayPal，就把这个类型CreditCardProcessor替换成指定的实现子类PayPalCreditCardProcessor。

3.Provides 方法绑定：适用于对象的创建需要多行代码，多个步骤，就把这几行代码放入一个单独的方法中，并加上Provides注释

```
public class BillingModule extends AbstractModule {
  @Override
  protected void configure() {
    ...
  }

  @Provides
  @PayPal
  TransactionLog provideTransactionLog() {
    // 对象的创建需要多步
    DatabaseTransactionLog transactionLog = new DatabaseTransactionLog();
    transactionLog.setJdbcUrl("jdbc:mysql://localhost/pizza");
    transactionLog.setThreadPoolSize(30);
    return transactionLog;
  }
}
```

4.Provider 类绑定：适用于如果前面的Provides方法还是很复杂，那么可以把它单独做成一个类，用于对象的创建。

```
public interface Provider<T> {
  T get();
}
// 自定义一个provider来创建TransactionLog
public class DatabaseTransactionLogProvider implements Provider<TransactionLog> {
  private final Connection connection;

  @Inject
  public DatabaseTransactionLogProvider(Connection connection) {
    this.connection = connection;
  }

  public TransactionLog get() {
    DatabaseTransactionLog transactionLog = new DatabaseTransactionLog();
    transactionLog.setConnection(connection);
    return transactionLog;
  }
}
// 绑定TransactionLog到DatabaseTransactionLogProvider，也就是告诉Guice使用
// DatabaseTransactionLogProvider来创建TransactionLog类型对象
public class BillingModule extends AbstractModule {
  @Override
  protected void configure() {
    bind(TransactionLog.class)
        .toProvider(DatabaseTransactionLogProvider.class);
  }
```

5.Constructor Bindings 构造函数绑定：一个类型的绑定，取决于其构造函数的参数。前面的几个例子，proivder，provides，都需要手动的构造依赖。而Constructor Binding要解决的问题就是：在不需要手动的构造依赖的情况下，自动的根据依赖的构造函数，来完成依赖注入

```
public class BillingModule extends AbstractModule {
  @Override 
  protected void configure() {
    try {
      // TransactionLog这个接口绑定到 DatabaseTransactionLog这个类
      // 并且选择需要DatabaseConnection作为参数的那个构造函数（也就是第三种），来完成初始DatabaseTransactionLog
      bind(TransactionLog.class).toConstructor(
          DatabaseTransactionLog.class.getConstructor(DatabaseConnection.class));
    } catch (NoSuchMethodException e) {
      addError(e);
    }
  }
}

// DatabaseTransactionLog有多个构造函数，但是只有第三种构造函数会被使用
public class DatabaseTransactionLog implements TransactionLog {
   public DatabaseTransactionLog() {....}
   public DatabaseTransactionLog(String jdbcUrl) {....}
   public DatabaseTransactionLog(DatabaseConnection conn) {....}
   
}
```

6.JIT bindings：及时类型绑定。

```
@ImplementedBy(PayPalCreditCardProcessor.class)
public interface CreditCardProcessor {
  ChargeResult charge(String amount, CreditCard creditCard)
      throws UnreachableException;
}
//等价于
bind(CreditCardProcessor.class).to(PayPalCreditCardProcessor.class);

@ProvidedBy(DatabaseTransactionLogProvider.class)
public interface TransactionLog {
  void logConnectException(UnreachableException e);
  void logChargeResult(ChargeResult result);
}
//等价于
bind(TransactionLog.class).toProvider(DatabaseTransactionLogProvider.class)
```

# Guice模块
在一个项目中，你可以创建任意多个Guice的Module（继承AbstractModule），当项目不断地增大，越来越多的Module会遍布于各处，如果一个Module依赖于另一个Module，那就很麻烦了。会带来两个问题：
- 如何管理Guice Module间的依赖？一个解决方法是创建一个top level的ApplicationModule，然后把所有的Module在其中加载。
- 如何更好的划分Guice Module？最常见的是根据package边界来划分和创建Module，一个package一个Module：module-per-package。此方法最大的好处就是所有的implementation都只是package private的，这就强制了使用你的api的用户，针对接口编程，而不是依赖于具体的实现。

# Guice 和 Spring 区别
1. Guice更偏向于以注释的方式来解决依赖注入的逻辑，这种方式能在简洁性和可维护性之间取得最好的平衡。Spring要么是XML注释，要么是auto wiring，在Guice看来，前者太琐碎，后者太隐式。