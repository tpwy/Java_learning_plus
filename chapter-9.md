# 第九章 容器类

## 9.1 Java容器类框架

+ 容器类是Java以类库的形式供用户开发程序时可直接使用的各种数据结构(Data Structures)
+ 注意，在课本教材中，为了防止名字冲突将`Collection`翻译作容器，而将`Set`翻译作集合。
+ 数据结构是可以存储对象的集合，在这里对象也称为元素
+ 与现代的数据结构类库的常见情况一样，Java容器类库也将接口(interface)与实现(implemention)分离
+ 从JDK5开始，容器框架全部采用泛型实现，并存放在`java.util`包中

容器框架中的接口和实现接口的类的继承关系

+ java.lang.Object
    + java.lang.Iterable(可迭代接口)
        + Collection(容器接口)
            + List(列表接口)
                + LinkedList(链表类)
                + ArrayList(数组列表类)
            + Set(集合接口)
                + HashSet(哈希集合类)
                    + LinkedHashSet(链表哈希集合类)
                + SortedSet(有序集合接口)
                    + TreeSet(树集合类)
    + Map(映射接口)
        + HashMap(哈希映射类)
        + SortedMap(有序映射接口)
            + TreeMap(数映射类)
    + Iterator(迭代器接口)
        + ListIterator(列表迭代器接口)

## 9.2 Collection接口

| 方法 | 功能说明 |
|-|-|
| int size() | 返回容器中元素的个数 |
| boolean isEmpty() | 判断容器是否为空 |
| boolean contains(Object obj) | 判断容器是否含有元素obj |
| boolean add(E element) | 向同期添加元素element，添加成功返回true,若容器中已包含element且不允许有重复元素，则返回false |
| int hasCode() | 返回容器的哈希码值 |
| Object[] toArray() | 将容器转换为数组 |
| boolean remove(Object obj) | 从容器中删除元素obj，若删除成功则返回true，如容器不包含obj则返回false |
| void clear() | 删除容器中所有元素 |
| Iterator<E> iterator() |返回容器的迭代器 |
| boolean equals(Object obj) | 比较该容器是否与obj相等 |
| void shuffle(List<?> list) | 以随机方式重排list中的元素 |
| boolean containsAll(Collection<?> c) | 判断当前容器中是否包含容器c的所有元素 |
| boolean addAll(Collection<? extends E> c) | 将容器c中的所有元素添加到该容器中，为集合并运算 |
| boolean removeAll(Collection<?> c) | 在该容器中删除所有容器c包含的元素，为集合差运算 |
| boolean remainAll(Collection<?> c) | 在该容器中仅保留容器c包含的元素，为集合交运算 |

## 9.2 迭代器

iterator 接口

| 方法 | 功能说明 |
|-|-|
| public abstract boolean hasNext() | 判断是否还有后续元素，有则返回true |
| public abstract E next() | 返回后续元素 |
| public abstract void remove() | 删除迭代器当前指向的元素(即最具一次next()或previous()方法调用返回的元素) |
| default void forEachRemaining(Consumer<? super E> action) | 对迭代器的每一个元素调用lambda表达式直到没有元素为止 |

ListIterator支持对List对象的双向遍历

| 方法 | 功能说明 |
|-|-|
| public abstract boolean hasPrevious | 判断是否有前驱元素 |
| public abstract E previous | 返回前驱元素 |
| public abstract void add(E e) | 若next()方法的返回值非空，该元素被插入到next()方法返回的元素之前；若previous()方法的返回值非空，该元素被插入到previous()方法返回的元素之后；若元素没有元素，则将该元素加入其中 |
| public abstract void set(E e) | 用元素e替换列表的当前元素 |
| public abstract int nextIndex() | 返回基于next()调用的元素符号 |
| public abstract int previousIndex() | 返回基于previous()调用的元素符号 |

Java集合类库中的迭代器与其他类库的迭代器在概念上有重要的区别，在传统的集合类库中(C++)，迭代器是根据数组索引建模的，如果给定这样一个迭代器，就可以查看指定位置上的元素，就像知道数组索引i就可以查看数组元素a[i]一样，不需要查找元素就可以将迭代器向前移动一个位置，这与不需要执行查找操作就可以通过i++将数组索引向前移动一样。但是Java迭代器中查找操作和位置变更是紧密相连的，查找元素的唯一方法是调用next，而执行查找操作的同时，迭代器的位置随之向前移动

