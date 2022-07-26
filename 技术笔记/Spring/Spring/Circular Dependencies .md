# Circular Dependencies 

## Circular Dependencies

循环依赖：当Bean A依赖Bean B，Bean B依赖Bean A；



## Circular Dependencies in Spring

程序启动时，Spring容器会加载并实例化所有的Bean。

使用**构造器**完成依赖注入时，遇到循环依赖的Bean，Spring无法决定去优先加载哪一个Bean，此时程序会抛出BeanCurrentlyInCreationException异常：

```java
Caused by: 
org.springframework.beans.factory.BeanCurrentlyInCreationException: 
Error creating bean with name 'xxx': Requested bean is currently in creation: Is there an unresolvable circular reference?
```



## 解决Circular Dependencies

1. 重新设计组件，使其责任分离；

2. 使用**@Lazy**注解，加了@Lazy注解的依赖，在依赖注入时，会创建一个代理类注入到Bean里去；

3. 使用**Setter/Field**注入；

4. 使用**@PostConstruct**注解

   在其中一个 bean 上使用@Autowired注入一个依赖项，然后使用一个用@PostConstruct注释的方法来设置另一个依赖项。

   ```java
   @Component
   public class CircularDependencyA {
   
       @Autowired
       private CircularDependencyB circB;
   
       @PostConstruct
       public void init() {
           circB.setCircA(this);
       }
   
       public CircularDependencyB getCircB() {
           return circB;
       }
   }
   ```

5. 实现**ApplicationContextAware**和**InitializingBean**

   ```java
   @Component
   public class CircularDependencyA implements ApplicationContextAware, InitializingBean {
   
       private CircularDependencyB circB;
   
       private ApplicationContext context;
   
       public CircularDependencyB getCircB() {
           return circB;
       }
   
       @Override
       public void afterPropertiesSet() throws Exception {
           circB = context.getBean(CircularDependencyB.class);
       }
   
       @Override
       public void setApplicationContext(final ApplicationContext ctx) throws BeansException {
           context = ctx;
       }
   }
   ```

   

## 三级缓存解决 Circular Dependencies

