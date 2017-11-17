---
layout: post
title: 算法题
date: 2013-03-10 18:28:58
categories:
- algorithm
tags:
---

### 排序类

### 数组处理类

#### 不使用额外空间删除已排序数组重复元素

给定排好序的数组A，大小为n，请给出一个O(n)的算法，删除重复元素，且不能使用额外空间。

```c
/*
思路：index记录重排位置，从第二个开始遍历，若与index位置值不等，移到index+1，index++。
*/
#include <stdio.h>

void print(int a[], int n)
{
  int i = 0;
  for(; i < n; i++) {
    printf("%d ", a[i]);
  }
  printf("\n");
}

int deldup(int a[], int n)
{
  int i;
  int index = 0;
  for(i = 1; i < n; i++) {
    if(a[i] == a[index]) {
      continue;
    } else {
      a[index+1] = a[i];
      index++;
    }
  }
  return index+1;
}

int main()
{
  int a[] = {1,1,2,2,2,3,3,3,3,5,5,7,7,7,9,9,9};

  print(a, 17);
  int n=deldup(a, 17);
  print(a, n);

  return 0;
}
```

### 字符串处理类

### 链表类

### 树

- Size Balanced Tree(SBT)
- 二叉查找树(BST) 查找最好时间复杂度O(logN)，最坏时间复杂度O(N)。
- 平衡二叉查找树(AVL) 查找的时间复杂度维持在O(logN) ，严格平衡策略以牺牲建立查找结构(插入，删除操作)的代价。
- 红黑树(RBT)  查找 效率最好情况下时间复杂度为O(logN)，但在最坏情况下比AVL要差一些，但也远远好于BST。
- Treap 

#### Binary Search Tree 与 Size Balanced Tree 对比


