## Sources

- [Java static keyword](<https://www.javatpoint.com/static-keyword-in-java>)



# Class

## Constructor Rules

There are few mandatory rules for creating the constructors in java.

1. Constructor name MUST be same as name of the class.
2. There cannot be any return type in constructor definition.
3. There cannot be any return statement in constructor.
4. Constructors can be overloaded by different arguments.
5. If you want to use `super()` i.e. super class constructor then it must be first statement inside constructor.

## Java instance initializer features

The instance initializers have following features.

- We can define **multiple initializers** in a class.
- All initializers execute in sequence in order they appear in class body.
- Initializers run after the parent class constructor has been invoked and before executing child class constructor. Please note that Java inserts the default constructor of parent class `super()`, if we do not explicitly provide the constructor as the **first statement** in child class’s constructor.
- After all the initializers have executed, constructor’s statements are executed.
- We can use call the constructors of this class and parent classes inside initializers.

## Java instance initialization sequence flow

Based on above given features, let’s draw an outline how the instance initialization of an object flows.

1. Child class constructor is invoked.
2. Child class constructor has first statement as **super()** (or provided explicit constructor) so parent class constructor is invoked.
3. Parent class’s initializers are executed in sequence of their appearance.
4. Parent class constructor statements are executed.
5. Child class’s initializers are executed in sequence of their appearance.
6. Child class constructor statements are executed.

## Encapsulation vs Abstraction

> `Encapsulation` is realization of your desired `abstraction`.

**Abstraction is more about hiding the implementation details**. In Java abstraction is achieved through abstract classes and interfaces.

**Encapsulation is about wrapping the implementation (code) and the data it manipulates (variables) within the same class**. A Java class, where all instance variables are private and only the methods within the class can manipulate those variables, is an example of an encapsulated class.

abstraction is essentially an idea, which helps in setting the guidelines. Encapsulation is the mechanism by which we achieve the desired abstraction.

In short, from OOAD perspective:

- *Abstraction is more about ‘**What**‘ a class can do. [**Idea**]*
- *Encapsulation is more about ‘**How**‘ to achieve that functionality. [**Implementation**]*

## Inheritance

- Inheritance is also known **IS-A** relationship.
- It provides child class the ability to inherit non-private members of parent class.
- In java, inheritance is achieved via `extends` keyword.
- From [Java 8](https://howtodoinjava.com/java-8-tutorial/) onward, you can use interfaces with default methods to achieve multiple inheritance.
- Member fields are accessed from reference type class.
- Member methods are accessed from actual instance types.

## Polymorphism

In java language, polymorphism is essentially considered into two versions.

1. Compile time polymorphism (static binding or method overloading)
2. Runtime polymorphism (dynamic binding or method overriding)

**Summary**

1. Polymorphism is the ability to create a variable, a function, or an object that has more than one form.
2. In java, polymorphism is divided into two parts : method overloading and method overriding.
3. Some may argue that method overloading is not polymorphism. Then what does the term compile time “polymorphism” means??
4. Another term operator overloading is also there, e.g. “+” operator can be used to add two integers as well as concat two sub-strings. Well, this is the only available support for operator overloading in java, and you can not have your own custom defined operator overloading in java.



## Method Overloading And Overriding Rules

**Method Overloading Rules：**

**1)** First and important rule to overload a method in java is to **change method signature**. Method signature is made of **number of arguments, type of arguments and order of arguments** if they are of different types.

**2)** Return type of method is never part of method signature, so only **changing the return type of method does not amount to method overloading**.

**3)** Thrown exceptions from methods are also not considered when overloading a method. So your overloaded method throws the same exception, a different exception or it simply does no throw any exception; **no effect at all on method loading**.

**Method Overriding Rules：**

**1)** The method **argument list in overridden and overriding methods must be exactly same** If they don’t match, you will end up with an overloaded method.

