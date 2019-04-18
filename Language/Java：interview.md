# Base

## 1. How to create a immutable object in Java? Count all benefits?

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

## **Is Java Pass by Reference or Pass by Value?**

The Java Spec says that ***everything in Java is pass-by-value***. There is no such thing as “*pass-by-reference*” in Java. These terms are associated with method calling and passing variables as method parameters. Well, primitive types are always pass by value without any confusion. But, the concept should be understood in context of method parameter of complex types.

In java, when we pass a reference of complex types as any method parameters, always the memory address is copied to new reference variable bit by bit. See in below picture:

![pass-by-value](https://howtodoinjava.files.wordpress.com/2013/03/pass-by-value.jpg)

In above example, address bits of first instance are copied to another reference variable, thus resulting both references to point a single memory location where actual object is stored. Remember, making another reference to null will not make first reference also null. But, changing state from either reference variable have impact seen in other reference also.

> Read in detail here: [Java Pass by Value or Reference?](<https://howtodoinjava.com/java/basics/java-is-pass-by-value-lets-see-how/>)

## 2. What is the use of the finally block? Is finally block in Java guaranteed to be called? When finally block is NOT called?

The `finally` block always executes when the `try` block exits. This ensures that the `finally` block is executed even if an unexpected exception occurs. But `finally` is useful for more than just exception handling — it allows having cleanup code accidentally bypassed by a `return`, `continue`, or `break`. Putting cleanup code in a `finally` block is always a good practice, even when no exceptions are anticipated.

If the JVM exits while the `try` or `catch` code is being executed, then the `finally` block may not execute. Likewise, if the thread executing the `try` or `catch` code is interrupted or killed, the `finally` block may not execute even though the application as a whole continues.

## 3. Why there are two Date classes; one in java.util package and another in java.sql?

A `java.util.Date` represents date and time of day, a `java.sql.Date` only represents a date. The complement of `java.sql.Date` is `java.sql.Time`, which only represents a time of day.
The `java.sql.Date` is a subclass (an extension) of `java.util.Date`. So, what changed in `java.sql.Date`:

– `toString()` generates a different string representation: **yyyy-mm-dd**
– a `static valueOf(String)` methods to create a date from a string with above representation
– the getters and setter for hours, minutes and seconds are deprecated

The `java.sql.Date` class is used with JDBC and it was intended to not have a time part, that is, hours, minutes, seconds, and milliseconds should be zero… but this is not enforced by the class.

![image](https://wx2.sinaimg.cn/large/69d4185bly1g26mhxqqbyj20at09it9c.jpg)

## 4. Explain marker interfaces?

The marker interface pattern is a design pattern in computer science, used with languages that **provide run-time type information about objects**. It provides **a means to associate metadata with a class where the language does not have explicit support for such metadata.** In java, it is used as interfaces with no method specified.

A good example of use of marker interface in java is [Serializable](https://howtodoinjava.com/java/serialization/a-mini-guide-for-implementing-serializable-interface-in-java/) interface. A class implements this interface to indicate that its non-transient data members can be written to a byte steam or file system.

A *major problem* with marker interfaces is that an interface defines a contract for implementing classes, and that contract is inherited by all subclasses. This means that **you cannot “un-implement” a marker**. In the example given, if you create a subclass that you do not want to serialize (perhaps because it depends on transient state), you must resort to explicitly throwing NotSerializableException.



## 5. Why main() in java is declared as public static void?

**Why public?** main method is `public` so that it can be accessible everywhere and to every object which may desire to use it for launching the application. Here, i am not saying that JDK/JRE had similar reasons because java.exe or javaw.exe (for windows) use Java Native Interface (JNI) calls to invoke method, so they can have invoked it either way irrespective of any access modifier.

**Why static?** Lets suppose we do not have main method as `static`. Now, to invoke any method you need an instance of it. Right? Java can have overloaded constructors, we all know. Now, which one should be used and from where the parameters for overloaded constructors will come.

**Why void?** Then there is no use of returning any value to JVM, who actually invokes this method. The only thing application would like to communicate to invoking process is: normal or abnormal termination. This is already possible using `System.exit(int)`. A non-zero value means abnormal termination otherwise everything was fine.

## 6. What is the difference between creating String as new() and literal?

When we create `String` with `new()` it’s created in heap and also added into string pool, while `String` created using literal are created in String pool only which exists in Perm area of heap.

Well you really need to know the concept of string pool very deeply to answer this question or similar questions. My advise.. “Study Hard” about [string class and string pool](https://howtodoinjava.com/java/string/interview-stuff-about-string-class-in-java/).



## 7. Difference between interfaces and abstract classes?

This is very common question if you are appearing interview for junior level programmer. Well, most noticeable differences are as below:

- Variables declared in a Java interface is by default `final`. An abstract class may contain non-final variables.
- Java interface are implicitly `abstract` and cannot have implementations. A Java abstract class can have instance methods that implements a default behavior.
- Members of a Java interface are public by default. A Java abstract class can have the usual flavors of class members like `private`, `abstract`.
- Java interface should be implemented using keyword “**implements**“; A Java abstract class should be extended using keyword “**extends**“.
- A Java class can implement multiple interfaces but it can extend only one abstract class.
- Interface is ~~absolutely abstract and~~ cannot be instantiated; A Java abstract class also cannot be instantiated, but can be invoked if a main() exists. Since Java 8, you can define [**default methods in interfaces**](https://howtodoinjava.com/java8/default-methods-in-java-8/).
- Abstract class are slightly faster than interface because interface involves a search before calling any overridden method in Java. This is not a significant difference in most of cases but if you are writing a time critical application than you may not want to leave any stone unturned.

## 8. When do you override hashCode() and equals()?

`hashCode()` and `equals()` methods have been defined in `Object` class which is parent class for java objects. For this reason, all java objects inherit a default implementation of these methods.

`hashCode()` method is used to get a unique integer for given object. This integer is used for determining the bucket location, when this object needs to be stored in some `HashTable` like data structure. By default, Object’s `hashCode()` method returns and integer representation of memory address where object is stored.
`equals()` method, as name suggest, is used to simply verify the equality of two objects. Default implementation simply check the object references of two objects to verify their equality.

Note that it is generally necessary to override the hashCode method whenever this method is overridden, so as to maintain the general contract for the `hashCode()` method, which states that equal objects must have equal hash codes.

- `equals()` must define an equality relation (it must be **reflexive, symmetric and transitive**). In addition, it must be consistent (if the objects are not modified, then it must keep returning the same value). Furthermore, `o.equals(null)` must always return `false`.
- `hashCode()` must also be consistent (if the object is not modified in terms of `equals()`, it must keep returning the same value).

The relation between the two methods is:

Whenever `a.equals(b)` then `a.hashCode()` must be same as `b.hashCode()`.

## 9. Why finalize() method should be avoided?

We all know the basic statement that `finalize()` method is called by garbage collector thread before reclaiming the memory allocated to the object. See [this program](https://howtodoinjava.com/java/related-concepts/why-not-to-use-finalize-method-in-java/) which prove that `finalize()` invocation is not guaranteed at all. Other reasons can be:

1. `finalize()` methods do not work in chaining like constructors. It means like when you call a constructor then constructors of all super classes will be invokes implicitly. But, in case of finalize methods, this is not followed. Super class’s finalize() should be called explicitly.

2. Any Exception thrown by finalize method is ignored by GC thread and it will not be propagated further, in fact it will not be logged in your log files. So bad, isn’t it?

3. Also, There is some performance penalty when finalize() in included in your class. In Effective java (2nd edition ) Joshua bloch says,

   “Oh, and one more thing: there is a severe performance penalty for using finalizers. On my machine, the time to create and destroy a simple object is about 5.6 ns. Adding a finalizer increases the time to 2,400 ns. In other words, it is about 430 times slower to create and destroy objects with finalizers.”

## Explain abstraction and encapsulation? How are they related?

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





# Data Structure



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





## Map Questions

### 1. Explain the working of HashMap. How duplicate collision is resolved?

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

### 2. Why HashMap should not be used in multithreaded environment? Can it cause infinite loop as well?

We know that `HashMap` is non-synchronized collection where as its synchronized counter-part is `HashTable`. So, when you are accessing the collection in multithreaded environment and all threads are accessing a single instance of collection, then its safer to use `HashTable` for various obvious reasons e.g. to avoid dirty reads and to maintain data consistency. In worst case, this mutithreaded environment can result in infinite loop as well.

Yes, it is true. `HashMap.get()` can cause an infinite loop. Lets see how??

If you look at the source code [HashMap.get(Object key)](http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/6-b14/java/util/HashMap.java#HashMap.get%28java.lang.Object%29) method, it looks like this:

```java
`public` `Object get(Object key) {``    ``Object k = maskNull(key);``    ``int` `hash = hash(k);``    ``int` `i = indexFor(hash, table.length);``    ``Entry e = table[i];``    ``while` `(``true``) {``        ``if` `(e == ``null``)``            ``return` `e;``        ``if` `(e.hash == hash &amp;&amp; eq(k, e.key))``            ``return` `e.value;``        ``e = e.next;``    ``}``}`
```

`while(true){...}` can always be a victim of infinite loop at runtime in multithreaded environment, IF, somehow `e.next` can point to itself. This will result in infinite loop. But, how `e.next` will point to itself (i.e.).

This can happen in `void transfer(Entry[] newTable)` method, which is invoked at time the `HashMap` resizing is done.

```java
`do` `{``    ``Entry next = e.next;``    ``int` `i = indexFor(e.hash, newCapacity);``    ``e.next = newTable[i];``    ``newTable[i] = e;``    ``e = next;``} ``while` `(e != ``null``);`
```

This piece of code is prone to produce above condition, if resizing happen and at the same time other threads tried to modify the map instance.

Only way to avoid this scenario is to use synchronization in code, or better, use synchronized collection.



# Thread





# I/O