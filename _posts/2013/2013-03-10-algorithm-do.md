---
layout: post
title: 算法题
date: 2013-03-10 18:28:58
categories:
- algorithm
tags:
---

#### 不使用额外空间删除已排序数组重复元素

给定排好序的数组A，大小为n，请给出一个O(n)的算法，删除重复元素，且不能使用额外空间。

{% highlight c++%}
#include <stdio.h>

void print(int a[], int n)
{
  int i = 0;
  for(; i < n; i++) {
    printf("%d ", a[i]);
  }
  printf("\n");
}

int deldup(int *a, int n)
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
{% endhighlight %}