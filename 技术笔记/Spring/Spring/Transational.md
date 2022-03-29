# Transactional

## 一、Transactional

事务：逻辑中的一组操作，要么全部成功，要么全部失败。

事务的基本特征ACID

- 原子性
- 一致性
- 隔离性
- 持久性

保证隔离性的事务隔离级别

- 未提交读
- 已提交读
- 可重复度
- 持久化



## 二、Spring事务

### 1.实现方式

- **编程式事务**

  1. TransactionTemplate 手动管理事务；
  2. TransactionManager 手动管理事务。

- **声明式事务**

  1. 基于 AspectJ，使用< tx> 和< aop>标签实现；
  2. 基于 @Transactional 注解实现；
  3. 基于 TransactionInterceptor 拦截器实现；
  4. 基于 TransactionProxyFactoryBean 代理Bean实现。




### 2.事务管理器（TransactionalManager）



### 3.事务拦截器（TransactionInterceptor）





### 4.@Transactional

Spring定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface Transactional {
   
   //指定事务管理器
   @AliasFor("transactionManager")
   String value() default "";
   //指定事务管理器
   @AliasFor("value")
   String transactionManager() default "";

   String[] label() default {};

   //设置事务的传播行为，取值0-6
   //@Transactional(propagation=Propagation.NOT_SUPPORTED)
   Propagation propagation() default Propagation.REQUIRED;
   
   //设置底层数据库的事务隔离级别，事务隔离级别用于处理多事务并发的情况，通常使用数据库的默认隔离级别即可，基本不需要进行设置
    //@Transactional(propagation=Isolation.READ_UNCOMMITTED)
   Isolation isolation() default Isolation.DEFAULT;

   //设置事务的超时秒数，默认值为-1表示永不超时
   //@Transactional(timeout=90)
   int timeout() default TransactionDefinition.TIMEOUT_DEFAULT;
   
   //设置超时字符串值，默认空
   //@Transactional(timeoutString="90")
   String timeoutString() default "";
 
   //当前事务是否为只读事务，设置为true表示只读，false则表示可读写，默认值为false
   //@Transactional(readOnly=true)
   boolean readOnly() default false;
  
   //设置需要进行回滚的异常类数组，当方法中抛出指定异常数组中的异常时，则进行事务回滚
   //@Transactional(rollbackFor={RuntimeException.class, Exception.class})
   Class<? extends Throwable>[] rollbackFor() default {};

   //设置需要进行回滚的异常类名称数组，当方法中抛出指定异常名称数组中的异常时，则进行事务回滚
   //@Transactional(rollbackForClassName={"RuntimeException","Exception"})
   String[] rollbackForClassName() default {};

   //设置不需要进行回滚的异常类数组，当方法中抛出指定异常数组中的异常时，不进行事务回滚
   //@Transactional(noRollbackFor={RuntimeException.class, Exception.class})
   Class<? extends Throwable>[] noRollbackFor() default {};
  
   //置需不回滚的异常类名称数组，当方法中抛出指定异常名称数组中的异常时，不进行回滚
   //@Transactional(noRollbackForClassName={"RuntimeException","Exception"})
   String[] noRollbackForClassName() default {};

}
```

#### @Transactional注意事项

1. @Transactional 只作用于public方法，对于非public方法事务会失效；
2. @Transactional 可应用于接口、接口方法、类、类方法上，但是建议在具体的类或方法上使用，而不要使用在类所要实现的任何接口上，
3. @Transactional 默认遇到运行期异常(RuntimeException)、错误（Error）回滚，遇到检查异常（checked）不回滚。可通过注解属性设置回滚生效/失效异常。
4. 避免同一个类中调用 `@Transactional` 注解的方法，这样会导致事务失效；
5. 正确的设置 `@Transactional` 的 `rollbackFor` 和 `propagation` 属性，否则事务可能会回滚失败;
6. 被 `@Transactional` 注解的方法所在的类必须被 Spring 管理，否则不生效；
7. 底层使用的数据库必须支持事务机制，否则不生效；



### 5.TransactionDefinition

事务属性类，定义了事务的隔离级别、传播行为、超时时间、只读事务等

```java
public interface TransactionDefinition {
   /**
    * 传播行为
    */
   int PROPAGATION_REQUIRED = 0;

   int PROPAGATION_SUPPORTS = 1;

   int PROPAGATION_MANDATORY = 2;
 
   int PROPAGATION_REQUIRES_NEW = 3;

   int PROPAGATION_NOT_SUPPORTED = 4;

   int PROPAGATION_NEVER = 5;

   int PROPAGATION_NESTED = 6;
   
   /**
    * 隔离级别
    */
   int ISOLATION_DEFAULT = -1;

