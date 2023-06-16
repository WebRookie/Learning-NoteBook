## Collection类关系图

知识体系结构

![img](https://www.pdai.tech/images/java_collections_overview.png)

##### 简介

容器，就是可以容他其他Java对象。优点是：

* 降低编程难度
* 提高程序性能
* 提高API间互操作性
* 降低学习难度
* 降低设计和实现相关API的难度
* 增加程序的重用性

Java容器里只能放对象，对于基本类型（int,long,float,double等）需要将其包装成对象类型后（Integer, Long, Float, Double等）才能放到容器里，很多时候拆包装和解包装能够自动完成，这虽然会导致额外的性能和空间开销，但是简化了设计和编程。



#### Collection

> 容器主要包括Collection和Map两种，Collection存储着对象的集合，而Map存储着键值对（两个对象）的映射表

##### Set

**TreeSet**

基于红黑树实现，支持有序性操作， 例如根据一个返回查找元素的操作，但是查找效率不如HashSet，HashSet查找，HashSet查找的时间复杂度为O（1），TreeSet则为O（logN）.

**HashSet**

基于哈希表实现，支持快速查找，但不支持有序性操作。并且失去了元素的插入顺序信息，也就是说使用Iterator遍历HashSet得到的结果是不确定的。



**LinkedHashSet**

具有HashSet的查找效率，且内部使用双向链表维护元素的插入顺序。



##### List

**ArrayList**

基于动态数组实现，支持随机访问。

**Vector**

和ArrayList类似，但是它是线程安全的。

**LinkedList**

基于双向链表实现，只能顺序访问，但是可以快速地在链表中间插入和删除元素。不仅如此， LinkedList还可以用作栈、队列和双向队列。



##### Queue

**LinkedList**

可以用它来实现双向队列

**PriorityQueue**

基于堆栈结构实现，可以用它来实现优先队列



##### Map

**TreeMap**

基于红黑树实现



**HashMap**

基于哈希表实现。

**HashTable**

和HashMap类似，但是它是线程安全的，这意味着同一时刻多个线程可以同时写入HashTable并且不会导致数据的不一致。它是遗留类，不应该去使用它，现在可以使用ConcurrentHashMap来支持线程安全，并且ConcurrentHashMap效率会更高，因为ConcurrentHashMap引入了分段锁

**LinkedHashMap**

使用双向链表来维护元素的顺序，顺序为插入顺序或者最近最少使用（LRU）顺序。



### ArrayList源码解析

#### 概述

ArrayList实现了List接口，是顺序容器，即元素存放过的数据与放进去的顺序相同，允许放入`null`元素，底层通过**数组实现**。除该类未实现同步外，其余跟Vector大致相同。每个ArrayList都有一个容量（capacity），表示底层数组的实际大小，容器内存储元素的个数不能多于当前容量。当向容器中添加元素时，如果容量不足，容器会自动增加底层数组的大小。总所周知：Java泛型只是编译器提供的语法糖，这里的数组是一个Object数组，以便能够容纳任何类型的对象。



size()，isEmpty(), get(), set() 方法均能在常数时间内完成，add（）方法的时间开销和插入的位置有关，addAll（）方法的时间开销跟添加元素的个数成正比，其余方法大多都是线性时间。

为了追求效率，ArrayList没有实现同步(synchronized)， 如果需要多个线程并发访问，用户可以手动同步，也可以使用Vector替代。



#### ArrayList的实现

##### 自动扩容

每当向数组添加元素的时候，都要去检查添加后元素买的个数是否会超出当前数组的长度，如果超出，数组将会扩容，已满足添加数据的需求。数组扩容通过一个公开的方法，ensureCapacity（int minCapacity）来实现，在实际添加大量元素前，也可以使用ensureCapactiy来手动添加arrayList实例的容量，以减少递增式再分配的数量。



数组进行扩容时，会将老数组的元素重新拷贝一份到新的数组中，每次数组容量的增长大约是其原容量的1.5倍，这种操作的代价是恒哦啊的，因此在实际的使用时，我们应该避免数组容量的扩张，每当预知要保存元素的多少时，在构造ArrayList实例时，就指定其容器，以避免数组扩容的发生，或者根据实际需求，通过调用ensureCapactiy方法来手动增加ArrayList实例的容量。



**add , addAll()**

add (E e) 向数组最后插入一个元素，add(int index, E e),向数组指定位置插入一个元素。每个操作都会导致capacity不足，因此，再添加元素之前，都需要进行剩余空间检查。如果需要则进行自动扩容。扩容的最终方法是通过`grow()`实现



##### set()

既然底层是一个数组 ArrayList 的`set()` 方法也就变得非常简单，直接对数组的指定位置赋值即可。

```java
public E set(int index, E element) {
  rangeCheck(idnex); // 下标越界检查
  E oldValue = elementData(index);
  elementData[index] = element; //赋值到指定位置，复制引用。
  return oldValue;
}
```



##### get()

`get()`方法同样很简单，唯一要注意的是由于底层数组是Object[]， 得到元素后需要进行类型转换。

```java 
public E get(int index) {
  rangeCheck(index);
  return (E) elementData[index]; //注意类型转换
}
```



##### remove()

`remove()` 方法也有两个版本，一个是`remove(int index)`删除指定位置的元素，另一个是`remove(Object o)`删除第一个满足`o.equals(elementData[index])`的元素。删除操作是`add()`操作的逆过程，需要将删除后的元素向前移动一个位置。需要注意的是为了让GC起作用，必须显示的为最后一个位置赋值`null`值

```java
public E remove(inte index) {
  rangeCheck(index);
  modCount++;
  E oldValue = elementData(index);
 	int numMoved = size - index -1;
  if(numMoved > 0) 
    System.arraycopy(elementData, index + 1, elementData, index, numMoved);
  elementData[--size] = null; // 清除该位置的引用，让GC起作用。
  return oldValue;
}
```

关于Java GC这里需要特别说明一下，**有了垃圾收集器并不意味着一定不会有内存泄漏**。对象能否被GC的依据是否还有其他引用指向它，上面的代码中如果不手动赋`null`值，除非对应的位置被其他元素覆盖，否则原来的对象就一定不会被回收。





## Collection-LinkedList源码解析

#### 概述

LinkedList同时实现了List接口和Deque接口，也就是说它既可以看作一个顺序容器，又可以看作一个队列(Queue)，同时又可以看作一个栈（Stack）。关于栈或者队列，现在的首选是ArrayDeque，它有着比LinkedList(当作栈或队列使用时)有着更好的性能。



### LinkedList实现

#### 底层数据结构

LinkedList底层**通过双向链表实现**。双向链表的每个节点用内部类Node表示。LinkedList通过`first`和`last`引用分别指向链表的第一个和最后一个元素。注意，当链表为空的时候`first`和`last`都指向`null`



#### add()

add()方法有两个版本，一个是`add(E e)`,该方法在LinkedList的末尾插入元素，因为有`last`指向链表末尾，在末尾插入元素的话费是常数时间，只需要简单的修改几个相关引用即可；另一个`add(int index, E element)`，该方法是在指定下表处插入元素，需要先通过线性查找到具体位置，然后修改相关引用，完成插入操作。



#### clear()

为了让GC更快可以回收放置的元素，需要将node之间的引用关系赋空



### Colletion - Stack & Queue源码解析

#### Stack & Queue概述

Java里有个叫做Stack的类，却没有叫做Queue的类（它是一个接口的名字）。但需要使用栈时，Java 已经不推荐使用Stack，而是推荐使用更高效的ArrayDeque；既然Queue只是一个接口，当需要使用队列时也就首选了ArrayDeque了（其次选LinkedList）



#### Queue

Queuej继承自Collection接口。除了最基本Collection的方法之外，它还支持额外的insertion，extraction，和inspection 操作。一组是抛出异常的实现；另一组是返回值的实现（没有则返回null）









