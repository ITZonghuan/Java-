# Java- （重点参考 敖丙 大佬的github 地址是：https://github.com/AobingJava/JavaFamily）
适合秋招的后台开发岗复习
# 第一节 集合
1、ArrayList
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

⑤
    