可以理解成迭代器位于两个元素之间，但调用next时，迭代器就越过一个元素，并返回这个元素的引用

## 9.3 列表接口List

List<E>接口的常用方法

| 方法 | 功能说明 |
|-|-|
| E get(int index) |
| E set(int index, E element)
| int intdexOf(Object o) | 返回元素o首次出现的序号，若o不存在则返回-1 |
| int lastIndexOf(Object o) | 返回元素ｏ最后出现的序号 ｜
| void add(int index, E element) 
| boolean add(E element) |
| E remove(int index) |
| boolean addAll(Collection<? extends E> c)
| boolean addAll(int index, Collection<? extends E> c)
| ListIterator<E> listIterator() |
| ListIterator<E> listIterator(int index)


实现List接口的类有两个：

+ 链表类LinkedList
    + 采用链表结构保存对象，使用循环双链表实现List
    + 这种结构向链表中任意位置插入、删除元素时不需要移动其他元素，
    + 链表的大小可以动态增大或减小，但不具有随机存取特性
+ 数组列表ArrayList。
    + 使用一维数组实现List，也是可变数组，允许所有元素。
    + 插入删除元素需要移动其他元素，在元素很多时，操作较慢。
    + 向ArrayList添加元素时，容器会自动增大，但不能自动缩小，可以使用trimToSize()方法将数组的容量减小到数组列表的大小。
    + 具有随机存取特性

选用原则:

+ 若是通过下标随机访问元素，但除了末尾外不在其他位置插入或删除元素，则选择ArrayList类
+ 若需要在线性表的任意位置上进行插入或删除操作，则选择LinkedList类

### 9.3.1 LinkedList

LinkedList<E>类的构造方法

| 构造方法 | 功能说明 |
|-|-|
| public LinkedList() | 
| public LinkedList(Collection<? extends E> c) | 创建包含容量c中所有元素的链表 |

LinkedList<E> 类的常用方法

| 构造方法 | 功能说明 |
|-|-|
| public void addFirst(E e)
| public void addList(E e)
| public E getFirst() 
| public E getLast()
| public E removeFirst() 
| public E removeLast() 

### 9.3.2 ArrayList

Array<E>类的构造方法

| 构造方法 | 功能说明 |
|-|-|
| public ArrayList |
| public ArrayList(int initialCapacity) | 创建容量为initialCapacity的数组 |
| public ArrayList(Collection <? extends E> c) |

ArrayList<E> 类的常用方法

| 方法 | 功能说明 |
|-|-|
| public void trimToSize() | 将ArrayList对象的容量缩小到该列表的当前大小 |


## 9.4 集合接口set

Set是不含重复元素的集合接口，Set集合中的对象不按特定的方式排序，只是简单的把对象加入集合中即可，但加入的对象一定不能重复。集合中元素的顺序与元素加入集合的顺序无关。

实现Set接口的两个主要类是哈希集合HashSet和树集合TreeSet

### 9.4.2 散列集

+ 散列表(hash table)可以快速地查找所需要的对象。
+ 散列表为每个对象计算一个整数，称为散列码(hash code)
+ 注意在自定义类时自己实现的hashCode方法应该和equals方法兼容
+ Java集合类库提供了一个HashSet类实现了基于散列表的集

### 9.4.1 哈希集合HashSet

说明：构造方法中的上座率也称装填因子，上座率的值在0.1~1.0之间，表示集合的饱满度，当集合中的元素个数超过了容量与上座率的成绩，容量就会自动翻倍

HashSet<E>类的常用构造方法

| 构造方法 | 功能说明 |
|-|-|
| public HashSet() | 创建默认初始容量是16，默认上座率是0.75的空哈希集合 |
| public HashSet(int initialCapacity) | 创建初始容量是initialCapacity，默认上座率是0.75的空哈希集合 |
| public HashSet(int initialCapacity, float loadFactor) | 创建初始容量是initialCapacity，上座率是loadFactor的空哈希集合 |
| public HashSet(Collection<? extends E> c) | 创建包含容器c中的所有元素，默认上座率为0.75的哈希集合 |