   int ISOLATION_READ_UNCOMMITTED = 1;  // same as java.sql.Connection.TRANSACTION_READ_UNCOMMITTED;

   int ISOLATION_READ_COMMITTED = 2;  // same as java.sql.Connection.TRANSACTION_READ_COMMITTED;

   int ISOLATION_REPEATABLE_READ = 4;  // same as java.sql.Connection.TRANSACTION_REPEATABLE_READ;

   int ISOLATION_SERIALIZABLE = 8;  // same as java.sql.Connection.TRANSACTION_SERIALIZABLE;
   
   int TIMEOUT_DEFAULT = -1;

   default int getPropagationBehavior() {
      return PROPAGATION_REQUIRED;
   }

   default int getIsolationLevel() {
      return ISOLATION_DEFAULT;
   }

   default int getTimeout() {
      return TIMEOUT_DEFAULT;
   }

   default boolean isReadOnly() {
      return false;
   }

   @Nullable
   default String getName() {
      return null;
   }

   static TransactionDefinition withDefaults() {
      return StaticTransactionDefinition.INSTANCE;
   }

}
```



### 6.事务传播行为

事务传播行为（propagation behavior）：指的就是当一个事务方法被另一个事务方法调用时，这个事务方法应该如何进行。

#### Propagation枚举类

以methodA()、methodB()举例。

```java
public enum Propagation {
  
  //默认事务传播行为；A存在事务，B加入A事务；单独调用B，B开启新事务
	REQUIRED(TransactionDefinition.PROPAGATION_REQUIRED),

  //A存在事务，B执行A事务；单独调用B，B以非事务状态执行
	SUPPORTS(TransactionDefinition.PROPAGATION_SUPPORTS),

  //A存在事务，B执行A事务；单独调用B，B抛出异常
	MANDATORY(TransactionDefinition.PROPAGATION_MANDATORY),

  //A存在事务，B挂起A事务并新开事务；单独执行B时，B开启新事务；AB事务互不干扰
  //需要使用JAT事务管理器TransactionManager作为事务管理器
	REQUIRES_NEW(TransactionDefinition.PROPAGATION_REQUIRES_NEW),

  //A存在事务，B挂起A事务并以非事务执行；单独执行B时，B以非事务执行
  //需要使用JAT事务管理器TransactionManager作为事务管理器
	NOT_SUPPORTED(TransactionDefinition.PROPAGATION_NOT_SUPPORTED),
 
  //B总是非事务地执行，如果存在一个活动事务，则抛出异常
	NEVER(TransactionDefinition.PROPAGATION_NEVER),

  //A存在事务，B运行在A的嵌套事务中；单独调用B, B开启新事务
  //重点：保存A事务的savePoint，嵌套事务中的B失败时，回滚至savePoint，A事务提交/回滚B事务也会提交/回滚
	NESTED(TransactionDefinition.PROPAGATION_NESTED);

	private final int value;

	Propagation(int value) {
		this.value = value;
	}

	public int value() {
		return this.value;
	}

}
```

#### REQUIRES_NEW 与 NESTED

REQUIRED、REQUIRES_NEW与NESTED有相同功能，单独调用B时，B开启新的事务；

区别在于：

- REQUIRED：A、B事务相关联，互相影响；
- REQUIRES_NEW：A、B的事务完全独立，互不影响；
- NESTED：B的事务是A的事务的嵌套事务，即，保留A事务的savePoint，B失败，回滚至savePoint，A事务提交/回滚B事务也会提交/回滚。



### 7.事务隔离级别

#### Isolation枚举类

```java
public enum Isolation {
   //使用数据库默认的隔离级别
   DEFAULT(TransactionDefinition.ISOLATION_DEFAULT),
   //未提交读
   READ_UNCOMMITTED(TransactionDefinition.ISOLATION_READ_UNCOMMITTED),
   //已提交读
   READ_COMMITTED(TransactionDefinition.ISOLATION_READ_COMMITTED),
   //可重复读
   REPEATABLE_READ(TransactionDefinition.ISOLATION_REPEATABLE_READ),
   //序列化
   SERIALIZABLE(TransactionDefinition.ISOLATION_SERIALIZABLE);

   private final int value;

   Isolation(int value) {
      this.value = value;
   }

   public int value() {
      return this.value;
   }
}
```



## 三、@Transactional事务实现原理

@Transactional的工作机制是基于 AOP 实现的，AOP 又是使用动态代理实现的。

如果目标对象实现了接口，默认情况下会采用 JDK 的动态代理；如果目标对象没有实现了接口，会使用 CGLIB 动态代理。





## 四、Spring Transactional存在问题

### 事务失效



### 多线程事务