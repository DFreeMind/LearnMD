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

[Generics](https://docs.oracle.com/javase/tutorial/extra/generics/) in java were introduced as one of [**features in JDK 5**](https://howtodoinjava.com/java-5/). It provide the type  checking at compile time. If you use generics, you need not to perform the type casting explicitly. Java compiler applies strong type checking if you use generics in your code and shows errors if the code violates the type safety. Thus removing the risk of ClassCastException.

“**Java Generics**” is a technical term denoting a set of language features related to the definition and use of generic types and methods . In java, Generic types or methods differ from regular types and methods in that they have type parameters.

> “Java Generics are a language feature that allows for definition and use of generic types and methods.”

Generic types are instantiated to form parameterized types by providing actual type arguments that replace the formal type parameters. A class like `LinkedList<E>` is a generic type, that has a type parameter E . Instantiations, such as `LinkedList<Integer>` or a `LinkedList<String>`, are called parameterized types, and String and Integer are the respective actual type arguments.



## Why Generics

Programmers like us often wanted to specify that a collection contains elements only of a certain type e.g. `Integer` or `String` or `Employee`. In original collection framework, having homogeneous collections was not possible without adding extra checks before adding some checks in code. Generics were introduced to remove this limitation to be very specific. They add this type checking of parameters in your code at compile time, automatically. This saves us writing a lot of un-necessary code which actually does not add any value in run-time, if written correctly.

> “In layman,this term, generics force type safety in java language.”

Without this type safety, your code could have infected by various bugs which get revealed only in runtime. Using generics, makes them highlighted in compile time itself and make you code robust even before you get the bytecode of your java sourcecode files.

> “Generics add stability to your code by making more of your bugs detectable at compile time.”

## How Generics works

In the heart of generics is “[**type safety**](https://en.wikipedia.org/wiki/Type_safety)“. What exactly is type safety? It’s just a guarantee by compiler that if correct Types are used in correct places then there should not be any `ClassCastException` in runtime. A usecase can be list of `Integer`i.e. `List<Integer>`. If you declare a list in java like `List<Integer>`, then java guarantees that it will detect and report you any attempt to insert any non-integer type into above list.

```java
List<Integer> list = new ArrayList<Integer>();
 
list.add(1000);     //works fine
 
list.add("lokesh"); //compile time error;
```

When you write above code and compile it, you will get below error: “*The method add(Integer) in the type List<Integer> is not applicable for the arguments (String)*“. Compiler warned you. This exactly is generics sole purpose i.e. Type Safety.

Another important term in java generics is “[**type erasure**](https://en.wikipedia.org/wiki/Type_erasure)“. It essentially means that all the extra information added using generics into sourcecode will be removed from bytecode generated from it. Inside bytecode, it will be old java syntax which you will get if you don’t use generics at all. This necessarily helps in generating and executing code written prior java 5 when generics were not added in language.

```java
List list = new ArrayList();
 
list.add(1000); 
```

Second part is getting byte code after removing second line from above example. If you compare the bytecode of above example with/without generics, then there will not be any difference. Clearly compiler removed all generics information. So, above code is very much similar to below code without generics.

Another example：

```java
import java.util.ArrayList;
class Demo{
	public static void main(String[] args) {
		ArrayList<Number> integers = new ArrayList<Number>();
		integers.add(12);
		integers.add(12L);
		integers.add(12d);

		int[] ints = new int[3];
		ints[0]=1;
		ints[1]=2;
	}
}
```

![image](https://ws2.sinaimg.cn/large/69d4185bly1g236eo4q6nj20g309fwg8.jpg)

through the bytecode tool, we can see the details:

```java
import java.util.ArrayList;
class Demo{
  public static void main(String[] paramArrayOfString){
    ArrayList localArrayList = new ArrayList();
    localArrayList.add(Integer.valueOf(12));
    localArrayList.add(Long.valueOf(12L));
    localArrayList.add(Double.valueOf(12.0D));
    
    int[] arrayOfInt = new int[3];
    arrayOfInt[0] = 1;
    arrayOfInt[1] = 2;
  }
}
```

> “Precisely, Generics in Java is nothing but a syntactic sugar to your code for Type Safety and all such type information is erased by Type Erasure feature by compiler.”

## Types of Generics

### Generic Type Class or Interface

A class is generic if it declares one or more type variables. These type variables are known as the type parameters of the class.

`DemoClass` is simple java class, which have one property t (can be more than one also); and type of property is Object.

```java
class DemoClass {
   private Object t;
 
   public void set(Object t) { this.t = t; }
    
   public Object get() { return t; }
}
```

Here we want that once initialized the class with a certain type, class should be used with that particular type only. e.g. If we want one instance of class to hold value t of type ‘`String`‘, then programmer should set and get the only `String` type. Since we have declared property type to `Object`, there is no way to enforce this restriction. A programmer can set any object; and can expect any return value type from get method since all java types are subtypes of `Object` class.

To enforce this type restriction, we can use generics as below:

```java
class DemoClass<T> {
   //T stands for "Type"
   private T t;
 
   public void set(T t) { this.t = t; }
    
   public T get() { return t; }
}
```

Now we can be assured that class will not be misused with wrong types. A sample usage of `DemoClass` will look like this:

```java
DemoClass<String> instance = new DemoClass<String>();
instance.set("lokesh");   //Correct usage
instance.set(1);        //This will raise compile time error
```

Above analogy is true for interface as well. Let’s quickly look at an example to understand, how generics type information can be used in interfaces in java.

```java
//Generic interface definition
interface DemoInterface<T1, T2>{
   T2 doSomeOperation(T1 t);
   T1 doReverseOperation(T2 t);
}
 
//A class implementing generic interface
class DemoClass implements DemoInterface<String, Integer>{
   public Integer doSomeOperation(String t){
      //some code
   }
   public String doReverseOperation(Integer t){
      //some code
   }
}
```

### Generic Type Method or Constructor

Generic methods are much similar to generic classes. They are different only in one aspect that scope of type information is inside method (or constructor) only. Generic methods are methods that introduce their own type parameters.

```java
public static <T> int countAllOccurrences(T[] list, T item) {
   int count = 0;
   if (item == null) {
      for ( T listItem : list )
         if (listItem == null)
            count++;
   } else {
      for ( T listItem : list )
         if (item.equals(listItem))
            count++;
   }
   return count;
}  
```

If you pass a list of `String` and another string to search in this method, it will work fine. But if you will try to find an `Number` into list of `String`, it will give compile time error.

Same as above can be example of generic constructor. 

```java
class Dimension<T> {
   private T length;
   private T width;
   private T height;
 
   //Generic constructor
   public Dimension(T length, T width, T height){
      super();
      this.length = length;
      this.width = width;
      this.height = height;
   }
}
```

In this example, `Dimension` class’s constructor has the type information also. So you can have an instance of dimension with all attributes of a single type only.

## Generic Type Arrays

Array in any language have same meaning i.e. an array is a collection of similar type of elements. In java, pushing any incompatible type in an array on runtime will throw `ArrayStoreException`. It means array preserve their type information in runtime, and generics use type erasure or remove any type information in runtime. Due to above conflict, instantiating a generic array in java is not permitted.

```java
public class GenericArray<T> {
    // this one is fine
    public T[] notYetInstantiatedArray;
  
    // causes compiler error; Cannot create a generic array of T
    public T[] array = new T[5];
}
```

In the same line as above generic type classes and methods, we can have generic arrays in java. As we know that an array is a collection of similar type of elements and pushing any incompatible type will throw `ArrayStoreException` in runtime; which is not the case with `Collection` classes.

```java
`Object[] array = ``new` `String[``10``];``array[``0``] = ``"lokesh"``;``array[``1``] = ``10``;      ``//This will throw ArrayStoreException`
```

Above mistake is not very hard to make. It can happen anytime. So it’s better to provide the type information to array also so that error is caught at compile time itself.

Another reason why arrays does not support generics is that arrays are co-variant, which means that an array of supertype references is a supertype of an array of subtype references. That is, `Object[]` is a supertype of `String[]` and a string array can be accessed through a reference variable of type `Object[]`.

```java
`Object[] objArr = ``new` `String[``10``];  ``// fine``objArr[``0``] = ``new` `String();`
```

## Generics with Wildcards

In generic code, the question mark (?), called the wildcard, represents an unknown type. **A wildcard parameterized type is an instantiation of a generic type where at least one type argument is a wildcard.** Examples of wildcard parameterized types are `Collection<?>`, `List<? extends Number>`, `Comparator<? super String>` and `Pair<String,?>`. The wildcard can be used in a variety of situations: as the type of a parameter, field, or local variable; sometimes as a return type (though it is better programming practice to be more specific). The wildcard is never used as a type argument for a generic method invocation, a generic class instance creation, or a supertype.

Having wildcards at difference places have different meanings as well. e.g.

- **Collection** denotes all instantiations of the Collection interface regardless of the type argument.
- **List** denotes all list types where the element type is a subtype of Number.
- **Comparator<? super String>** denotes all instantiations of the Comparatorinterface for type argument types that are supertypes of String.

```java
Collection<?> coll = new ArrayList<String>();
//OR
List<? extends Number> list = new ArrayList<Long>();
//OR
Pair<String,?> pair = new Pair<String,Integer>();
```

And below are not valid uses of wildcards:

```java
List<? extends Number> list = new ArrayList<String>();  //String is not subclass of Number; so error
//OR
Comparator<? super String> cmp = new RuleBasedCollator(new Integer(100)); //Integer is not superclass of String
```

#### Bounded wildcard parameterized type

Bounded wildcards put some restrictions over possible types, you can use to instantiate a parametrized type. This restriction is enforced using keywords “super”(lower bounded) and “extends”(upper bounded). To differentiate more clearly, let’s divide them into upper bounded wildcards and lower bounded wildcards.

## What is not allowed to do with Generics

#### You can’t have static field of type

You can not define a static generic parameterized member in your class. Any attempt to do so will generate compile time error: Cannot make a static reference to the non-static type T.

```java
public class GenericsExample<T>{
   private static T member; //This is not allowed
}
```

#### You can not create an instance of T

Any attempt to create an instance of T will fail with error: Cannot instantiate the type T.

```java
public class GenericsExample<T>{
   public GenericsExample(){
      new T();
   }
}
```

#### Generics are not compatible with primitives in declarations

Yes, it’s true. You can’t declare generic expression like List or Map<String, double>. Definitely you can use the wrapper classes in place of primitives and then use primitives when passing the actual values. These value primitives are accepted by using auto-boxing to convert primitives to respective wrapper classes.

```java
final List<int> ids = new ArrayList<>();    //Not allowed
 
final List<Integer> ids = new ArrayList<>(); //Allowed
```

#### You can’t create Generic exception class

Sometimes, programmer might be in need of passing an instance of generic type along with exception being thrown. This is not possible to do in Java.

```java
// causes compiler error
public class GenericException<T> extends Exception {}
```

When you try to create such an exception, you will end up with message like this: The generic class `GenericException` may not subclass `java.lang.Throwable`.

[Why doesn't Java allow generic subclasses of Throwable?](https://stackoverflow.com/questions/501277/why-doesnt-java-allow-generic-subclasses-of-throwable)

[为什么泛型类无法继承自 Throwable](<https://blog.csdn.net/ziwang_/article/details/56288597>)

As mark said, the types are not reifiable, which is a problem in the following case:

```java
try {
   doSomeStuff();
} catch (SomeException<Integer> e) {
   // ignore that
} catch (SomeException<String> e) {
   crashAndBurn()
}
```

Both `SomeException<Integer>` and `SomeException<String>` are erased to the same type, there is no way for the JVM to distinguish the exception instances, and therefore no way to tell which `catch` block should be executed.

we should know that  generic class can't extend from `Throwable` but generic can extend from `Throwable`, example:

```java
// causes compiler error
public class GenericException<T> extends Exception {}
// but this valid
public class Test<T extends Throwable> {}
```

## Generics PECS

Look at two methods in java collection APIS.

This method is responsible for adding all members of collection “c” into another collection where this method is called.

```java
boolean addAll(Collection<? extends E> c);
```

This method is called for adding “elements” to collection “c”.

```java
public static <T> boolean addAll(Collection<? super T> c, T... elements);
```

Both seems to be doing simple thing, so why they both have different syntax.

### Understanding <? extends T>

This is the first part of **PECS** i.e. **PE (Producer extends)**. To more relate it to real life terms, let’s use an analogy of a basket of fruits (i.e. collection of fruits). When we pick a fruit from basket, then we want to be sure that we are taking out only fruit only and nothing else; so that we can write generic code like this:

```java
Fruit get = fruits.get(0);
```

In above case, we need to declare the collection of fruits as `List<? extends Fruit>`. e.g.

```java
class Fruit {
   @Override
   public String toString() {
      return "I am a Fruit !!";
   }
}
 
class Apple extends Fruit {
   @Override
   public String toString() {
      return "I am an Apple !!";
   }
}
 
public class GenericsExamples{
   public static void main(String[] args){
      //List of apples
      List<Apple> apples = new ArrayList<Apple>();
      apples.add(new Apple());
       
      //We can assign a list of apples to a basket of fruits;
      //because apple is subtype of fruit
      List<? extends Fruit> basket = apples;
       
      //Here we know that in basket there is nothing but fruit only
      for (Fruit fruit : basket){
         System.out.println(fruit);
      }
       
      //basket.add(new Apple()); //Compile time error
      //basket.add(new Fruit()); //Compile time error
   }
}
```

Look at the for loop above. It ensures that whatever it comes out from basket is definitely going to be a fruit; so you iterate over it and simply cast it a Fruit. Now in last two lines, I tried to add an `Apple` and then a `Fruit` in basket, but compiler didn’t allowed me. Why?

The reason is pretty simple, if we think about it; the `<? extends Fruit>` wildcard tells the compiler that we’re dealing with a subtype of the type Fruit, but **we cannot know which fruit as there may be multiple subtypes**. Since there’s no way to tell, and we need to guarantee type safety (invariance), you won’t be allowed to put anything inside such a structure.

On the other hand, since we know that whichever type it might be, it will be a subtype of `Fruit`, we can get data out of the structure with the guarantee that it will be a `Fruit`.

> In above example, we are taking elements out of collection “`List<? extends Fruit> basket`“; so here this basket is actually producing the elements i.e. fruits. In simple words, when you want to ONLY retrieve elements out of a collection, treat it as a producer and use “`? extends T>`” syntax. “**Producer extends**” now should make more sense to you.

### Understanding <? super T>

Now look at above usecase in different way. Let’s assume we are defining a method where we will only be adding different fruits inside this basket. Just like we saw the method in start of post “`addAll(Collection<? super T> c, T... elements)`“. In such case, basket is used for storing the elements so it should be called **consumer of elements**.

```java
class AsianApple extends Apple {
   @Override
   public String toString() {
      return "I am an AsianApple !!";
   }
}
 
public class GenericsExamples{
   public static void main(String[] args){
      //List of apples
      List<Apple> apples = new ArrayList<Apple>();
      apples.add(new Apple());
       
      //We can assign a list of apples to a basket of apples
      List<? super Apple> basket = apples;
       
      basket.add(new Apple());      //Successful
      basket.add(new AsianApple()); //Successful
      basket.add(new Fruit());      //Compile time error
   }
}
```

We are able to add apple and even Asian apple inside basket, but we are not able to add Fruit (super type of apple) to basket. Why?

Reason is that basket is a **reference to a List of something that is a supertype of Apple**. Again, **we cannot know which supertype it is**, but we know that Apple and any of its subtypes (which are subtype of Fruit) can be added to be without problem (*you can always add a subtype in collection of supertype*). So, now we can add any type of Apple inside basket.

What about getting data out of such a type? It turns out that you the only thing you can get out of it will be `Object` instances: since we cannot know which supertype it is, the compiler can only guarantee that it will be a reference to an `Object`, since `Object` is the supertype of any Java type.

> In above example, we are putting elements inside collection “`List<? super Apple> basket`“; so here this basket is actually consuming the elements i.e. apples. In simple words, when you want to ONLY add elements inside a collection, treat it as a consumer and use “`? super T>`” syntax. Now, “**Consumer super**” also should make more sense to you.

### Summary

1. Use the `<? extends T>` wildcard if you need to retrieve object of type T from a collection.
2. Use the `<? super T>` wildcard if you need to put objects of type T in a collection.
3. If you need to satisfy both things, well, don’t use any wildcard. As simple as it is.
4. In short, remember the term **PECS. Producer extends Consumer super**. Really easy to remember.



# Reflection

[Java Reflection API Tutorial with Example](<https://www.guru99.com/java-reflection-api.html>)

[Java Reflection Tutorial](<http://tutorials.jenkov.com/java-reflection/index.html>)

[Java Reflection：神秘的Java反射机制了解一下？](<https://juejin.im/entry/5b3d6e2fe51d45199940bd39>)



Java Reflection is the process of analyzing and modifying all the capabilities of a class at runtime. Reflection API in Java is used to manipulate class and its members which include fields, methods, constructor, etc. at runtime.

Java Reflection is quite powerful and can be very useful. For instance, Java Reflection can be used to map properties in JSON files to getter / setter methods in Java objects, like [Jackson, GSON, Boon etc.](http://tutorials.jenkov.com/java-json/index.html)does. Or, Reflection can be used to map the column names of a [JDBC](http://tutorials.jenkov.com/jdbc/index.html) ResultSet to getter / setter methods in a Java object.

**Class in java.lang.reflect Package**

- **Field**: This class is used to gather declarative information such as datatype, access modifier, name and value of a variable.
- **Method**: This class is used to gather declarative information such as access modifier, return type, name, parameter types and exception type of a method.
- **Constructor**: This class is used to gather declarative information such as access modifier, name and parameter types of a constructor.
- **Modifier**: This class is used to gather information about a particular access modifier.

**Methods used in java.lang.Class**

- **Public String getName ()**: Returns the name of the class.
- **public Class getSuperclass()**: Returns the super class reference
- **Public Class[] getInterfaces()** : Returns an array of interfaces implemented by the specified class
- **Public in getModifiers ():** Returns an integer value representing the modifiers of the specified class which needs to be passed as a parameter to "**public static String toString (int i )"** method which returns the access specifier for the given class.

To get the information about variables , methods and contructors of a class. First we need to create an object of the class. There is several methods to get an object of class, example:

```java
class ClassObjectCreation {
    public static void main (String[] args) throws ClassNotFoundException {
        //1 - By using Class.forname() method
        Class c1 = Class.forName("ClassObjectCreation");
        //2- By using getClass() method
        ClassObjectCreation guru99Obj = new ClassObjectCreation();
        Class c2 = guru99Obj.getClass();
        //3- By using .class
        Class c3= ClassObjectCreation.class;
    }
}
```

## Get Metadata of Class

We will get the metadata of below class named Guru99Base.class:

```java
import java.io.Serializable;
public abstract class DemoBase implements Serializable, Cloneable {}
```

![image](https://wx4.sinaimg.cn/large/69d4185bly1g23isp0ar7j20lx03r74n.jpg)

1. Name of the class is: DemoBase
2. It's access modifiers are: public and abstract
3. It has implemented interfaces: Serializable and Cloneable
4. Since it has not extended any class explicitly, it's super class is: java.lang.Object

```java
import java.lang.reflect.Modifier;

public class DemoGetClassMetadata {

    public static int demoInt = 1111;
    static int demoInt2 = 222;
    static String demoString = "weduoo.com";
    static String demoString2 = "learn reflection API";
    // private String demoPrivate = "weduoo.cn";
		public void setDemoInt2(int int2){
        demoInt2 = int2;
    }
  
    public static void main(String[] args) {
        Class democls = DemoBase.class;

        // print name of the class
        System.out.println("Name of the class is:" + democls.getName());

        // print super class name
        System.out.println("Name of the super class is:" + democls.getSuperclass().getName());

        // get the list of implemented interfaces in the form of Class array using
        // getInterface() method
        Class[] list = democls.getInterfaces();
        for (Class demo: list) {
            System.out.println("interfaces :" + demo.getName());
        }

        // Get access modifiers using getModifiers() method and toString() method of
        // java.lang.reflect.Modifier class
        int mo = democls.getModifiers();
        System.out.println("Access modifiers of the class∂ are:" + Modifier.toString(mo));

    }
}

```

```
Name of the class is:com.weduoo.reflect.DemoBase
Name of the super class is:java.lang.Object
interfaces :java.io.Serializable
interfaces :java.lang.Cloneable
Access modifiers of the class∂ are:public abstract
```

## Get Metadata of Variable

- `getFields()` method returns metadata of the public variable from the specified class *as well as from its super class*.

- `getDeclaredFields()` method returns metadata of the all the variables from the specified class only.

- `getName()` get the name of the variables

- `getType()` get the datatype of the variables

-  `get (Field)` get the value of the variable
- `getModifier()、Modifier.toString(int i)` get the access modifiers of the variables 

```java
public class DemoVarivableMetadataTest {
    public static void main(String[] args) throws IllegalAccessException {
        // create Class object for DemoGetClassMetadata.class
        DemoGetClassMetadata democls = new DemoGetClassMetadata();
        Class aClass = democls.getClass();
      
      	DemoGetClassMetadata metadata = new DemoGetClassMetadata();
        metadata.setDemoInt2(123);

        // get the metadata of all the fields of the class DemoGetClassMetadata
        Field[] declaredFields = aClass.getDeclaredFields();

        // print name, datatype, access modifiers and values of the
        // variables of the specified class
        System.out.println("declared fields---->");
        for (Field field : declaredFields ) {
            System.out.println("Variable Name:" + field.getName());
            System.out.println("datatype of the Variable:" + field.getType());
            int mod = field.getModifiers();
            System.out.println("Access Modifiers of the Variable:" + Modifier.toString(mod));
            System.out.println("Value of the variable:" + field.get(metadata));
            System.out.println("*******************");
        }
    }
}
```

```
declared fields---->
Variable Name:demoInt
datatype of the Variable:int
Access Modifiers of the Variable:public static
Value of the variable:1111
*******************
Variable Name:demoInt2
datatype of the Variable:int
Access Modifiers of the Variable:static
Value of the variable:123
*******************
Variable Name:demoString
datatype of the Variable:class java.lang.String
Access Modifiers of the Variable:static
Value of the variable:weduoo.com
*******************
Variable Name:demoString2
datatype of the Variable:class java.lang.String
Access Modifiers of the Variable:static
Value of the variable:learn reflection API
*******************
```

##  Get Metadata of Method

- `getMethods()` method returns metadata of the public methods from the specified class as well as from its super class.

- `getDeclaredMethods()` method returns metadata of the all the methods from the specified class only.

- `getName()` get the name of the method

- `getReturnType()` Get the return type of the method 

-  `getModifiers() 、 Modifiers.toString(int i)` Get access modifiers of the methods

- `getParameterTypes()` Get method parameter types which returns a class array.

- `getExceptionTypes()` Get thrown exception which returns a class array

```java
import java.sql.SQLException;

public class DemoMethodMetadata {
    public void demoAdd(int firstElement, int secondElement , String result)
            throws ClassNotFoundException, ClassCastException{
        System.out.println("Demo method for Reflextion  API");
    }
    public String demoSearch(String searchString)
            throws ArithmeticException, InterruptedException{
        System.out.println("Demo method for Reflection API");
        return null;
    }
    public void demoDelete(String deleteString)
            throws SQLException{
        System.out.println("Demo method for Reflection API");
    }
}
```

```java
public class DemoMethodMetadataTest {
    public static void main (String[] args) {
        // Create Class object for Guru99Method MetaData.class
        Class classObj = DemoMethodMetadata.class;

        // Get the metadata or information of all the methods of the class using getDeclaredMethods()
        Method[] demoMethods = classObj.getDeclaredMethods();

        for(Method method : demoMethods) {
            // Print the method names
            System.out.println("Name of the method : "+ method.getName());

            // Print return type of the methods
            System.out.println("Return type of the method : "+ method.getReturnType());

            //Get the access modifier list and print
            int modifier = method.getModifiers();
            System.out.println("Method access modifiers : "+ Modifier.toString(modifier));

            // Get and print parameters of the methods
            Class[] paramList= method.getParameterTypes();
            System.out.print ("Method parameter types : ");
            for (Class class1 : paramList){
                System.out.println(class1.getName()+" ");
            }
            System.out.println();

            // Get and print exception thrown by the method
            Class[] exceptionList = method.getExceptionTypes();
            System.out.print("Excpetion thrown by method :");
            for (Class class1 : exceptionList) {
                System.out.println (class1.getName() +" ");
            }
            System.out.println("* * * * * * * * * * * * * * * * * * * ");
        }
    }
}
```

```
com.weduoo.reflect.DemoMethodMetadataTest
Name of the method : demoAdd
Return type of the method : void
Method access modifiers : public
Method parameter types : int 
int 
java.lang.String 

Excpetion thrown by method :java.lang.ClassNotFoundException 
java.lang.ClassCastException 
* * * * * * * * * * * * * * * * * * * 
Name of the method : demoSearch
Return type of the method : class java.lang.String
Method access modifiers : public
Method parameter types : java.lang.String 

Excpetion thrown by method :java.lang.ArithmeticException 
java.lang.InterruptedException 
* * * * * * * * * * * * * * * * * * * 
Name of the method : demoDelete
Return type of the method : void
Method access modifiers : public
Method parameter types : java.lang.String 

Excpetion thrown by method :java.sql.SQLException 
* * * * * * * * * * * * * * * * * * * 
```

## Get Metadata of Constructors

- `getConstructors()` Get all the constructor information in the Constructor array

   

```java
public class DemoConstructor {
    public DemoConstructor(int no) throws ClassCastException ,ArithmeticException{  }
    public DemoConstructor(int no, String name) throws RemoteException, SQLException {  }
    public DemoConstructor(int no, String name, String address) throws InterruptedException{  }
}

public class DemoConstructorTest {
    public static void main (String[] args) {
        // Create Class object for demoConstructor.class
        Class demoClass = DemoConstructor.class;

        // Get all the constructor information in the Constructor array
        Constructor[] demoConstructorList = demoClass.getConstructors();

        for (Constructor constructor : demoConstructorList) {
            // Print all name of each constructor
            System.out.println("Constrcutor name : "+ constructor.getName());

            //Get and print access modifiers of each constructor
            int demoModifiers= constructor.getModifiers();
            System.out.println("Constrctor modifier : "+ Modifier.toString(demoModifiers));

            // Get and print parameter types
            Class[] demoParamList=constructor.getParameterTypes();
            System.out.print ("Constrctor parameter types :");
            for (Class class1 : demoParamList) {
                System.out.println(class1.getName() +" ");
            }

            // Get and print exception thrown by constructors
            Class[] demoExceptionList=constructor.getExceptionTypes();
            System.out.println("Exception thrown by constructors :");
            for (Class class1 : demoExceptionList) {
                System.out.println(class1.getName() +" ");
            }
            System.out.println("************************");
        }
    }
}
```

```
com.weduoo.reflect.DemoConstructorTest
Constrcutor name : com.weduoo.reflect.DemoConstructor
Constrctor modifier : public
Constrctor parameter types :int 
Exception thrown by constructors :
java.lang.ClassCastException 
java.lang.ArithmeticException 
************************
Constrcutor name : com.weduoo.reflect.DemoConstructor
Constrctor modifier : public
Constrctor parameter types :int 
java.lang.String 
Exception thrown by constructors :
java.rmi.RemoteException 
java.sql.SQLException 
************************
Constrcutor name : com.weduoo.reflect.DemoConstructor
Constrctor modifier : public
Constrctor parameter types :int 
java.lang.String 
java.lang.String 
Exception thrown by constructors :
java.lang.InterruptedException 
************************
```

## Dynamic Proxies

Using Java Reflection you create dynamic implementations of interfaces at runtime. You do so using the class `java.lang.reflect.Proxy`. Dynamic proxies are known to be used for at least the following purposes:

- Database Connection and Transaction Management
- Dynamic Mock Objects for Unit Testing
- Adaptation of DI Container to Custom Factory Interfaces
- AOP-like Method Interception

You create dynamic proxies using the `Proxy.newProxyInstance()` method. The `newProxyInstance()` methods takes 3 parameters:

1. The `ClassLoader` that is to "load" the dynamic proxy class.
2. An array of interfaces to implement.
3. An `InvocationHandler` to forward all methods calls on the proxy to.

```java
InvocationHandler handler = new MyInvocationHandler();
MyInterface proxy = (MyInterface) Proxy.newProxyInstance(
                            MyInterface.class.getClassLoader(),
                            new Class[] { MyInterface.class },
                            handler);

public class MyInvocationHandler implements InvocationHandler{
  public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    //do something "dynamic"
  }
}
```



# Proxy

[Java三种代理模式：静态代理、动态代理和cglib代理](https://segmentfault.com/a/1190000011291179)

[Java代理和动态代理机制分析和应用](https://github.com/crazycodeboy/Java_Advanced/tree/master/Java_Proxy_Test)

[Java的静态代理和动态代理]([http://1csh1.github.io/2017/02/20/Java%E7%9A%84%E9%9D%99%E6%80%81%E4%BB%A3%E7%90%86%E5%92%8C%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86/](http://1csh1.github.io/2017/02/20/Java的静态代理和动态代理/))



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