**2)** The **return type of overriding method can be child class of return type declared in overridden method**.

**3)** Above all rules, **private, static and final methods can not be overridden** in java in any way. 

**4)** **Overriding method can not throw checked Exception higher in hierarchy** than thrown by overridden method. Let’s say for example overridden method in parent class throws `FileNotFoundException`, the overriding method in child class can throw `FileNotFoundException`; but it is not allowed to throw `IOException` or `Exception`, because `IOException` or `Exception` are higher in hierarchy i.e. super classes of `FileNotFoundException`.

*More to it, you can omit the exception declaration from overriding method. It’s allowed and perfectly valid. Also overriding method can throw any unchecked (runtime) exception, regardless of whether the overridden method declares the exception.*

## Differences between extends vs implements

In Java, **extends** is used for extending a class and **implements** is used for implementing the interfaces. It’s the main **difference between extends vs implements**.

1. extends keyword is used to inherit a class; while implements keyword is used to inherit the interfaces.
2. A class can extend only one class; but can implement any number of interfaces.
3. A subclass that extends a superclass may override some of the methods from superclass. A class must implement all the methods from interfaces.

## Java instanceof with arrays

In Java, arrays are also considered objects and have fields and methods associated with them. So we can use instanceof operator with [arrays](https://howtodoinjava.com/java-array/) as well.

- **Primitive arrays** are instance of Object and self type. e.g. int[] is type of Object and int[]. Both comparison returns true.
- **Object arrays** are types of Object, Object array, classtype array, parent class type array. e.g. Integer[] is type of Object, Object[], Integer[] and Number[] (`Integer extends Number`).

## Association, Aggregation and Composition

### Association in Java

We call **association** those relationships whose **objects have an independent lifecycle** and where there is **no ownership between the objects**.

Let’s take an example of a teacher and student. Multiple students can associate with a single teacher, and a single student can associate with multiple teachers, but both have their own lifecycles (both can be create and delete independently); so when a teacher leaves the school, we don’t need to delete any students, and when a student leaves the school, we don’t need to delete any teachers.

![association](https://cdn1.howtodoinjava.com/wp-content/uploads/2017/02/association.jpg)

### Aggregation in Java

We call **aggregation** those relationships whose **objects have an independent lifecycle, but there is ownership**, and child objects cannot belong to another parent object.

Let’s take an example of a cell phone and a cell phone battery. A single battery can belong to a phone, but if the phone stops working, and we delete it from our database, the phone battery will not be deleted because it may still be functional. So in aggregation, while there is ownership, objects have their own lifecycle.

![aggregation](https://cdn2.howtodoinjava.com/wp-content/uploads/2017/02/aggregation.jpg)

### Composition in Java

We use the term **composition** to refer to relationships whose objects **don’t have an independent lifecycle**, and *if the parent object is deleted, all child objects will also be deleted*.

Let’s take an example of the relationship between questions and answers. Single questions can have multiple answers, and answers cannot belong to multiple questions. If we delete questions, answers will automatically be deleted.

![composition](https://cdn2.howtodoinjava.com/wp-content/uploads/2017/02/composition.jpg)

### Summary

Sometimes, it can be a complicated process to decide if we should use association, aggregation, or composition. This difficulty is caused in part because aggregation and composition are subsets of association, meaning they are specific cases of association.

![Association, Aggregation and Composition Relationship](https://cdn1.howtodoinjava.com/wp-content/uploads/2017/02/Association-Aggregation-and-Composition-Relationship.jpg)



# Static

## What is Static Variable in Java?

Static variable in Java is variable which belongs to the class and initialized only once at the start of the execution.

- It is a variable which belongs to the class and not to object(instance)
- Static variables are initialized only once, at the start of the execution. These variables will be initialized first, before the initialization of any instance variables
- A single copy to be shared by all instances of the class
- A static variable can be accessed directly by the class name and doesn’t need any object



## What is Static Method in Java?

Static method in Java is a method which belongs to the class and not to the object. A static method can access only static data.

- It is a method which belongs to the class and not to the object(instance)
- A static method can access only static data. It can not access non-static data (instance variables)
- A static method can call only other static methods and can not call a non-static method from it.
- A static method can be accessed directly by the class name and doesn’t need any object
- A static method cannot refer to "this" or "super" keywords in anyway

![static](https://www.guru99.com/images/uploads/2012/07/static.jpg)

**Q) Why is the Java main method static?**
Ans) It is because the object is not required to call a static method. If it were a non-static method, JVM creates an object first then call `main()` method that will lead the problem of extra memory allocation.



## **Java Static Block**

- Is used to initialize the static data member.
- It is executed before the main method at the time of classloading.



The static block is a block of statement inside a Java class that will be executed when a class is first loaded into the JVM

```java
class Test{
 static {
 //Code goes here
 }
}
```

```java
class A2{  
  static{System.out.println("static block is invoked");}  
  public static void main(String args[]){  
   System.out.println("Hello main");  
  }  
}  
```

```
Output:static block is invoked
       Hello main
```

A **static block helps to initialize the static data members**, just like constructors help to initialize instance members

```java
public class Demo {
 static int a;
 static int b;
 static {
    a = 10;
    b = 20;
 	}
 }
```

 **Q) Can we execute a program without main() method?**

Ans) No, one of the ways was the static block, but it was possible till JDK 1.6. Since JDK 1.7, it is not possible to execute a java class without the main method.

```java
class A3{  
  static{  
  System.out.println("static block is invoked");  
  System.exit(0);  
  }  
} 
```

Output:

```
static block is invoked
```

Since JDK 1.7 and above, output would be:

```
Error: Main method not found in class A3, please define the main method as:
   public static void main(String[] args)
or a JavaFX application class must extend javafx.application.Application
```

# Abstract & Interface

## Abstract

JVM identifies abstract class as **incomplete class**, which has not defined its complete behavior. Declaring a class `abstract` enforces only one thing: you can not create an instance of this class, and that’s it.

An **abstract method**, is a method which is not implemented in place. An abstract method adds the incompleteness to class, thus compiler wants to declare whole class abstract.

The only way to use an abstract class in your application is to extend this class. Its subclasses if not declared `abstract` again, can be instantiated. The feature that subclass inherits the behavior of the superclass and superclass can hold the reference of subclass increases the importance of abstract classes many folds.

### Abstract Class in Java: Important Points

- An abstract class **may** also have concrete (complete) methods.
- For design purpose, a class can be declared abstract even if it does not contain any abstract methods
- Reference of an abstract class can point to objects of its sub-classes thereby achieving run-time polymorphism Ex: Shape obj = new Rectangle();
- A class must be compulsorily labeled abstract, if it has one or more abstract methods.



## Interface

Interface define contracts, which implementing classes need to honor. These contracts are essentially unimplemented methods. Java already has a keyword for unimplemented methods i.e. *abstract*. **Java has the provision where any class can implement any interface, so all the methods declared in interfaces need to be public only**.

There is only one scenario when you implement an interface and do not override the method i.e. declare the implementing class itself `abstract`.

```java
public interface TestInterface{
    void implementMe();
}

public abstract class TestMain implements TestInterface{
    //No need to override implement Me
}
```

Otherwise, you must implement the method `implementMe()` in you class without any other exception.

```java
public class TestMain implements TestInterface{
    @Override
    public void implementMe() {
        // TODO Auto-generated method stub
    }
}
```

**An interface only provide contracts and it is the responsibility of implementing classes to implement each and every single contract provided to it**.

An interface is the best fit for cases where you want to **define only the characteristics of class**, and you want to force all implementing entities to implement those characteristics.

### Must know facts about Interface

- A Java class can implement multiple Java Interfaces. It is necessary that the class must implement all the methods declared in the interfaces.
- Class should override all the abstract methods declared in the interface
- The interface allows sending a message to an object without concerning which classes it belongs.
- Class needs to provide functionality for the methods declared in the interface.
- All methods in an interface are implicitly public and abstract
- An interface cannot be instantiated
- An interface reference can point to objects of its implementing classes
- An interface can extend from one or many interfaces. Class can extend only one class but implement any number of interfaces
- An interface cannot implement another Interface. It has to extend another interface if needed.
- An interface which is declared inside another interface is referred as nested interface
- At the time of declaration, interface variable must be initialized. Otherwise, the compiler will throw an error.
- The class cannot implement two interfaces in java that have methods with same name but different return type.



## Interface Vs. Abstract Class

1. Interfaces have all methods inherently *public* and *abstract*. You can not override this behavior by trying to reduce accessibility of methods. You can not even declare the static methods. Only public and abstract.

   On the other side, abstract classes are flexible in declaring the methods. You can define abstract methods with protected accessibility also. Additionally, you can define static methods as well, provided they are not abstract. Non-abstract static methods are allowed.

2. Interfaces can’t have fully defined methods. By definition, interfaces are meant to provide the only contract.

   Abstract classes can have non-abstract methods without any limitation. You can use any keyword with non-abstract methods as you will do in any other class.

3. Any class which want to use abstract class can extend abstract class using keyword `extends` , whereas for implementing interfaces keyword used is `implements`.

   A class can extend only one class but can implement any number of interfaces. This property is often referred as simulation of **multiple inheritance** in java.

4. Interface is absolutely `abstract` and cannot be instantiated; A Java abstract class also cannot be instantiated, but can be invoked if a main() exists.

Next, a question may come if we have abstract methods and main class both, we may try to call the abstract method from `main()`. But this attempt will fail, as `main()` method is always static and abstract methods can never be static, so you can never access any non-static method inside the static method.



**Important Reasons For Using Interfaces**

- Interfaces are used to achieve abstraction.
- Designed to support dynamic method resolution at run time
- It helps you to achieve loose coupling.
- Allows you to separate the definition of a method from the inheritance hierarchy

**Important Reasons For Using Abstract Class**

- Abstract classes offer default functionality for the subclasses.
- Provides a template for future specific classes
- Helps you to define a common interface for its subclasses
- Abstract class allows code reusability.

| **Parameters**               | **Interface**                                                | **Abstract class**                                           |
| :--------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Speed                        | Slow                                                         | Fast                                                         |
| Multiple Inheritances        | Implement several Interfaces                                 | Only one abstract class                                      |
| Structure                    | Abstract methods                                             | Abstract & concrete methods                                  |
| When to use                  | Future enhancement                                           | To avoid independence                                        |
| Inheritance/ Implementation  | A Class can implement multiple interfaces                    | The class can inherit only one Abstract Class                |
| Default Implementation       | While adding new stuff to the interface, it is a nightmare to find all the implementors and implement newly defined stuff. | In case of Abstract Class, you can take advantage of the default implementation. |
| Access Modifiers             | The interface does not have access modifiers. Everything defined inside the interface is assumed public modifier. | Abstract Class can have an access modifier.                  |
| When to use                  | It is better to use interface when various implementations share only method signature. Polymorphic hierarchy of value types. | It should be used when various implementations of the same kind share a common behavior. |
| Data fields                  | the interface cannot contain data fields.                    | the class can have data fields.                              |
| Multiple Inheritance Default | A class may implement numerous interfaces.                   | A class inherits only one abstract class.                    |
| Implementation               | An interface is abstract so that it can't provide any code.  | An abstract class can give complete, default code which should be overridden. |
| Use of Access modifiers      | You cannot use access modifiers for the method, properties, etc. | You can use an abstract class which contains access modifiers. |
| Usage                        | Interfaces help to define the peripheral abilities of a class. | An abstract class defines the identity of a class.           |
| Defined fields               | No fields can be defined                                     | An abstract class allows you to define both fields and constants |
| Inheritance                  | An interface can inherit multiple interfaces but cannot inherit a class. | An abstract class can inherit a class and multiple interfaces. |
| Constructor or destructors   | An interface cannot declare constructors or destructors.     | An abstract class can declare constructors and destructors.  |
| Limit of Extensions          | It can extend any number of interfaces.                      | It can extend only one class or one abstract class at a time. |
| Abstract keyword             | In an abstract interface keyword, is optional for declaring a method as an abstract. | In an abstract class, the abstract keyword is compulsory for declaring a method as an abstract. |
| Class type                   | An interface can have only public abstract methods.          | An abstract class has protected and public abstract methods. |

# Generics

[Generics](https://docs.oracle.com/javase/tutorial/extra/generics/) in java were introduced as one of [**features in JDK 5**](https://howtodoinjava.com/java-5/).



# Other

## This

Here is given the 6 usage of java this keyword.

1. this can be used to refer current class instance variable.(If local variables(formal arguments) and instance variables are different, there is no need to use this keyword like in the following program)
2. this can be used to invoke current class method (implicitly)
3. `this()` can be used to invoke current class constructor.(Call to `this()` must be the first statement in constructor.)
4. this can be passed as an argument in the method call.
5. this can be passed as argument in the constructor call.
6. this can be used to return the current class instance from the method.



## Final Keyword in Java

The final modifier applies to classes, methods, and variables. The meaning of final varies from context to context, but the essential idea is the same.

- A final class may not be inherited
- A final variable becomes a constant and its value can not be changed.
- A final method may not be overridden. This is done for security reasons, and these methods are used for optimization.
- A final class can not be inherited

## Thread Life Cycle in Java

The Lifecycle of a thread:

[![Multithreading in Java](https://www.guru99.com/images/4-2016/042616_0819_Multithread1.png)](https://www.guru99.com/images/4-2016/042616_0819_Multithread1.png)

There are various stages of life cycle of thread as shown in above diagram:

1. New
2. Runnable
3. Running
4. Waiting
5. Dead
6. **New:** In this phase, the thread is created using class "Thread class".It remains in this state till the program **starts** the thread. It is also known as born thread.
7. **Runnable:** In this page, the instance of the thread is invoked with a start method. The thread control is given to scheduler to finish the execution. It depends on the scheduler, whether to run the thread.
8. **Running:** When the thread starts executing, then the state is changed to "running" state. The scheduler selects one thread from the thread pool, and it starts executing in the application.
9. **Waiting:** This is the state when a thread has to wait. As there multiple threads are running in the application, there is a need for synchronization between threads. Hence, one thread has to wait, till the other thread gets executed. Therefore, this state is referred as waiting state.
10. **Dead:** This is the state when the thread is terminated. The thread is in running state and as soon as it completed processing it is in "dead state".



## Metadata of Class

Following example shows how to get metadata such as: Class name, super class name, implemented interfaces, and access modifiers of a class.

We will get the metadata of below class named Guru99Base.class:

[![Java Reflection API - Tutorial](https://www.guru99.com/images/9-2015/082715_1155_JavaReflect2.jpg)](https://www.guru99.com/images/9-2015/082715_1155_JavaReflect2.jpg)

1. Name of the class is: Guru99Base
2. It's access modifiers are: public and abstract
3. It has implemented interfaces: Serializable and Cloneable
4. Since it has not extended any class explicitly, it's super class is: java.lang.Object



## What is Stack Memory?

Stack in java is a section of memory which contains methods, local variables, and reference variables. Stack memory is always referenced in Last-In-First-Out order. Local variables are created in the stack.

## What is Heap Memory?

Heap is a section of memory which contains Objects and may also contain reference variables. Instance variables are created in the heap







