# AOP key terms



```java
@Aspect
public class EmployeeCRUDAspect {
     
    @Before("execution(* EmployeeManager.getEmployeeById(..))")
    public void logBeforeV1(JoinPoint joinPoint){
        System.out.println("EmployeeCRUDAspect.logBeforeV1() : " + joinPoint.getSignature().getName());
    }
     
    @After("execution(* EmployeeManager.getEmployeeById(..))")
    public void logAfterV1(JoinPoint joinPoint){
        System.out.println("EmployeeCRUDAspect.logAfterV1() : " + joinPoint.getSignature().getName());
    }
}
```

```java
@Component
public class EmployeeManager{
    public EmployeeDTO getEmployeeById(Integer employeeId) {
        System.out.println("Method getEmployeeById() called");
        return new EmployeeDTO();
    }
 
    public List<EmployeeDTO> getAllEmployee() {
        System.out.println("Method getAllEmployee() called");
        return new ArrayList<EmployeeDTO>();
    }
 
    public void createEmployee(EmployeeDTO employee) {
        System.out.println("Method createEmployee() called");
    }
 
    public void deleteEmployee(Integer employeeId) {
        System.out.println("Method deleteEmployee() called");
    }
 
    public void updateEmployee(EmployeeDTO employee) {
        System.out.println("Method updateEmployee() called");
    }
}
```

The `applicationContext.xml` file has below configuration to enable AOP and IoC container using annotation configuration.

```xml
<aop:aspectj-autoproxy />
<context:component-scan base-package="com.howtodoinjava.demo.aop" />
<bean id="loggingAspect" class="com.howtodoinjava.demo.aop.EmployeeCRUDAspect" />
```

Let us now define some central AOP concepts and terminology and relate with above example.

![spring-aop-diagram](https://cdn2.howtodoinjava.com/wp-content/uploads/2015/01/spring-aop-diagram.jpg)

**1) Aspect :** a modularization of a concern that cuts across multiple classes. Transaction management is a good example of a crosscutting concern in enterprise Java applications.

**2) Join point :** a point during the execution of a program, such as the execution of a method or the handling of an exception. In Spring AOP, a join point always represents a method execution.In our example, all the **methods** defined inside `EmployeeManager` are joint points.

**3) Advice :** **action** taken by an aspect at a particular join point. Different types of advice include “around,” “before” and “after” advice. Many AOP frameworks, including Spring, model an advice as an interceptor, maintaining a chain of interceptors around the join point.In our example, all `logBefore()` and `logAfter()` methods are advices.

**4) Pointcut :** **a predicate that matches join points.** Advice is associated with a pointcut expression and runs at any join point matched by the pointcut (for example, the execution of a method with a certain name). The concept of join points as matched by pointcut expressions is central to AOP, and Spring uses the AspectJ pointcut expression language by default.

In our example, the expressions passed in `@Before` and `@After` annotations (i.e. “`execution(* EmployeeManager.getEmployeeById(..))`“) are pointcuts.

**5) Introduction :** declaring additional methods or fields on behalf of a type. Spring AOP allows you to introduce new interfaces (and a corresponding implementation) to any advised object. For example, you could use an introduction to make a bean implement an IsModified interface, to simplify caching.

**6) Target object :** object being advised by one or more aspects. Also referred to as the advised object. Since Spring AOP is implemented using runtime proxies, this object will always be a proxied object.

In our example, `EmployeeManager` is advised object hence it is the target object.

**7) AOP proxy :** an object created by the AOP framework in order to implement the aspect contracts (advise method executions and so on). In the Spring Framework, an AOP proxy will be a JDK dynamic proxy or a CGLIB proxy.

So we are good now and can relate the key terms of spring AOP with actual pieces of codes. Now moving further, let’s list down **types of advices** which are available for use in spring AOP.

1. **Before advice :** Advice that executes before a join point, but which does not have the ability to prevent execution flow proceeding to the join point (unless it throws an exception). To use this advice, use @Before annotation as used in our example above.
2. **After returning advice :** Advice to be executed after a join point completes normally: for example, if a method returns without throwing an exception. To use this advice, use @AfterReturning annotation.
3. **After throwing advice :** Advice to be executed if a method exits by throwing an exception. To use this advice, use @AfterThrowing annotation.
4. **After advice :** Advice to be executed regardless of the means by which a join point exits (normal or exceptional return). To use this advice, use @After annotation as used in our example above.
5. **Around advice :** Advice that surrounds a join point such as a method invocation. This is the most powerful kind of advice. To use this advice, use @Around annotation.



Suppose you have multiple aspects in your application and they are can be applied on a certain method. When there’s more than one aspect applied to the same join point, the precedence/order of the aspects will not be determined unless you have explicitly specified it using either `@Order`annotation or `org.springframework.core.Ordered` interface

```java
@Aspect
@Order(0)
public class EmployeeCRUDTransactionAspect{
    @Before("execution(* EmployeeManager.getEmployeeById(..))")
    public void getEmployeeById(JoinPoint joinPoint){
        System.out.println("EmployeeCRUDTransactionAspect.getEmployeeById() : " + joinPoint.getSignature().getName());
    }
}
 
@Aspect
@Order(1)
public class EmployeeCRUDLoggingAspect{
    @Before("execution(* EmployeeManager.getEmployeeById(..))")
    public void logBefore(JoinPoint joinPoint){
        System.out.println("EmployeeCRUDAspect.logBefore() : " + joinPoint.getSignature().getName());
    }
}
```

or

```java
@Aspect
public class EmployeeCRUDLoggingAspect implements Ordered{
    //Override this method
    public int getOrder() {
        return 0;
    }
     
    @Before("execution(* EmployeeManager.getEmployeeById(..))")
    public void logBefore(JoinPoint joinPoint){
        System.out.println("EmployeeCRUDAspect.logBefore() : " + joinPoint.getSignature().getName());
    }
}
 
@Aspect
public class EmployeeCRUDTransactionAspect implements Ordered{
    //Override this method
    public int getOrder() {
        return 1;
    }
 
    @Before("execution(* EmployeeManager.getEmployeeById(..))")
    public void getEmployeeById(JoinPoint joinPoint){
        System.out.println("EmployeeCRUDTransactionAspect.getEmployeeById() : " + joinPoint.getSignature().getName());
    }
}
```

