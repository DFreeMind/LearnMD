# Singleton Design Pattern

## What is Singleton Design Pattern

If you are in Java then you must have used the new keyword. This new keyword creates an Object of class whenever required. But there are some scenarios where you don’t want to create individual Object for a different purpose. **Singleton Pattern**ensures that one and only one Object is instantiated for a given class. Whenever an object of a given class is required, only single(No more than one object) Object get returned.

There are different ways you can implement Singleton. Before you start implementation, you need to understand what are the different ways to create an Object.

**Different Ways to create an Object**

- ***new*** Keyword
- Using **the New Instance** (Reflection)
- Cloning of Object
- Using Classloader
- Using Object De-Serialization

To make a singleton class you have to disable all these ways to create an object. One way to achieve this is to make the constructor of a given class private. But when you make the constructor private there is no way to create even a single object of the class.  So first make a constructor private and then prepare a way to create an object(Single) of that class.



## Implementation One

Here we will create a private Constructor and also a static method to create an object of the same class.

```java
public class Singleton {
    private static Singleton instance;
    private Singleton(){}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

**Problem**

What will happen if 2 different thread enters the **getInstance** method at the same time when the instance is null. In that case, threads will create 2 different objects of Singleton. Which is against the Singleton pattern. In the next approach, this problem can be solved.



## Implementation Two

In this approach, we will make createInstance method synchronized so only one thread is allowed in that class and only one object will be created instead of Two.

```java
public class Singleton {
    private static Singleton instance;
    private Singleton(){}

