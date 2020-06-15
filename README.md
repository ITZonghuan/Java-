# Java- （重点参考 敖丙 大佬的github 地址是：https://github.com/AobingJava/JavaFamily）
适合秋招的后台开发岗复习
# 第一节 集合
ArrayList
面试问答：

①介绍一下ArrayList

它是JDK1.2出来的数组列表，主要用来装载数据，当装载的是基本数据类型，如int long boolean short byte...时，只能存储他们对应的包装类。它的底层实现是数组Object[] elementData. 主要特点就是：查询效率高，增删效率低，线程不安全。使用频率很高。

transient Object[] elementData 中的关键字transient是表示“将不需要序列化的属性前添加关键字transient，序列化对象的时候，这个属性就不会序列化到指定的目的地中，换句话说，这个字段的生命周期仅存于调用者的内存中而不会写到磁盘里持久化。”

与它类似的是LinkedList，和LinkedList相比，它的查找和访问元素速度较快，但新增，删除的速度较慢。

至于为啥线程不安全，还使用他呢？是因为正常的使用场景中，都是用来查询的，不太会涉及频繁的增删，如果实际场景中要涉及频繁的增删，可以使用JDK1.2版本出现的LinkedList，如果需要线程安全，可以使用JDK1.0版本就已经有的Vector，实际开发过程中，还是ArrayList使用的最多。

②说一下ArrayList的扩容机制吧

ArrayList可以通过构造方法在初始化的时候指定底层数组的大小。
通过无参构造初始化时，赋值底层Object[] elementData 为一个默认空数组Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {} 所以此时数组容量为0，只有真正对数据进行添加，调用add()函数时，才分配默认容量 DEFAULT_CAPACITY = 10

有参构造与无参构造如下：
public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
}

public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}

③数组是定长的，ArrayList是如何实现长度不受限制呢？

比如现在有一个长度为10的数组，现在需要增加一个元素，ArrayList会重新定义一个长度为 newCapacity = oldCapacity + (oldCapacity >> 1) 的数组，也即15个长度的新数组，然后复制原数组到新数组，这时再把指向原数组的地址换到新数组，实现扩容完成。至于为什么ArrayList的默认数组大小为什么是10？是sun公司程序员对一系列广泛使用的程序代码进行了调研，发现10这个长度的数组是最常用的最有效率的。

④ArrayList为啥增删慢？

ArrayList有制定新增和直接新增两种方式，在这之前会有一步校验长度的判断ensureCapacityInternal，如果长度不够，是需要扩容的，扩容时，老版本的jdk和8以后的版本是有区别的，8之后的效率更高了，采用位运算符，右移动一位，其实就是除以2的操作。

指定位置新增的时候，在校验之后的操作很简单，就是数组的copy。将index位置及其后面的所有元素全部后移一位，为index创建新的位置。当数据量大的时候，涉及到扩容和大量数据的移动，所以效率低，速度慢。

⑤ArrayList(int initialCapacity)会不会初始化数组大小？

答：不会初始化数组大小。
而且将构造函数与initialCapacity结合使用，然后使用set()会抛出IndexOutOfBoundsException异常，尽管该数组已经创建，但是大小设置不正确。

进行此工作的唯一方法就是在使用构造函数后，根据需要使用add()多次，会实现size++操作。

⑥ArrayList插入删除一定慢么？他的删除怎么实现的？

取决于删除的元素离数组末端有多远，ArrayList拿来作为堆栈来用还是挺合适的，push和pop操作完全不涉及数据移动操作。
删除的原理和新增是一样的，虽然叫删除，其实是数据的copy覆盖，将要删除的元素后面的所有元素往前移动一位，要删除的元素被成功覆盖，实现删除。

⑦ArrayList是线程安全的么？

当然不是，线程安全的版本的数组容器是Vector。
Vector的实现很简单，就是把所有的方法统统加上synchronized关键字就可以了。
也可以不使用Vector，用collections.synchronizedList把一个普通ArrayList包装成一个线程安全版本的数组容器也可以，原理同Vector一样，就是给所有方法套上一层synchronized。

⑧ArrayList用来做队列合适么？数组合适么？

不合适。
队列是FIFO，如果用ArrayList做队列，就需要在数组尾部追加数据，数组头部删除数据，反过来也可以。
但无论如何，都会有一个操作涉及到数组的数据搬迁，比较耗费性能。

数组非常合适做队列。
用两个偏移量来标记数组的读位置和写位置，如果超过长度就折回到数组开头，前提是定长数组。

⑨ArrayList的遍历和LinkedList遍历性能比较如何？

ArrayList要比LinkedList快得多，ArrayList遍历最大的优势在于内存的连续性，CPU的内部缓存结构会缓存连续的内存片段，可以大幅降低读取内存的性能开销。

⑩说一说ArrayList常用的方法

boolean add(E e)
将指定的元素添加到此列表的尾部。

void add(int index, E element)
将指定的元素插入此列表中的指定位置。

boolean addAll(Collection c)
按照指定 collection 的迭代器所返回的元素顺序，将该 collection 中的所有元素添加到此列表的尾部。

boolean addAll(int index, Collection c)
从指定的位置开始，将指定 collection 中的所有元素插入到此列表中。

void clear()
移除此列表中的所有元素。

Object clone()
返回此 ArrayList 实例的浅表副本。

boolean contains(Object o)
如果此列表中包含指定的元素，则返回 true。

void ensureCapacity(int minCapacity)
如有必要，增加此 ArrayList 实例的容量，以确保它至少能够容纳最小容量参数所指定的元素数。

E get(int index)
返回此列表中指定位置上的元素。

int indexOf(Object o)
返回此列表中首次出现的指定元素的索引，或如果此列表不包含元素，则返回 -1。

boolean isEmpty()
如果此列表中没有元素，则返回 true

int lastIndexOf(Object o)
返回此列表中最后一次出现的指定元素的索引，或如果此列表不包含索引，则返回 -1。

E remove(int index)
移除此列表中指定位置上的元素。

boolean remove(Object o)
移除此列表中首次出现的指定元素（如果存在）。

protected void removeRange(int fromIndex, int toIndex)
移除列表中索引在 fromIndex（包括）和 toIndex（不包括）之间的所有元素。

E set(int index, E element)
用指定的元素替代此列表中指定位置上的元素。

int size()
返回此列表中的元素数。

Object[] toArray()
按适当顺序（从第一个到最后一个元素）返回包含此列表中所有元素的数组。

T[] toArray(T[] a)
按适当顺序（从第一个到最后一个元素）返回包含此列表中所有元素的数组；返回数组的运行时类型是指定数组的运行时类型。

void trimToSize()
将此 ArrayList 实例的容量调整为列表的当前大小。

end！
