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

## Why should I care?

Factory design pattern is used when we have a super class with multiple sub-classes and based on input, we need to return one of the sub-class. This pattern take out the responsibility of instantiation of a class from client program to the factory class.

```java
public void getFirst(List<String> input) {
   if(input != null && !input.isEmpty())
       return input.get(0);
   return null;
}
```

Here we accept a List of String objects — the implementation does not matter. And List is an interface. We could naturally use `ArrayList` as expected parameter but this would limit some calls to our method. For example, someone has a `LinkedList` (which is a different implementation of List) and in this case, he/she has to convert all the elements into an ArrayList to call our method — so using `List` makes it more usable for others.

## Factory Design Pattern Super Class

Super class in factory design pattern can be an interface, **abstract class** or a normal java class. For our factory design pattern example, we have abstract super class with [overridden](https://www.journaldev.com/817/java-override-method-overriding)`toString()` method for testing purpose.