    public synchronized static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

**Problem**
The moment we use synchronized keyword it will create a problem for our multi-threaded application in terms of performance. So on one side, we are resolving the problem on another side we are creating one more problem. In the next approach, we will solve both this problem.



## Implementation Three

```java
public class Singleton {
    private static Singleton instance = new Singleton();
    private Singleton(){}

    public static Singleton getInstance() {
        return instance;
    }
}
```

**Problem**
Here we are creating the object of `Singleton` when class gets loaded. The object gets created even when it is not required. The object should be created only when it is required(Lazy Loading).  In the next approach, we will try to resolve this problem by using Double checking locking.

## Implementation Four

```java
public class Singleton {
    private static Singleton instance;
    private Singleton(){}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class){
                if (instance == null) {
                    instance = new Singleton()
                }
            }
        }
        return instance;
    }
}
```

**Problem**
All problem has been solved in this approach still synchronized keyword is used(Once) and that should be avoided.

## Implementation Five

```java
public class Singleton {
    private Singleton(){}
    private static class SingletonHelper{
        private static final Singleton INSTANCE = new Singleton();
    }
    public static Singleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```



# Factory design pattern

[Factory design pattern](<https://javabeginnerstutorial.com/design-pattern/factory-design-pattern-java/>)

[Factory Design Pattern in Java](<https://www.journaldev.com/1392/factory-design-pattern-in-java>)

## Why should I care?

Factory, as the name suggests, is a place to create some different products which are somehow similar in features yet divided into categories.In Java, factory pattern is used to create instances of different classes of the same type.

Factory design pattern is used when we have a super class with multiple sub-classes and based on input, we need to return one of the sub-class. This pattern take out the responsibility of instantiation of a class from client program to the factory class.

```java
public void getFirst(List<String> input) {
   if(input != null && !input.isEmpty())
       return input.get(0);
   return null;
}
```

Here we accept a List of String objects — the implementation does not matter. And List is an interface. We could naturally use `ArrayList` as expected parameter but this would limit some calls to our method. For example, someone has a `LinkedList` (which is a different implementation of List) and in this case, he/she has to convert all the elements into an ArrayList to call our method — so using `List` makes it more usable for others.

## When to use factory pattern?

Factory pattern introduces **loose coupling between classes** which is the most important principle one should consider and apply while designing the application architecture. Loose coupling can be introduced in application architecture by programming against abstract entities rather than concrete implementations. This not only makes our architecture more flexible but also less fragile.

![factory pattern java, factory pattern, factory design pattern, factory pattern class diagram](https://cdn.journaldev.com/wp-content/uploads/2013/05/factory-pattern-java-450x327.png)

If we are unfortunate then we will create instances of car types (e.g. `SmallCar`) in our application classes and thus we will expose the car building logic to the outside world and this is certainly not good. It also prevents us in making changes to car making process because the code is not centralized, and making changes in all composing classes seems not feasible.

## Factory Pattern Example

Super class in factory design pattern can be an interface, **abstract class** or a normal java class. For our factory design pattern example, we have abstract super class with [overridden](https://www.journaldev.com/817/java-override-method-overriding)`toString()` method for testing purpose.

### Super Class

```java
public abstract class Computer{
   public abstract String getRAM();
   public abstract String getHDD();
   public abstract String getCPU();

   public String toStirng(){
       return "RAM= "+this.getRAM()+", HDD="+this.getHDD()+", CPU="+this.getCPU();
   }
}
```

### Sub Classes

Let’s say we have two sub-classes PC and Server with below implementation.both the classes are extending `Computer` super class.

```java
class PC extends Computer{
   private String ram;
   private String hdd;
   private String cpu;

   public PC(String ram, String hdd, String cpu) {
      this.ram = ram;
      this.hdd = hdd;
      this.cpu = cpu;
   }

   @Override
   public String getRAM() {
      return this.ram;
   }

   @Override
   public String getHDD() {
      return this.hdd;
   }

   @Override
   public String getCPU() {
      return this.cpu;
   }
}

class Server extends Computer{
   private String ram;
   private String hdd;
   private String cpu;

   public Server(String ram, String hdd, String cpu) {
      this.ram = ram;
      this.hdd = hdd;
      this.cpu = cpu;
   }

   @Override
   public String getRAM() {
      return this.ram;
   }

   @Override
   public String getHDD() {
      return this.hdd;
   }

   @Override
   public String getCPU() {
      return this.cpu;
   }
}
```

### Factory Class

Now that we have super classes and sub-classes ready, we can write our factory class. Here is the basic implementation.

```java
public class ComputerFactory{
   public static Computer getComputer(String type, String ram,
                                      String hdd, String cpu){
      if ("PC".equalsIgnoreCase(type)) {
         return new PC(ram, hdd, cpu);
      }else if ("Server".equalsIgnoreCase(type)) {
         return new Server(ram, hdd, cpu);
      }
      return null;
   }
}
```

Some important points about Factory Design Pattern method are;

1. We can keep Factory class [Singleton](https://www.journaldev.com/1377/java-singleton-design-pattern-best-practices-examples) or we can keep the method that returns the subclass as [static](https://www.journaldev.com/1365/static-keyword-in-java).
2. Notice that based on the input parameter, different subclass is created and returned. `getComputer` is the factory method.

Here is a simple test client program that uses above factory design pattern implementation.

```java
class TestFactory {
   public static void main(String[] args) {
      Computer pc = ComputerFactory.getComputer("pc","2 GB",
              "500 GB","2.4 GHz");
      Computer server = ComputerFactory.getComputer("server","16 GB",
              "1 TB","2.9 GHz");
      System.out.println("Factory PC Config::"+pc.toStirng());
      System.out.println("Factory Server Config::"+server.toStirng());
   }
}
// Factory PC Config::RAM= 2 GB, HDD=500 GB, CPU=2.4 GHz
// Factory Server Config::RAM= 16 GB, HDD=1 TB, CPU=2.9 GHz
```



## Another Example

A picture is worth a thousand words. Let’s see how a factory implementation will look like.

![class_diagram_of_factory_pattern_in_java](https://howtodoinjava.files.wordpress.com/2012/10/class_diagram_of_factory_pattern_in_java1.png)

Above class-diagram depicts a common scenario using an example of a car factory which is able to build 3 types of cars i.e. small, sedan and luxury. Building a car requires many steps from allocating accessories to final makeup. These steps can be written in programming as methods and should be called while creating an instance of a specific car type.

#### Object types

`CarType` will hold the types of car and will provide car types to all other classes.

```java
enum CarType{
   SMALL, SEDAN, LUXURY
}
```

#### Object implementations

`Car` is parent class of all car instances and it will also contain the common logic applicable in car making of all types.

```java
abstract class Car{
   private CarType model;

   public Car(CarType model) {
      this.model = model;
   }

   private void arrangeParts(){
      // Do one time processing here
   }

   // Do subclass level processing in this method
   protected abstract void construct();

   public CarType getModel() {
      return model;
   }

   public void setModel(CarType model) {
      this.model = model;
   }
}
```

### LuxuryCar

`LuxuryCar` is concrete implementation of car type `LUXURY`.

```java
class LuxuryCar extends Car{
   public LuxuryCar() {
      super(CarType.LUXURY);
      construct();
   }

   @Override
   protected void construct() {
      System.out.println("Building luxury car");
   }
}
```

### SmallCar

`SmallCar` is concrete implementation of car type `SMALL`.

```java
class SmallCar extends Car{
   public SmallCar() {
      super(CarType.SMALL);
      construct();
   }

   @Override
   protected void construct() {
      System.out.println("Building small car");
   }
}
```

### SedanCar

`SedanCar` is concrete implementation of car type `SEDAN`.

```java
class SedanCar extends Car{
   public SedanCar() {
      super(CarType.SEDAN);
      construct();
   }

   @Override
   protected void construct() {
      System.out.println("Building sedan car");
   }
}
```

### Factory method

```java
class CarFactory{
   public static Car buildCar(CarType model){
      Car car = null;
      switch (model){
         case SMALL:
            car = new SmallCar();
            break;
         case SEDAN:
            car = new SedanCar();
            break;
         case LUXURY:
            car = new LuxuryCar();
            break;
          default:
             break;
      }
      return car;
   }
}
```

### Test Car Factory

```java
class TestCarFactory{
   public static void main(String[] args) {
      System.out.println(CarFactory.buildCar(CarType.SMALL));
      System.out.println(CarFactory.buildCar(CarType.SEDAN));
      System.out.println(CarFactory.buildCar(CarType.LUXURY));
   }
}
```



### Factory Design Pattern Advantages

1. Factory design pattern provides approach to code for interface rather than implementation.
2. Factory pattern removes the instantiation of actual implementation classes from client code. Factory pattern makes our code more robust, less coupled and easy to extend. For example, we can easily change PC class implementation because client program is unaware of this.
3. Factory pattern provides abstraction between implementation and client classes through inheritance.
4. The lifetime management of the generated objects must be centralized to ensure a consistent behavior within the application.

## Final notes

**Factory pattern is most suitable where there is some complex object creation steps are involved**. To ensure that these steps are centralized and not exposed to composing classes, factory pattern should be used. We can see many realtime examples of factory pattern in JDK itself e.g.

- [java.sql.DriverManager#getConnection()](https://docs.oracle.com/javase/10/docs/api/java/sql/DriverManager.html#getConnection(java.lang.String))
- [java.net.URL#openConnection()](https://docs.oracle.com/javase/10/docs/api/java/net/URL.html#openConnection())
- [java.lang.Class#newInstance()](https://docs.oracle.com/javase/10/docs/api/java/lang/Class.html#newInstance())
- [java.lang.Class#forName()](https://docs.oracle.com/javase/10/docs/api/java/lang/Class.html#forName(java.lang.String))



# Abstract Factory Design Pattern

[Abstract Factory Design Pattern in Java](https://www.journaldev.com/1418/abstract-factory-design-pattern-in-java)

**Abstract factory pattern** is yet another [creational design pattern](https://howtodoinjava.com/category/design-patterns/creational/) and is considered as another layer of abstraction over [factory pattern](https://howtodoinjava.com/design-patterns/creational/implementing-factory-design-pattern-in-java/). If you are familiar with [**factory design pattern in java**](https://www.journaldev.com/1392/factory-design-pattern-in-java), you will notice that we have a single Factory class. This factory class returns different subclasses based on the input provided and factory class uses if-else or switch statement to achieve this.

In the Abstract Factory pattern, we get rid of if-else block and have a factory class for each sub-class. Then an Abstract Factory class that will return the sub-class based on the input factory class. At first, it seems confusing but once you see the implementation, it’s really easy to grasp and understand the minor difference between Factory and Abstract Factory pattern.

## Super class and Subclasses

```java
abstract class Computer {
    public abstract String getRAM();

    public abstract String getHDD();

    public abstract String getCPU();

    public String toStirng() {
        return "RAM= " + this.getRAM() + ", HDD=" + this.getHDD() + ", CPU=" + this.getCPU();
    }
}
```

`PC`

```java
class PC extends Computer {
    private String ram;
    private String hdd;
    private String cpu;

    public PC(String ram, String hdd, String cpu) {
        this.ram = ram;
        this.hdd = hdd;
        this.cpu = cpu;
    }

    @Override
    public String getRAM() {
        return this.ram;
    }

    @Override
    public String getHDD() {
        return this.hdd;
    }

    @Override
    public String getCPU() {
        return this.cpu;
    }
}
```

`Server`

```java
class Server extends Computer {
    private String ram;
    private String hdd;
    private String cpu;

    public Server(String ram, String hdd, String cpu) {
        this.ram = ram;
        this.hdd = hdd;
        this.cpu = cpu;
    }

    @Override
    public String getRAM() {
        return this.ram;
    }

    @Override
    public String getHDD() {
        return this.hdd;
    }

    @Override
    public String getCPU() {
        return this.cpu;
    }
}
```

## Factory Class for Each subclass

First of all we need to create a Abstract Factory interface or [**abstract class**](https://www.journaldev.com/1582/abstract-class-in-java).

```java
interface ComputerAbstractFactory{
    public Computer createComputer();
}
```

Notice that `createComputer()` method is returning an instance of super class `Computer`. Now our factory classes will implement this interface and return their respective sub-class.

`PCFactory.java`

```java
class PCFactory implements ComputerAbstractFactory{

    private String ram;
    private String hdd;
    private String cpu;

    public PCFactory(String ram, String hdd, String cpu) {
        this.ram = ram;
        this.hdd = hdd;
        this.cpu = cpu;
    }

    @Override
    public Computer createComputer() {
        return new PC(ram, hdd, cpu);
    }
}
```

Similarly we will have a factory class for `Server` subclass.

`ServerFactory.java`

```java
class ServerFactory implements ComputerAbstractFactory{

    private String ram;
    private String hdd;
    private String cpu;

    public ServerFactory(String ram, String hdd, String cpu) {
        this.ram = ram;
        this.hdd = hdd;
        this.cpu = cpu;
    }

    @Override
    public Computer createComputer() {
        return new Server(ram, hdd, cpu);
    }
}
```

Now we will create a consumer class that will provide the entry point for the client classes to create sub-classes.

`ComputerFactory.java`

```java
class ComputerFactory{
    public static Computer getComputer(ComputerAbstractFactory factory){
        return factory.createComputer();
    }
}
```

Notice that its a simple class and `getComputer` method is accepting `ComputerAbstractFactory` argument and returning `Computer` object. At this point the implementation must be getting clear.

Let’s write a simple test method and see how to use the abstract factory to get the instance of sub-classes.

```java
class AbstractFactoryTest{
    public static void main(String[] args) {
        Computer pc = ComputerFactory.getComputer(
                new PCFactory("2 GB", "500 GB" , "2.4 Ghz")
        );
        Computer server = ComputerFactory.getComputer(
                new ServerFactory("16 GB", "1 TB" , "2.9 Ghz")
        );
        System.out.println("AbstractFactory PC config:" + pc.toStirng());
        System.out.println("AbstractFactory server Config:" + server.toStirng());
    }
}
// AbstractFactory PC config:RAM= 2 GB, HDD=500 GB, CPU=2.4 Ghz
// AbstractFactory server Config:RAM= 16 GB, HDD=1 TB, CPU=2.9 Ghz
```

## Abstract Factory Design Pattern Benefits

- Abstract Factory design pattern provides approach to code for interface rather than implementation.
- Abstract Factory pattern is “factory of factories” and can be easily extended to accommodate more products, for example we can add another sub-class Laptop and a factory LaptopFactory.
- Abstract Factory pattern is robust and avoid conditional logic of Factory pattern.

## Abstract Factory Design Pattern Examples in JDK

- javax.xml.parsers.DocumentBuilderFactory#newInstance()
- javax.xml.transform.TransformerFactory#newInstance()
- javax.xml.xpath.XPathFactory#newInstance()



# Proxy Design Pattern

[Proxy Design Pattern](<https://www.journaldev.com/1572/proxy-design-pattern>)

[Proxy Design Pattern](<https://howtodoinjava.com/design-patterns/structural/proxy-design-pattern/>)

Proxy design pattern intent according to GoF is:

**Provide a surrogate or placeholder for another object to control access to it.**

The definition itself is very clear and proxy design pattern is used when we want to provide controlled access of a functionality.

## When to use proxy design pattern

A proxy object hides the original object and control access to it. We can use proxy when we may want to use a class that can perform as an interface to something else.

Proxy is heavily used to implement lazy loading related usecases where we do not want to create full object until it is actually needed.

A proxy can be used to add an additional security layer around the original object as well. Let’s say we have a class that can run some command on the system. Now if we are using it, its fine but if we want to give this program to a client application, it can have severe issues because client program can issue command to delete some system files or change some settings that you don’t want.

## Real world example of proxy pattern

- In `hibernate`, we write the code to fetch entities from the database. Hibernate returns an object which a proxy (by dynamically constructed by Hibernate by extending the domain class) to the underlying entity class. The client code is able to read the data whatever it needs to read with the proxy.

  These proxy entity classes help in implementing lazy loading scenarios where associated entities are fetched only when they are requested explicitly. It helps in improving performance of DAO operations.

- In corporate networks, internet access is guarded behind a network proxy. All network requests goes through proxy which first check the requests for allowed websites and posted data to network. If request looks suspicious, proxy block the request – else request pass through.

- In aspect oriented programming (AOP), an object created by the AOP framework in order to implement the aspect contracts (advise method executions and so on). For example, in the [Spring AOP](https://howtodoinjava.com/spring-aop/spring-aop-aspectj-example-tutorial-using-annotation-config/), an AOP proxy will be a JDK dynamic proxy or a CGLIB proxy.

## Design participants

- **Subject** – is an interface which expose the functionality available to be used by the clients.
- **Real Subject** – is a class implementing `Subject` and it is concrete implementation which needs to be hidden behind a proxy.
- **Proxy** – hides the real object by extending it and clients communicate to real object via this proxy object. Usually frameworks create this proxy object when client request for real object.

![Proxy design pattern](https://cdn2.howtodoinjava.com/wp-content/uploads/2018/12/Proxy-design-pattern.jpg)

## Proxy design pattern example

In given example, we have a `RealObject` which client need to access to do something. It will ask the framework to provide an instance of `RealObject`. But as the access to this object needs to be guarded, framework returns the reference to `RealObjectProxy`.

Any call to proxy object is used for additional requirements and the call is passed to real object.

```java
interface RealObject{
    public void doSomething();
}

class RealObjectImpl implements RealObject{

    @Override
    public void doSomething() {
        System.out.println("Performing work in real object");
    }
}

class RealObjectProxy extends RealObjectImpl{
    @Override
    public void doSomething() {
        System.out.println("Delegating work on real object");
        super.doSomething();
    }
}

class Client{
    public static void main(String[] args) {
        RealObjectProxy proxy = new RealObjectProxy();
        proxy.doSomething();
    }
}
```



## Another Example

```java
interface CommmandExecutor{
    public void runCommand(String cmd) throws Exception;
}

class CommandExcutorImpl implements CommmandExecutor{
    @Override
    public void runCommand(String cmd) throws IOException {
        // some heavy implementation
        Runtime.getRuntime().exec(cmd);
        System.out.println("'" + cmd + "' command executed.");
    }
}
```

Now we want to provide only admin users to have full access of above class, if the user is not admin then only limited commands will be allowed. Here is our very simple proxy class implementation.

```java
class CommandExecutorProxy implements CommandExecutor{
    private boolean isAdmin;
    private CommandExecutor executor;

    public CommandExecutorProxy(String user, String pwd) {
        if ("Jack".equals(user) && "123456".equals(pwd)){
            isAdmin = true;
        }
        executor = new CommandExcutorImpl();
    }

    @Override
    public void runCommand(String cmd) throws Exception {
        if (isAdmin) {
            executor.runCommand(cmd);
        }else{
            if (cmd.trim().startsWith("rm")) {
                throw new Exception("rm command is not allowed for non-admin users.");
            }else{
                executor.runCommand(cmd);
            }
        }
    }
}
```

test proxy

```java
class ProxyPatternTest{
    public static void main(String[] args) {
        CommandExecutor executor = new CommandExecutorProxy("Jack", "12d3456");
        try{
            executor.runCommand("ls -ltr");
            executor.runCommand("rm -rf abc.pdf");
        } catch (Exception e){
            System.out.println(e.getMessage());
        }

    }
}
// 'ls -ltr' command executed.
// rm command is not allowed for non-admin users.
```

## what are different types of proxies

Proxies are generally divided into four types –

1. **Remote proxy** – represent a remotely lactated object. To talk with remote objects, the client need to do additional work on communication over network. A proxy object does this communication on behalf of original object and client focuses on real talk to do.
2. **Virtual proxy** – delay the creation and initialization of expensive objects until needed, where the objects are created on demand. Hibernate created proxy entities are example of virtual proxies.
3. **Protection proxy** – help to implement security over original object. They may check for access rights before method invocations and allow or deny access based on the conclusion.
4. **Smart Proxy** – performs additional housekeeping work when an object is accessed by a client. An example can be to check if the real object is locked before it is accessed to ensure that no other object can change it.

# Decorator Design Pattern

[Decorator Design Pattern in Java](<https://howtodoinjava.com/design-patterns/structural/decorator-design-pattern/>)

[Decorator Design Pattern in Java Example](<https://www.journaldev.com/1540/decorator-design-pattern-in-java-example>)



In software engineering, **decorator design pattern** is used to add additional features or behaviors to a particular instance of a class, while not modifying the other instances of same class at **runtime**. At the same time other instances of the same class will not be affected by this, so individual object gets the modified behavior. Decorators provide a flexible alternative to sub-classing for extending functionality. Please note that the description above implies that decorating an object changes its behavior but not its interface.

This pattern is very important to understand because once you know the techniques of decorating, you’ll be able to give your (or someone else’s) objects new responsibilities without making any code changes to the underlying classes. 

## **Design participants**

A typical diagram of decorator pattern looks like this.

![decorator design pattern participants](https://cdn1.howtodoinjava.com/wp-content/uploads/decorator-design-pattern-participants.png)

Following are the participants of the Decorator Design pattern:

- **Component** – this is the wrapper which can have additional responsibilities associated with it at runtime.
- **Concrete component**– is the original object to which the additional responsibilities are added in program.
- **Decorator**-this is an abstract class which contains a reference to the component object and also implements the component interface.
- **Concrete decorator**-they extend the decorator and builds additional functionality on top of the Component class.

> You have an instance, and you put another instance inside of it. They both support the same (or similar) interfaces. The one on the outside is a “decorator.” You use the one on the outside. It either masks, changes, or pass-troughs the methods of the instance inside of it.

## Car Example

We use [inheritance](https://www.journaldev.com/644/inheritance-java-example) or composition to extend the behavior of an object but this is done at compile time and its applicable to all the instances of the class. We can’t add any new functionality of remove any existing behavior at runtime – this is when Decorator pattern comes into picture.

Suppose we want to implement different kinds of cars – we can create interface Car to define the assemble method and then we can have a Basic car, further more we can extend it to Sports car and Luxury Car. The implementation hierarchy will look like below image.

![decorator pattern, decorator design pattern, decorator pattern java](https://cdn.journaldev.com/wp-content/uploads/2013/07/inheritance-hierarchy-450x318.png)

But if we want to get a car at runtime that has both the features of sports car and luxury car, then the implementation gets complex and if further more we want to specify which features should be added first, it gets even more complex. Now imagine if we have ten different kind of cars, the implementation logic using inheritance and composition will be impossible to manage. To solve this kind of programming situation, we apply decorator pattern in java.



## Implement

### Component Interface

The interface or **abstract class** defining the methods that will be implemented. In our case `Car` will be the component interface.

```java
interface Car{
    public void assemble();
}
```

### Component Implementation

The basic implementation of the component interface. We can have `BasicCar` class as our component implementation.

```java
class BasicCar implements Car{
    @Override
    public void assemble() {
        System.out.println("Basic Car.");
    }
}
```

### Decorator 

Decorator class implements the component interface and it has a HAS-A relationship with the component interface. The component variable should be accessible to the child decorator classes, so we will make this variable protected.

```java
class CarDecorator implements Car{
    protected Car car;

    public CarDecorator(Car car) {
        this.car = car;
    }

    @Override
    public void assemble() {
        this.car.assemble();
    }
}
```

### Concrete Decorators

Extending the base decorator functionality and modifying the component behavior accordingly. We can have concrete decorator classes as `LuxuryCar`and `SportsCar`.

```java
class SportsCar extends CarDecorator{
    public SportsCar(Car car) {
        super(car);
    }

    @Override
    public void assemble() {
        super.assemble();
        System.out.println("Adding feature of sports car.");
    }
}
class LuxuryCar extends CarDecorator{
    public LuxuryCar(Car car) {
        super(car);
    }

    @Override
    public void assemble() {
        super.assemble();
        System.out.println("Adding features of Luxury Car.");
    }
}
```

![decorator design pattern, decorator design pattern in java](https://cdn.journaldev.com/wp-content/uploads/2013/07/decorator-pattern-450x312.png)

### Test

```java
class DecoratorPatternTest{
    public static void main(String[] args) {
        Car sportsCar = new SportsCar(new BasicCar());
        sportsCar.assemble();
        System.out.println("\n");
        Car sportsLuxuryCar = new SportsCar(new LuxuryCar(new BasicCar()));
        sportsLuxuryCar.assemble();
    }
}
// Basic Car.
// Adding feature of sports car.

// Basic Car.
// Adding features of Luxury Car.
// Adding feature of sports car.
```

Notice that client program can create different kinds of Object at runtime and they can specify the order of execution too.



## Important Points

- Decorator design pattern is helpful in providing runtime modification abilities and hence more flexible. Its easy to maintain and extend when the number of choices are more.
- The disadvantage of decorator design pattern is that it uses a lot of similar kind of objects (decorators).
- Decorator pattern is used a lot in [Java IO](https://www.journaldev.com/942/java-io-tutorial) classes, such as [FileReader, BufferedReader](https://www.journaldev.com/867/java-read-text-file) etc.



## **Interview questions on decorator pattern**

**A) How to decide when to use decorator pattern?**

If we drill down more on the concept, we find that Decorator Design Pattern has several requirement indicators to suggest that it is potential solution e.g.

- We have an object that requires the extension e. a window control that requires additional “optional” features like scrollbars, titlebar and statusbar.
- Several objects that support the extension by “decoration”. Usually, those objects share a common interface, traits, or superclass, and sometimes, additional, intermediate super-classes .
- The decorated object (class or prototype instantiation), and the decorator objects have one or several common features. In order to ensure that functionality, the decorated object & the decorators have a common interface, traits, or class inheritance.

**B) Difference between decorator pattern and Adapter pattern**

No. AdapterPattern is used to convert the interface of an object into something else. DecoratorPattern is used to extend the functionality of an object while maintaining its interface. Both of these are probably sometimes called Wrapper Pattern since both of them do “wrap” an object.

**C) Difference between a DecoratorPattern and Subclassing**

The difference between a DecoratorPattern and subclassing is that In subclassing you can decorate any class that implements an interface “with a single class”. Say I wanted to give myself a java.util.Map that printed a message whenever I added or removed a key. If I only ever actually used java.util.HashMap I could just create PrintingMap? as a subclass of HashMap and override put & remove. But if I want to create a printing version of TreeMap then I either create PrintingTreeMap? (which has almost identical code to PrintingMap?



# Observer Design Pattern

[Observer Design Pattern in Java](https://www.journaldev.com/1739/observer-design-pattern-in-java)

[Observer Design Pattern](https://howtodoinjava.com/design-patterns/behavioral/observer-design-pattern/)

## Observer & Subject

**Observer Pattern** is one of the **behavioral design pattern**. Observer design pattern is useful when you are interested in the state of an object and want to get notified whenever there is any change.  In observer pattern, the object that watch on the state of another object are called **Observer** and the object that is being watched is called **Subject**. It is also referred to as the **publish-subscribe pattern**.A observer object can register or unregister from subject at any point of time. It helps is making the objects objects **loosely coupled**.

![Observer Pattern](https://cdn2.howtodoinjava.com/wp-content/uploads/2019/01/observer-pattern.png)



**Subject** contains a list of observers to notify of any change in it’s state, so it should provide methods using which observers can register and unregister themselves. Subject also contain a method to notify all the observers of any change and either it can send the update while notifying the observer or it can provide another method to get the update.

Observer should have a method to set the object to watch and another method that will be used by Subject to notify them of any updates.

Java provides inbuilt platform for implementing Observer pattern through *java.util.Observable*class and *java.util.Observer* interface. However it’s not widely used because the implementation is really simple and most of the times we don’t want to end up extending a class just for implementing Observer pattern as java doesn’t provide multiple inheritance in classes.

Java Message Service (JMS) uses **Observer design pattern** along with [**Mediator pattern**](https://www.journaldev.com/1730/mediator-design-pattern-java) to allow applications to subscribe and publish data to other applications.

Model-View-Controller (MVC) frameworks also use Observer pattern where Model is the Subject and Views are observers that can register to get notified of any change to the model.

The flow is very simple to understand. Application creates the concrete subject object. All concrete observers register themselves to be notified for any further update in the state of subject.

As soon as the state of subject changes, subject notifies all the registered observers and the observers can access the updated state and act accordingly.

![Observer Pattern Sequence Diagram](https://cdn2.howtodoinjava.com/wp-content/uploads/2019/01/observer_pattern_seq.png)



## Real world example of observer pattern

- A real world example of observer pattern can be any social media platform such as Facebook or twitter. When a person updates his status – all his followers gets the notification.

  A follower can follow or unfollow another person at any point of time. Once unfollowed, person will not get the notifications from subject in future.

- In programming, observer pattern is the basis of message oriented applications. When a application has updated it’s state, it notifies the subscribers about updates. Frameworks like [HornetQ](https://howtodoinjava.com/hornetq/basic-jms-messaging-example-using-hornetq-stand-alone-server/), [JMS](https://howtodoinjava.com/jms/jms-java-message-service-tutorial/) work on this pattern.

- Similarly, Java UI based programming, all keyboard and mouse events are handled by it’s listeners objects and designated functions. When user click the mouse, function subscribed to the mouse click event is invoked with all the context data passed to it as method argument.

## Architecture & Design participants

![Observer Pattern Architecture](https://cdn2.howtodoinjava.com/wp-content/uploads/2019/01/observer-pattern-arch.jpg)

The observer pattern has four participants.

- **Subject** – [interface or abstract class](https://howtodoinjava.com/oops/exploring-interfaces-and-abstract-classes-in-java/) defining the operations for attaching and de-attaching observers to the subject.
- **ConcreteSubject** – concrete Subject class. It maintain the state of the object and when a change in the state occurs it notifies the attached Observers.
- **Observer** – interface or abstract class defining the operations to be used to notify this object.
- **ConcreteObserver** – concrete Observer implementations.

## Observer Pattern Example

For our observer pattern java program example, we would implement a simple topic and observers can register to this topic. Whenever any new message will be posted to the topic, all the registers observers will be notified and they can consume the message.

Based on the requirements of Subject, here is the base Subject interface that defines the contract methods to be implemented by any concrete subject.

```java
interface Subject{
    //methods to register and unregister observers
    public void register(Observer obj);
    public void unregister(Observer obj);

    //method to notify observers of change
    public void notifyObservers();

    //method to get updates from subject
    public Object getUpdate(Observer obj);
}
```

Next we will create contract for Observer, there will be a method to attach the Subject to the observer and another method to be used by Subject to notify of any change.

```java
public interface Observer {
    //method to update the observer, used by subject
    public void update();

    //attach with subject to observe
    public void setSuject(Subject sub);
}
```

Now our contract is ready, let’s proceed with the concrete implementation of our topic.

```java
class MyTopic implements Subject{

    private List<Observer> observers;
    private String message;
    private boolean changed;
    private final Object MUTEX = new Object();

    public MyTopic() {
        this.observers = new ArrayList<>();
    }

    @Override
    public void register(Observer obj) {
        if (obj == null) {
            throw new NullPointerException("Null Observer");
        }
        synchronized(MUTEX){
            if(!observers.contains(obj)){
                observers.add(obj);
            }
        }
    }

    @Override
    public void unregister(Observer obj) {
        synchronized (MUTEX){
            observers.remove(obj);
        }
    }

    @Override
    public void notifyObservers() {
        List<Observer> observersLocal = null;
        //synchronization is used to make sure any observer
        // registered after message is received is not notified
        synchronized (MUTEX){
            if (!changed) return;
            observersLocal = new ArrayList<>(this.observers);
            this.changed = false;
        }
        for (Observer observer : observersLocal) {
            observer.update();
        }

    }

    @Override
    public Object getUpdate(Observer obj) {
        return this.message;
    }

    //method to post message to the topic
    public void postMessage(String msg){
        System.out.println("Message Posted to Topic");
        this.message = msg;
        this.changed = true;
        notifyObservers();
    }
}
```

The method implementation to register and unregister an observer is very simple, the extra method is *postMessage()* that will be used by client application to post String message to the topic. Notice the boolean variable to keep track of the change in the state of topic and used in notifying observers. This variable is required so that if there is no update and somebody calls *notifyObservers()* method, it doesn’t send false notifications to the observers.

Also notice the use of [synchronization](https://www.journaldev.com/1061/thread-safety-in-java) in *notifyObservers()* method to make sure the notification is sent only to the observers registered before the message is published to the topic.

Here is the implementation of Observers that will watch over the subject.

```java
class MyTopicSubscriber implements Observer{

    private String name;
    private Subject topic;

    public MyTopicSubscriber(String name) {
        this.name = name;
    }

    @Override
    public void update() {
        String msg = (String) topic.getUpdate(this);
        if (msg == null) {
            System.out.println(name + ":: No new message");
        }else{
            System.out.println(name + ":: Consuming message ::" + msg);
        }
    }

    @Override
    public void setSuject(Subject sub) {
        
    }
}
```

Notice the implementation of *update()* method where it’s calling Subject *getUpdate()* method to get the message to consume. We could have avoided this call by passing message as argument to *update()* method.



## Test

```java
class ObserverPatternTest{
    public static void main(String[] args) {
        //create subject
        MyTopic topic = new MyTopic();

        //create observers
        MyTopicSubscriber obj1 = new MyTopicSubscriber("Obj1");
        MyTopicSubscriber obj2 = new MyTopicSubscriber("Obj2");
        MyTopicSubscriber obj3 = new MyTopicSubscriber("Obj3");

        //register observers to the subject
        topic.register(obj1);
        topic.register(obj2);
        topic.register(obj3);

        //attach observer to subject
        obj1.setSuject(topic);
        obj2.setSuject(topic);
        obj3.setSuject(topic);

        //check if any update is available
        obj1.update();

        //now send message to subject
        topic.postMessage("New Message");
    }
}

// Obj1:: No new message
// Message Posted to Topic
// Obj1:: Consuming message ::New Message
// Obj2:: Consuming message ::New Message
// Obj3:: Consuming message ::New Message
```

## Another Example

### Message & Observer & Subject

```java
class Message{
    final String messageContent;
    public Message(String m){
        this.messageContent = m;
    }
    public String getMessageContent(){
        return messageContent;
    }
}
interface Observer{
    public void update(Message m);
}
interface Suject{
    public void attach(Observer observer);
    public void detach(Observer observer);
    public void notifyUpdate(Message message);
}
```

### MessagePublisher

```java
class MessagePublisher implements Suject{

    private List<Observer> observers = new ArrayList<>();
    @Override
    public void attach(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void detach(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyUpdate(Message message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }
}
```

### MessageSubscriber

```java
class MessageSubscriber implements Observer{

    private String name;

    public MessageSubscriber(String name) {
        this.name = name;
    }

    @Override
    public void update(Message m) {
        System.out.println(this.name + " : " + m.getMessageContent());
    }
}
```

### Test

```java
class ObserverPatternTest{
    public static void main(String[] args) {
        MessageSubscriber sub1 = new MessageSubscriber("subscriber1");
        MessageSubscriber sub2 = new MessageSubscriber("subscriber2");
        MessageSubscriber sub3 = new MessageSubscriber("subscriber3");

        MessagePublisher publisher = new MessagePublisher();
        publisher.attach(sub1);
        publisher.attach(sub2);

        //s1 and s2 will receive the update
        publisher.notifyUpdate(new Message("First Message"));

        publisher.detach(sub1);
        publisher.attach(sub3);

        //s2 and s3 will receive the update
        publisher.notifyUpdate(new Message("Second Message"));
    }
}
// subscriber1 : First Message
// subscriber2 : First Message
// subscriber2 : Second Message
// subscriber3 : Second Message
```



## FAQs

- Can different types of observers register to one subject?

  The nature and functionality of observers can be different but they all must implement the one common `Observer` interface which the subject support for registering and deregistering.

- Can I add or remove observers at runtime?

  Yes. We can add or remove the observers at any point of time.

- Difference between observer pattern and chain of responsibility pattern?

  In an observer pattern, all registered handler objects get notifications at the same time and they process the update at same time.

  But in a chain of responsibility pattern, handler objects in the chain are notified one by one, and this process continues until one object fully handles the notification.

- Benefits of the observer pattern?

  The subject and observers make a loosely coupled system. They do not need to know each other explicitly. We can independently add or remove observers at any time.