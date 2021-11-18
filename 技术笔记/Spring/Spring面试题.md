# Spring面试题

### 1、IOC与DI

- IOC：控制反转，Bean的创建、初始化、销毁等操作，由Spring去管理；

- DI：依赖注入，去使用Bean；

### 2、设计原则（松耦合设计）

单一职责

接口分离

依赖倒置

### 3、BeanFactory的作用



### 4、BeanDefinition的作用



### 5、BeanFactory与ApplicationContext的区别？



### 6、BeanFactory和FactoryBean的区别？



### 7、什么是Bean？和对象的区别？

bean：被spring容器管理的对象，由springIOC容器实例化

java对象：java实体实例

### 8、配置Bean有几种方式？

1. xml文件的<bean>标签
2. 注解@Component、@Controller、@Service等
3. JavaConfig @Bean
4. @Import

### 9、@Component与@Bean

@Component由Spring通过反射自动创建。

@Bean需要自己控制实例化过程。

### 10、Bean的作用域

通过Scope属性设置

- singleton：单例默认
- prototype：多例，bean被定义为在每次注入时都会创建一个新的对象；
- request：每个HTTP请求中创建并一个单例对象；
- Session：在一个session的生命周期内创建一个单例对象；
- Application：在ServletContext的生命周期中复用一个单例对象；
- Websocket：在websocket的生命周期中复用一个单例对象。

### 11、单例Bean的优势（单例模式的优势）

1. 减少新实例创建的内存消耗
2. 减少JVM的垃圾回收负担
3. 快速从缓存中去取bean

### 12、Spring如何处理线程安全问题

1. 将单例的变量声明在方法中
2. 将bean设置为多例
3. 将成员变量设置到ThreadLocal中（线程私有）
4. 方法加锁

### 13、Spring实例化Bean的几种方式

1. 构造器方式
2. 静态工厂方式
3. 实例工厂方式
4. FactoryBean类实现接口FactoryBean重写getObject()方法

### 14、Bean的手动装配（注入）、自动装配（自动注入）、注解装配

手动装配：\<bean>标签里指定\<property>属性，@Value

自动装配：

