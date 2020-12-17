1. `ArrayList` 的初始化和扩容问题，还有缩容问题



`ArrayList` 在创建时是空的，在添加第一个元素的时候，才会进行初始化，初始化长度为10，扩容机制是每一次扩容，长度为自己的1.5倍。集合长度不·	会进行自动缩容，需要通过手动调用`trimTosize`方法，才会进行缩容，每次缩容为当前集合长度变为元素的数目，放掉多余不用的空间。



2. `hashMap` 的底层实现原理

Java集合初始化的值，会选择一个大于所选的值的 2 的幂，如果初始化为 25 那么会创建 长度为32 的数组。

一般是采用数组+单链表的形式进行实现的，每个节点存储一个键值对，在`JDK8` 中，当链表的长度超过8，链表就会自动转化为红黑树，这样是为了提高查找速率

`put` 方法的实质，都是以 `key`  作为主键，根据 `key` 的哈希值来进行判断他是插入那个桶中，具体判断方法`hashmap` 对一个对象的哈希码再次取了哈希值

```java
// 二次哈希 
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
}
```

然后根据哈希值对数组长度进行取模判断数组插入位置 `hash & length - 1` 相当于取模  ，如果不进行二次哈希，哈希码每次应用的只有低位，高位是不参与运算的，这样就导致数据传入的参数比较少，最后取得的模值容易相同，可能会导致最后所有对象都放在一个桶上，容易造成哈希冲突。所有进行了二次哈希。经过二次哈希之后，哈希码的高位和低位都用上了，这样会获得更优的哈希码，这样会提高效率。这其实是解决哈希冲突的一种方法，再哈希法。

根据哈希码找到相应的桶之后，然后判断桶中的链表，`JDK7` 是通过头插的 ，`JDK8` 就变成了尾插，因为在多线程情况下，头插可能会产生环。虽然`hashmap` 是线程不安全的，但是开发人员还是更改了这个情况。

初始化的过程和扩容的过程

`hashmap` 在初始化的时候是空的，在添加第一组键值对的时候，才会进行初始化，长度16 ， 扩容时机，长度每次扩容二倍。扩容之后重新计算哈希值，再次对数组进行填充。重新插入之后，每个链表的相对顺序都反过来了，因为是尾插的原因，也是从尾部开始。

扩容时机，当哈希表的负载因子超过0.75之后，此时发生哈希碰撞，就会扩容。

负载因子等于哈希表中总元素数量/哈希表长度



3. 关于集合是否线程安全的问题



Java 中线程安全的集合一共分三类

老一辈：`Vector`   采用 `synchronized` 关键字 ，使用锁来实现线程安全的 ，效率十分慢集合已经不用了 

```java
public synchronized boolean add(E e) {
    modCount++;
    ensureCapacityHelper(elementCount + 1);
    elementData[elementCount++] = e;
    return true;
}
```

`Collections` 类中给出线程安全的集合，同样也是采用锁进行实现的

这些线程安全的集合都是属于`Collections` 类的内部类，他们都属于 `SynchronizedCollection` 的子类，都是继承了该类中的线程安全的方法

```java
public int size() {
    synchronized (mutex) {return c.size();}
}
public boolean isEmpty() {
    synchronized (mutex) {return c.isEmpty();}
}
public boolean contains(Object o) {
    synchronized (mutex) {return c.contains(o);}
}
public Object[] toArray() {
    synchronized (mutex) {return c.toArray();}
}
public <T> T[] toArray(T[] a) {
    synchronized (mutex) {return c.toArray(a);}
}

public Iterator<E> iterator() {
    return c.iterator(); // Must be manually synched by user!
}

public boolean add(E e) {
    synchronized (mutex) {return c.add(e);}
}
public boolean remove(Object o) {
    synchronized (mutex) {return c.remove(o);}
}

public boolean containsAll(Collection<?> coll) {
    synchronized (mutex) {return c.containsAll(coll);}
}
public boolean addAll(Collection<? extends E> coll) {
    synchronized (mutex) {return c.addAll(coll);}
}
public boolean removeAll(Collection<?> coll) {
    synchronized (mutex) {return c.removeAll(coll);}
}
public boolean retainAll(Collection<?> coll) {
    synchronized (mutex) {return c.retainAll(coll);}
}
public void clear() {
    synchronized (mutex) {c.clear();}
}
public String toString() {
    synchronized (mutex) {return c.toString();}
}
```

