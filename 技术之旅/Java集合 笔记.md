## 概括

### 1. 关系

![Java集合-2](./img/Java集合-2.png)

###2. 要点

* Collection

  * List

    * ArrayList： 底层数据结构是数组，查询快，增删慢；线程不安全，效率高；
    * Vector： 底层数据结构是数组，查询快，增删慢；线程安全，效率低（几乎不会使用）；
    * LinkedList： 底层数据结构是链表，查询慢，增删快；线程不安全，效率高；

  * Set

    * HashSet： 
      1. 不能保证元素顺序，不可重复，不是线程安全，元素可以为null；
      2. 底层其实是一个数组，为了加快查询速度。索引就是通过hash值转换，即index=hash(hashCode)；
    * LinkedHashSet：不可重复，有序；
    * TreeSet： 有序，不可重复，底层使用红黑树算法，擅长范围查询；

    <font color=red>注：实现线程安全的方法： `Set set = Collections.synchronizedSet(setObj);`</font>