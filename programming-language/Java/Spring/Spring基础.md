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