HashSet<E>类的常用方法

| 方法 | 功能说明 |
|-|-|
| public boolean add(E e) | 若集合中未包含元素e，添加该元素并返回true；若集合中已包含该元素则返回false |
| public void clear() | 
| public boolean contains(Object o) | 
| public int size() | 

### 9.4.2 树集合TreeSet

TreeSet的工作原理与HashSet相似，但TreeSetui是一个有序集合(sorted collection),可以以任意顺序将元素插入到集合中，在对集合进行遍历时每个值将自动地按照排序后的顺序呈现。

TreeSet<E>类常用的构造方法

| 构造方法 | 功能说明 |
|-|-|
| public TreeSet() | 
| public TreeSet(Collection<? extends E> c) |

TreeSet<E> 类的常用方法 

| 方法 | 功能说明 |
|-|-|
| public E first() |
| public E second() |
| public SortedSet<E> headSet(E toElement) | 返回一个新集合，新集合元素是toElement之前的所有元素 |
| public SortedSet<E> tailSet(E fromElement) | 新集合元素包含fromElement和from之后的所有元素 |
| public SortedSet<E> subSet(E fromElement, toElement) | 新集合元素包含从fromElement到toElement(不包含toElement)之后的所有元素 |
| public E lower(E, e) | 返回严格小于给定元素e的最大元素，如果不存在则返回null |
| public E higher(E, e) | 
| public E floor(E, e) | 返回严格小于或等于给定元素e的最大元素，如果不存在则返回null |
| public E ceiling(E, e) | 

## 9.5 映射接口Map

+ Map中的元素都是成对出现的，它提供了键(key)到值(value)的映射
+ 值是要存入Mapo中的元素，键决定了元素在Map中的存储位置。
+ 一个键和它对应的值构成一个条目，真正在Map中存储的是这个条目
+ Map中的每一个键都是唯一的，且每个键并映射到一个值。

Map<K, V> 接口的常用方法

| 方法 | 功能说明 |
|-|-|
| V put (K key , V value) | 以K为键向Map中添加值为value的元素，若key已存在，新添加的值会取代原来的值 |
| void putAll(Map<? extends K, ? extends V> m) | 将映射m中的所有映射关系复制到该映射中 |
| boolean containsKey(Object key) | 判断是否包含指定的键key |
| boolean containsValue(Object value) | 
| V get(Object key) | 返回键key所映射的值，key不存在则返回null |
| Set<K> keySet() | 返回该集合中所有键对象形成的set集合 |
| Collection<V> values() | 返回该集合中所有键对象形成的Collection集合 |
| V remove(Object key) | 将键为key的条目，从Map对象中删除 |
| Set<Map.Entry<K, V>> entrySet() | 返回映射中的键-值对的集合 |

映射接口常用的实现类有
+ HashMap
    + 通过哈希码对其内部的映射关系进行快速查找，因此添加和删除映射关系效率高
    + 允许使用null值和null键，但必须保证键的唯一性
+ TreeMap
    + TreeMap中的映射关系存在一定的顺序
    + 不允许键对象是null值

## 9.5.1 HashMap<K, V>类

HashMap<K, V> 类常用的构造方法

| 构造方法 | 功能说明 |
|-|-|
| public HashMap() | 构造一个具有默认初始容量(16)，默认上座率(0.75)的空HashMap对象 |
| public HashMap(int initailaCapatity) |
| public HashMap (Map <? extends K, ? extends V> m) |

## 9.5.2 TreeMap<K, V>

TreeMap<K, V>类构造方法

| 构造方法 | 功能说明 |
|-|-|
| public TreeMap() |
| public TreeeMap(Map <?extends K, ?extends V> m) |

TreeMap<K, V>类常用的方法

| 方法 | 功能说明 |
|-|-|
| public K firstKey() |
| public K lastKey() |
| public SortedMap<K, V> headMap(K toKey) |
| public SortedMap<K, V> tailMap(K fromKey) |
| public K lowerKey(K key) |
| public K floorKey(K key) |
| public K higherKey(K key) |
| public K ceilingKey(K key) |

