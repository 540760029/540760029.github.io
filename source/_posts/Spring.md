---
title: Spring
description: Spring 系列文章
copyright: false
date: 2022-09-03 20:43:13
tags: spring
categories: Spring 系列
---

ArrayList:

底层通过数组实现

初始容量：10

扩容  1.5倍

LinkedList 双向链表



hashMap

初始容量：16

最大容量： 2<sup>30</sup>

默认加载因子：0.75

转换成树的阈值：8

取消树的阈值：6

对容器进行树化的最小容量：64

《*代码大全2*》

《*黑客与画家*》

《*万历十五年*》

全球通史

浮生六记

白夜行

百年孤独

少年得到

java7

默认容量是16 必须是2的幂次方，不是的话向上调整为2的幂

-为什么2的幂

  hash &( length-1 ) 

length-1 二进制全为1 进行按位与，快速拿到数组的下标

负载因子0.75

- 时间和空间提供的折中

当数据到达容量*负载因子的时候，扩容一倍



允许传入key 为null 的值，放在第0位

**get()方法**

首先通过key 取hash,取hash用的是 hash(k)&(length-1), 得到桶的位置，在遍历冲突链，判断key是否相等返回value值 

**put()方法**

将对应的key 和value 放到map里，会做一次查找，看是否包含该元组，如果找到直接返回，找不到，则插入新的entry,插入的方式是头插法。

先扩容再插入元素

**remove()方法**

删除key对应的entry，找到key,删除





java8 的改进

数组+链表+红黑树

当链表的元素到达8个时，会将链表转换为红黑树，在这些位置进行查找时可以降低时间复杂度为O(logN)。

put()

第一次put 的时候会触发resize 操作进行初始化数组长度

找到具体的数组下标，如果此位置没有值，那么直接初始化一个node,并放在这个位置

如果有数据，首先判断该位置的第一个数据和我们要插入的数据，key 是不是相等，如果是，覆盖value，并取出返回这个节点旧值，

如果这个节点是红黑树的节点，调用红黑树的插入方法，如果不是，则为链表，插入到链表的最后面如果插入的是链表中的第8个，会触发转红黑树的操作

如果插入的值导致了size 超过了阈值，触发扩容操作。每次扩容容量为原来的2倍，并进行数据迁移
： 将数组扩大一倍，将阈值扩大一倍 ，遍历数组，进行数据迁移

get()

计算key的hash值找到数组下标

判断数组该位置的key 是不是刚好是要找的值，如果不是

判断元素类型是否为TreeNode,如果是，用红黑树的方法读取数据，如果不是

遍历链表，直到找到相等的key,返回value

concurrentHashMap:

 1.7 

采用分段锁技术，默认情况下将数组分成16个段， 每个段内的结构类似于hashMap,有数组和链表组成

put()

再进行put 操作的过程中，通过key 的hash值来确定具体的桶位置，如果这个桶没有初始化，则用第0个segment的数据进行初始化，

存储值的时候，首先要对这个segment进行加锁，如果被其他线程获得，则调用scanAndLockForPut()自旋获直到获取锁为止，然后通过 hash&(length-1)找到具体的数组下标，没有元素，则放入该键值对，有值就进行遍历，看有没有相同的key,覆盖value 返回旧值，没有相同key 的时候，先判断需不需要进行扩容，扩容后插入该值，不需要则插到头节点，然后释放锁



1.8

采用 数组+链表+红黑树的方式实现，加锁采用的是CAS+synchronized实现

put()

1. 传入一个不为空的键值对，如果为空就直接报错了。hashMap 是可以为空的

- 基于并发考虑，hashMap 在get 到空值的时候，可以通过contains(key )来判断是否包这个key

    而并发的map,在get(key)时候得null 值，不能判断是映射的value是null,还是没有找到对应的key,

    并发环境下contains(key)不靠谱

