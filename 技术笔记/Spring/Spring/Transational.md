# Transactional

## Transactional

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



## Spring事务

### 实现方式

- **编程式事务**

  通过  TransactionTemplate 或者 TransactionManager 手动管理事务。

- **声明式事务**

  1. 基于AspectJ，使用< tx> 和< aop>标签实现；
  2. 基于 @Transactional 注解实现；
  3. 基于 TransactionInterceptor实现；
  4. 基于 TransactionProxyFactoryBean实现。




### @Transactional

Spring定义

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface Transactional {
   
   @AliasFor("transactionManager")
   String value() default "";

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

1. 1
2. 2
3. 3
4. 4
5. 5
6. 6
7. 7



## TransactionDefinition

事务属性类，定义了事务的隔离级别、传播行为、超时时间、只读事务等

```java
public interface TransactionDefinition {

   int PROPAGATION_REQUIRED = 0;

   int PROPAGATION_SUPPORTS = 1;

   int PROPAGATION_MANDATORY = 2;
 
   int PROPAGATION_REQUIRES_NEW = 3;

   int PROPAGATION_NOT_SUPPORTED = 4;

   int PROPAGATION_NEVER = 5;

   int PROPAGATION_NESTED = 6;

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

### 事务传播行为



### 事务隔离级别



### 事务超时属性



### 事务只读属性



### 事务回滚规则



### 实现原理



## Spring Transactional存在问题

#### 事务失效



#### 多线程事务