可以看到该类中线程安全的方法都是将关键代码块同步来实现的线程安全的，所以这些方法执行效率也不是很高。

第三类：并发包中的集合

`CopyOnWriteArrayList`  采用了写时复制的方法实现的线程安全，

写时复制其实就是字面理解，要修改一个公共变量，先将公共变量复制出来，对当前变量修改，然后将指针从原变量直线当前修改过的变量

```java
public void add(int index, E element) {
    final ReentrantLock lock = this.lock;
    lock.lock();
    try {
        Object[] elements = getArray();
        int len = elements.length;
        if (index > len || index < 0)
            throw new IndexOutOfBoundsException("Index: "+index+
                                                ", Size: "+len);
        Object[] newElements;
        int numMoved = len - index;
        if (numMoved == 0)
            newElements = Arrays.copyOf(elements, len + 1);
        else {
            newElements = new Object[len + 1];
            System.arraycopy(elements, 0, newElements, 0, index);
            System.arraycopy(elements, index, newElements, index + 1,
                             numMoved);
        }
        newElements[index] = element;
        setArray(newElements);
    } finally {
        lock.unlock();
    }
}
// 可以发现这里也用到了 ReentrantLock 锁 
// 写时复制的好处将读写分离，尽管在多线程的情况下，每个线程修改的都不是原来的共享数据的值，所以读操作根本不需要进行加锁，只要写操作需要进行加锁。
// 写操作就是将原数组copy一份，然后在这份副本上进行修改，修改后将原指针指向当前副本。
// 因为多线程过程中访问集合的多数情况下都是读，写操作比较少，所以提高了速度
```





线程安全的集合 

1. `List` 系列    

   -   `Vector`集合 
   -  `SynchronizedList` 集合 
   -    `CopyOnWriteArrayList` 集合 

2. `Set` 系列

   - `SynchronizedSet` 集合
   - `CopyOnWriteArraySet` 集合 
   - `ConcurrentSkipListSet` 集合  

3. `Map` 系列 

   - `HashTable` 集合

   - `SynchronizedMap` 集合

   - `CopyOnWriteMap` 集合 **  此集合在Java中并没有实现

   - `ConcurrentHashMap` 集合   

     采用锁分段技术，减少锁的粒度，效率高，其中该集合锁只会锁住一个桶，这样力度就会非常小

   - `ConcurrentSkipListMap` 集合 ，该集合是采用跳表`SkipList`来实现的，与上面的`ConcurrentHashMap` 相比，该集合更支持并发的场景，切并发越高的情况下，该集合的性能越好。

   

4. `Queue` 系列 

   - 阻塞队列    当队列是空的时候，如果线程想要获取队列中的元素，那么线程会进入阻塞状态，同样地，如果队列已经满了，线程要想添加元素，也会进入阻塞状态。在Java中阻塞队列有很多，比如 `ArrayBlockingQueue` ，注意阻塞队列在初始化的时候要初始化队列的长度。

   - 非阻塞队列   `ConcurrentLinkedQueue` ，非阻塞队列的长度不受限制的，所以要注意栈溢出。采用`CAS` 算法的思路进行实现 

5. `Stack` 系列 

   - `Stack` 集合 





Java集合中`Map` 和 `Set` 有很大的关联，因为在Java中`Set` 都是由`Map` 实现的，`Set` 的值作为`map` 的`key` 