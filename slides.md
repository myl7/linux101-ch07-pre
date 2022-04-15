<!-- Copyright (c) 2022 ustclug -->
<!-- SPDX-License-Identifier: CC-BY-SA-4.0 -->

# Linux 101 Ch07：<br>Linux 上的编程

Linux Coding

Speaker：明宇龙

---

# 导言

要回答的问题

在 Linux 上进行日常的编程开发：

- Linux 上的 C/C++ 开发
- Linux 上的 Python 开发
- Linux 上编程语言开发的范式与共性

---

# C 语言开发

综述

近乎系统级的支持

带有图形界面的 IDE 的编译往往是封装了各种提供命令行接口的编译器 -> 直接调用这些提供命令行接口的编译器进行编译

---

# C 语言开发

编译器实现

- Linux: gcc（by GNU）、clang（by LLVM）
- Windows: cl.exe（by Microsoft）-> Visual C++ (MSVC)
- Mac OS X: 也是 gcc、clang
  - gcc on Mac OS X 其实是 clang

---

# C 语言开发

单文件编译

以 gcc 为例：

```c
// main.c
#include <stdio.h>

int main() {
  printf("Hello World!\n");
  return 0;
}
```

```shell
$ gcc main.c -o main
$ ./main
Hello World!
```

- Rule of Silence: When a program has nothing surprising to say, it should say nothing.

---

# C 语言开发

多文件编译

```c
// main.c
#include "print.h"

int main() {
  print();
  return 0;
}
```

```c
// print.c
#include "print.h"

#include <stdio.h>

void print() {
  printf("Hello World!\n");
}
```

---

# C 语言开发

多文件编译

```c
// print.h
#ifndef PRINT
#define PRINT

void print();

#endif  // PRINT
```

```shell
$ gcc main.c -c  # 生成 main.o
$ gcc print.c -c  # 生成 print.o
$ gcc main.o print.o -o main
$ ./main
Hello World!
```

---

# C 语言开发

编译步骤

- 预处理（preprocessing）、编译（compilation）、汇编（assembly）、链接（linking）
- cpp、cc1、ar、ld
- 处理 # 开头的预编译指令、将源码编译为汇编代码、将汇编代码编译为二进制代码、组合众多二进制代码生成可执行文件
- gcc -E、gcc -S、gcc -c、gcc
- main.c -> main.i -> main.s -> main.o -> main

---

# C 语言开发

使用构建工具（Build tools）

- 手动地一一编译实在太麻烦，太浪费精力；
- 这些源文件的编译有顺序要求，为了满足此依赖关系需要设计一个流程；
- 编译整个项目需要难以忍受的大量时间，应当考虑到一部分未更改的源文件不需要重新编译。

---

# C 语言开发

Makefile

```makefile
main.o: main.c print.h
print.o: print.c print.h
main: main.o print.o
```

```shell
$ make main
$ ./main
Hello World!
```

---

# C 语言开发

其他的构建工具：CMake，ninja……

“套娃”：用一个程序来生成构建所需的配置

Modern CMake、vcpkg

ninja：更好的多线程编译支持

---

# C++ 语言开发

与 C 相似

- gcc -> g++，clang -> clang++
- 复用 Makefile、CMake

---

# Python 语言开发

综述

---

# 附录

项目信息

URL：[101.lug.ustc.edu.cn](https://101.lug.ustc.edu.cn)

GitHub Repo：[ustclug/Linux101-docs](https://github.com/ustclug/Linux101-docs)

SPDX-License-Identifier: CC-BY-SA-4.0

---
layout: center
---

# 谢谢！

<style>
h1 {
  font-size: 3.75rem !important;
}
</style>
