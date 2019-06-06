# 事务

- [wikipedia - 数据库事务](https://zh.wikipedia.org/wiki/数据库事务)

**数据库事务**（简称：**事务**）是[数据库管理系统](https://zh.wikipedia.org/wiki/数据库管理系统)执行过程中的一个**逻辑单位**，由一个有限的[数据库](https://zh.wikipedia.org/wiki/数据库)**操作序列**构成。

数据库事务通常包含了一个序列的对数据库的读/写操作。包含有以下两个目的：

1. 为数据库操作序列提供了一个从失败中恢复到正常状态的方法，同时提供了数据库即使在异常状态下仍能保持一致性的方法。
2. 当多个[应用程序](https://zh.wikipedia.org/wiki/应用程序)在[并发](https://zh.wikipedia.org/wiki/并发)访问数据库时，可以在这些应用程序之间提供一个隔离方法，以防止彼此的操作互相干扰。

当事务被提交给了[数据库管理系统](https://zh.wikipedia.org/wiki/数据库管理系统)（DBMS），则DBMS需要确保该事务中的所有操作都成功完成且其结果被永久保存在数据库中，如果事务中有的操作没有成功完成，则事务中的所有操作都需要[回滚](https://zh.wikipedia.org/wiki/回滚_(数据管理))，回到事务执行前的状态；同时，该事务对数据库或者其他事务的执行无影响，所有的事务都好像在独立的运行。

某人要在商店使用[电子货币](https://zh.wikipedia.org/wiki/電子貨幣)购买100元的东西，当中至少包括两个操作：

1. 该人账户减少100元
2. 商店账户增加100元

支持事务的数据库管理系统（transactional DBMS）就是要确保以上两个操作（整个“事务”）都能完成，或一起取消；否则就会出现100元平白消失或出现的情况。

但在现实情况下，失败的风险很高。在一个数据库事务的执行过程中，有可能会遇上事务操作失败、[数据库系统](https://zh.wikipedia.org/wiki/数据库系统)／[操作系统](https://zh.wikipedia.org/wiki/操作系统)出错，甚至是存储介质出错等情况。这便需要DBMS对一个执行失败的事务执行恢复操作，将其数据库状态恢复到一致状态（数据的[一致性](https://zh.wikipedia.org/wiki/一致性)得到保证的状态）。为了实现将数据库状态恢复到一致状态的功能，DBMS通常需要维护[事务日志](https://zh.wikipedia.org/w/index.php?title=事务日志&action=edit&redlink=1)以追踪事务中所有影响数据库数据的操作。

## ACID特性



数据库事务拥有以下四个特性，习惯上称为 ACID 特性：

- **原子性（Atomicity）**：事务作为一个整体被执行，包含在其中的对数据库的操作要么全部被执行，要么都不执行。
- **一致性（Consistency）**：事务应确保数据库的状态从一个一致状态转变为另一个一致状态。*一致状态*的含义是数据库中的数据应满足完整性约束。
- **隔离性（Isolation）**：多个事务并发执行时，一个事务的执行不应影响其他事务的执行。
- **持久性（Durability）**：已被提交的事务对数据库的修改应该永久保存在数据库中。



## SQL

[SQL](https://zh.wikipedia.org/wiki/SQL)国际标准使用`START TRANSACTION`开始一个事务（也可以用方言命令`BEGIN`）。`COMMIT`语句使事务成功完成。`ROLLBACK`语句结束事务，放弃从`BEGIN TRANSACTION`开始的一切变更。若[autocommit](https://zh.wikipedia.org/wiki/Autocommit)被`START TRANSACTION`的使用禁止，在事务结束时autocommit会重新激活。



## 回滚

**回滚**（英语：rollback）是[数据库](https://zh.wikipedia.org/wiki/数据库)技术中的操作，放弃修改，使数据库状态恢复到此前的某个时刻。这对[数据完整性](https://zh.wikipedia.org/wiki/数据完整性)具有关键意义。回滚是[数据库事务](https://zh.wikipedia.org/wiki/数据库事务)管理重要一环。回滚特性通常用[数据库日志](https://zh.wikipedia.org/w/index.php?title=数据库日志&action=edit&redlink=1)实现，但也可以用[多版本并发控制](https://zh.wikipedia.org/wiki/多版本并发控制)实现。

*级联回滚*(cascading rollback)是指数据库的一个事务的失败引起多个事务随之失败，都要各自回滚。

[SQL](https://zh.wikipedia.org/wiki/SQL)中, `ROLLBACK`是一条命令，引起从最后一次`BEGIN WORK`或`START TRANSACTION`开始的数据改变被抛弃，数据库状态恢复到改变之前。

一条`ROLLBACK`语句也释放任何已存的[savepoint](https://zh.wikipedia.org/wiki/Savepoint)。

在大多数SQL语言实现中，`ROLLBACK`是特定于连接。即如果两个连接指向同一个数据库，在一个连接上的`ROLLBACK`并不影响另一个连接

# 隔离级别

参考：

- [Spring Boot中的事务管理](http://blog.didispace.com/springboottransactional/)





Spring 中有五种隔离级别，定义在 `org.springframework.transaction.annotation.Isolation`

中：

```java
public enum Isolation {
    DEFAULT(-1),
    READ_UNCOMMITTED(1),
    READ_COMMITTED(2),
    REPEATABLE_READ(4),
    SERIALIZABLE(8);
}
```

每种隔离级别的第一如下：

- `DEFAULT`：这是默认值，表示使用底层数据库的默认隔离级别。对大部分数据库而言，通常这值就是：`READ_COMMITTED`。
- `READ_UNCOMMITTED`：该隔离级别表示一个事务可以读取另一个事务修改但还没有提交的数据。该级别不能防止脏读和不可重复读，因此很少使用该隔离级别。
- `READ_COMMITTED`：该隔离级别表示一个事务只能读取另一个事务已经提交的数据。该级别可以防止脏读，这也是大多数情况下的推荐值。
- `REPEATABLE_READ`：该隔离级别表示一个事务在整个过程中可以多次重复执行某个查询，并且每次返回的记录都相同。即使在多次查询之间有新增的数据满足该查询，这些新增的记录也会被忽略。该级别可以防止脏读和不可重复读。
- `SERIALIZABLE`：所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及幻读。但是这将严重影响程序的性能。通常情况下也不会用到该级别。

可以通过如下方式来使用隔离级别：

```java
@Transactional(isolation = Isolation.DEFAULT)
```



# 传播行为



Spring 中提供了 7 中传播方式，定义在 `org.springframework.transaction.annotation.Propagation` 中，如下：

```java
public enum Propagation {
    REQUIRED(0),
    SUPPORTS(1),
    MANDATORY(2),
    REQUIRES_NEW(3),
    NOT_SUPPORTED(4),
    NEVER(5),
    NESTED(6);
}
```

每种类型的意义如下：

- `REQUIRED`：如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。
- `SUPPORTS`：如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。
- `MANDATORY`：如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。
- `REQUIRES_NEW`：创建一个新的事务，如果当前存在事务，则把当前事务挂起。
- `NOT_SUPPORTED`：以非事务方式运行，如果当前存在事务，则把当前事务挂起。
- `NEVER`：以非事务方式运行，如果当前存在事务，则抛出异常。
- `NESTED`：如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于`REQUIRED`。

使用的方式如下：

```java
@Transactional(propagation = Propagation.REQUIRED)
```