## 9.6 扩展

+ Deque(队列)接口——提供双端队列
    + ArrayDeque类
    + LinkedList类

+ 优先级队列(PriorityQueue)——元素可以按照任意顺序插入，却总是按照排序的顺序进行检索
+ LinkedHashSet和LinkedHashMap类用来记住插入元素项的顺序。且链接散列映射将使用访问顺序而不是插入顺序来进行迭代
+ EnumSet是一个枚举类型元素集的高效实现，EnumMap是一个键类型为枚举类型的映射
+ 类IndentityHashMap调用System.indentityHashCode方法计算键的散列值(根据对象的内存地址计算散列码)，所以对两个对象进行比较时使用`==`而不是`equals`。不同的键对象，即使内容相同，也被视为是不同的对象。

### 9.6.1 映射视图

+ 集合框架不认为映射本身是一个集合。(其他数据结构框架认为映射是一个键/值对集合，或者是由键索引的值集合)。不过可以得到映射的视图
+ 有3种视图：键集、值集合(不是一个集)以及键/值对集
+ 如果在键集视图上调用迭代器的remove方法，事实上会从映射中删除这个键和与它关联的值，但不能想键集视图中增加元素

### 9.6.2 弱散列映射

+ 如果对某个键的最后一次引用已经消亡，不再有任何途径引用这个值的对象了，但垃圾回收器无法回收这些无用的值
+ WeakHashMap使用弱引用(weak reference)来保存键。

## 9.7 视图与包装器

+ 使用视图(views)可以获得其他的实现了Collection接口和Map接口的对象。如keySet方法
+ 但这里并没有创建了一个新集，而是返回一个实现Set接口的类对象，这个对象能够对原映射进行操作。这种集合称为视图

### 9.7.1 轻量级集合包装器

Arrays类的静态方法asList将返回一个包装了普通Java数组的List包装器，这个方法可以将数组传递给一个期望得到列表或集合参数的方法

```
Card[] cardDeck = new Card[52];
...
List<Card> cardList = Arrays.asList(cardDeck);  
// cardList是一个视图对象，带有访问底层数组的get和set方法
// 改变数组大小的方法都会抛出Unsupported OperationException异常

Collections.singleton(anObeject) 
// 将返回一个试图对象，实现了Set接口，一个不可修改的单元素集。

// 对于集合框架的每一个接口，还有一些方法能生成空集、列表、映射。同时还可以吹接推导出集的类型
Set<String> deepThoughts = Collections.emptySet();
```

### 9.7.2 子范围

+ 可以为很多集合建立子范围(subrange)视图，即使用subList方法获得一个列表的自范围视图
+ 对于有序集和映射，可以使用排序顺序而不是元素位置建立子范围
+ Java SE6 引入的NavigableSet接口赋予子范围操作更多的控制能力

### 9.7.3 不可修改的视图

8中方法获得不可修改视图：

Collections.unmodifiableCollection
Collections.unmodifiableList
Collections.unmodifiableSet
Collections.unmodifiableSortedSet
Collections.unmodifiableNavigableSet
Collections.unmodifiableMap
Collections.unmodifiableSortedMap
Collections.unmodifiableNavigableMap


+ 不可修改视图不是指不是集合本身不可修改，通过集合的原始引用仍然可以对集合进行修改，并且仍然可以让集合的元素调用更改器方法
+ 由于视图只是包装了接口而不是实际的集合对象，因此只能访问接口中定义的方法
+ unmodifiableCollection方法将返回一个集合，它的equals方法不调用底层集合的equals方法，而是继承Object类的equals方法

### 9.7.4 同步视图

+ 类库的设计者使用视图机制来确保常规集合的线程安全，而不是实现线程安全的集合类
+ Collections类中的静态`synchronizedMap`方法可以将任何一个映射表转换成具有同步访问方法的Map

### 9.7.5 受查视图

+ "受查"视图用于对泛型类型发生问题时提供调试支持
+ 错误的add命令在运行时检测不到，但调用get方法时候就会抛出异常
+ 使用`checkedList`方法转换后的列表在插入时就会检测插入的对象是否属于给定的类
+ 受查视图仅限于虚拟机可以运行的运行时检查