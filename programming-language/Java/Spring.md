# Spring IOC容器
![spring-ioc容器初始化](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/spring/spring-ioc%E5%AE%B9%E5%99%A8%E5%88%9D%E5%A7%8B%E5%8C%96.png)

# Bean的生命周期
![Bean的生命周期](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/spring/Bean%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)
1. 根据BeanDefinition信息，实例化对象，Constructor构造方法;  

2. 根据BeanDefinition信息，配置Bean的所有属性（将bean的引用注入到bean对应的属性，*可能存在循环依赖问题）;  

3. 如果Bean实现了BeanNameAware接口，工厂调用Bean的setBeanName，参数为Bean的Id;  

4. 如果Bean实现BeanFactoryAware接口，Spring将调用setBeanFactory()方法，将BeanFactory容器实例传入；

5. 如果Bean实现ApplicationContextAware接口，Spring将调用setApplicationContext()方法，将bean所在的应用上下文的引用传入进来；

5. 如果存在类实现了BeanPostProcessor接口，执行这些实现类的postProcessBeforeInitialization方法，这相当于在Bean初始化之前插入逻辑  ；

6. 如果Bean实现InitializingBean接口， 执行afterPropertiesSet方法；

7. 如果Bean指定了init-method方法，就会调用该方法。例：\<bean init-method="init">  ；

8. 如果存在类实现了BeanPostProcessor接口，执行这些实现类的postProcessAfterInitialization方法，这相当于在Bean初始化之后插入逻辑  ；

9. 这个阶段Bean已经可以使用了，scope为singleton的Bean会被缓存在IOC容器中  

10. 如果Bean实现了DisposableBean接口， 在容器销毁的时候执行destroy方法。  

11. 如果配置了destory-method方法，就调用该方法。例：\<bean destroy-method="customerDestroy"> 

# SpringMVC中请求的生命周期
![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/spring/SpringMVC%E4%B8%AD%E8%AF%B7%E6%B1%82%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)

SpringMVC主流程主要有三个阶段：
1. 处理器映射阶段（HandlerMapping）：通过解析url中的路径来找到对应的处理器
2. 处理器适配阶段（HandlerAdapter）：通过解析请求中的参数来适配处理器，并执行处理器
3. 视图解析阶段（ViewResolver）：如果处理器返回的是逻辑视图，那么会在这个阶段绘制成可展示的视图

而前端控制器DispatcherServlet统筹整个流程，它的工作除了流程扭转之外，还有执行各种拦截器以及统一异常管理等。

# SpringMVC生命周期总控——DispatcherServlet
![image](https://raw.githubusercontent.com/lewiszlw/notebooks/master/assets/spring/SpringMVC%E4%B8%ADDispatcherServlet%E4%B8%BB%E6%B5%81%E7%A8%8B.png)

上图中不带有颜色的处理块的工作都是DispatcherServlet负责的。主要是拦截器的前置操作、后置操作、完成/异常操作以及最终完成操作。并且还负责统一管理主流程中出现的异常。

而蓝色块的工作属于处理器映射器，红色块的工作属于处理器适配器，绿色块的工作属于视图解析器。

# Spring循环依赖问题
循环依赖：一个或多个对象实例之间存在直接或间接的依赖关系，这种依赖关系构成了构成一个环形调用。如对象A依赖对象B，对象B依赖对象A。

循环依赖场景
- 单例的setter注入（能解决）
- 多例的setter注入（不能解决）
- 构造器注入（不能解决）
- 单例的代理对象setter注入（有可能解决）
- DependsOn循环依赖（不能解决）

Spring三级缓存
- singletonObjects 一级缓存，用于保存实例化、注入、初始化完成的bean实例
- earlySingletonObjects 二级缓存，用于保存实例化完成的bean实例
- singletonFactories 三级缓存，用于保存bean创建工厂，以便于后面扩展有机会创建代理对象。

解决循环依赖 - 单例的setter注入
Spring是通过递归的方式获取目标bean及其所依赖的bean的；Spring实例化一个bean的时候，是分两步进行的，首先实例化目标bean，然后为其注入属性。
Spring在实例化一个bean的时候，是首先递归的实例化其所依赖的所有bean，直到某个bean没有依赖其他bean，此时就会将该实例返回，然后反递归的将获取到的bean设置为各个上层bean的属性的。
通俗来讲，对象A依赖对象B，对象B依赖对象A，设置属性会先把A中的B引用指向半成品的B，B中的A引用指向半成品的A，当A和B完成初始化后，这些引用也就自然指向了成品。


循环依赖就说明程序设计有问题，一般推荐使用构造注入，这样当有循环依赖时会直接报错无法启动，而setter注入会成功。