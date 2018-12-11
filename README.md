# JdkCodeTree
Java源代码阅读之Object,String,List,Map

###Object类

![](https://i.imgur.com/RI5eo02.png)

![](https://i.imgur.com/yS7vOOi.png)

<pre>
Object
</pre>

###String类

![](https://i.imgur.com/6mncKyx.png)

![](https://i.imgur.com/8OJsns1.png)

<pre>
String
</pre>

###List类

![](https://i.imgur.com/7A82kyn.png)

<pre>
List

      ArrayList, Vector, LinkedList

           Arraylist和Vector是采用数组方式存储数据，此数组元素数大于实际存储的数据以便
           增加插入元素，都允许直接序号索引元素，但是插入数据要涉及到数组元素移动等内存
           操作，所以插入数据慢，查找有下标，所以查询数据快，Vector由于使用
           了synchronized方法-线程安全，所以性能上比ArrayList要差，LinkedList使用双
           向链表实现存储，按序号索引数据需要进行向前或向后遍历，但是插入数据时只需要记录
           本项前后项即可，插入数据较快

      List是有序的Collection，使用此接口能够精确的控制每个元素插入的位置。用户能够使用索
          引（元素在List中的位置，类似于数组下标）来访问List中的元素，这类似于Java的数组

      ArrayList类
　　      ArrayList实现了可变大小的数组。它允许所有元素，包括null。ArrayList没有同步，
         每个ArrayList实例都有一个容量（Capacity），即用于存储元素的数组的大小。这个容
         量可随着不断添加新元素而自动增加

      Vector类
         Vector非常类似ArrayList，但是Vector是同步的。由Vector创建的Iterator，虽然
         和ArrayList创建的Iterator是同一接口，但是，因为Vector是同步的，当一个
         Iterator被创建而且正在被使用，另一个线程改变了Vector的状态（例如，添加或删除了
         一些元素），这时调用Iterator的方法时将抛出ConcurrentModificationException
         ，因此必须捕获该异常
</pre>

###Hash类

![](https://i.imgur.com/T0laCEh.png)

rehash

![](https://i.imgur.com/4XeBYiV.png)

<pre>
HashMap扩容

      HashMap内部实现是一个桶数组，每个桶中存放着一个单链表的头结点。其中每个节点存储的是一
   个键值对整体（Entry），HashMap采用拉链法来解决Hash冲突。

      哈希冲突无法完全避免，因此为了提高HashMap的性能，HashMap不得尽量缓解哈希冲突以缩
   短每个桶的外挂链表长度。

      频繁产生哈希冲突最重要的原因就像是要存储的Entry太多，而桶不够，因此，当HashMap中的
   存储的Entry较多的时候，我们就要考虑增加桶的数量，这样对于后续要存储的Entry来讲，就会
   大大缓解哈希冲突

   HashMap的扩容时机

      带参数的构造函数：
               public HashMap(int initialCapacity, float loadFactor) 
                 
               第一个参数：初始容量，指明初始的桶的个数；相当于桶数组的大小。
               第二个参数：装载因子，是一个0-1之间的系数，根据它来确定需要扩容的阈值，
                          默认值是0.75

      扩容触发的机制：
               当map中包含的Entry的数量大于等于threshold = loadFactor * capacity的
               时候，且新建的Entry刚好落在一个非空的桶上，此刻触发扩容机制，将其容量扩
               大为2倍

      这里很容易就想到多线程情况下，隐约感觉这个transfer方法在多线程环境下会乱套。事实上
      也是这样的，由于缺乏同步机制，当多个线程同时resize的时候，某个线程t所持有的引用
      next（参考上面代码next指向原桶数组中某个桶外挂单链表的下一个需要转移的Entry），可
      能已经被转移到了新桶数组中，那么最后该线程t实际上在对新的桶数组进行transfer操作。

      如果有更多的线程出现这种情况，那很可能出现大量线程都在对新桶数组进行transfer，那么
      就会出现多个线程对同一链表无限进行链表反转的操作，极易造成死循环，数据丢失等等，因
      此HashMap不是线程安全的，考虑在多线程环境下使用并发工具包下的ConcurrentHashMap


      HashTable  VS  HashMap  VS  ConcurrentHashMap

      HashTable:
           1）底层数组+链表实现，无论key还是value都不能为null，线程安全，实现线程安全的方
           式是在修改数据时锁住整个HashTable，效率低，ConcurrentHashMap做了相关优化。

      HashMap：
           1）底层数组+链表实现，可以存储null键和null值，线程不安全。
           2）扩容针对整个Map，每次扩容时，原来数组中的元素依次重新计算存放位置，并重新插

      ConcurrentHashMap
           1）底层采用分段的数组+链表实现，线程安全
           2）通过把整个Map分为N个Segment，可以提供相同的线程安全，但是效率提升N倍，默
           认提升16倍。(读操作不加锁，由于HashEntry的value变量是 volatile的，也能保证
           读取到最新的值。
           3）Hashtable的synchronized是针对整张Hash表的，即每次锁住整张表让线程
           独占，ConcurrentHashMap允许多个修改操作并发进行，其关键在于使用了锁分离技
           术。
           5）扩容：段内扩容（段内元素超过该段对应Entry数组长度的75%触发扩容，不会对整
           个Map进行扩容），插入前检测需不需要扩容，有效避免无效扩容
</pre>

###Integer类

![](https://i.imgur.com/lfr5cXw.png)

![](https://i.imgur.com/EUvQder.png)

<pre>
Integer类
</pre>