- [Top 75 Java Interview Questions You Must Prepare In 2019](<https://www.edureka.co/blog/interview-questions/java-interview-questions/>)



# Java Base

## Common Questions

### Explain public static void main(String args[]).

- **public** : Public is an access modifier, which is used to specify who can access this method. Public means that this Method will be accessible by any Class.

- **static** : It is a keyword in java which identifies it is class based i.e it can be accessed without creating the instance of a Class.
- **void** : It is the return type of the method. Void defines the method which will not return any value.
- **main**: It is the name of the method which is searched by JVM as a starting point for an application with a particular signature only. It is the method where the main execution occurs.
- **String args[]** : It is the parameter passed to the main method.

### What are wrapper classes?

Wrapper classes converts the java primitives into the reference types (objects). Every primitive data type has a class dedicated to it. These are known as wrapper classes because they “wrap” the primitive data type into an object of that class. Refer to the below image which displays different primitive type, wrapper class and constructor argument. 

| Primitive | Wrapper Class | Constructor Argument    |
| --------- | ------------- | ----------------------- |
| boolean   | Boolean       | boolean or String       |
| byte      | Byte          | byte or String          |
| char      | Character     | char                    |
| int       | Integer       | int or String           |
| float     | Float         | float, double or String |
| double    | Double        | double or String        |
| long      | Long          | long or String          |
| short     | Short         | short or String         |

### **What is the difference between equals() and == ?**

- `.equals(...)` will only compare what it is written to compare, no more, no less.
- If a class does not override the equals method, then it defaults to the `equals(Object o)`method of the closest parent class that has overridden this method.
- If no parent classes have provided an override, then it defaults to the method from the ultimate parent class, Object, and so you're left with the `Object equals(Object o)` method. Per the Object API this is the same as `==`; that is, it returns true *if and only if* both variables refer to the same object, if their references are one and the same. Thus you will be testing for **object equality** and not **functional equality**.
- Always remember to override `hashCode` if you override `equals` so as not to "break the contract". As per the API, the result returned from the `hashCode()` method for two objects **must**be the same if their `equals` methods show that they are equivalent. The converse is *not*necessarily true.

### **What are the differences between Heap and Stack Memory?**

The major difference between Heap and Stack memory are:

|     **Features**      |                          **Stack**                           |                           **Heap**                           |
| :-------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|      **Memory**       |    Stack memory is used only by one thread of execution.     |   Heap memory is used by all the parts of the application.   |
|      **Access**       |       Stack memory can’t be accessed by other threads.       |     Objects stored in the heap are globally accessible.      |
| **Memory Management** |             Follows LIFO manner to free memory.              | Memory management is based on generation associated to each object. |
|     **Lifetime**      |       Exists until the end of execution of the thread.       | Heap memory lives from the start till the end of application execution. |
|       **Usage**       | Stack memory only contains local primitive and reference variables to objects in heap space. | Whenever an object is created, it’s always stored in the Heap space. |

### What is Polymorphism?

![img](https://www.edureka.co/blog/wp-content/uploads/2017/04/Polymorphism-483x300.png)

Polymorphism is briefly described as “one interface, many implementations”. Polymorphism is a characteristic of being able to assign a different meaning or usage to something in different contexts – specifically, to allow an entity such as a variable, a function, or an object to have more than one form. There are two types of polymorphism:

1. Compile time polymorphism
2. Run time polymorphism

Compile time polymorphism is method overloading whereas Runtime time polymorphism is done using inheritance and interface.

### **Can you override a private or static method in Java?**

You cannot override a private or static method in Java. If you create a similar method with same return type and same method arguments in child class then it will hide the super class method; this is known as method hiding. Similarly, you cannot override a private method in sub class because it’s not accessible there. What you can do is create another private method with the same name in the child class. Let’s take a look at the example below to understand it better.

### **What is difference between Error and Exception?**

An error is an irrecoverable condition occurring at runtime. Such as OutOfMemory error. These JVM errors you can not repair them at runtime.Though error can be caught in catch block but the execution of application will come to a halt and is not recoverable.

While exceptions are conditions that occur because of bad input or human error etc. e.g. FileNotFoundException will be thrown if the specified file does not exist. Or a NullPointerException will take place if you try using a null reference. In most of the cases it is possible to recover from an exception (probably by giving user a feedback for entering proper values etc.

### **How can you handle Java exceptions?**

There are five keywords used to handle exceptions in java: 

1. try
2. catch
3. finally
4. throw
5. throws





## Class or Object

### What are constructors in Java?

In Java, constructor refers to a block of code which is used to initialize an object. It must have the same name as that of the class. Also, it has no return type and it is automatically called when an object is created.

There are two types of constructors:

1. Default constructor
2. Parameterized constructor

### **What is singleton class and how can we make a class singleton?**

- [Java Singleton Design Pattern Best Practices with Examples](<https://www.journaldev.com/1377/java-singleton-design-pattern-best-practices-examples>)

Singleton class is a class whose only one instance can be created at any given time, in one JVM. A class can be made singleton by making its constructor private.

There is serveral methods to ceate singleton object, as follow:

```java
// Eager initialization
public class Singleton{
    private static final Singleton instance = new Singleton();
    private Singleton(){}
    public static Singleton getInstance(){
        return instance;
    }
}

// Lazy Initialization
public class Singleton{
    private static Singleton instance;
    private Singleton(){}
    public static Singleton getInstance(){
        if(instance == null){
            instance = new Singleton();
        }
        return instance;
    }
}

// Thread Safe Singleton
public class Singleton{
    private static Singleton instance;
    private Singleton(){}
    public static synchronized Singleton getInstance(){
        if(instance == null){
            instance = new Singleton();
        }
        return instance;
    }
}

//inner static helper class
public class Singleton{
    private Singleton(){}
    private static class SingletonHelper{
        private static final Singleton INSTANCE = new Singleton();
    }
    public static Singleton getInstance(){
        return SingletonHelper.INSTANCE;
    }
}
```



### How to create a immutable object in Java? Count all benefits?

An immutable class is one whose state can not be changed once created. Here, state of object essentially means the values stored in instance variable in class whether they are primitive types or reference types.

To make a class immutable, below steps needs to be followed:

1. Don’t provide “setter” methods or methods that modify fields or objects referred to by fields. Setter methods are meant to change the state of object and this is what we want to prevent here.
2. Make all fields `final` and `private`. Fields declared `private` will not be accessible outside the class and making them `final` will ensure the even accidentally you can not change them.
3. Don’t allow subclasses to override methods. The simplest way to do this is to declare the class as `final`. Final classes in java can not be overridden.
4. Always remember that your instance variables will be either mutable or immutable. Identify them and return new objects with copied content for all mutable objects (object references). Immutable variables (primitive types) can be returned safely without extra effort.

Also, you should memorize following benefits of immutable class. You might need them during interview. Immutable classes –

- are simple to construct, test, and use
- are automatically thread-safe and have no synchronization issues
- do not need a copy constructor
- do not need an implementation of clone
- allow hashCode to use lazy initialization, and to cache its return value
- do not need to be copied defensively when used as a field
- make good `Map` keys and `Set` elements (these objects must not change state while in the collection)
- have their class invariant established once upon construction, and it never needs to be checked again
- always have “**failure atomicity**” (a term used by Joshua Bloch) : if an immutable object throws an exception, it’s never left in an undesirable or indeterminate state.

Apart from your written classes, JDK itself has lots of immutable classes. Given is such a list of immutable classes in Java.

1. String
2. Wrapper classes such as Integer, Long, Double etc.
3. Immutable collection classes such as Collections.singletonMap() etc.
4. java.lang.StackTraceElement
5. Java enums (ideally they should be)
6. java.util.Locale
7. java.util.UUID

### Is Java Pass by Reference or Pass by Value?

The Java Spec says that ***everything in Java is pass-by-value***. There is no such thing as “*pass-by-reference*” in Java. These terms are associated with method calling and passing variables as method parameters. Well, primitive types are always pass by value without any confusion. But, the concept should be understood in context of method parameter of complex types.

In java, when we pass a reference of complex types as any method parameters, always the memory address is copied to new reference variable bit by bit. See in below picture:

![pass-by-value](https://howtodoinjava.files.wordpress.com/2013/03/pass-by-value.jpg)

In above example, address bits of first instance are copied to another reference variable, thus resulting both references to point a single memory location where actual object is stored. Remember, making another reference to null will not make first reference also null. But, changing state from either reference variable have impact seen in other reference also.

> Read in detail here: [Java Pass by Value or Reference?](<https://howtodoinjava.com/java/basics/java-is-pass-by-value-lets-see-how/>)

### 2. What is the use of the finally block? Is finally block in Java guaranteed to be called? When finally block is NOT called?

The `finally` block always executes when the `try` block exits. This ensures that the `finally` block is executed even if an unexpected exception occurs. But `finally` is useful for more than just exception handling — it allows having cleanup code accidentally bypassed by a `return`, `continue`, or `break`. Putting cleanup code in a `finally` block is always a good practice, even when no exceptions are anticipated.

If the JVM exits while the `try` or `catch` code is being executed, then the `finally` block may not execute. Likewise, if the thread executing the `try` or `catch` code is interrupted or killed, the `finally` block may not execute even though the application as a whole continues.

### 3. Why there are two Date classes; one in java.util package and another in java.sql?

A `java.util.Date` represents date and time of day, a `java.sql.Date` only represents a date. The complement of `java.sql.Date` is `java.sql.Time`, which only represents a time of day.
The `java.sql.Date` is a subclass (an extension) of `java.util.Date`. So, what changed in `java.sql.Date`:

– `toString()` generates a different string representation: **yyyy-mm-dd**
– a `static valueOf(String)` methods to create a date from a string with above representation
– the getters and setter for hours, minutes and seconds are deprecated

The `java.sql.Date` class is used with JDBC and it was intended to not have a time part, that is, hours, minutes, seconds, and milliseconds should be zero… but this is not enforced by the class.

![image](https://wx2.sinaimg.cn/large/69d4185bly1g26mhxqqbyj20at09it9c.jpg)

### 4. Explain marker interfaces?

The marker interface pattern is a design pattern in computer science, used with languages that **provide run-time type information about objects**. It provides **a means to associate metadata with a class where the language does not have explicit support for such metadata.** In java, it is used as interfaces with no method specified.

A good example of use of marker interface in java is [Serializable](https://howtodoinjava.com/java/serialization/a-mini-guide-for-implementing-serializable-interface-in-java/) interface. A class implements this interface to indicate that its non-transient data members can be written to a byte steam or file system.

A *major problem* with marker interfaces is that an interface defines a contract for implementing classes, and that contract is inherited by all subclasses. This means that **you cannot “un-implement” a marker**. In the example given, if you create a subclass that you do not want to serialize (perhaps because it depends on transient state), you must resort to explicitly throwing NotSerializableException.



### 5. Why main() in java is declared as public static void?

**Why public?** main method is `public` so that it can be accessible everywhere and to every object which may desire to use it for launching the application. Here, i am not saying that JDK/JRE had similar reasons because java.exe or javaw.exe (for windows) use Java Native Interface (JNI) calls to invoke method, so they can have invoked it either way irrespective of any access modifier.

**Why static?** Lets suppose we do not have main method as `static`. Now, to invoke any method you need an instance of it. Right? Java can have overloaded constructors, we all know. Now, which one should be used and from where the parameters for overloaded constructors will come.

**Why void?** Then there is no use of returning any value to JVM, who actually invokes this method. The only thing application would like to communicate to invoking process is: normal or abnormal termination. This is already possible using `System.exit(int)`. A non-zero value means abnormal termination otherwise everything was fine.

### 6. What is the difference between creating String as new() and literal?

When we create `String` with `new()` it’s created in heap and also added into string pool, while `String` created using literal are created in String pool only which exists in Perm area of heap.

Well you really need to know the concept of string pool very deeply to answer this question or similar questions. My advise.. “Study Hard” about [string class and string pool](https://howtodoinjava.com/java/string/interview-stuff-about-string-class-in-java/).



### 8. When do you override hashCode() and equals()?

`hashCode()` and `equals()` methods have been defined in `Object` class which is parent class for java objects. For this reason, all java objects inherit a default implementation of these methods.

`hashCode()` method is used to get a unique integer for given object. This integer is used for determining the bucket location, when this object needs to be stored in some `HashTable` like data structure. By default, Object’s `hashCode()` method returns and integer representation of memory address where object is stored.
`equals()` method, as name suggest, is used to simply verify the equality of two objects. Default implementation simply check the object references of two objects to verify their equality.

Note that it is generally necessary to override the hashCode method whenever this method is overridden, so as to maintain the general contract for the `hashCode()` method, which states that equal objects must have equal hash codes.

- `equals()` must define an equality relation (it must be **reflexive, symmetric and transitive**). In addition, it must be consistent (if the objects are not modified, then it must keep returning the same value). Furthermore, `o.equals(null)` must always return `false`.
- `hashCode()` must also be consistent (if the object is not modified in terms of `equals()`, it must keep returning the same value).

The relation between the two methods is:

Whenever `a.equals(b)` then `a.hashCode()` must be same as `b.hashCode()`.

### 9. Why finalize() method should be avoided?

We all know the basic statement that `finalize()` method is called by garbage collector thread before reclaiming the memory allocated to the object. See [this program](https://howtodoinjava.com/java/related-concepts/why-not-to-use-finalize-method-in-java/) which prove that `finalize()` invocation is not guaranteed at all. Other reasons can be:

1. `finalize()` methods do not work in chaining like constructors. It means like when you call a constructor then constructors of all super classes will be invokes implicitly. But, in case of finalize methods, this is not followed. Super class’s finalize() should be called explicitly.

2. Any Exception thrown by finalize method is ignored by GC thread and it will not be propagated further, in fact it will not be logged in your log files. So bad, isn’t it?

3. Also, There is some performance penalty when finalize() in included in your class. In Effective java (2nd edition ) Joshua bloch says,

   “Oh, and one more thing: there is a severe performance penalty for using finalizers. On my machine, the time to create and destroy a simple object is about 5.6 ns. Adding a finalizer increases the time to 2,400 ns. In other words, it is about 430 times slower to create and destroy objects with finalizers.”

   

### 10. Explain abstraction and encapsulation? How are they related?

#### Abstraction

> [Abstraction](https://howtodoinjava.com/object-oriented/understanding-abstraction-in-java/) captures only those details about an object that are relevant to the current perspective.

In [object-oriented programming](https://howtodoinjava.com/object-oriented/object-oriented-principles/) theory, abstraction involves the facility to define objects that represent abstract “actors” that can perform work, report on and change their state, and “communicate” with other objects in the system.

Abstraction in any programming language works in many ways. It can be seen from creating subroutines to defining interfaces for making low level language calls. Some abstractions try to limit the breadth of concepts a programmer needs, by completely hiding the abstractions they in turn are built on, e.g. design patterns.

Typically abstraction can be seen in two ways:

**Data abstraction** is the way to create complex data types and exposing only meaningful operations to interact with data type, where as hiding all the implementation details from outside works.

**Control abstraction** is the process of identifying all such statements and expose them as a unit of work. We normally use this feature when we create a function to perform any work.

#### Encapsulation

> Wrapping data and methods within classes in combination with implementation hiding (through access control) is often called encapsulation. The result is a data type with characteristics and behaviors. Encapsulation essentially has both i.e. information hiding and implementation hiding.
>

“***Whatever changes, encapsulate it***“. It has been quoted as a famous design principle. For that matter in any class, changes can happen in data in runtime and changes in implementation can happen in future releases. So, encapsulation applies to both i.e. data as well as implementation.

SO, they can relate like following :

– Abstraction is more about ‘What‘ a class can do. [Idea]
– Encapsulation is more about ‘How‘ to achieve that functionality. [Implementation]

### 11. Explain transient and volatile keywords in java?

- [关于 Java volatile 关键字](<https://juejin.im/entry/59b0050ff265da249a203a9d>)
- [Java Volatile Keyword](<http://tutorials.jenkov.com/java-concurrency/volatile.html>)

#### Transient

“*The **transient** keyword in Java is used to indicate that a field should not be serialized.*” According to language specification: Variables may be marked `transient` to indicate that they are not part of the persistent state of an object. For example, you may have fields that are derived from other fields, and should only be done so programmatically, rather than having the state be persisted via serialization.

For example, in class `BankPayment.java` fields like `principal` and `rate` can be serialized while `interest` can be calculated any time even after de-serialization.

If we recall, each thread in java has its own local memory space as well and it does all read/write operations in its local memory. Once all operations are done, it write back the modified state of variable in main memory from where all threads access this variable. Normally, this is the default flow inside JVM. But, the `volatile` modifier tells the JVM that a thread accessing the variable must always reconcile its own private copy of the variable with the master copy in memory. It means every time thread want to read the state of variable, it must flush its local memory state and update the variable from main memory.

#### Volatile

`volatile` is most useful in lock-free algorithms. You mark the variable holding shared data as volatile when you are not using locking to access that variable and you want changes made by one thread to be visible in another, or you want to create a “happens-after” relation to ensure that computation is not re-ordered, again, to ensure changes become visible at the appropriate time.

The `volatile` should be used to safely publish immutable objects in a multi-threaded Environment. Declaring a field like public `volatile` ImmutableObject foo secures that all threads always see the currently available instance reference.



### 12. Deep copy and shallow copy?

A clone is an exact copy of the original. In java, it essentially means the ability to create an object with similar state as the original object. The clone() method provides this functionality.

Shallow copies duplicate as little as possible.  By default, java cloning is shallow copy or ‘field by field copy’ i.e. as the Object class does not have idea about the structure of class on which clone() method will be invoked. So, JVM when called for cloning, do following things:

1) If the class has only primitive data type members then a completely new copy of the object will be created and the reference to the new object copy will be returned.

2) If the class contains members of any class type then only the object references to those members are copied and hence the member references in both the original object as well as the cloned object refer to the same object.

Deep copies duplicate everything. A deep copy of a collection is two collections with all of the elements in the original collection duplicated. Here, we want a clone which is independent of original and making changes in clone should not affect original.

*Deep cloning requires satisfaction of following rules.*

1. No need to separately copy primitives.
2. All the member classes in original class should support cloning and in clone method of original class in context should call super.clone() on all member classes.
3. If any member class does not support cloning then in clone method, one must create a new instance of that member class and copy all its attributes one by one to new member class object. This new member class object will be set in cloned object.

[Read more about cloning here](https://howtodoinjava.com/java/cloning/a-guide-to-object-cloning-in-java/).

### 13. Can you assign null to this reference variable?

NO. You can’t. In java, left hand side of an assignment statement must be a variable. ‘this’ is a special keyword which represent the current instance always. This is not any variable.

Similarly, null can not be assigned to ‘super’ or any such keyword for that matter.



### 14. What if the difference between && and &??

& is bitwise and && is logical.

- & evaluates both sides of the operation.
- && evaluates the left side of the operation, if it’s true, it continues and evaluates the right side.



### 15. How to override equals and hashCode() methods?

hashCode() and equals() methods have been defined in Object class which is parent class for java objects. For this reason, all java objects inherit a default implementation of these methods.

hashCode() method is used to get a unique integer for given object. This integer is used for determining the bucket location, when this object needs to be stored in some HashTable like data structure. By default, Object’s hashCode() method returns and integer representation of memory address where object is stored.

equals() method, as name suggest, is used to simply verify the equality of two objects.  Default implementation simply check the object references of two objects to verify their equality.

Below are the important points to keep remember while overriding these functions.

1. Always use same attributes of an object to generate hashCode() and equals() both. As in our case, we have used employee id.
2. equals() must be consistent (if the objects are not modified, then it must keep returning the same value).
3. Whenever a.equals(b), then a.hashCode() must be same as b.hashCode().
4. If you override one, then you should override the other.

[Read more interesting facts and how to guide here.](https://howtodoinjava.com/java/related-concepts/working-with-hashcode-and-equals-methods-in-java/)

### 16. Explain all access modifiers?

Java classes, fields, constructors and methods can have one of four different access modifiers:

***private***

If a method or variable is marked as private, then only code inside the same class can access the variable, or call the method. Code inside subclasses cannot access the variable or method, nor can code from any external class.
If a class is marked as private then no external class an access the class. This doesn’t really make so much sense for classes though. Therefore, the access modifier private is mostly used for fields, constructors and methods.

***default***

The default access level is declared by not writing any access modifier at all. Default access levels means that code inside the class itself + code inside classes in the same package as this class, can access the class, field, constructor or method. Therefore, the default access modifier is also sometimes called a package access modifier.

Subclasses cannot access methods and member variables in the superclass, if they have default accessibility declared, unless the subclass is located in the same package as the superclass.

***protected***

The protected acces modifier does the same as the default access, except subclasses can also access protected methods and member variables of the superclass. This is true even if the subclass is not located in the same package as the superclass.

***public***

The public access modifier means that all code can access the class, field, constructor or method, regardless of where the accessing code is located.

| **Modifiers** | *Same Class* | *Same Package* | *Subclass* | *Other packages* |
| ------------- | ------------ | -------------- | ---------- | :--------------- |
| public        | Y            | Y              | Y          | Y                |
| protected     | Y            | Y              | Y          | N                |
| default       | Y            | Y              | N          | N                |
| private       | Y            | N              | N          | N                |

### 17. What is garbage collection? Can we enforce it?

Garbage collection is an automatic memory management feature in many modern programming languages, such as Java and languages in the .NET framework. Languages that use garbage collection are often interpreted or run within a virtual machine like the JVM. In each case, the environment that runs the code is also responsible for garbage collection. A GC has two goals: any unused memory should be freed, and no memory should be freed unless the program will not use it anymore.

Can you force garbage collection?? Nope, System.gc() is as close as you can get. Your best option is to call System.gc() which simply is a hint to the garbage collector that you want it to do a collection. There is no way to force and immediate collection though as the garbage collector is non-deterministic. Also, under the documentation for OutOfMemoryError it declares that it will not be thrown unless the VM has failed to reclaim memory following a full garbage collection. So if you keep allocating memory until you get the error, you will have already forced a full garbage collection.

[Read more about garbage collection here.](https://howtodoinjava.com/java/garbage-collection/revisiting-memory-management-and-garbage-collection-mechanisms-in-java/)

### 18. What is native keyword? Explain in detail?

The native keyword is applied to a method to indicate that the method is implemented in native code using JNI. It marks a method, that it will be implemented in other languages, not in Java.

Native methods were used in the past to write performance critical sections but with Java getting faster this is now less common. Native methods are currently needed when

- You need to call a library from Java that is written in other language.
- You need to access system or hardware resources that are only reachable from the other language (typically C). Actually, many system functions that interact with real computer (disk and network IO, for instance) can only do this because they call native code.

The downsides of using native code libraries are also significant:

1. JNI / JNA have a tendency to destabilize the JVM, especially if you try to do something complicated. If your native code gets native code memory management wrong, there’s a chance that you will crash the JVM. If your native code is non-reentrant and gets called from more than one Java thread, bad things will happen … sporadically. And so on.
2. Java with native code is harder to debug than pure Java or pure C/C++.
3. Native code can introduce significant platform dependencies / issues for an otherwise platform independent Java app.
4. Native code requires a separate build framework, and that may have platform / portability issues as well.

### 19. What is serialization? Explain the catches?

In computer science, in the context of data storage and transmission, serialization is the process of translating data structures or object state into a format that can be stored  and “resurrected” later in the same or another computer environment.  When the resulting series of bits is reread according to the serialization format, it can be used to create a semantically identical clone of the original object.

Java provides automatic serialization which requires that the object be marked by implementing the java.io.Serializable interface. Implementing the interface marks the class as “okay to serialize,” and Java then handles serialization internally. There are no serialization methods defined on the Serializable interface, but a serializable class can optionally define methods with certain special names and signatures that if defined, will be called as part of the serialization/deserialization process.

Once an object is serialized, changes in its class break the de-serialization process. To identify the future changes in your class which will be compatible and others which will prove incompatible, please read the **full guide here**. In short, I am listing down here:

**Incompatible changes**

- Deleting fields
- Moving classes up or down the hierarchy
- Changing a non-static field to static or a non-transient field to transient
- Changing the declared type of a primitive field
- Changing the writeObject or readObject method so that it no longer writes or reads the default field data
- Changing a class from Serializable to Externalizable or vice-versa
- Changing a class from a non-enum type to an enum type or vice versa
- Removing either Serializable or Externalizable
- Adding the writeReplace or readResolve method to a class

**Compatible changes**

- Adding fields
- Adding/ Removing classes
- Adding writeObject/readObject methods [defaultReadObject or defaultWriteObject should be called first]
- Removing writeObject/readObject methods
- Adding java.io.Serializable
- Changing the access to a field
- Changing a field from static to non-static or transient to non transient

## Difference

### What is the difference between abstract classes and interfaces?

This is very common question if you are appearing interview for junior level programmer. Well, most noticeable differences are as below:

- Variables declared in a Java interface is by default `final`. An abstract class may contain non-final variables.
- Java interface are implicitly `abstract` and cannot have implementations. A Java abstract class can have instance methods that implements a default behavior.
- Members of a Java interface are public by default. A Java abstract class can have the usual flavors of class members like `private`, `abstract`.
- Java interface should be implemented using keyword “**implements**“; A Java abstract class should be extended using keyword “**extends**“.
- A Java class can implement multiple interfaces but it can extend only one abstract class.
- Interface is ~~absolutely abstract and~~ cannot be instantiated; A Java abstract class also cannot be instantiated, but can be invoked if a main() exists. Since Java 8, you can define [**default methods in interfaces**](https://howtodoinjava.com/java8/default-methods-in-java-8/).
- Abstract class are slightly faster than interface because interface involves a search before calling any overridden method in Java. This is not a significant difference in most of cases but if you are writing a time critical application than you may not want to leave any stone unturned.

| **Abstract Class**                                           | **Interfaces**                                               |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| An abstract class can provide complete, default code and/or just the details that have to be overridden. | An interface cannot provide any code at all,just the signature. |
| In case of abstract class, a class may extend only one abstract class. | A Class may implement several interfaces.                    |
| An abstract class can have non-abstract methods.             | All methods of an Interface are abstract.                    |
| An abstract class can have instance variables.               | An Interface cannot have instance variables                  |
| An abstract class can have any visibility: public, private, protected. | An Interface visibility must be public (or) none.            |
| If we add a new method to an abstract class then we have the option of providing default implementation and therefore all the existing code might work properly | If we add a new method to an Interface then we have to track down all the implementations of the interface and define implementation for the new method |
| An abstract class can contain constructors                   | An Interface cannot contain constructors                     |
| Abstract classes are fast                                    | Interfaces are slow as it requires extra indirection to find corresponding method in the actual class |

### What is the difference between overloading and overriding?

**Method Overloading :**

- In Method Overloading, Methods of the same class shares the same name but each method must have different number of parameters or parameters having different types and order.
- Method Overloading is to “add” or “extend” more to method’s behavior.
- It is a compile time polymorphism.
- The methods must have different signature.
- It may or may not need inheritance in Method Overloading.

**Method Overriding:**  

- In Method Overriding, sub class have the same method with same name and exactly the same number and type of parameters and same return type as a super class.
- Method Overriding is to “Change” existing behavior of method.
- It is a run time polymorphism.
- The methods must have same signature.
- It always requires inheritance in Method Overriding.

### **What are the differences between Checked Exception and Unchecked Exception?**

#### **Checked Exception**

- The classes that extend Throwable class except RuntimeException and Error are known as checked exceptions. 
- Checked exceptions are checked at compile-time.
- Example: IOException, SQLException etc.

#### **Unchecked Exception**

- The classes that extend RuntimeException are known as unchecked exceptions. 
- Unchecked exceptions are not checked at compile-time.
- Example: ArithmeticException, NullPointerException etc.





# Data Structure

## Common Questions

### What is the Java Collection framework? List down its advantages?

By definition, a collection is **an object that represents a group of objects**. Like in set theory, a set is group of elements. 

Its most noticeable **benefits of java collections** can be listed as:

- Reduced programming effort due to ready to use code
- Increased performance because of high-performance implementations of data structures and algorithms
- Provides interoperability between unrelated APIs by establishing a common language to pass collections back and forth
- Easy to learn APIs by learning only some top level interfaces and supported operations

### Why Collection interface does not extend Cloneable and Serializable interface?

Well, simplest answer is “**there is no need to do it**“. Extending an interface simply means that you are creating a subtype of interface, in other words a more specialized behavior and Collection interface is not expected to do what Cloneable and Serializable interfaces do.

Another reason is that not everybody will have a reason to have Cloneable collection because if it has very large data, then every **unnecessary clone operation will consume a big memory**. Beginners might use it without knowing the consequences.

Another reason is that **Cloneable and Serializable are very specialized behavior** and so should be implemented only when required. For example, many concrete classes in collection implement these interfaces. So if you want this feature. use these collection classes otherwise use their alternative classes.

### How to make a collection read only?

Use following methods:

- Collections.unmodifiableList(list);
- Collections.unmodifiableSet(set);
- Collections.unmodifiableMap(map);

These methods takes collection parameter and return a new read-only collection with same elements as in original collection.

### How to make a collection thread safe?

Use below methods:

- Collections.synchronizedList(list);
- Collections.synchronizedSet(set);
- Collections.synchronizedMap(map);

Above methods take collection as parameter and return same type of collection which are synchronized and thread safe.

### What do you understand by iterator fail-fast property?

**Fail-fast Iterators fail as soon as they realized that structure of Collection has been changed since iteration has begun**. Structural changes means adding, removing or updating any element from collection while one thread is Iterating over that collection.

Fail-fast behavior is implemented by keeping a modification count and if iteration thread realizes the change in modification count it throws ConcurrentModificationException.

### How to avoid ConcurrentModificationException while iterating a collection?

You should first try to **find another alternative iterator which are fail-safe**. For example if you are using List and you can use ListIterator. If it is legacy collection, you can use enumeration.

If above options are not possible then you can use one of three changes:

- If you are using JDK1.5 or higher then you can use ConcurrentHashMap and CopyOnWriteArrayList classes. It is the recommended approach.
- You can convert the list to an array and then iterate on the array.
- You can lock the list while iterating by putting it in a synchronized block.

### Which collection classes provide random access of it’s elements?

ArrayList, HashMap, TreeMap, Hashtable classes provide random access to it’s elements.



### What are Collections and Arrays classes?

**Collections and Arrays classes are special utility classes to support collection framework core classes.** They provide utility functions to get read-only/ synchronized collections, sort the collection on various ways etc.

Arrays also helps array of objects to convert in collection objects. Arrays also have some functions which helps in copying or working in part of array objects.



## String Questions

### 1. How to compare two Strings in java program?

Always use `equals()` method to verify string equality. Never use `"=="` operator. Double equal operator always check the object references in memory. `equals()` method checks the String content.

```java
String blogName = "HowToDoInJava.com";       
String anotherString = new String("HowToDoInJava.com");
System.out.println(blogName == anotherString);     //false
System.out.println(blogName.equals(anotherString));//true
```

###  2. Why Char array is preferred over String for storing password?

We know that strings are stored in the constant pool in Java. Once a string is created in the string pool, it stays in the pool until unless garbage collected. By this time, any malicious program can access the memory location in the physical memory location and access the string as well.

If we store the password as a string, then it will also be stored in spring pool and will be available in memory for the longer duration than required, because garbage collection cycles are unpredictable. This makes sensitive password strings **vulnerable to hacking and data theft**.

Can we make String blank after using it? No, we cannot. We know that once a String is created, we cannot manipulate it e.g. you cannot change its content. Strings are final and immutable.

But char arrays are mutable, their content can be overwritten after use it. So your application shall use char[] to store password text, and after using the password, replace array content with a blank.

```java
String password = "123456";     //Do not use it
         
char[] passwordChars = new char[4];      //Get password from some system such as database
 
//use password
 
for(char c : passwordChars) {
    c = ' ';
}
```

### 3. Why String is popular HashMap key in Java?

In Java, A key which has be to used in `Map` – shall be immutable and should honor the contract between `equals()` and `hashCode()` method. `String` class satisfies both conditions.

Also, String class provides many useful methods to compare, sort, tokenize or lower-upper cases. These methods can be used while performing CRUD operations on `Map`. It makes it a very useful class to use in `Map` rather than creating your own class.

### 4. Difference between String, StringBuffer and StringBuilder?

- `String` class represents a sequence of characters and provides useful methods to work with characters. String class instances are immutable. So each time you perform string concatenation using string class, a new object will be created with the concatenated string.

- `StringBuilder` class is used to perform string concatenation operations in more memory efficient way. It internally maintains a `char array` and manipulate the content in this array only.

  When you need to get the complete concatenated string after performing all operations, it creates a new String with character array content.

- `StringBuffer` is very much same as `StringBuilder` class. Only difference is that it is thread-safe. It’s all methods are `synchronized`.

### 5. How many objects will be created with string initialization code?

```java
String s1 = "howtodoinjava.com";
String s2 = "howtodoinjava.com";
String s3 = new String("howtodoinjava.com");
```

1. Above code will create **2 objects**.
2. First object will be created in string pool by first statement.
3. Second statement will not create any new object, and `s2` will refer to same string constant as `s1`.
4. Third statement will create a new string object in heap memory.

### 6. How StringBuffer save the memory?

A [String](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) is implemented as an [immutable object](https://howtodoinjava.com/java/related-concepts/how-to-make-a-java-class-immutable/); that is, when you initially decide to put something into a `String` object, the JVM allocates a fixed-width array of exactly the size of your initial value. This is then treated as a constant inside the JVM, which allows for very significant performance savings in the case where the String’s value is not changed. However, if you decide to change the String’s contents in any way, what the JVM then essentially does is copy the contents of the original String into a temporary space, make your changes, then save those changes into a whole new memory array. Thus, making changes to a String’s value after initialization is a fairly expensive operation.

[StringBuffer](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuffer.html), on the other hand, is implemented as a dynamically – growable array inside the JVM, which means that any change operation can occur on the existing memory location, with new memory allocated only as-needed. However, there is no opportunity for the JVM to make optimizations around the `StringBuffer`, since its contents are assumed to be changeable at any instance.



## List Questions

### Why we use List interface? What are main classes implementing List interface?

A java list is a **“ordered” collection of elements**. This ordering is a **zero based index**. It does not care about duplicates. Apart from methods defined in Collection interface, it does **have its own methods** also which are largely to manipulate the collection **based on index location of element**. These methods can be grouped as search, get, iteration and range view. All above operations support index locations.

The main classes implementing List interface are: **Stack, Vector, ArrayList and LinkedList**.



### How to convert an array of String to arraylist?

This is more of a programmatic question which is seen at beginner level. The intent is to check the knowledge of applicant in Collection utility classes. For now, lets learn that there are two utility classes in Collection framework which are mostly seen in interviews i.e. **Collections and Arrays**.

Collections class provides some static functions to perform specific operations on collection types. And Arrays provide utility functions to be performed on array types.

```java
//String array
String[] words = {"ace", "boom", "crew", "dog", "eon"};
//Use Arrays utility class
List wordList = Arrays.asList(words);
//Now you can iterate over the list
```

Please note that this function is not specific to String class, it will return List of element of any type, of which the array is. e.g.

```java
//String array
Integer[] nums = {1,2,3,4};
//Use Arrays utility class
List numsList = Arrays.asList(nums);
```

### How to reverse the list?

This question is just like above to test your knowledge of **Collections** utility class. Use it **reverse()** method to reverse the list.

```java
`Collections.reverse(list);`
```







## Map Questions

### Why we use Map interface? What are main classes implementing Map interface?

Map interface is a special type of collection which is **used to store key-value pairs**. It does not extend Collection interface for this reason. This interface provides methods to add, remove, search or iterate over various views of Map.

Main classes implementing Map interface are: **HashMap, Hashtable, EnumMap, IdentityHashMap, LinkedHashMap and Properties.**



### Why Map interface does not extend Collection interface?

A good answer to this interview question is “**because they are incompatible**“. Collection has a method add(Object o). Map can not have such method because it need key-value pair. There are other reasons also such as Map supports keySet, valueSet etc. Collection classes does not have such views.

Due to such big differences, Collection interface was not used in Map interface, and it was build in separate hierarchy.



### Explain the working of HashMap. How duplicate collision is resolved?

If anybody asks me to describe “How HashMap works?”, I simply answer: “**On principles of Hashing**“. As simple as it is.

Now, Hashing in its simplest form, is a way to assigning a unique code for any variable/object after applying any formula/ algorithm on its properties.

**A map by definition is : “An object that maps keys to values”**. 

When, someone tries to store a key value pair in a `HashMap`, following things happen:

- First of all, key object is checked for null. If key is null, value is stored in table[0] position. Because hash code for null is always 0.

- Then on next step, a hash value is calculated using key’s hash code by calling its `hashCode()` method. This hash value is used to calculate index in array for storing `Entry`object. JDK designers well assumed that there might be some poorly written `hashCode()`functions that can return very high or low hash code value. To solve this issue, they introduced another `hash()` function, and passed the object’s hash code to this `hash()`function to bring hash value in range of array index size.

- Now `indexFor(hash, table.length)` function is called to calculate exact index position for storing the Entry object.

- Here comes the main part. Now, as we know that two unequal objects can have same hash code value, how two different objects will be stored in same array location [called bucket]. Answer is `LinkedList`

  . If you remember, `Entry` class had an attribute “ `next` ”. This attribute always points to next object in chain. This is exactly the behavior of `LinkedList`.

  So, in case of collision, `Entry` objects are stored in `LinkedList` form. When an `Entry` object needs to be stored in particular index, `HashMap` checks whether there is already an entry?? If there is no entry already present, `Entry` object is stored in this location.

  If there is already an object sitting on calculated index, its `next` attribute is checked. If it is **null**, and current `Entry` object becomes `next` node in `LinkedList`. If `next` variable is not null, procedure is followed until `next` is evaluated as null.

  What if we add the another value object with same key as entered before. Logically, it should replace the old value. How it is done? Well, after determining the `index` position of `Entry` object, while iterating over `LinkedList` on calculated index, `HashMap` calls `equals()`method on key object for each `Entry` object. All these `Entry` objects in `LinkedList` will have similar hash code but `equals()` method will test for true equality. If **key.equals(k)** will be true then both keys are treated as same key object. This will cause the replacing of value object inside `Entry` object only.

In this way, `HashMap` ensure the uniqueness of keys.

### How to design a good key for hashmap?

Another good question usually followed up after answering how hashmap works. Well, the most important constraint is **you must be able to fetch the value object back in future**. Otherwise, there is no use of having such a data structure. If you understand the working of hashmap, you will find it largely depends on hashCode() and equals() method of Key objects.

So a good key object **must provide same hashCode() again and again**, no matter how many times it is fetched. Similarly, same keys **must return true when compare with equals() method and different keys must return false**.

For this reason, **immutable classes are considered best candidate for HashMap keys**.

Read more : [**How to design a good key for HashMap?**](https://howtodoinjava.com/java/collections/how-to-design-a-good-key-for-hashmap/)



### What are different Collection views provided by Map interface?

Map interface provides 3 views of key-values pairs stored in it:

- key set view
- value set view
- entry set view

All the views can be navigated using iterators.



### Why HashMap should not be used in multithreaded environment? Can it cause infinite loop as well?

We know that `HashMap` is non-synchronized collection where as its synchronized counter-part is `HashTable`. So, when you are accessing the collection in multithreaded environment and all threads are accessing a single instance of collection, then its safer to use `HashTable` for various obvious reasons e.g. to avoid dirty reads and to maintain data consistency. In worst case, this mutithreaded environment can result in infinite loop as well.

Yes, it is true. `HashMap.get()` can cause an infinite loop. Lets see how??

If you look at the source code [HashMap.get(Object key)](http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/6-b14/java/util/HashMap.java#HashMap.get%28java.lang.Object%29) method, it looks like this:

```java
public Object get(Object key) {
  Object k = maskNull(key);
  int hash = hash(k);
  int i = indexFor(hash, table.length);
  Entry e = table[i];
  while (true) {
    if (e == null) return e;
    if ( e.hash == hash && eq(k, e.key))return e.value;
    e = e.next;
  }
}
```

`while(true){...}` can always be a victim of infinite loop at runtime in multithreaded environment, IF, somehow `e.next` can point to itself. This will result in infinite loop. But, how `e.next` will point to itself (i.e.).

This can happen in `void transfer(Entry[] newTable)` method, which is invoked at time the `HashMap` resizing is done.

```java
do {
  Entry next = e.next;
  int i = indexFor(e.hash, newCapacity);
  e.next = newTable[i];
  newTable[i] = e;
  e = next;
} 
```

This piece of code is prone to produce above condition, if resizing happen and at the same time other threads tried to modify the map instance.

Only way to avoid this scenario is to use synchronization in code, or better, use synchronized collection.

### When to use HashMap or TreeMap?

HashMap is well known class and all of us know that. So, I will leave this part by saying that it is used to store key-value pairs and allows to perform many operations on such collection of pairs.

TreeMap is special form of HashMap. **It maintains the ordering of keys** which is missing in HashMap class. This ordering is **by default “natural ordering”**. The default ordering can be override by providing an instance of Comparator class, whose compare method will be used to maintain ordering of keys.

Please note that **all keys inserted into the map must implement the Comparable interface**(this is necessary to decide the ordering). Furthermore, all such keys must be mutually comparable: k1.compareTo(k2) must not throw a ClassCastException for any keys k1 and k2 in the map. If the user attempts to put a key into the map that violates this constraint (for example, the user attempts to put a string key into a map whose keys are integers), the put(Object key, Object value) call will throw a ClassCastException.

### What are IdentityHashMap and WeakHashMap?

**IdentityHashMap** is similar to HashMap except that **it uses reference equality when comparing elements**. IdentityHashMap class is not a widely used Map implementation. While this class implements the Map interface, it intentionally violates Map’s general contract, which mandates the use of the equals() method when comparing objects. IdentityHashMap is designed for use only in the rare cases wherein reference-equality semantics are required.

**WeakHashMap** is an implementation of the Map interface **that stores only weak references to its keys**. Storing only weak references allows a key-value pair to be garbage collected when its key is no longer referenced outside of the WeakHashMap. This class is intended primarily for use with key objects whose equals methods test for object identity using the == operator. Once such a key is discarded it can never be recreated, so it is impossible to do a look-up of that key in a WeakHashMap at some later time and be surprised that its entry has been removed.



## Set Questions

### Why we use Set interface? What are main classes implementing Set interface?

It **models the mathematical set in set theory**. Set interface is like List interface but with some differences. First, it is **not ordered collection**. So no ordering is preserved while adding or removing elements. The main feature it does provide is “**uniqueness of elements**“. It does not support duplicate elements.

Set also adds a stronger contract on the behavior of the equals and hashCode operations, allowing Set instances to be compared meaningfully even if their implementation types differ. Two Set instances are equal if they contain the same elements.

Based on above reasons, it **does not have operations based on indexes of elements like List**. It only has methods which are inherited by Collection interface.

Main classes implementing Set interface are : **EnumSet, HashSet, LinkedHashSet, TreeSet**. 



### How HashSet store elements?

You must know that HashMap store key-value pairs, with one condition i.e. keys will be unique. HashSet uses Map’s this feature to ensure uniqueness of elements. In HashSet class, a map declaration is as below:

```java
private transient HashMap<E,Object> map;
 
//This is added as value for each key
private static final Object PRESENT = new Object();
```

So **when you store a element in HashSet, it stores the element as key in map and “PRESENT” object as value**. (See declaration above).

```java
public boolean add(E e) {
	return map.put(e, PRESENT)==null;
}
```



### Can a null element added to a TreeSet or HashSet?

As you see, There is no null check in add() method in previous question. And HashMap also allows one null key, so **one “null” is allowed in HashSet**.

TreeSet uses the same concept as HashSet for internal logic, but uses NavigableMap for storing the elements.

```java
private transient NavigableMap<E,Object> m;
 
// Dummy value to associate with an Object in the backing Map
private static final Object PRESENT = new Object();
```

NavigableMap is subtype of SortedMap which does not allow null keys. So essentially, **TreeSet also does not support null keys**. It will throw NullPointerException if you try to add null element in TreeSet.

## Queue Questions

### What is BlockingQueue?

**A Queue that additionally supports operations that wait for the queue to become non-empty when retrieving an element, and wait for space to become available in the queue when storing an element.**

BlockingQueue methods come in four forms: one throws an exception, the second returns a special value (either null or false, depending on the operation), the third blocks the current thread indefinitely until the operation can succeed, and the fourth blocks for only a given maximum time limit before giving up.

Read the example usage of blocking queue in post : [**How to use blocking queue?**](https://howtodoinjava.com/java-5/how-to-use-blockingqueue-and-threadpoolexecutor-in-java/)





## Collection Difference

### Difference between Set and List?

The most noticeable differences are :

- Set is unordered collection where List is ordered collection based on zero based index.
- List allow duplicate elements but Set does not allow duplicates.
- List does not prevent inserting null elements (as many you like), but Set will allow only one null element.

### Difference between List and Map?

Perhaps most easy question. **List is collection of elements where as map is collection of key-value pairs**. There is actually lots of differences which originate from first statement. They have**separate top level interface, separate set of generic methods, different supported methods and different views of collection**.

### Difference between HashMap and HashTable?

- [Why doesn't a hash table allow null key and value, and why does Hash Map allow one null key and multiple values?](https://www.quora.com/Why-doesnt-a-hash-table-allow-null-key-and-value-and-why-does-Hash-Map-allow-one-null-key-and-multiple-values)

There are several differences between HashMap and Hashtable in Java:

- Hashtable is synchronized, whereas HashMap is not.
- Hashtable does not allow null keys or values. HashMap allows one null key and any number of null values.
- The third significant difference between HashMap vs Hashtable is that Iterator in the HashMap is a fail-fast iterator while the enumerator for the Hashtable is not.

### Difference between Vector and ArrayList?

Lets note down the differences:

- All the methods of Vector is synchronized. But, the methods of ArrayList is not synchronized.
- Vector is a Legacy class added in first release of JDK. ArrayList was part of JDK 1.2, when collection framework was introduced in java.
- By default, Vector doubles the size of its array when it is re-sized internally. But, ArrayList increases by half of its size when it is re-sized.

| **Array List**                                               | **Vector**                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Array List is not synchronized.                              | Vector is synchronized.                                      |
| Array List is fast as it’s non-synchronized.                 | Vector is slow as it is thread safe.                         |
| If an element is inserted into the Array List, it increases its Array size by 50%. | Vector defaults to doubling size of its array.               |
| Array List does not define the increment size.               | Vector defines the increment size.                           |
| Array List can only use Iterator for traversing an Array List. | Except Hashtable, Vector is the only other class which uses both Enumeration and Iterator. |



### Difference between Iterator and Enumeration?

Iterators differ from enumerations in three ways:

- Iterators allow the caller to remove elements from the underlying collection during the iteration with its remove() method. You can not add/remove elements from a collection when using enumerator.
- Enumeration is available in legacy classes i.e Vector/Stack etc. whereas Iterator is available in all modern collection classes.
- Another minor difference is that Iterator has improved method names e.g. Enumeration.hasMoreElement() has become Iterator.hasNext(), Enumeration.nextElement() has become Iterator.next() etc.

### Difference between HashMap and HashSet?

HashMap is collection of key-value pairs whereas HashSet is un-ordered collection of unique elements.

### Difference between Iterator and ListIterator?

We can use `Iterator` to traverse a `Set` or a `List` or a `Map`. But `ListIterator` can only be used to traverse a `List` only. Other differences can be listed as below.

You can –

1. iterate backwards.
2. obtain the index at any point.
3. add a new value at any point.
4. set a new value at that point.

### Difference between TreeSet and SortedSet?

SortedSet is an interface which TreeSet implements.

### Difference between ArrayList and LinkedList?

- LinkedList store elements within a doubly-linked list data structure. ArrayList store elements within a dynamically resizing array.
- LinkedList allows for constant-time insertions or removals, but only sequential access of elements. In other words, you can walk the list forwards or backwards, but grabbing an element in the middle takes time proportional to the size of the list. ArrayLists, on the other hand, allow random access, so you can grab any element in constant time. But adding or removing from anywhere but the end requires shifting all the latter elements over, either to make an opening or fill the gap.
- LinkedList has more memory overhead than ArrayList because in ArrayList each index only holds actual object (data) but in case of LinkedList each node holds both data and address of next and previous node.

###  What is difference between fail-fast and fail-safe?

You have understood fail-fast in previous question. **Fail-safe iterators** are just opposite to fail-fast. **They never fail if you modify the underlying collection on which they are iterating**, because they work on clone of Collection instead of original collection and that’s why they are called as fail-safe iterator.

Iterator of CopyOnWriteArrayList is an example of fail-safe Iterator also iterator written by ConcurrentHashMap keySet is also fail-safe iterator and never throw ConcurrentModificationException.

### What is Queue and Stack, list down their differences?

**A collection designed for holding elements prior to processing.** Besides basic Collection operations, queues provide additional insertion, extraction, and inspection operations.
**Queues typically, but do not necessarily, order elements in a FIFO (first-in-first-out) manner.**

**Stack is also a form of Queue but one difference, it is LIFO (last-in-first-out).**

Whatever the ordering used, the head of the queue is that element which would be removed by a call to remove() or poll(). Also note that Stack and Vector are both synchronized.

**Usage:** Use a queue if you want to process a stream of incoming items in the order that they are received.Good for work lists and handling requests.
Use a stack if you want to push and pop from the top of the stack only. Good for recursive algorithms.

### What is Comparable and Comparator interface?

In java. all collection which have feature of automatic sorting, uses compare methods to ensure the correct sorting of elements. For example classes which use sorting are TreeSet, TreeMap etc.

**To sort the data elements a class needs to implement Comparator or Comparable interface**. That’s why all Wrapper classes like Integer,Double and String class implements Comparable interface.

**Comparable helps in preserving default natural sorting, whereas Comparator helps in sorting the elements in some special required sorting pattern.** The instance of comparator if passed usually as collection’s constructor argument in supporting collections.





# Thread

## 1. What is synchronization? Object level locking and class level locking?

***Synchronization*** refers to multi-threading. A synchronized block of code can only be executed by one thread at a time. Java supports multiple threads to be executed. This may cause two or more threads to access the same fields or objects. Synchronization is a process which keeps all concurrent threads in execution to be in synch. Synchronization avoids memory consistence errors caused due to inconsistent view of shared memory. When a method is declared as synchronized; the thread holds the monitor for that method’s object If another thread is executing the synchronized method, your thread is blocked until that thread releases the monitor.

Synchronization in java is achieved using synchronized keyword. You can use synchronized keyword in your class on defined methods or blocks. Keyword can not be used with variables or attributes in class definition.

***Object level locking*** is mechanism when you want to synchronize a non-static method or non-static code block such that only one thread will be able to execute the code block on given instance of the class. This should always be done to make instance level data thread safe.

***Class level locking*** prevents multiple threads to enter in synchronized block in any of all available instances on runtime. This means if in runtime there are 100 instances of  DemoClass, then only one thread will be able to execute demoMethod() in any one of instance at a time, and all other instances will be locked for other threads. This should always be done to make static data thread safe.

[Read more about synchronization here.](https://howtodoinjava.com/java/multi-threading/thread-synchronization-object-level-locking-and-class-level-locking/)

## 2. Difference between sleep() and wait()?

sleep() is a method which is used to hold the process for few seconds or the time you wanted but in case of wait() method thread goes in waiting state and it won’t come back automatically until we call the notify() or notifyAll().

The major difference is that wait() releases the lock or monitor while sleep() doesn’t releases any lock or monitor while waiting. Wait is used for inter-thread communication while sleep is used to introduce pause on execution, generally.

Thread.sleep() sends the current thread into the “Not Runnable” state for some amount of time. The thread keeps the monitors it has aquired — i.e. if the thread is currently in a synchronized block or method no other thread can enter this block or method. If another thread calls t.interrupt() it will wake up the sleeping thread. Note that sleep is a static method, which means that it always affects the current thread (the one that is executing the sleep method). A common mistake is to call t.sleep() where t is a different thread; even then, it is the current thread that will sleep, not the t thread.

object.wait() sends the current thread into the “Not Runnable” state, like sleep(), but with a twist. Wait is called on a object, not a thread; we call this object the “lock object.” Before lock.wait() is called, the current thread must synchronize on the lock object; wait() then releases this lock, and adds the thread to the “wait list” associated with the lock. Later, another thread can synchronize on the same lock object and call lock.notify(). This wakes up the original, waiting thread. Basically, wait()/notify() is like sleep()/interrupt(), only the active thread does not need a direct pointer to the sleeping thread, but only to the shared lock object.

[Read the difference in detail here.](https://howtodoinjava.com/java/multi-threading/difference-between-sleep-and-wait/)

## Can we override wait() or notify() methods?

In Object.java, methods getClass(), notify(), notifyAll() and three wait() methods are final, so you can’t override them.





# I/O







# Servlet

### **What is the life-cycle of a servlet?**

There are 5 stages in the lifecycle of a servlet:

![LifeCycleServlet - Java Interview Questions - Edureka](https://www.edureka.co/blog/wp-content/uploads/2017/04/Life-Cycle-Servlet-300x284.png)**

1. Servlet is loaded
2. Servlet is instantiated
3. Servlet is initialized
4. Service the request
5. Servlet is destroyed

