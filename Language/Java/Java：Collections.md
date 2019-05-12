# Java Collections

- [Java提高篇（三四）-----fail-fast机制](<https://blog.csdn.net/chenssy/article/details/38151189>)

## Java Collections Hierarchy

![Java Collections Hierarchy](https://cdn2.howtodoinjava.com/wp-content/uploads/2018/11/Java-collections-interfaces.gif)

1. Collection

**Collection interface** is at the root of the hierarchy. Collection interface provides all general purpose methods which all collections classes must support (or throw `UnsupportedOperationException`). It **extends Iterable** interface which adds support for iterating over collection elements using the “[**for-each loop**](https://howtodoinjava.com/java/basics/enhanced-for-each-loop-in-java/)” statement.

All other collection interfaces and classes (except Map) either extend or implement this interface. For example, List *(indexed, ordered)* and Set *(sorted)* interfaces implement this collection.

2. List

**Lists** represents an **ordered** collection of elements. Using lists, we can access elements by their integer index (position in the list), and search for elements in the list. index start with `0`, just like an array.

3. Set

**Sets** represents a collection of **sorted** elements. Sets do not allow the duplicate elements. Set interface does not provides no guarantee to return the elements in any predictable order; though some Set implementations store elements in their [natural ordering](https://howtodoinjava.com/java/collections/java-comparable-interface/) and guarantee this order.

4. Map

The **Map** interface enable us to store data in *key-value pairs* (keys should be immutable). A map cannot contain duplicate keys; each key can map to at most one value.

The Map interface provides three collection views, which allow a map’s contents to be viewed as a set of keys, collection of values, or set of key-value mappings. Some map implementations, like the TreeMap class, make specific guarantees as to their order; others, like the HashMap class, do not.

5. Stack

The Java **Stack** interface represents a classical stack data structure, where elements can be pushed to last-in-first-out (LIFO) stack of objects. In Stack we push an element to the top of the stack, and popped off from the top of the stack again later.

6. Queue

A queue data structure is intended to hold the elements (put by producer threads) prior to processing by consumer thread(s). Besides basic Collection operations, queues provide additional insertion, extraction, and inspection operations.

Queues typically, but do not necessarily, order elements in a FIFO (first-in-first-out) manner. One such exception is priority queue which order elements according to a supplied [Comparator](https://howtodoinjava.com/java/collections/java-comparator/), or the elements’ natural ordering.

In general, queues do not support blocking insertion or retrieval operations. Blocking queue implementations classes implement **BlockingQueue** interface.

Some useful classes which implement `Map` interface are – ArrayBlockingQueue, ArrayDeque, ConcurrentLinkedDeque, ConcurrentLinkedQueue, DelayQueue, LinkedBlockingDeque, LinkedBlockingQueue, LinkedList, LinkedTransferQueue, PriorityBlockingQueue, PriorityQueue and SynchronousQueue.

7. Deque

A double ended queue (pronounced “**deck**“) that supports element insertion and removal at both ends. When a deque is used as a queue, [FIFO (First-In-First-Out)](https://en.wikipedia.org/wiki/FIFO_(computing_and_electronics)) behavior results. When a deque is used as a stack, LIFO (Last-In-First-Out) behavior results.

This interface should be used in preference to the legacy Stack class. When a deque is used as a stack, elements are pushed and popped from the beginning of the deque.

Some common known classes implementing this interface are ArrayDeque, ConcurrentLinkedDeque, LinkedBlockingDeque and LinkedList.



By purpose, [generics](https://howtodoinjava.com/java/generics/complete-java-generics-tutorial/) provide type safety. It detects the incompatible types (in method arguments) and prevent **ClassCastException** in runtime. In Java collections as well, we can define a collection class to store only a certain type of objects. All other types should be disallowed. This is done via generics.

## Benefits of Java Collections

- Consistent and reusable APIs – This is any framework does. It provides a consistent set of classes methods which can be used to solve a similar set of problems over and over, without getting unpredictable results. Java collections framework also helps in solving common problems related to a group of objects – in consistent manner.

  All collection classes have consistent implementation and provide some common methods like add, get, put, remove etc. No matter what kind of data structure you are dealing with, these methods work according to underlying implementation and perform actions transparently.

- **Less development time** – A common and predictable framework always decreases the development time and helps in writing application program in speedy manner. Java collection also helps in performing some most repeated common tasks with objects and collections and thus improve time factor.

- **Performance** – The Java collection APIs are written by some most brilliant minds of industry and their performance is top notch in most of the scenarios. Ongoing development work by Oracle and very enthusiastic Java developer community helps in making it better.

- Clean code – These APIs have been written with all good coding practices and documented very well. They follow a certain standard across whole Java collection framework. It makes the programmer code look good and clean.

  The code is easier to read as well because of consistent class and method names.

# Array(List)

An **array** is a container object that holds a **fixed number of values** of a **single type** in a **contiguous memory location**. It is a data structure which is used to store finite number of elements and all elements must be of similar data type.

![Array in memory](https://cdn2.howtodoinjava.com/wp-content/uploads/2018/10/Array-in-memory.png)

## Array Features

- Arrays are also a subtype of `Object` in Java.
- Arrays are objects so we can find the length of the array using attribute `'length'`.
- Java array are types. we can declare the [variables](https://howtodoinjava.com/java/basics/java-variables/) of array type.
- Arrays are ordered and each have an index beginning from `'0'` for the first element.
- Arrays can store primitives as well as objects. But all must be of a single type in one array instance.
- Just like other variables, arrays can also be `static`, `final` or used as method arguments.
- The size of an array must be specified by an `int` value.
- Java arrays are `Cloneable` and `Serializable`.



## Common Methods

- `Arrays.toString()`
- `Arrays.deepToString()`
- `System.arrayCopy()`
- `Arrays.copyOf()`
- `Arrays.copyOfRange()`
- `object.clone()` : shadow copy
- `SerializationUtils.clone(object)`：deep copy (org.apache.commons.lang3.SerializationUtils)， the object must be serializeable.

# ArrayList

[Java Collection Framework : List](https://blog.csdn.net/justloveyou_/article/details/52955619)



Java ArrayList class extends `AbstractList` class which implements `List` interface. The List interface extends `Collection` and `Iterable` interfaces in hierarchical order.

![ArrayList Hierarchy](https://cdn1.howtodoinjava.com/wp-content/uploads/2018/10/ArrayList.jpg)

![image](https://ws2.sinaimg.cn/large/69d4185bly1g2to9z6z22j20kc09nq39.jpg)

## ArrayList Features

ArrayList has following features –

1. **Ordered** – Elements in arraylist preserve their ordering which is by default the order in which they were added to the list.
2. **Index based** – Elements can be randomly accessed using index positions. Index start with `'0'`.
3. **Dynamic resizing** – ArrayList grows dynamically when more elements needs to be added than it’s current size.
4. **Non synchronized** – ArrayList is not synchronized, by default. Programmer needs to use `synchronized` keyword appropiately or simply use **Vector** class.
5. **Duplicates allowed** – We can add duplicate elements in arraylist. It is not possible in sets.



## Common method

- `add()` ：**ArrayList add() method** is used to add an element in the list. We can add elements of any type in arraylist, but make program behave in more predicatable manner, we should add elements of one certain type only in any goven list instance.

  ```java
  //ArrayList without generics, not recommended
  ArrayList ages = new ArrayList();
   
  ages.add("1");
  ages.add("2");
  ages.add(3);
  ages.add(new Long(4l));
  ```

- `addAll()`: **ArrayList addAll() method** is used to append all of the elements of argument collection to the list at the end. The order of appended elements is that they are returned by the argument collection’s Iterator.

- `clear()`: **ArrayList clear()** method is used to removes all of the elements from the list. The list will be empty after this call returns. 

  this method does simple thing. It iterates the backing array inside arraylist and assign all elements `'null'` value and set the `size` attribute to `'0'`.

- `clone()`:**ArrayList clone()** method is used to create a **shallow copy** of the list. In the new list, only object references are copied.

  If we change the object state inside first arraylist, then changed object state will be reflected in cloned arraylist as well.

- `contains()`:**ArrayList contains()** method is used to **check if the specified element exists in the given arraylist** or not. If element exist then method returns `true`, else `false`.

- `ensureCapacity()`: To make sure whether there is any space left in array or not. Generally initial size should be given in ArrayList construtor like new ArrayList(5). But if we do not pass any size, the default size is used which is 10.

- `get()`:**ArrayList.get(int index)** method returns the element at the specified position `'index'` in the list.

- `indexOf()`:This method returns the index of the first occurrence of the specified element in this list. It will return `'-1'` if the list does not contain the element.

- `lastIndexOf()`:This method returns the index of the last occurrence of the specified element in this list. It will return `'-1'` if the list does not contain the element.

- `listIterator()`: **ArrayList listIterator()** returns a list iterator over the elements in this list. It is a **bi-directional**iterator which is **fail-fast** in nature. By default, elements returned by the list iterator are in proper sequence. 

  - **ListIterator listIterator()** – Returns a list iterator over the elements in this list.
  - **ListIterator listIterator(int index)** – Returns a list iterator over the elements in this list (in proper sequence), starting at the specified position in the list. The specified `index`indicates the first element that would be returned by an initial call to next. An initial call to previous would return the element with the specified `index` minus one.

- `remove()`:**ArrayList remove()** removes the first occurrence of the specified element from this list, if it is present. If the list does not contain the element, list remain unchanged.

- `removeAll()`:**ArrayList removeAll()** removes all of matching elements that are contained in the specified method argument collection. It removes all occurrences of matching elements, not only first occurrence.

- `removeIf()`:**ArrayList removeIf()** iterate the list and removes all of the elements of this list that satisfy the given predicate.

- `retainAll():` **ArrayList retainAll()** retains only the elements in this list that are contained in the specified method argument collection. Rest all elements are removed from the list. This method is exact opposite to [removeAll()](https://howtodoinjava.com/java/collections/arraylist/arraylist-removeall/) method.

- `sort()`:**ArrayList sort()** method sorts the list according to the order induced by the specified **Comparator** instance. All elements in the list must must be mutually comparable.

- `spliterator()`:**ArrayList spliterator()** returns the instance of **Spliterator** which is *last-binding* and *fail-fast*. Java Spliterator is used for traversing and partitioning elements of a source such as `array`, `Set`, `List` or IO channel.







## ArrayList clear vs new

An empty arraylist has zero elements. A new arraylist also has zero elements. But there is differenece between them.

The difference between an empty and a new arraylist is the **size of backing array**. As clear() method does not resize the backing array, so after clear method you may have a list which has backing array of a larger size (if list was pretty big before clear() method was called).

Except above difference in capacity, there is no difference between both kind of lists.

## Differences between Iterator and ListIterator

| ITERATOR                                        | LISTITERATOR                                             |
| ----------------------------------------------- | -------------------------------------------------------- |
| Can be used to iterate all collection classes.  | Can be used to iterate only `List` implemented classes.  |
| Supports only forward direction only iteration. | Supports both forward and backward direction iterations. |
| Supports only READ and DELETE operations.       | Supports all CRUD operations.                            |
| Obtained through `iterator()` method.           | Obtained through `listIterator()` method.                |

## Differences between Iterator and Spliterator

| ITERATOR                                       | SPLITERATOR                                                  |
| ---------------------------------------------- | ------------------------------------------------------------ |
| Since Java 1.2.                                | Since Java 8.                                                |
| Can be used to iterate all collection classes. | Can be used to iterate array, stream, list and set. Not possible with map. |
| Does not support parallel processing.          | Supports parallel processing.                                |



# LinkedList

[Java集合---LinkedList源码解析](http://www.cnblogs.com/ITtangtang/p/3948610.html)

**Java LinkedList** class is doubly-linked list implementation of the `List` and `Deque` interfaces. It implements all optional list operations, and permits all elements (including null).

```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable {
    //implementation
}
```

![LinkedList Hierarchy](https://cdn2.howtodoinjava.com/wp-content/uploads/2018/10/LinkedList-Hierarchy.png)

![image](https://ws2.sinaimg.cn/large/69d4185bly1g2tq7fr464j20hk0br74r.jpg)

## LinkedList Features

- **Doubly linked list** implementation which implements List and Deque interfaces. Therefore, It can also be used as a Queue, Deque or Stack.
- Permits all elements including duplicates and NULL.
- LinkedList maintains the **insertion order** of the elements.
- It is **not synchronized**. If multiple threads access a linked list concurrently, and at least one of the threads modifies the list structurally, it *must* be synchronized externally.
- Use Collections.synchronizedList(new LinkedList()) to get synchronized linkedlist.
- The iterators returned by this class are fail-fast and may throw `ConcurrentModificationException`.
- It does not implement [RandomAccess](https://docs.oracle.com/javase/7/docs/api/java/util/RandomAccess.html) interface. So we can access elements in sequential order only. It does not support accessing elements randomly.
- We can use [ListIterator](https://docs.oracle.com/javase/7/docs/api/java/util/ListIterator.html) to iterate LinkedList elements.

## Common Methods

1. **boolean add(Object o)** : appends the specified element to the end of a list.
2. **void add(int index, Object element)** : inserts the specified element at the specified position index in a list.
3. **void addFirst(Object o)** : inserts the given element at the beginning of a list.
4. **void addLast(Object o)** : appends the given element to the end of a list.
5. **int size()** : returns the number of elements in a list
6. **boolean contains(Object o)** : return `true` if the list contains a specified element, else `false`.
7. **boolean remove(Object o)** : removes the first occurence of the specified element in a list.
8. **Object getFirst()** : returns the first element in a list.
9. **Object getLast()** : returns the last element in a list.
10. **int indexOf(Object o)** : returns the index in a list of the first occurrence of the specified element, or -1 if the list does not contain specified element.
11. **lastIndexOf(Object o)** : returns the index in a list of the last occurrence of the specified element, or -1 if the list does not contain specified element.
12. **Iterator iterator()** : returns an iterator over the elements in this list in proper sequence.
13. **Object[] toArray()** : returns an array containing all of the elements in this list in proper sequence.
14. **List subList(int fromIndex, int toIndex)** : returns a view of the portion of this list between the specified fromIndex (inclusive) and toIndex (exclusive).

Java example to sort a LinkedList using **Collections.sort()** method. Please note that for custom sorting of objects, we can use **Collections.sort(linkedList, comparator)** method.

## Performance

In Java LinkedList class, manipulation is fast because no shifting needs to be occurred. So essentially, all add and remove method provide very good performance **O(1)**.

- add(E element) method is of O(1).
- get(int index) and add(int index, E element) methods are of O(n).
- remove(int index) method is of O(n).
- Iterator.remove() is O(1).
- ListIterator.add(E element) is O(1).

LinkedList should be preferred there are no large number of random access of element while there are a large number of add/remove operations.

# HashMap

[Map 综述（一）：彻头彻尾理解 HashMap](https://blog.csdn.net/justloveyou_/article/details/62893086)

[Java HashMap工作原理及实现]([https://yikun.github.io/2015/04/01/Java-HashMap%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86%E5%8F%8A%E5%AE%9E%E7%8E%B0/](https://yikun.github.io/2015/04/01/Java-HashMap工作原理及实现/))



**HashMap in Java** in a collection class which implements **Map** interface. It is used to store **key & value** pairs. Each key is mapped to a single value in the map.

Keys are unique. It means we can insert a key ‘K’ only once in a map. Duplicate keys are not allowed. Though a value `'V'` can be mapped to multiple keys.

```java
public class HashMap<K,V> extends AbstractMap<K,V>
                implements Map<K,V>, Cloneable, Serializable 
```

![HashMap Hierarchy](https://cdn1.howtodoinjava.com/wp-content/uploads/2018/10/HashMap-Hierarchy.png)

The key-value pairs are stored as instance of inner class `HashMap.Entry` which has key and value mapping stored as attributes. key has been marked as `final`.

All instances of Entry class are stored in an array declard as `'transient Entry[] table'`. For each key-value to be stored in HashMap, a hash value is calculated using the key’s hash code. This hash value is used to calculate the **index** in the array for storing Entry object.

In case of **collision**, where multiple keys are mapped to single index location, a **linked list** of formed to store all such key-value pairs which should go in single array index location.

While retrieving the value by key, first index location is found using key’s hashcode. Then all elements are iterated in the linkedlist and correct value object is found by identifying the correct key using it’s **equals()** method.

## Features

- HashMap cannot contain duplicate keys.
- HashMap allows multiple `null` values but only one `null` key.
- HashMap is an **unordered collection**. It does not guarantee any specific order of the elements.
- HashMap is **not thread-safe**. You must explicitly synchronize concurrent modifications to the HashMap. Or you can use **Collections.synchronizedMap(hashMap)** to get the synchronized version of HashMap.
- A value can be retrieved only using the associated key.
- HashMap stores only object references. So primitives must be used with their corresponding wrapper classes. Such as `int` will be stored as `Integer`.
- HashMap implements **Cloneable** and **Serializable** interfaces.

## Methods

List of methods in HashMap class and their short description.

1. **void clear()** : removes all the key-value pairs from the HashMap.
2. **Object clone()** : returns a shallow copy of the specified HashMap.
3. **boolean containsKey(Object key)** : returns `true` or `false` based on whether the specified key is found in the map or not.
4. **boolean containsValue(Object Value)** : Similar to containsKey() method, it looks for the specified value instead of key.
5. **Object get(Object key)** : returns the value for the specified key in the HashMap.
6. **boolean isEmpty()** : checks whether the map is empty.
7. **Set keySet()** : returns the **Set** of the all keys stored in the HashMap.
8. **Object put(Key k, Value v)** : Inserts key-value pair into the HashMap.
9. **int size()** : returns the size of the map which is equal to the number of key-value pairs stored in the HashMap.
10. **Collection values()** : returns a collection of all values in the map.
11. **Value remove(Object key)** : removes the key-value pair for the specified key.
12. **void putAll(Map m)** : copies all the elements of a map to the another specified map.

Please note that iterators of this class are **fail-fast** and if any structure modification is done after creation of iterator, it will throw `ConcurrentModificationException`.

## How HashMap works

If anybody asks me to describe “**How HashMap works?**“, I simply answer: “**On principle of Hashing**“. 

[Hashing](https://en.wikipedia.org/wiki/Hash_function) in its simplest form, is a way to assigning a unique code for any variable/object after applying any formula/algorithm on its properties. A true [hash function](https://howtodoinjava.com/security/how-to-generate-secure-password-hash-md5-sha-pbkdf2-bcrypt-examples/) must follow this rule –

“Hash function should **return the same hash code each and every time** when the function is applied on same or equal objects. In other words, two equal objects must produce the same hash code consistently.”

A map by definition is : **“An object that maps keys to values”**. Very easy.. right?

So, there must be some mechanism in `HashMap` to store this key-value pair. The answer is YES. `HashMap` has an inner class `Entry`, which looks like this:(In 1.8 JDK use Node)

```java
static class Entry<K ,V> implements Map.Entry<K, V>{
    final K key;
    V value;
    Entry<K ,V> next;
    final int hash;
    ...//More code goes here
}
```

Surely `Entry` class has `key` and `value` mapping stored as attributes. `key` has been marked as `final` and two more fields are there: `next` and `hash`.

### Put Method

Now look at code implementation of `put()` method:

```java
public V put(K key, V value) {
    if (key == null)
    return putForNullKey(value);
    int hash = hash(key.hashCode());
    int i = indexFor(hash, table.length);
    for (Entry<K , V> e = table[i]; e != null; e = e.next) {
        Object k;
        if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
            V oldValue = e.value;
            e.value = value;
            e.recordAccess(this);
            return oldValue;
        }
    }
 
    modCount++;
    addEntry(hash, key, value, i);
    return null;
}
```

Let’s note down the internal working of put method in hashmap.

1. First of all, the `key` object is checked for `null`. If the `key` is `null`, the `value` is stored in `table[0]` position. Because hashcode for `null` is always 0.
2. Then on next step, a hash value is calculated using the key’s hash code by calling its `hashCode()` method. This hash value is used to calculate the index in the array for storing `Entry` object. JDK designers well assumed that there might be some poorly written `hashCode()` functions that can return very high or low hash code value. To solve this issue, they introduced another `hash()` function and passed the object’s hash code to this `hash()` function to bring hash value in the range of array index size.
3. Now `indexFor(hash, table.length)` function is called to calculate exact index position for storing the `Entry` object.

### Collision Resolve

Here comes the main part. Now, as we know that two unequal objects can have the same hash code value, how two different objects will be stored in same array location called **bucket**.

The answer is `LinkedList`. If you remember, `Entry` class had an attribute `"next"`. This attribute always points to the next object in the chain. This is exactly the behavior of `LinkedList`.

1. So, in case of collision, `Entry` objects are stored in linked list form. When an `Entry` object needs to be stored in particular index, HashMap checks whether there is already an entry?  If there is no entry already present, the entry object is stored in this location.

   If there is already an object sitting on calculated index, its `next` attribute is checked. If it is `null`, and current entry object becomes next node in linkedlist. If `next` variable is not `null`, procedure is followed until `next` is evaluated as `null`.

2. What if we add the another `value` object with same key as entered before. Logically, it should replace the old value. How it is done? Well, after determining the index position of `Entry` object, while iterating over linkedist on calculated index, `HashMap` calls equals method on `key` object for each entry object.

   All these entry objects in linkedlist will have similar hashcode but `equals()`method will test for true equality. If `key.equals(k)` will be true then both keys are treated as same `key` object. This will cause the replacing of `value` object inside entry object only.

   **In this way, HashMap ensure the uniqueness of keys**.

### Key Notes

1. Data structure to store entry objects is an array named `table` of type `Entry`.
2. A particular index location in array is referred as bucket, because it can hold the first element of a linkedlist of entry objects.
3. Key object’s `hashCode()` is required to calculate the index location of Entry object.
4. Key object’s `equals()` method is used to maintain uniqueness of keys in map.
5. Value object’s `hashCode()` and `equals()` method are not used in HashMap’s `get()` and `put()` methods.
6. Hash code for `null` keys is always zero, and such entry object is always stored in zero index in `Entry[]`.

### HashMap improvements in Java 8

As part of the work for [JEP 180](http://openjdk.java.net/jeps/180), there is a performance improvement for HashMap objects where there are lots of collisions in the keys by using balanced trees rather than linked lists to store map entries. The principal idea is that **once the number of items in a hash bucket grows beyond a certain threshold, that bucket will switch from using a linked list of entries to a balanced tree. In the case of high hash collisions, this will improve worst-case performance from O(n) to O(log n)**.

Basically when a bucket becomes too big (**currently: TREEIFY_THRESHOLD = 8**), HashMap dynamically replaces it with an ad-hoc implementation of the treemap. This way rather than having pessimistic O(n) we get much better O(log n).



## How to design good custom key object

Can we use an object as a key for a hashmap in java? This is a very [popular interview question](https://howtodoinjava.com/java-interview-questions/) indeed. It is asked immediately after “[How HashMap works](https://howtodoinjava.com/java/collections/how-hashmap-works-in-java/)?”. Lets make a reasoning around *user defined object as key in hashmap in java*.

The very basic need for designing a good key is that “***we should be able to retrieve the value object back from the map without failure***“, otherwise no matter how fancy data structure you build, it will be of no use.

Key’s hash code is used primarily in conjunction to its `equals()` method, for putting a key in map and then getting it back from map. So, our only focus point is these two methods. So if hash code of key object changes after we have put a key value pair in map, then its almost impossible to fetch the value object back from map. It is a case of memory leak.

On runtime, JVM computes hash code for each object and provide it on demand. When we modify an object’s state, JVM set a flag that object is modified and hash code must be AGAIN computed. So, next time you call object’s `hashCode()` method, JVM recalculate the hash code for that object.

For above basic reasoning, key objects are suggested to be **IMMUTABLE**. Immutability allows you to get same hash code every time, for a key object.

But remember that **immutability is recommended and not mandatory**. If you want to make a mutable object as key in hashmap, then you have to make sure that state change for key object does not change the hash code of object. This can be done by overriding the `hashCode()` method. But, you must make sure you are honoring the contract with `equals()` also.

## Synchronize HashMap

### ConcurrentHashMap

Our first choice should always be using the `ConcurrentHashMap` class if we wish to use a Map in concurrent environment. `ConcurrentHashMap` support concurrent access to it’s key-value pairs by design. We do not need to perform any additional code modifications to enable synchronization on the map.

Please note that **iterator** obtained from `ConcurrentHashMap` does not throw `ConcurrentModificationException`. However, iterators are designed to be used by only one thread at a time. It means each iterator we obtain from a ConcurrentHashMap is designed to be used by a single thread and should not be passed around.

If we do so then there is no guarantee that one thread will see the changes to the map that the other thread performs (without obtaining a new iterator from the map). The **iterator is guaranteed to reflect the state of the map at the time of it’s creation**.

```java
ConcurrentHashMap<Integer, String> concurrHashMap = new ConcurrentHashMap<>();
//Put require no synchronization
concurrHashMap.put(1, "A");
concurrHashMap.put(2, "B");
Iterator<Integer> itr = concurrHashMap.keySet().iterator();
//Using synchronized block is advisable
synchronized (concurrHashMap){
  while(itr.hasNext()) {
    System.out.println(concurrHashMap.get(itr.next()));
  }
}
```

### Collections.synchronizedMap()

**Synchronized HashMap** also works very similar to ConcurrentHashMap, with few differences.

`SynchronizedHashMap` is allows only one thread to perform read/write operations at a time because all of its methods are declared **synchronized**. `ConcurrentHashMap`allows multiple threads to work independently on different segments in the map. This allows higher degree of concurrency in ConcurrentHashMap and thus improve performance of the application in whole.

Iterators from both classes should be used inside `synchronized` block but the iterator from SynchronizedHashMap is **fail-fast**. ConcurrentHashMap iterators are not fail-fast.

```java
Map<Integer, String> syncHashMap = Collections.synchronizedMap(new HashMap<>());

//Put require no synchronization
syncHashMap.put(1, "A");
syncHashMap.put(2, "B");

Iterator<Integer> itr = syncHashMap.keySet().iterator();     
//Using synchronized block is advisable
synchronized (syncHashMap){
  while(itr.hasNext()) {
    System.out.println(syncHashMap.get(itr.next()));
  }
}
```



# HashTable

**Java Hashtable** class is an implementation of hash table data structure. It is very much similar to [HashMap](https://howtodoinjava.com/java-hashmap/) in Java, with most significant difference that Hashtable is **synchronized** while HashMap is not.

Hashtable internally contains buckets in which it stores the key/value pairs. The Hashtable uses the key’s [hashcode](https://howtodoinjava.com/java/basics/java-hashcode-equals-methods/) to determine to which bucket the key/value pair should map.

![Java Hashtable](https://cdn1.howtodoinjava.com/wp-content/uploads/2018/10/hashtable.gif)

The function to get bucket location from Key’s hashcode is called **hash function**. In theory, a hash function is a function which when given a key, generates an address in the table. A hash function always returns a number for an object. Two equal objects will always have the same number while two unequal objects might not always have different numbers.

When we put objects into a hashtable, it is possible that different objects (by the equals() method) might have the same hashcode. This is called a **collision**. To resolve collisions, hashtable uses an **array of lists**. The pairs mapped to a single bucket (array index) are stored in a list and list reference is stored in array index.

![Hashtable collision](https://cdn2.howtodoinjava.com/wp-content/uploads/2018/10/Hashtable-collision.png)

## Features

The important things to learn about Java Hashtable class are:

1. It is similar to HashMap, but it is synchronized while HashMap is not [synchronized](https://howtodoinjava.com/java/multi-threading/what-is-thread-safety/).
2. It does not accept `null` key or value.
3. It does not accept duplicate keys.
4. It stores key-value pairs in hash table data structure which internally maintains an array of list. Each list may be referred as a bucket. In case of collisions, pairs are stored in this list.
5. Enumerator in Hashtable is not fail-fast.

## Methods

The methods in Hashtable class are very similar to HashMap. Take a look.

- **void clear()** : It is used to remove all pairs in the hashtable.
- **boolean contains(Object value)** : It returns `true` if specified value exist within the hash table for any pair, else return `false`. Note that this method is identical in functionality to `containsValue()` function.
- **boolean containsValue(Object value)** : It returns `true` if specified value exist within the hash table for any pair, else return `false`.
- **boolean containsKey(Object key)** : It returns `true` if specified key exist within the hash table for any pair, else return `false`.
- **boolean isEmpty()** : It returns `true` if the hashtable is empty; returns `false` if it contains at least one key.
- **void rehash()** : It is used to increase the size of the hash table and rehashes all of its keys.
- **Object get(Object key)** : It returns the value to which the specified key is mapped. Returns null if no such key is found.
- **Object put(Object key, Object value)** : It maps the specified `key` to the specified `value` in this hashtable. Neither the key nor the value can be `null`.
- **Object remove(Object key)** : It removes the key (and its corresponding value) from hashtable.
- **int size()** : It returns the number of entries in the hash table.

## Performance

Performance wise HashMap performs in O(log(n)) in comparion to O(n) in Hashtable for most common operations such as get(), put(), contains() etc.

The naive approach to thread-safety in Hashtable (“synchronizing every method”) makes it very much worse for threaded applications. We are better off externally synchronizing a HashMap. A well thought design will perform much better than Hashtable.

Hashtable is obsolete. Best is to use **ConcurrentHashMap** class which provide much higher degree of concurrency.

# LinkedHashMap

[Map 综述（二）：彻头彻尾理解 LinkedHashMap](https://blog.csdn.net/justloveyou_/article/details/71713781)



**LinkedHashMap** in Java is used to store key-value pairs very similar to `HashMap`class. Difference is that LinkedHashMap maintains the order of elements inserted into it while HashMap is unordered.

![image](https://ws4.sinaimg.cn/large/69d4185bly1g224sm0tryj20d507lwem.jpg)

## Features

The important things to learn about Java LinkedHashMap class are:

- It stores key-value pairs similar to HashMap.
- It contains only unique keys. Duplicate keys are not allowed.
- It may have one `null` key and multiple `null` values.
- It maintains the order of K,V pairs inserted to it by adding elements to internally managed **doubly-linked list**.

**LinkedHashMap(int capacity, float fillRatio, boolean Order):** initializes both the capacity and fill ratio for a LinkedHashMap along with whether to maintain the insertion order or access order.

- `'true'` enable access order.
- `'false'` enable insertion order. This is default value behavior when using other constructors.

In access ordered map, keys are sorted on the basis of access order last time they were accessed using any method of LinkedHashMap. Invoking the put, putIfAbsent, get, getOrDefault, compute, computeIfAbsent, computeIfPresent, or merge methods results in an access to the corresponding entry.

The keys are sorted from least recently accessed used to most recently accessed and build a LRU cache.

To create access order map, LinkedHashMap has a special constructor argument. When set to `true`, LinkedHashMap maintains the access order.

```java
//3rd parameter set access order
LinkedHashMap<Integer, String> pairs = new LinkedHashMap<>(2, .75f, true);
 
pairs.put(1,  "A");
pairs.put(2,  "B");
pairs.put(3,  "C");
pairs.put(4,  "D");
 
//Access 3rd pair
pairs.get(3);
 
//Access 1st pair
pairs.getOrDefault(2, "oops");
 
pairs.forEach((key, value) -> {
    System.out.println("Key:"+ key + ", Value:" + value);
});    
```

out put:

```
Key:1, Value:A
Key:4, Value:D
Key:3, Value:C
Key:2, Value:B
```

Notice the output that how most recently accessed entry goes to the end of order.

## Methods

The important methods we should learn about LinkedHashMap are as follows:

1. **void clear():** It removes all the key-value pairs from the map.
2. **void size():** It returns the number of key-value pairs present in this map.
3. **void isEmpty():** It returns true if this map contains no key-value mappings..
4. **boolean containsKey(Object key):** It returns `'true'` if a specified key is present in the map.
5. **boolean containsValue(Object key):** It returns `'true'` if a specified value is mapped to at least one key in the map.
6. **Object get(Object key):** It retrieves the `value` mapped by the specified `key`.
7. **Object remove(Object key):** It removes the key-value pair for the specified key from the map if present.
8. **boolean removeEldestEntry(Map.Entry eldest):** It returns `'true'` when the map removes its eldest entry from the access ordered map.

## Usecases

We can use LinkedHashMap in almost all situations where we require to use HashMap. Functionlity wise it can replace HashMap very transparentely.

Additionally, LinkedHashMap maintains the insertion order which makes it super useful when we want to maintain the order of pairs added to the Map.

Access ordered LinkedHashMap provides a great starting point for creating a **LRU Cache** functionality by overriding the `removeEldestEntry()` method to impose a policy for automatically removing stale when new mappings are added to the map. This lets you expire data using some criteria that you define.

## Performance

HashMap and LinkedHashMap performs the basic operations of add, remove and contains in constant-time performance. LinkedHashMap performs a little wose than HashMap because it has to maintain a doubly-linkedlist and HashMap maintain only linked list.

On the other hand, looping over Map in the case of LinkedHashMap is slightly faster than HashMap because the time required is proportional to ‘size’ only. In case of HashMap, iteration performance of proportional to ‘size + capacity’.

# ConcurrentHashMap

[Java Concurrent Collection - ConcurrentHashMap Examples](https://www.codejava.net/java-core/concurrency/java-concurrent-collection-concurrenthashmap-examples)





![image](https://ws4.sinaimg.cn/large/69d4185bly1g2trkb1q8rj20d905agln.jpg)

`ConcurrentHashMap` is a `Map` implementation like `HashMap` and `Hashtable`, with additional support for concurrency features:

- Unlike `Hastable` or `synchronizedMap` which locks the entire map exclusively to gain thread-safety feature, `ConcurrentHashMap` allows concurrent writer and reader threads. That means it allows some threads to modify the map and other threads to read values from the map at the same time, while `Hashtable` or `synchronizedMap` allows only one thread to work on the map at a time. More specifically, `ConcurrentHashMap` allows any number of concurrent reader threads and a limited number of concurrent writer threads, and both reader and writer threads can operate on the map simultaneously.
  - Reader threads perform retrieval operations such as `get`, `containsKey`, `size`, `isEmpty`, and `iterate` over keys set of the map.
  - Writer threads perform update operations such as `put` and `remove`.
- Iterators returned by `ConcurrentHashMap` are weakly consistent, meaning that the iterator may not reflect latest update since it was constructed. An iterator should be used by only one thread and no `ConcurrentModificationException` will be thrown if the map is modified while the iterator is being used.



## Method

`ConcurrentHashMap` is an implementation of `ConcurrentMap` which is a subtype of the Map interface. A ConcurrentMap defines the following atomic operations:

- `putIfAbsent(K key, V value)`: associates the specified key to the specified value if the key is not already associated with a value. This method is performed atomically, meaning that no other threads can intervene in the middle of checking absence and association.

- `remove(Object key, Object value)`: removes the entry for a key only if currently mapped to some value. This method is performed atomically.

- `replace(K key, V value)`: replaces the entry for a key only if currently mapped to some value. This method is performed atomically.

- `replace(K key, V oldValue, V newValue)`: replaces the entry for a key only if currently mapped to a given value. This method is performed atomically.

Also note that the methods `size()` and `isEmpty()` may return an approximation instead of an exact count due to the concurrent nature of the map. `ConcurrentHashMap` does not allow **null** key and **null** value.





# TreeMap

**TreeMap** in Java is used to store key-value pairs very similar to `HashMap` class. Difference is that TreeMap provides an efficient way to **store key/value pairs in sorted order**. It is a **red-Black tree** based `NavigableMap` implementation.

![image](https://ws2.sinaimg.cn/large/69d4185bly1g225iqavg4j20gk081q33.jpg)

## Features

The important points about Java TreeMap class are:

- It stores key-value pairs similar to like HashMap.
- It allows only distinct keys. Duplicate keys are not possible.
- It cannot have `null` key but can have multiple `null` values.
- It stores the keys in sorted order (natural order) or by a `Comparator` provided at map creation time.
- It provides guaranteed **log(n)** time cost for the `containsKey`, `get`, `put` and `remove` operations.
- It is not [synchronized](https://howtodoinjava.com/java/multi-threading/what-is-thread-safety/). Use `Collections.synchronizedSortedMap(new TreeMap())` to work in concurrent environment.
- The iterators returned by the `iterator` method are **fail-fast**.

## Methods

The important methods we should learn about TreeMap are as follows:

1. **void clear():** It removes all the key-value pairs from the map.
2. **void size():** It returns the number of key-value pairs present in this map.
3. **void isEmpty():** It returns true if this map contains no key-value mappings..
4. **boolean containsKey(Object key):** It returns `'true'` if a specified key is present in the map.
5. **boolean containsValue(Object key):** It returns `'true'` if a specified value is mapped to at least one key in the map.
6. **Object get(Object key):** It retrieves the `value` mapped by the specified `key`, or null if this map contains no mapping for the key.
7. **Object remove(Object key):** It removes the key-value pair for the specified key from the map if present.
8. **Comparator comparator():** It returns the comparator used to order the keys in this map, or null if this map uses the natural ordering of its keys.
9. **Object firstKey():** It returns the first (least) key currently in the tree map.
10. **Object lastKey():** It returns the last (greatest) key currently in the tree map.
11. **Object ceilingKey(Object key):** It returns the least key greater than or equal to the given key, or null if there is no such key.
12. **Object higherKey(Object key):** It returns the least key strictly greater than the specified key.
13. **NavigableMap descendingMap():** It returns a **reverse order view** of the mappings contained in this map.

## Performance

TreeMap provides the performance of **log(n)** for most operations like add(), remove() and contains(). HashMap performs with constant-time performance O(1) for same operations. In that way, HashMap performs much better than TreeMap.

TreeMap has better performance in memory management as it does not maintain an array internally to store key-value pairs. In HashMap, array size is determined while initialization or resizing which if is often more than needed at the time. It waste the memory. There is no such problem with TreeMap.

# HashSet

**Java HashSet** class implements the `Set` interface, backed by a hash table(actually a `HashMap` instance). If does not offer any guarantees as to the iteration order, and allows `null` element.

![image](https://ws3.sinaimg.cn/large/69d4185bly1g226cxl2f5j20es09swer.jpg)

## Features

- It implements `Set` Interface.
- Duplicate values are not allowed in HashSet.
- One NULL element is allowed in HashSet.
- It is un-ordered collection and makes no guarantees as to the iteration order of the set.
- This class offers constant time performance for the basic operations(add, remove, contains and size).
- HashSet is not synchronized. If multiple threads access a hash set concurrently, and at least one of the threads modifies the set, it must be synchronized externally.
- Use **Collections.synchronizedSet(new HashSet())** method to get the synchronized hashset.
- The iterators returned by this class’s iterator method are **fail-fast** and may throw `ConcurrentModificationException` if the set is modified at any time after the iterator is created, in any way except through the iterator’s own `remove()` method.
- HashSet also implements Searlizable and [Cloneable](https://howtodoinjava.com/java/cloning/a-guide-to-object-cloning-in-java/) interfaces.

## Methods

1. **public boolean add(E e)** : adds the specified element to the Set if not already present. This method internally uses **equals()** method to check for duplicates. If element is duplicate then element is rejected and value is NOT replaced.
2. **public void clear()** : removes all the elements from the hashset.
3. **public boolean contains(Object o)** : returns `true` if the hashset contains the specified element, othrweise `false`.
4. **public boolean isEmpty()** : returns `true` if hashset contains no element, otherwise `false`.
5. **public int size()** : returns the number of elements in the hashset.
6. **public Iterator<E> iterator()** : returns an iterator over the elements in this hashset. The elements are returned from iterator in no specific order.
7. **public boolean remove(Object o)** : removes the specified element from the hashset if it is present and return `true`, else returns `false`.
8. **public boolean removeAll(Collection<?> c)** : remove all the elements in the hashset that are part of the specified collection.
9. **public Object clone()** : returns a shallow copy of the hashset.
10. **public Spliterator<E> spliterator()** : creates a late-binding and fail-fast **Spliterator** over the elements in this hashset.

## Performance

- HashSet class offers **constant time performance of O(1)**for the basic operations(add, remove, contains and size), assuming the hash function disperses the elements properly among the buckets.
- Iterating over this set requires time proportional to the sum of the HashSet instance’s size (the number of elements) plus the”capacity” of the backing HashMap instance (the number ofbuckets). Thus, it’s very important not to set the initial capacity too high (or the load factor too low) if iteration performance is important.

# LinkedHashSet

**Java LinkedHashSet** class **extends HashSet** and `implements Set` interface. It is very very similar to [HashSet](https://howtodoinjava.com/java/collections/java-hashset/) class, except if offers the ***predictable iteration order***.

![image](https://ws4.sinaimg.cn/large/69d4185bly1g226mwhok1j20e40c2aag.jpg)

## Features

- It extends `HashSet` class which extends `AbstractSet` class.
- It implements `Set` interface.
- **Duplicate values are not allowed** in LinkedHashSet.
- One NULL element is allowed in LinkedHashSet.
- It is an **ordered collection** which is the order in which elements were inserted into the set (**insertion-order**).
- Like HashSet, this class offers **constant time performance** for the basic operations(add, remove, contains and size).
- LinkedHashSet is **not synchronized**. If multiple threads access a hash set concurrently, and at least one of the threads modifies the set, it must be synchronized externally.
- Use **Collections.synchronizedSet(new LinkedHashSet())** method to get the synchronized LinkedHashSet.
- The iterators returned by this class’s iterator method are **fail-fast** and may throw `ConcurrentModificationException` if the set is modified at any time after the iterator is created, in any way except through the iterator’s own `remove()` method.
- LinkedHashSet also implements Searlizable and [Cloneable](https://howtodoinjava.com/java/cloning/a-guide-to-object-cloning-in-java/) interfaces.

## Methods

1. **public boolean add(E e)** : adds the specified element to the Set if not already present. This method internally uses **equals()** method to check for duplicates. If element is duplicate then element is rejected and value is NOT replaced.
2. **public void clear()** : removes all the elements from the LinkedHashSet.
3. **public boolean contains(Object o)** : returns `true` if the LinkedHashSet contains the specified element, othrweise `false`.
4. **public boolean isEmpty()** : returns `true` if LinkedHashSet contains no element, otherwise `false`.
5. **public int size()** : returns the number of elements in the LinkedHashSet.
6. **public Iterator<E> iterator()** : returns an iterator over the elements in this LinkedHashSet. The elements are returned from iterator in no specific order.
7. **public boolean remove(Object o)** : removes the specified element from the LinkedHashSet if it is present and return `true`, else returns `false`.
8. **public boolean removeAll(Collection<?> c)** : remove all the elements in the LinkedHashSet that are part of the specified collection.
9. **public Object clone()** : returns a shallow copy of the LinkedHashSet.
10. **public Spliterator<E> spliterator()** : creates a late-binding and fail-fast Spliterator over the elements in this LinkedHashSet. It has following intialization properties `Spliterator.DISTINCT`, `Spliterator.ORDERED`.

## Performance

- LinkedHashSet class offers **constant time performance of O(1)**for the basic operations(add, remove, contains and size), assuming the hash function disperses the elements properly among the buckets.

- Performance is likely to be just slightly below that of HashSet, due to the added expense of maintaining the linked list, with one exception of iteration.

  Iteration over a LinkedHashSet requires time proportional to the size of the set, regardless of its capacity. Iteration over a HashSet is likely to be more expensive, requiring time proportional to its capacity. Thus LinkedHashSet may provide better performance than HashSet while iteration.

# TreeSet

**Java TreeSet** class **extends AbstractSet** and `implements NavigableSet` interface. It is very similar to HashSet class, except it stores the element in **sorted order**.

The sort order is either natural order or by a [Comparator](https://howtodoinjava.com/java8/using-comparator-becomes-easier-with-lambda-expressions-java-8/) provided at treeset creation time, depending on which [constructor](https://howtodoinjava.com/oops/java-constructors/) is used.

![image](https://wx2.sinaimg.cn/large/69d4185bly1g226trnuv2j20i90bwt94.jpg)

## Features

- It extends `AbstractSet` class which extends `AbstractCollection` class.
- It implements `NavigableSet` interface which extends `SortedSet` interface.
- Duplicate values are not allowed in TreeSet.
- NULL is not allowed in TreeSet.
- It is an **ordered collection** which store the elements in sorted order.
- Like [HashSet](https://howtodoinjava.com/java/collections/java-hashset/), this class offers constant time performance for the basic operations(add, remove, contains and size).
- TreeSet does not allow to insert heterogeneous objects because it must compare objects to determine sort order.
- TreeSet is not [synchronized](https://howtodoinjava.com/java/multi-threading/wait-notify-and-notifyall-methods/). If multiple threads access a hash set concurrently, and at least one of the threads modifies the set, it must be synchronized externally.
- Use **Collections.synchronizedSortedSet(new TreeSet())** method to get the synchronized TreeSet.
- The iterators returned by this class’s iterator method are **fail-fast** and may throw `ConcurrentModificationException` if the set is modified at any time after the iterator is created, in any way except through the iterator’s own `remove()` method.
- TreeSet also implements Searlizable and [Cloneable](https://howtodoinjava.com/array/java-array-clone-shallow-copy/) interfaces.

## Methods

1. **boolean add(E e)** : adds the specified element to the Set if not already present.
2. **Comparator comparator()** : returns the comparator used to order the elements in this set, or null if this set uses the natural ordering of its elements.
3. **Object first()** : returns the first (lowest) element currently in this set.
4. **Object last()** : returns the last (greatest) element currently in this set.
5. **void clear()** : removes all the elements from the TreeSet.
6. **boolean contains(Object o)** : returns `true` if the TreeSet contains the specified element, othrwise `false`.
7. **boolean isEmpty()** : returns `true` if TreeSet contains no element, otherwise `false`.
8. **int size()** : returns the number of elements in the TreeSet.
9. **Iterator<E> iterator()** : returns an iterator over the elements in this set in **ascending order**.
10. **Iterator<E> descendingIterator()** : returns an iterator over the elements in this set in descending order.
11. **NavigableSet<E> descendingSet()** : returns a reverse order view of the elements contained in this set.
12. **boolean remove(Object o)** : removes the specified element from the TreeSet if it is present and return `true`, else returns `false`.
13. **Object clone()** : returns a shallow copy of the TreeSet.
14. **Spliterator<E> spliterator()** : creates a late-binding and fail-fast Spliterator over the elements in this TreeSet. It has same ordering as treeset provides.

## Usecases

TreeSet is very much like HashSet (unique elements) and provides predictable iteration order (sorted). Sorted order can overridden using custom comparator.

TreeSet uses **Red-Black tree** under the hood. So the set could be thought as a dynamic search tree. When you need a structure which is operated read/write frequently and also should keep order, the TreeSet is a good choice.

If you want to keep a collection sorted and you are mostly appending the elements, TreeSet with a Comparator is your best bet.

## Performance

- TreeSet provides guaranteed **log(n**) time cost for the basic operations (add, remove and contains).
- The operations like iterating the elements in sorted order takes **O(n**) time.

# PriorityQueue

**Java PriorityQueue** class is a queue data structure implementation in which objects are processed based on their **priority**. It is different from standard queues where [FIFO](https://en.wikipedia.org/wiki/FIFO_(computing_and_electronics)) (First-In-First-Out) algorithm is followed.

![image](https://wx2.sinaimg.cn/large/69d4185bly1g227ikk9u9j20ae09nmxd.jpg)

In a priority queue, added objects are according to their priority. By default, the priority is determined by objects’ natural ordering. Default priority can be overridden by a [Comparator](https://howtodoinjava.com/java/collections/java-comparator/) provided at queue construction time.

![Priority Queue](https://cdn2.howtodoinjava.com/wp-content/uploads/2018/10/priority-queue-pattern.png)

## Features

Let’s note down few important points on the PriorityQueue.

- PriorityQueue is an unbounded queue and grows dynamically. The default initial capacity is `'11'` which can be overridden using **initialCapacity** parameter in appropriate constructor.
- It does not allow NULL objects.
- Objects added to PriorityQueue MUST be comparable.
- The objects of the priority queue are ordered **by default in natural order**.
- A Comparator can be used for custom ordering of objects in the queue.
- The **head** of the priority queue is the **least** element based on the natural ordering or comparator based ordering. When we poll the queue, it returns the head object from the queue.
- If multiple objects are present of same priority the it can poll any one of them randomly.
- PriorityQueue is **not thread safe**. Use `PriorityBlockingQueue` in concurrent environment.
- It provides **O(log(n))** time for add and poll methods.

## Methods

PriorityQueue class has below given important methods, you should know.

- **boolean add(object)** : Inserts the specified element into this priority queue.
- **boolean offer(object)** : Inserts the specified element into this priority queue.
- **boolean remove(object)** : Removes a single instance of the specified element from this queue, if it is present.
- **Object poll()** : Retrieves and removes the head of this queue, or returns null if this queue is empty.
- **Object element()** : Retrieves, but does not remove, the head of this queue, or returns null if this queue is empty.
- **Object peek()** : Retrieves, but does not remove, the head of this queue, or returns null if this queue is empty.
- **void clear()** : Removes all of the elements from this priority queue.
- **Comparator comparator()** : Returns the comparator used to order the elements in this queue, or null if this queue is sorted according to the natural ordering of its elements.
- **boolean contains(Object o)** : Returns true if this queue contains the specified element.
- **Iterator iterator()** : Returns an iterator over the elements in this queue.
- **int size()** : Returns the number of elements in this queue.
- **Object[] toArray()** : Returns an array containing all of the elements in this queue.

```java
public class Employee implements Comparable<Employee> {
    private Long id;
    private String name;
    private LocalDate dob;
 
    public Employee(Long id, String name, LocalDate dob) {
        super();
        this.id = id;
        this.name = name;
        this.dob = dob;
    }
     
    @Override
    public int compareTo(Employee emp) {
        return this.getId().compareTo(emp.getId());
    }
 
    //Getters and setters
 
    @Override
    public String toString() {
        return "Employee [id=" + id + ", name=" + name + ", dob=" + dob + "]";
    }
}
```

```java
PriorityQueue<Employee> priorityQueue = new PriorityQueue<>();
         
priorityQueue.add(new Employee(1l, "AAA", LocalDate.now()));
priorityQueue.add(new Employee(4l, "CCC", LocalDate.now()));
priorityQueue.add(new Employee(5l, "BBB", LocalDate.now()));
priorityQueue.add(new Employee(2l, "FFF", LocalDate.now()));
priorityQueue.add(new Employee(3l, "DDD", LocalDate.now()));
priorityQueue.add(new Employee(6l, "EEE", LocalDate.now()));
 
while(true){
    Employee e = priorityQueue.poll();
    System.out.println(e);
     
    if(e == null) break;
}
```

```java
//Comparator for name field
Comparator<Employee> nameSorter = Comparator.comparing(Employee::getName);
 
PriorityQueue<Employee> priorityQueue = new PriorityQueue<>( nameSorter );
         
priorityQueue.add(new Employee(1l, "AAA", LocalDate.now()));
priorityQueue.add(new Employee(4l, "CCC", LocalDate.now()));
priorityQueue.add(new Employee(5l, "BBB", LocalDate.now()));
priorityQueue.add(new Employee(2l, "FFF", LocalDate.now()));
priorityQueue.add(new Employee(3l, "DDD", LocalDate.now()));
priorityQueue.add(new Employee(6l, "EEE", LocalDate.now()));
 
while(true){
    Employee e = priorityQueue.poll();
    System.out.println(e);
     
    if(e == null) break;
}
```

# PriorityBlockingQueue

**Java PriorityBlockingQueue** class is **concurrent** blocking queue data structure implementation in which objects are processed based on their **priority**. The “blocking” part of the name is added to imply the **thread will block waiting until there’s an item available on the queue**.

![image](https://ws2.sinaimg.cn/large/69d4185bly1g227q4qsngj20iz09lt93.jpg)

## Features

Let’s note down few important points on the PriorityBlockingQueue.

- PriorityBlockingQueue is an unbounded queue and grows dynamically. The default initial capacity is `'11'` which can be overridden using **initialCapacity**parameter in appropriate constructor.
- It supplies blocking retrieval operations.
- It does not allow NULL objects.
- Objects added to PriorityBlockingQueue MUST be comparable otherwise it throws `ClassCastException`.
- The objects of the priority queue are ordered **by default in natural order**.
- A Comparator can be used for custom ordering of objects in the queue.
- The **head** of the priority queue is the **least** element based on the natural ordering or comparator based ordering. When we poll the queue, it returns the head object from the queue.
- If multiple objects are present of same priority the it can poll any one of them randomly.
- PriorityBlockingQueue is **thread safe**.
- The Iterator provided in method **iterator()** is not guaranteed to traverse the elements of the PriorityBlockingQueue in any particular order. If you need ordered traversal, consider using **Arrays.sort(pbq.toArray()).**
- The **drainTo()** can be used to remove some or all elements in priority order and place them in another collection.

## Methods

PriorityBlockingQueue class has below given important methods, you should know.

- **boolean add(object)** : Inserts the specified element into this priority queue.
- **boolean offer(object)** : Inserts the specified element into this priority queue.
- **boolean remove(object)** : Removes a single instance of the specified element from this queue, if it is present.
- **Object poll()** : Retrieves and removes the head of this queue, waiting up to the specified wait time if necessary for an element to become available.
- **Object poll(timeout, timeUnit)** : Retrieves and removes the head of this queue, waiting up to the specified wait time if necessary for an element to become available.
- **Object take()** : Retrieves and removes the head of this queue, waiting if necessary until an element becomes available.
- **void put(Object o)** : Inserts the specified element into this priority queue.
- **void clear()** : Removes all of the elements from this priority queue.
- **Comparator comparator()** : Returns the comparator used to order the elements in this queue, or null if this queue is sorted according to the natural ordering of its elements.
- **boolean contains(Object o)** : Returns true if this queue contains the specified element.
- **Iterator iterator()** : Returns an iterator over the elements in this queue.
- **int size()** : Returns the number of elements in this queue.
- **int drainTo(Collection c)** : Removes all available elements from this queue and adds them to the given collection.
- **int drainTo(Collection c, int maxElements)** : Removes at most the given number of available elements from this queue and adds them to the given collection.
- **int remainingCapacity()** : Always returns `Integer.MAX_VALUE` because a PriorityBlockingQueue is not capacity constrained.
- **Object[] toArray()** : Returns an array containing all of the elements in this queue.

Please note the **difference between take() and poll()** methods. The poll() retrieves and removes the head of this queue, or returns null if this queue is empty. It is not blocking operation.

The take() retrieves and removes the head of this queue, waiting if necessary until an element becomes available. It is blocking operation.

# ArrayBlockingQueue

**ArrayBlockingQueue** class is Java **concurrent** and **bounded** blocking queue implementation backed by an array. It orders elements FIFO (first-in-first-out).

The **head** of the ArrayBlockingQueue is that element that has been on the queue the longest time. The **tail** of the ArrayBlockingQueue is that element that has been on the queue the shortest time. New **elements are inserted at the tail** of the queue, and the queue **retrieval operations obtain elements at the head** of the queue.

![image](https://wx4.sinaimg.cn/large/69d4185bly1g227w9d7pbj20dp09lwes.jpg)

## Features

Let’s note down few important points on the ArrayBlockingQueue class.

- ArrayBlockingQueue is a bounded queue of fixed size backed by an array.
- It orders elements FIFO (first-in-first-out).
- Elements are inserted at the tail, and retrieved from the head of the queue.
- Once created, the capacity of the queue cannot be changed.
- It supplies **blocking insertion and retrieval operations**.
- It does not allow NULL objects.
- ArrayBlockingQueue is **thread safe**.
- The Iterator provided in method **iterator()** traverse the elements in order from first (head) to last (tail).
- ArrayBlockingQueue supports an optional **fairness policy** for ordering waiting producer and consumer threads. With fairness set to `true`, the queue grants threads access in FIFO order.

## Methods

ArrayBlockingQueue class has below given important methods, you should know.

- **void put(Object o)** : Inserts the specified element at the tail of this queue, waiting for space to become available if the queue is full.**boolean add(object)** : Inserts the specified element at the tail of this queue if it is possible to do so immediately without exceeding the queue’s capacity, returning true upon success and throwing an IllegalStateException if this queue is full.
- **boolean offer(object)** : Inserts the specified element at the tail of this queue if it is possible to do so immediately without exceeding the queue’s capacity, returning true upon success and throwing an IllegalStateException if this queue is full.
- **boolean remove(object)** : Removes a single instance of the specified element from this queue, if it is present.
- **Object peek()** : Retrieves, but does not remove, the head of this queue, or returns null if this queue is empty.
- **Object poll()** : Retrieves and removes the head of this queue, or returns null if this queue is empty.
- **Object poll(timeout, timeUnit)** : Retrieves and removes the head of this queue, waiting up to the specified wait time if necessary for an element to become available.
- **Object take()** : Retrieves and removes the head of this queue, waiting if necessary until an element becomes available.
- **void clear()** : Removes all of the elements from this the queue.
- **boolean contains(Object o)** : Returns true if this queue contains the specified element.
- **Iterator iterator()** : Returns an iterator over the elements in this queue in proper sequence.
- **int size()** : Returns the number of elements in this queue.
- **int drainTo(Collection c)** : Removes all available elements from this queue and adds them to the given collection.
- **int drainTo(Collection c, int maxElements)** : Removes at most the given number of available elements from this queue and adds them to the given collection.
- **int remainingCapacity()** : Returns the number of additional elements that this queue can ideally (in the absence of memory or resource constraints) accept without blocking.
- **Object[] toArray()** : Returns an array containing all of the elements in this queue, in proper sequence.

# LinkedTransferQueue

**Java TransferQueue** is a concurrent blocking queue implementation in which producers may wait for receipt of messages by consumers. **LinkedTransferQueue**class is an implementation of `TransferQueue` in Java.

![image](https://ws2.sinaimg.cn/large/69d4185bly1g2282lgx17j20f80brwew.jpg)

TransferQueue may be useful for example in message passing applications in which producers sometimes (using method **transfer()**) await receipt of elements by consumers invoking take or poll, while at other times enqueue elements (via method **put()**) without waiting for receipt.

```
When a producer reaches to TransferQueue to transfer a message and there are consumers waiting to take message, then producer directly transfers the message to consumer.
If there is no consumer waiting, then producer will not directly put the message and returned, rather it will wait for any consumer to be available to consume the message.
```

## Features

Let’s note down few important points on the LinkedTransferQueue in Java.

- LinkedTransferQueue is an **unbounded** queue on linked nodes.
- This queue orders elements FIFO (first-in-first-out) with respect to any given producer.
- Elements are inserted at the tail, and retrieved from the head of the queue.
- It supplies **blocking insertion and retrieval operations**.
- It does not allow NULL objects.
- LinkedTransferQueue is **thread safe**.
- The size() method is NOT a constant-time operation because of the asynchronous nature, so may report inaccurate results if this collection is modified during traversal.
- The bulk operations addAll, removeAll, retainAll, containsAll, equals, and toArray are not guaranteed to be performed atomically. For example, an iterator operating concurrently with an addAll operation might view only some of the added elements.

## Methods

LinkedTransferQueue class has below given important methods, you should know.

- **Object take()** : Retrieves and removes the head of this queue, waiting if necessary until an element becomes available.
- **void transfer(Object o)** : Transfers the element to a consumer, waiting if necessary to do so.
- **boolean tryTransfer(Object o)** : Transfers the element to a waiting consumer immediately, if possible.
- **boolean tryTransfer(Object o, long timeout, TimeUnit unit)** : Transfers the element to a consumer if it is possible to do so before the timeout elapses.
- **int getWaitingConsumerCount()** : Returns an estimate of the number of consumers waiting to receive elements via BlockingQueue.take() or timed poll.
- **boolean hasWaitingConsumer()** : Returns true if there is at least one consumer waiting to receive an element via BlockingQueue.take() or timed poll.
- **void put(Object o)** : Inserts the specified element at the tail of this queue.**boolean add(object)** : Inserts the specified element at the tail of this queue.
- **boolean offer(object)** : Inserts the specified element at the tail of this queue.
- **boolean remove(object)** : Removes a single instance of the specified element from this queue, if it is present.
- **Object peek()** : Retrieves, but does not remove, the head of this queue, or returns null if this queue is empty.
- **Object poll()** : Retrieves and removes the head of this queue, or returns null if this queue is empty.
- **Object poll(timeout, timeUnit)** : Retrieves and removes the head of this queue, waiting up to the specified wait time if necessary for an element to become available.
- **void clear()** : Removes all of the elements from this the queue.
- **boolean contains(Object o)** : Returns true if this queue contains the specified element.
- **Iterator iterator()** : Returns an iterator over the elements in this queue in proper sequence.
- **int size()** : Returns the number of elements in this queue.
- **int drainTo(Collection c)** : Removes all available elements from this queue and adds them to the given collection.
- **int drainTo(Collection c, int maxElements)** : Removes at most the given number of available elements from this queue and adds them to the given collection.
- **int remainingCapacity()** : Returns the number of additional elements that this queue can ideally (in the absence of memory or resource constraints) accept without blocking.
- **Object[] toArray()** : Returns an array containing all of the elements in this queue, in proper sequence.

# CopyOnWriteArrayList

**Java CopyOnWriteArrayList** is a [thread-safe](https://howtodoinjava.com/java/multi-threading/what-is-thread-safety/) variant of **ArrayList** in which all mutative operations (add, set, and so on) are implemented by making a fresh copy of the underlying [array](https://howtodoinjava.com/java-array/).

![image](https://wx1.sinaimg.cn/large/69d4185bly1g228dtxqnuj20fv07n3yo.jpg)

It’s **immutable snapshot** style iterator method uses a reference to the state of the array at the point that the [iterator](https://howtodoinjava.com/java/collections/java-iterator/) was created. This helps in usecases when traversal operations vastly outnumber list update operations and we do not want to synchronize the traversals and still want thread safety while updating the list.

## Features

The important things to learn about **Java CopyOnWriteArrayList** class are:

- CopyOnWriteArrayList class implement `List` and `RandomAccess` interfaces and thus provide all functionalities available in ArrayList class.
- Using CopyOnWriteArrayList is costly for update operations, because each mutation creates a cloned copy of underlying array and add/update element to it.
- It is thread-safe version of ArrayList. Each thread accessing the list sees its own version of snapshot of backing array created while initializing the iterator for this list.
- Because it gets snapshot of underlying array while creating iterator, it **does not throw ConcurrentModificationException**.
- Mutation operations on iterators (remove, set, and add) are not supported. These methods throw `UnsupportedOperationException`.
- CopyOnWriteArrayList is a concurrent replacement for a **synchronized List** and offers better concurrency when iterations outnumber mutations.
- It allows duplicate elements and heterogeneous Objects (use generics to get compile time errors).
- Because it creates a new copy of array everytime iterator is created, **performance is slower** than ArrayList.

```java
CopyOnWriteArrayList<Integer> list = new CopyOnWriteArrayList<>(new Integer[] {1,2,3});
 
System.out.println(list);   //[1, 2, 3]
 
//Get iterator 1
Iterator<Integer> itr1 = list.iterator();
 
//Add one element and verify list is updated
list.add(4);
System.out.println(list);   //[1, 2, 3, 4]
 
//Get iterator 2
Iterator<Integer> itr2 = list.iterator();
 
System.out.println("====Verify Iterator 1 content====");
itr1.forEachRemaining(System.out :: println);   //1,2,3
 
System.out.println("====Verify Iterator 2 content====");
itr2.forEachRemaining(System.out :: println);   //1,2,3,4
```

## Methods

CopyOnWriteArrayList class all the methods which are supported in ArrayList class. The behavior is different only in case of iterators (**snapshot iterator**) AND new backing array created during mutations in the list.

Additionally it provides few methods which are additional to this class.

- **boolean addIfAbsent(object o)** : Append the element if not present.
- **int addAllAbsent(Collection c)** : Appends all of the elements in the specified collection that are not already contained in this list, to the end of this list, in the order that they are returned by the specified collection’s iterator.

## Usecases

We can prefer to use CopyOnWriteArrayList over normal ArrayList in following cases:

1. When list is to be used in concurrent environemnt.
2. Iterations outnumber the mutation operations.
3. Iterators must have snapshot version of list at the time when they were created.
4. We don’t want to [synchronize the thread access](https://howtodoinjava.com/java/multi-threading/wait-notify-and-notifyall-methods/) programatically.

## Performance

Due to added step of creating a new backing array everytime the list is updated, it performs worse than ArrayList.
There is no performance overhead on read operations and both classes perform same.

# CopyOnWriteArraySet

**Java CopyOnWriteArraySet** is a [thread-safe](https://howtodoinjava.com/java/multi-threading/what-is-thread-safety/) variant of **HashSet** which uses a underlying `CopyOnWriteArrayList` for all of its operations.

![image](https://wx3.sinaimg.cn/large/69d4185bly1g228rcgmx5j209n09jwep.jpg)

Similar to CopyOnWriteArrayList, it’s **immutable snapshot** style [iterator](https://howtodoinjava.com/java/collections/java-iterator/) method uses a reference to the state of the array (inside the backing list) at the point that the iterator was created. This helps in usecases when traversal operations vastly outnumber set update operations and we do not want to synchronize the traversals and still want thread safety while updating the set.

##  Features

The important things to learn about Java CopyOnWriteArraySet class are:

- As normal set data structure, it does not allow duplicates.
- CopyOnWriteArraySet class implement `Serializable` interface and extends `AbstractSet` class.
- Using CopyOnWriteArraySet is costly for update operations, bacause each mutation creates a cloned copy of underlying array and add/update element to it.
- It is thread-safe version of HashSet. Each thread accessing the set sees its own version of snapshot of backing array created while initializing the iterator for this set.
- Because it gets snapshot of underlying array while creating iterator, it **does not throw ConcurrentModificationException**.
- Mutation operations on iterators are not supported. These methods throw `UnsupportedOperationException`.
- CopyOnWriteArraySet is a concurrent replacement for a **synchronized Set** and offers better concurrency when iterations outnumber mutations.
- It allows duplicate elements and heterogeneous Objects (use generics to get compile time errors).
- Because it creates a new copy of underlying array everytime iterator is created, **performance is slower** than HashSet.

## Methods

- **boolean add(object o)** : Adds the specified element to this set if it is not already present.
- **boolean addAll(collection c)** : Adds all of the elements in the specified collection to this set if they’re not already present.
- **void clear()** : Removes all of the elements from this set.
- **boolean contains(Object o)** : Returns true if this set contains the specified element.
- **boolean isEmpty()** : Returns true if this set contains no elements.
- **Iterator iterator()** : Returns an iterator over the elements contained in this set in the order in which these elements were added.
- **boolean remove(Object o)** : Removes the specified element from this set if it is present.
- **int size()** : Returns the number of elements in this set.

## Performance

Due to added step of creating a new backing array everytime the set is updated, it performs worse than HashSet.
There is no performance overhead on read operations and both classes perform same.



# Comparator Interface

## Comparator interface

Java `Comparator` interface imposes a **total ordering** on the objects which may not have a natural ordering.

For example, for a list of elpmoyees object, the natural order may be order by employee id. But in real life applications, we may want to sort the list of employees by their first name, date of birth or simply any other such criteria. In such conditions, we need to use `Comparator` interface.

We can use Comparator interface in following situations.

1. Sort the array or list of objects, but NOT in natural order.
2. Sort the array or list of objects where we can not modify the object’s source code to implement **Comparable** interface.
3. Sort same list or array of objects on different fields.
4. Using **group by sort** on list or array of objects on different fields.

## Comparator.compare()

To enable total ordering on objects, we need to create class which implements Comparator interface. Then we need to override it’s **compare(T o1, T o2)** method.

It compares its two arguments for order. It returns a negative integer, zero, or a positive integer as the first argument is less than, equal to, or greater than the second.

The implementor must also ensure that the relation is **transitive**: `((compare(x, y)>0) && (compare(y, z)>0))` implies `compare(x, z)>0`.

```java
import java.time.LocalDate;
 
public class Employee implements Serializable {
 
    private static final long serialVersionUID = 1L;
 
    private Long id;
    private String name;
    private LocalDate dob;
 
    //Getters and Setters
 
    @Override
    public String toString() {
        return "Employee [id=" + id + ", name=" + name + ", dob=" + dob + "]";
    }
}

```

For above class, an order by employee name can be imposed by creating Comparator like below.

```java
import java.util.Comparator;
 public class NameSorter implements Comparator<Employee>{
    @Override
    public int compare(Employee e1, Employee e2) {
        return e1.getName().compareToIgnoreCase( e2.getName() );
    }
}
```

## Collections.comparing()

This utility method accepts a function that extracts a sort key for the class. This is essentially a field on which the class objects will be sorted.

```java
//Order by name
Comparator.comparing(Employee::getName);
 
//Order by name in reverse order
Comparator.comparing(Employee::getName).reversed();
```

## Collections.thenComparing()

This utility method is used for **group by sort**. Using this method, we can chain multiple comparators to sort the list or array of objects on multiple fields.

It is very similar to **SQL GROUP BY clause** to order rows on different fields.

```java
//Order by name and then by age
Comparator.comparing(Employee::getName)
            .thenComparing(Employee::getDob);
 
//Order by name -> date of birth -> id
Comparator.comparing(Employee::getName)
            .thenComparing(Employee::getDob)
            .thenComparing(Employee::getId);
```



# Compare

##  LinkedList vs ArrayList

Both collections allow duplicate elements and maintain the insertion order of the elements.

`LinkedList` implements it with a **doubly-linked list**. `ArrayList` implements it with a **dynamically resizing array**. This will lead further differences in performance.

Until you are not dealing with very high volume of data, both the classes will give you same level of performance. Both provide ordered collection and both are non-synchronized as well.

`LinkedList` implements `Deque` interface as well, so it provides **queue** like **FIFO** functionality through methods such as `peek()` and `poll()`.

As seen in performance comparison, `ArrayList` is better for storing and accessing data. `LinkedList` is better for manipulating data.

LinkedList has more memory overhead than ArrayList because in ArrayList each index only holds actual object but in case of LinkedList each node holds both data and address of next and previous node.

### Add operation

Adding element in ArrayList is `O(1)` operation if it doesn’t require resize of Array. If array is resized then it becomes `O(log(n))`.

Appending an element in LinkedList is `O(1)` operation, as it doesn’t require any navigation.

### Remove operation

When we remove an element from ArrayList (in backing array), it moves all elements on right. It makes it close to `O(n)` in worst case (remove first element) and `O(1)` in best case (remove last element).

LinkedList remove operation gives `O(1)` performance because it just need to reset the pointers of previous and next nodes. No copy or movement is required.

### Iteration

Iteration is the `O(n)` operation for both LinkedList and ArrayList where n is a number of an element.

### Get operation

ArrayList provides `get(int index)` method which directly find the element at given index location. It is of order **O(1)**.

LinkedList also provide `get(int index)` method BUT it first traverses all nodes to reach the correct node. It makes the performance variable. In best case it is `O(1)` and in worst case it is `O(n)`.

## ArrayList vs Vector

### Thread safety

**Vector is a synchronized collection and ArrayList is not**.

It simply means that when working on concurrent applications, we can use Vector without any addtional synchronization control implemented by developer using `synchronized` keyword. Public methods inside vector are defined `synchronized` which make all operations in vector safe for concurrency needs.

To use arraylist in concurrent application, we must explicitely control the thread access to instance to make application work as intended. If we want to get sunchronized version of arraylist, then we can use `Collections.synchronizedList()` method. 

Returned list is an internal implementation of `List` interface different from arraylist. But it have the same capability as arraylist class.

### Capacity increment

By default when vector needs to increase capacity to add an element (when existing capacity is filled), it increases the capacity by **100%.** It means vector size grows to double of previous capacity. We can overide the default capacity using constructor `public Vector(int initialCapacity, int capacityIncrement)`. Here second argument is the amount by which the capacity is increased when the vector overflows.

In ArrayList, by default capacity grows by `50%` of existing capacity. In arraylist, we can define the initial capacity but not the capacity increment factor.

###  Performance

Both collections have a backing array on which they store and search elements. So essentially there is not much performance difference in **add** and **get** operations.

Real performance difference comes when we take synchronization into consideration. `ArrayList` is non-synchronized so there is no time lapse in thread safety. Whereas `Vector` is `synchronized`, so it has some overhead in thread management/ locking etc.

### ConcurrentModificationException

There is one difference on how these colelction handle the iteration while the collection is still modifying by program.

`ArrayList` provide iterators, which are **fail-fast**. As soon as we modify the arraylist structure (add or remove elements), the iterator will throw **ConcurrentModificationException** error.

`Vector` provide **iterator** as well as **enumeration**. Iterators are fail-fast by enumerations are not. If we modify the vector during iteration over enumeration, it does not fail.

## Synchronized HashMap and ConcurrentHashMap

1. Multiple threads can add/remove key-value pairs from ConcurrentHashMap, while only one thread is allowed to make change in case of SynchronizedHashMap. This results higher degree of concurrency in ConcurrentHashMap.
2. No need to lock the map to read a value in ConcurrentHashMap. A retrieval operation will return the value inserted by the most recent completed insert operation. A lock is required for read operation too in SynchronizedHashMap.
3. ConcurrentHashMap doesn’t throw a `ConcurrentModificationException` if one thread tries to modify it while another is iterating over it. The iterator reflects the state of the map at the time of it’s creation. SynchronizedHashMap returns Iterator, which fails-fast on concurrent modification.

## Hashtable vs HashMap

Let’s quickly list down the **differences between a hashmap and hashtable in Java**.

1. HashMap is non synchronized. Hashtable is synchronized.
2. HashMap allows one null key and multiple null values. Hashtable doesn’t allow any null key or value.
3. HashMap is fast. Hashtable is slow due to added synchronization.
4. HashMap is traversed by Iterator. Hashtable is traversed by Enumerator and Iterator.
5. Iterator in HashMap is fail-fast. Enumerator in Hashtable is not fail-fast.
6. HashMap inherits AbstractMap class. Hashtable inherits Dictionary class.

