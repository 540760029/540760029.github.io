---
title: 插入排序
description: 插入排序需要的交换操作和数组中倒置的数量相同，需要的比较次数大于等于倒置的数量，小于等于倒置加上数组的大小再-1
date: 2020-09-21 14:48:32
tags: 排序 
categories: 数据结构与算法
---

### 插入排序
通常人们整理桥牌的方法是一张一张的来，将每一张牌插入到其他已经有序的牌中的适当位置。再计算机中的实现中，为了给要插入的元素腾出空间，我们需要将其余的所有元素再插入之前都向右移动一位。这种算法叫做`插入排序`。
选择排序一样，当前索引左半边的所有元素都是有序的，但它们的最终位置还不确定，为了给更小的元素腾出空间，他们可能会被移动。但是当索引到达数组的右端时，数组就排序完成了。
和选择排序不同的是，插入排序所需的时间取决于输入中元素的初始顺序。例如，对一个很大且其中的元素已经有序的数组进行排序将会比随机顺序的数组或是逆序数组进行排序要快的多。
### 排序模板

[排序模板](/2020/09/19/algor1)

### 插入排序
```java
public class Insertion {
    public static void sort(Comparable[] a){
        int N = a.length;
        for (int i=1 ;i<N;i++){
            for(int j = i;j>0&&less(a[j],a[j-1]);j--){
                exch(a,j,j-1);
            }
        }
    }

    private static boolean less(Comparable v, Comparable w){
        return v.compareTo(w)<0;
    }

    private static void exch(Comparable[] a, int i,int j){
        Comparable t  = a[i];
        a[i] = a[j];
        a[j] = t;
    }

    private static void show(Comparable[] a){
        for (int i = 0;i<a.length;i++)
            StdOut.print(a[i] + " ");
        StdOut.println();
    }

    private static boolean isSorted(Comparable[] a ){
        for (int i=1;i<a.length;i++)
            if(less(a[i],a[i-1])) return false;
        return true;
    }

    public static void main(String[] args){
        String[] a = In.readStrings();
        sort(a);
        assert isSorted(a);
        show(a);
    }
}

```