2. 接着判断table是否为空，如果为空就进入初始化阶段       
3. 如果判断数组中某个指定的桶式空的，那就直接把键值对插入到桶中作为头节点，这个操作没锁
4. 如果要插入的桶中的hash 值为-1，也就是MOVED 状态，那就说明有线程正在进行扩容操作，那么当前线程就进入协助扩容阶段
5. 需要把数据插入到链表或者树中，如果这个节点是一个链表节点，那么就遍历这个链表，如果发现有相同的key值就更新value,如果遍历完了都没有发现相同的key,就需要在链表的尾部插入该数据，插入结束后判断该链表节点个数是否大于8，并且size>=64就进行树化，否则只进行扩容操作
6. 如果这个节点是一个红黑树的节点，那就需要按照树的插入规则进行插入。
7. put结束后，需要给map 已存储的数量+1，在addCount方法中判断是否需要扩容

```java
 final V putVal(K key, V value, boolean onlyIfAbsent) {
        
         if (key == null || value == null) throw new NullPointerException();
         //hash值   
         int hash = spread(key.hashCode());
         //记录相应链表的长度   
         int binCount = 0;
     	//无限循环状态
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh;
            
            //tab如果为空就进行初始化
            if (tab == null || (n = tab.length) == 0)
                tab = initTable();
           
            //找到hash值对应的数组下标，得到第一个节点f
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                //如果数组位置为空，用一次cas操作将这个新值放入其中即可，这个put
                //如果cas失败，那就是有并发操作，进到下一个循环就好了。
                if (casTabAt(tab, i, null,
                             new Node<K,V>(hash, key, value, null)))
                    break;                   // no lock when adding to empty bin
            }
            
            // hash 居然可以等于 MOVED，这个需要到后面才能看明白，
            // 不过从名字上也能猜到，肯定是因为在扩容
             else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
            //====================================================
            // 走到这说明 这个位置不空
             else {
                V oldVal = null;
                 //获取头节点的监视器锁
                synchronized (f) {
                    
                    if (tabAt(tab, i) == f) {
                        //------------------------
                        //头节点的hash大于0 说明式链表
                        if (fh >= 0) {
                            //链表的长度
                            binCount = 1;
                            //遍历链表
                            for (Node<K,V> e = f;; ++binCount) {
                                K ek;
                                //如果发现了相等的key,覆盖value，返回旧value
                                if (e.hash == hash &&
                                    ((ek = e.key) == key ||
                                     (ek != null && key.equals(ek)))) {
                                    oldVal = e.val;
                                    if (!onlyIfAbsent)
                                        e.val = value;
                                    break;
                                }
                                //不相等且遍历到了最后，则插入链表的末端
                                Node<K,V> pred = e;
                                if ((e = e.next) == null) {
                                    pred.next = new Node<K,V>(hash, key,
                                                              value, null);
                                    break;
                                }
                            }
                        }
                        
                        //---------------------------------------
                        //如果是红黑树
                        else if (f instanceof TreeBin) {
                            Node<K,V> p;
                            binCount = 2;
                            //调用红黑树的方法插入新节点
                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                           value)) != null) {
                                oldVal = p.val;
                                if (!onlyIfAbsent)
                                    p.val = value;
                            }
                        }
                    }
                }
                 
                 //
                if (binCount != 0) {
                    //如果链表的长度大于阈值，树化
                    if (binCount >= TREEIFY_THRESHOLD)
                        //如果长度小于64 是不会进行树化的，只能进行扩容操作
                        treeifyBin(tab, i);
                    if (oldVal != null)
                        return oldVal;
                    break;
                }
            }
            
            
         
```

get()

通过key 的hash,找到key所在的数组下标，如果不为空并且头节点就是要找的值，直接返回该值

如果 hash<0 则调用node的find方法找到与key 相同的节点,找不到返回null

如果不是，则调用链表的查找方法，找到该值。找不到返回null,







Synchronized

同一时刻，最多只有一个线程执行该段代码，以达到保证并发安全的效果





