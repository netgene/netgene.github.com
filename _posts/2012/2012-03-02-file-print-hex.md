---
layout: post
title: 二进制文件16进制打印
date: 2012-03-02 18:28:58
categories:
- c/c++
tags:
---

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>
#include <sys/stat.h>

int main(int argc, char **argv)
{
  FILE *fp = fopen(argv[1], "rb");
  if(!fp) return -1;

  struct stat fileStat;
  fstat(fileno(fp), &fileStat);
  int size = fileStat.st_size;

  char* d = (char*)malloc(size);
  fread(d, 1, size, fp);
  int i=0;
  for(i = 0; i < size; i++) {
    printf("%02x", uint8_t(d[i]));
  }

  free(d);
  return 0;
}
```