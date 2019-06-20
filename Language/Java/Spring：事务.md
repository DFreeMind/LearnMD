# 事务基础

- [wikipedia - 数据库事务](https://zh.wikipedia.org/wiki/数据库事务)

**数据库事务**（简称：**事务**）是[数据库管理系统](https://zh.wikipedia.org/wiki/数据库管理系统)执行过程中的一个**逻辑单位**，由一个有限的[数据库](https://zh.wikipedia.org/wiki/数据库)**操作序列**构成。事务具有不可分割性。

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

- [高级数据库三：浅谈数据库事务（transaction）](https://blog.csdn.net/u013007900/article/details/77927723)
- [Wikipedia - 事务隔离](https://zh.wikipedia.org/wiki/%E4%BA%8B%E5%8B%99%E9%9A%94%E9%9B%A2)
- [解决死锁之路 - 学习事务与隔离级别](https://www.aneasystone.com/archives/2017/10/solving-dead-locks-one.html)

数据库事务拥有以下四个特性，习惯上称为 ACID 特性：

- **原子性（Atomicity）**：事务作为一个整体被执行，包含在其中的对数据库的操作要么全部被执行，要么都不执行。
- **一致性（Consistency）**：事务应确保数据库的状态从一个一致状态转变为另一个一致状态。*一致状态*的含义是数据库中的数据应满足完整性约束。
- **隔离性（Isolation）**：多个事务并发执行时，一个事务的执行不应影响其他事务的执行。
- **持久性（Durability）**：已被提交的事务对数据库的修改应该永久保存在数据库中。



## 一致性

根据对于一致性要求级别的不同可以分为以下几种类别：

- 强一致性：读操作可以立即读到提交的更新操作。
- 弱一致性：提交的更新操作，不一定立即会被读操作读到，此种情况会存在一个不一致窗口，指的是读操作可以读到最新值的一段时间。
- 最终一致性：是弱一致性的特例。事务更新一份数据，最终一致性保证在没有其他事务更新同样的值的话，最终所有的事务都会读到之前事务更新的最新值。如果没有错误发生，不一致窗口的大小依赖于：通信延迟，系统负载等。
- 单调一致性：如果一个进程已经读到一个值，那么后续不会读到更早的值。

- 会话一致性：保证客户端和服务器交互的会话过程中，读操作可以读到更新操作后的最新值。



## 隔离性

多个事务并发访问时，一个事务不应该影响另一个事务。在并发环境中，当不同的事务同事操纵相同的数据时，每个事务都有各自完成的数据空间。由并发事务所做的修改，必须与任何其他并发事务所做的修改隔离。

并发时可能出现的问题：

- 脏读（dirty read）：事务A修改了一个数据，但未提交，事务B读到了事务A未提交的更新结果，如果事务A提交失败，事务B读到的就是脏数据。
- 不可重复度（unrepeatable  read）：在同一个事务中，对于同一份数据读取到的结果不一致。比如，事务B在事务A提交前读到的结果，和提交后读到的结果可能不同。
- 幻读（phantom read）：在同一个事务中，同一个查询多次返回的结果不一致。事务A新增了一条记录，事务B在事务A提交前后各执行了一次查询操作，发现后一次比前一次多了一条记录。



以下示例以此表为例：

| id   | name | balance |
| ---- | ---- | ------- |
| 1    | A    | 1000    |
| 2    | B    | 1000    |
| 3    | C    | 1000    |



### 脏读（dirty read）

假设有两个事务，一个进行转账操作，将A账户的 100 元 转到 B 账户，同事另一个事务在对 A 和 B 的账户余额进行求和统计，如下：

![image](https://wx1.sinaimg.cn/large/69d4185bgy1g3vv5bu55pj20pd0do0tl.jpg)

我们不考虑任何加锁机制，仅仅从程序运行的角度来看，事务 1 执行成功之后，A 成功转了 100 元到 B 账户，这时 A 余额还剩 900 元，B 余额剩 1100，总和为 2000；但是事务 2 的统计求和算出来的结果却是 A + B = 1900。这个就是上面说的，没有提交的事务被其他事务读取到了，这叫做 **脏读** 。

### 不可重复读（unrepeatable read）

如下图所示，事务 2 第一次获取 A 的账户余额为 1000 元，这个时候事务 1 对 A 的账户余额进行操作减去了 100 元，事务 2 再去查询 A 的账户余额发现变成了 900 元，这样就出现了**同一个事务对同一条记录读取**两遍，两次读出来的结果竟然不一样。

![image](https://wx4.sinaimg.cn/large/69d4185bgy1g3vvh6x08dj20oz0dswf9.jpg)

不可重复读和脏读的区别在于，脏读是读取了另一个事务未提交的修改，而不可重复读是读取了另一个事务提交之后的修改，本质上都是其他事务的修改影响了本事务的读取。那么不可重复读有什么问题呢？假设上面的事务 2 存在着两个子例程，第一个例程是读取所有账户的余额计算总和，可以得到所有人的余额总和为 3000，第二个例程是再次读取所有账户的余额计算平均值，3 个人总和 3000 按理应该是平均每人 1000 才对，却计算出了 2900/3 = 966，这就导致了数据不一致。



### 幻读（phantom read）

幻读，即：**同样的条件，第一次和第二次读出来的记录数不一样。**幻读和不可重复读的区别在于，后者是两次读取同一条记录，得到不一样的结果；而前者是两次读取同一个范围内的记录，得到不一样的记录数（这种说法其实只是便于理解，但并不准确，因为可能存在另一个事务先插入一条记录然后再删除一条记录的情况，这个时候两次查询得到的记录数也是一样的，但这也是幻读，所以严格点的说法应该是：**两次读取得到的结果集不一样**）。很显然，不可重复读是因为其他事务进行了 UPDATE 操作，幻读是因为其他事务进行了 INSERT 或者 DELETE 操作。同样的，下面举一个幻读的例子：

![image](https://wx4.sinaimg.cn/large/69d4185bly1g3zlt1t28kj20ro0ehdgs.jpg)

事务 2 的两次查询，第一次查出 2 条记录，第二次却查出 3 条记录，多出来的这条记录，正如 phantom（幽灵，幻影，错觉） 的意思，就像幽灵一样。



### 丢失更新（lost update）

上面说的三种情况，都是一个事务写，一个事务读，由于一个事务的写导致另一个事务读到了不该读的数据；那么如果两个事务都是写，又会发生什么呢？

假设两个事务同时对 A 的余额进行修改，他们都查出 A 的当前余额为 1000，然后事务 2 修改 A 的余额，将 A 的余额加 100 变成 1100 并提交，这个时候 A 的余额应该是 1100，但是这个时候事务 1 并不知道 A 的余额已经变动，而是继续在 1000 的基础上进行减 100 的操作并提交事务，就这样事务 2 的提交被覆盖掉了，事务 1 提交之后 A 的余额变成了 900 元。这就是说事务 1 的提交覆盖了事务 2 的提交，事务 2 的 UPDATE 操作完全丢失了，整个过程如下图所示：

![image](https://wx4.sinaimg.cn/large/69d4185bgy1g3zmc3wg1uj20qt0ds0ts.jpg)

这就是经典的 **丢失更新** 问题，由于最后一步是提交操作，所以又叫做 **提交覆盖**，有时候又叫 [**Read-Modify-Write 问题**](https://en.wikipedia.org/wiki/Read-modify-write)。一个典型的场景是并发对某个变量进行自增或自减，譬如商品表的库存字段，每次下单之后库存值需要减 1，大概的流程如下：

1. SELECT name, stock FROM product WHERE id = 100;
2. 判断 stock 值是否足够，如果足够，则下单：if (stock > n) process order;
3. 更新 stock 值，减去下单的商品数量：new_stock = stock - n;
4. UPDATE product SET stock = new_stock WHERE id = 100;

如果两个线程同时下单，很可能就会出现下面这样的情况：

1. 线程 A 获取库存值为 10；
2. 线程 B 获取库存值为 10；
3. 线程 A 需要买 5 个商品，校验通过，并下单；
4. 线程 B 需要买 5 个商品，校验通过，并下单；
5. 线程 A 下单完成，更新库存值为 10 - 5 = 5；
6. 线程 B 下单完成，更新库存值为 10 - 5 = 5；

两个线程下单结束后，商品的库存还剩 5 个，而实际上 10 个商品都已经卖光了。和提交覆盖相对的，还有另一个 **丢失更新** 问题，叫做 **回滚覆盖**，如下图所示：

![image](https://wx1.sinaimg.cn/large/69d4185bgy1g3zmfw2zrwj20qt0ca3zg.jpg)

操作和提交覆盖情景基本上一样，只是最后一步事务 1 的提交变成了回滚，这样 A 的余额恢复成原始值 1000，事务 2 的 UPDATE 操作也完全没有生效，想一想这真的是非常可怕，一个事务的回滚操作竟然影响了另一个正常提交的事务。回滚覆盖问题可以说是程序 bug 了，因此几乎所有的数据库都不允许回滚覆盖。

有时候我们把回滚覆盖称之为 **第一类丢失更新** 问题，提交覆盖称为 **第二类丢失更新** 问题。

### 隔离级别

为了解决上面出现的问题，数据库使用隔离技术来解决上面的问题，不同隔离的级别可以解决不同的问题，数据库的隔离级别有以下几种：

- 读未提交（Read Uncommited）：最低的隔离级别，什么都不需要做，一个事务可以读到另一个事务未提交的结果。所有的并发事务问题都会发生。会出现脏读问题。
- 读已提交（Read Commited）：只有在事务提交后，其更新结果才会被其他事务看见。可以解决脏读问题。但会出现不可重复读和幻读。但多数数据库默认隔离级别是 RC，MySQL的默认隔离级别为 RR。
- 可重复读（Repeatable Read）：在一个事务中，对于同一份数据的读取结果总是相同的，无论是否有其他事务对这份数据进行操作，以及这个事务是否提交。可以解决脏读、不可重复读，但依然不可避免幻读。MySQL InnoDB 的默认隔离级别。
- 串行化（Serialization）：事务串行化执行，隔离级别最高，牺牲了系统的并发性。可以解决并发事务的所有问题。



**隔离级别与读现象的关系**

|           隔离级别           | 脏读 | 丢失更新 | 不可重复读 | 幻读 | 并发模型 | 更新冲突检测 |
| :--------------------------: | :--: | :------: | :--------: | :--: | :------: | ------------ |
| 读未提交（Read Uncommitted） |  √   |    √     |     √      |  √   |   悲观   | ×            |
|  读已提交（Read Commited）   |  ×   |    ×     |     √      |  √   |   悲观   | ×            |
| 可重复读（Repeatable Read）  |  ×   |    ×     |     ×      |  √   |   悲观   | ×            |
|   串行化（serialization）    |  ×   |    ×     |     ×      |  ×   |   悲观   | ×            |

## SQL

[SQL](https://zh.wikipedia.org/wiki/SQL)国际标准使用`START TRANSACTION`开始一个事务（也可以用方言命令`BEGIN`）。`COMMIT`语句使事务成功完成。`ROLLBACK`语句结束事务，放弃从`BEGIN TRANSACTION`开始的一切变更。若[autocommit](https://zh.wikipedia.org/wiki/Autocommit)被`START TRANSACTION`的使用禁止，在事务结束时autocommit会重新激活。



## 回滚

**回滚**（英语：rollback）是[数据库](https://zh.wikipedia.org/wiki/数据库)技术中的操作，放弃修改，使数据库状态恢复到此前的某个时刻。这对[数据完整性](https://zh.wikipedia.org/wiki/数据完整性)具有关键意义。回滚是[数据库事务](https://zh.wikipedia.org/wiki/数据库事务)管理重要一环。回滚特性通常用[数据库日志](https://zh.wikipedia.org/w/index.php?title=数据库日志&action=edit&redlink=1)实现，但也可以用[多版本并发控制](https://zh.wikipedia.org/wiki/多版本并发控制)实现。

*级联回滚*(cascading rollback)是指数据库的一个事务的失败引起多个事务随之失败，都要各自回滚。

[SQL](https://zh.wikipedia.org/wiki/SQL)中, `ROLLBACK`是一条命令，引起从最后一次`BEGIN WORK`或`START TRANSACTION`开始的数据改变被抛弃，数据库状态恢复到改变之前。

一条`ROLLBACK`语句也释放任何已存的[savepoint](https://zh.wikipedia.org/wiki/Savepoint)。

在大多数SQL语言实现中，`ROLLBACK`是特定于连接。即如果两个连接指向同一个数据库，在一个连接上的`ROLLBACK`并不影响另一个连接

## Autocommit

在[数据库](https://zh.wikipedia.org/wiki/数据库)技术中，**autocommit**是一种[数据库连接](https://zh.wikipedia.org/wiki/数据库连接)模式，使得每条数据库操作（如每条[SQL](https://zh.wikipedia.org/wiki/SQL)语句）作为一条事务执行，因此不能被[回滚](https://zh.wikipedia.org/wiki/回滚_(数据管理))。

[Microsoft SQL Server](https://zh.wikipedia.org/wiki/Microsoft_SQL_Server)以及[ODBC](https://zh.wikipedia.org/wiki/ODBC)与[Microsoft OLE DB](https://zh.wikipedia.org/w/index.php?title=Microsoft_OLE_DB&action=edit&redlink=1), autocommit模式是改变数据的所有语句的默认模式，以保证单条语句遵从[ACID](https://zh.wikipedia.org/wiki/ACID)。

autocommit模式对立面是non-autocommit，意味着[SQL](https://zh.wikipedia.org/wiki/SQL)客户应用自身负责发出事务语句*start transaction*、*commit*、*rollback*命令。

# 事务的类型

[高级数据库三：浅谈数据库事务（transaction）](https://blog.csdn.net/u013007900/article/details/77927723)

## 平面型事务（FLAT TRANSACTIONS）

使用调用或语句级别的接口访问DBMS，一般开始于Begin，接着是一系列操作，最后以COMMIT或者ROLLBACK结束。

![image](https://ws3.sinaimg.cn/large/69d4185bgy1g3znsrdaizj20hh0agq3o.jpg)



**特点**

- 无内部结构
- 只能面向单一的DBMS，在同一时间只能运行一个事务
- 只适用于简单的应用

一旦回滚，则将整个事务的所有操作进行回滚，所有在没有COMMIT之前的操作全部丢失。

## 嵌套事务（NESTED TRANSACTIONS）

嵌套事务是一套有层次的操作，子事务的结果要依赖于它父亲事务的操作。

![image](https://ws1.sinaimg.cn/large/69d4185bgy1g3zo701tykj20ph0f0abk.jpg)



## 链式事务（CHAINED TRANSACTIOINS）

多个事务顺序执行，一个事务COMMIT之后，另一个事务马上BEGIN，没有事务可以两个事务之间对数据进行修改。

![1560419544020](C:\Users\LuQiu\AppData\Roaming\Typora\typora-user-images\1560419544020.png)





# Spring中隔离级别

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

- [spring事务-说说Propagation及其实现原理](https://blog.csdn.net/yanyan19880509/article/details/53041564)

- [MySQL事务隔离级别和Spring事务关系介绍](https://blog.csdn.net/a837199685/article/details/54563740)

- [理解Spring的事务传播机制](https://waylau.com/spring-transaction/)

- [深入分析spring事务传播行为](https://www.itcodemonkey.com/article/12302.html)

- [Spring事务管理与传播机制详解以及使用实例](https://blog.csdn.net/hcmony/article/details/77850183)



事务传播行为用于指定在多个事务方法间调用时，事务是如何在这些方法间传播的。



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

## REQUIRED

`required` 是spring 默认的事务传播方式，表示如果当前存在一个事务，则新的事务加入到已存在的事务中；如果当前不存在事务，则开启一个新的事务。

```java
@Transactional
methodA{
	methodB()
}
@Transactional
methondB(){}
```

spring 使用 AOP 来支持声明式事务，会根据事务属性，自动在调用方法之前决定是否开启一个事务，并在方法执行之后决定事务提交或者回滚。如果，是单独调用上面的，那么就相当于：

```java
main{ 
    Connection con=null; 
    try{ 
        con = getConnection(); 
        con.setAutoCommit(false); 
        //方法调用
        methodB(); 
        //提交事务
        con.commit(); 
    } Catch(RuntimeException ex) { 
        //回滚事务
        con.rollback();   
    } finally { 
        //释放资源
        closeCon(); 
    } 
}
```

spring 保证在方法 B 中多有的调用都在一个相同的连接之上。在调用方法 B 时，没有一个存在的事务，所以获得一个新的连接，开启一个新的事务。当是方法 A 里面调用方法 B 时，执行的效果相当于下面：

```java
main{ 
    Connection con = null; 
    try{ 
        con = getConnection(); 
        methodA(); 
        con.commit(); 
    } catch(RuntimeException ex) { 
        con.rollback(); 
    } finally {    
        closeCon(); 
    }  
}
```

调用方法 A  时，环境中还没有事务，所以开启一个新的事务。当在方法A中调用方法B时，环境中已经存在一个事务，所以将 B 加入当前事务。

![image](https://ws1.sinaimg.cn/large/69d4185bgy1g47l3kjnh5j20cx092wew.jpg)

使用 `required` 方式的事务，只要方法A或者B任何有异常事务就会回滚。

## SUPPORTS

`supports` 方式的事务表示，如果当前存在事务就加入当前事务，若没有事务则以非事务的方式执行。如下示例：

```java
@Transactional
methodA(){
	methodB()
}
@Transactional(propagation = Propagation.SUPPORTS)
methodB(){}
```

如上面的方式，如果单独调用方法B，因为当前没有事务，因此方法B以非事务的方式运行。而如果调用方法A那么B就加入A的事务，这样方法A或者B任何一个有异常整个事务就会回滚。

![image](https://ws2.sinaimg.cn/large/69d4185bgy1g47m55mfydj20u80bi0u3.jpg)

## MANDATORY

`MANDATORY` 表示方法必须在事务中运行，当前有事务则加入，若当前没有事务，则抛出异常。

![image](https://wx2.sinaimg.cn/large/69d4185bgy1g47mlrpztmj20u60bkta6.jpg)

当方法A有事务时，方法A或者B抛出异常，整个事务会回滚。

## REQUIRES_NEW

`REQUIRES_NEW` 表示总是创建一个新的事务，如果当前存在事务，那么就把当前的事务挂起。如下示例：

```java
@Transactional
methodA(){
    doSomeThingA();
    methodB();
    doSomeThingB();
}
 @Transactional(propagation = Propagation.REQUIRES_NEW)
 methodB(){}
```

那么调用方法A就相当于：

```java

main(){
    TransactionManager tm = null;
    try{
        //获得一个JTA事务管理器
        tm = getTransactionManager();
        tm.begin();//开启一个新的事务
        Transaction ts1 = tm.getTransaction();
        doSomeThing();
        tm.suspend();//挂起当前事务
        try{
            tm.begin();//重新开启第二个事务
            Transaction ts2 = tm.getTransaction();
            methodB();
            ts2.commit();//提交第二个事务
        } Catch(RunTimeException ex) {
            ts2.rollback();//回滚第二个事务
        } finally {
            //释放资源
        }
        //methodB执行完后，恢复第一个事务
        tm.resume(ts1);
        doSomeThingB();
        ts1.commit();//提交第一个事务
    } catch(RunTimeException ex) {
        ts1.rollback();//回滚第一个事务
    } finally {
        //释放资源
    }
}
```

在这里 `ts1` 称为外层事务，`ts2` 称为内层事务。从上面可以看到，`ts1`与 `ts2` 是两个独立的事务，互不相干。如果方法A在调用方法B之后调用的 `doSomeThingB` 失败了，但方法B所做的结果依然会被提交，而除了方法B之外的结果都会回滚。使用PROPAGATION_REQUIRES_NEW,需要使用 JtaTransactionManager作为事务管理器。

![image](https://wx1.sinaimg.cn/large/69d4185bgy1g47ncgz7e1j20i00as3z8.jpg)

## NESTED

`NESTED` 如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于`REQUIRED`，即方法运行在一个新事务中。

这是一个嵌套事务,使用JDBC 3.0驱动时，仅仅支持 `DataSourceTransactionManager` 作为事务管理器。需要 JDBC 驱动的 `java.sql.Savepoint` 类。有一些 JTA 的事务管理器实现可能也提供了同样的功能。使用 `PROPAGATION_NESTED`，还需要把`PlatformTransactionManager` 的 `nestedTransactionAllowed`属性设为 true；而 `nestedTransactionAllowed` 属性值默认为 false。

![image](https://wx1.sinaimg.cn/large/69d4185bgy1g47o0guqvoj20ua0bdjt7.jpg)

当方法A不存在事务，则方法B运行在一个新事务中，B抛出异常，B事务回滚，A不回滚。A抛出异常，A和B都不回滚。

若A存在事务，则A抛出异常A和B都会被回滚；若B抛出异常，A不捕获，则A和B都会回滚，若A捕获了异常，则只有B回滚，而A不回滚。

---

`PROPAGATION_NESTED` 与 `PROPAGATION_REQUIRES_NEW` 的区别是，`PROPAGATION_REQUIRES_NEW` 另开启一个事务，将会与它的父事务相互独立，而`PROPAGATION_NESTED` 的事务和它的父事务是相依的，它的提交要和它的父事务一起。也就是说，如果父事务最后回滚，它也要回滚。如果子事务回滚或提交，不会导致父事务回滚或提交，但父事务回滚将导致子事务回滚。

## NOT_SUPPORTED

`NOT_SUPPORTED` 以非事务方式运行，如果当前存在事务，则把当前事务挂起。

## NEVER

`NEVER`：以非事务方式运行，如果当前存在事务，则抛出异常。

