# Spring-boot自动配置的原理
@EnableAutoConfiguration注解。从classpath中搜索所有META-INF/spring.factories配置文件然后，将其中org.springframework.boot.autoconfigure.EnableAutoConfiguration 对应的配置项加载到spring容器，即它能根据类路径下的 jar 包和配置动态加载配置和注入bean。

# xxx