---
title: 选择排序
description: 
date: 2020-09-19 17:27:08
tags: 排序
categories: 数据结构与算法
---

### 排序模板

`less()`方法进行比较，`exch()`方法进行交换 ，通过`comparable`接口实现`less()`方法。
<!--more-->
```java
public class Example {
    public static void sort(Comparable[] a){
        /*排序算法*/
        }
    }
   // 比较两个元素的大小
    private static boolean less(Comparable v, Comparable w){
        return v.compareTo(w)<0;
    }
    //交换两个元素
    private static void exch(Comparable[] a, int i,int j){
        Comparable t  = a[i];
        a[i] = a[j];
        a[j] = t;
    }
   // 单行打印数组
    private static void show(Comparable[] a){
        for (int i = 0;i<a.length;i++)
            StdOut.print(a[i] + " ");
        StdOut.println();
    }
    //测试数组元素是否有序
    private static boolean isSorted(Comparable[] a ){
        for (int i=1;i<a.length;i++)
            if(less(a[i],a[i-1])) return false;
        return true;
    }

    public static void main(String[] args){
        /*从标准输入读取字符串，并排序输出*/
        String[] a = In.readStrings();
        sort(a);
        assert isSorted(a);
        show(a);
    }
}
```
### 数据类型
> 该模板适用于任何实现了Comparable 接口的数据类型。Java中 Integer 和 Double 以及String和其他许多高级数据类型（如File和URL）都实现了Comparable接口

### 选择排序

一种最简单的排序算法：首先，找到数组中最小的那个元素，其次将它和数组的第一个元素交换位置。再次在剩下的元素中找到最小的元素，将他和数组的第二个元素交换位置。如此往复，直到将整个数组排序。这种方法就叫做`选择排序`，因为不断的选择剩余元素之中的最小者。
算法的时间效率取决于比较的次数。
### 图解
![select](select.gif)
#### 复杂度分析
对于长度为N的数组，选择排序需要大约N<sup>2</sup>/2次比较和N 次交换

### 实现代码
```java
public class Selection {
    public static void sort(Comparable[] a){
        int N = a.length;
        for (int i=0 ;i<N;i++){
            int min = i;
            for (int j=i+1;j<N;j++){
                if (less(a[j],a[min])) min =j;
            }
            exch(a,i,min);
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

`运行时间和输入无关` 。为了找出最小元素而扫描一遍数组并不能为下一遍扫描提供什么信息。 某些情况下是缺点。比如 数据本身有序  
`数据移动是最少的`。每次交换都会改变两个数组的元素的值，因此选择排序用了N次交换--交换次数和数组的大小是线性关系。
