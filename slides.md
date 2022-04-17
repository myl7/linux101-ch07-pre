<!-- Copyright (c) 2022 ustclug -->
<!-- SPDX-License-Identifier: CC-BY-SA-4.0 -->

# Linux 101 Ch07：<br>Linux 上的编程

Coding on Linux

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

悠远而健全的支持：OS API，ABI，FFI 等

图形界面 IDE 封装了提供命令行接口编译器

编译器实现：

- Linux: gcc（by GNU）、clang（by LLVM）
- Windows: cl.exe（by Microsoft）-> Visual C++ & MSVC
- Mac OS X: （作为 BSD-based，同由 UNIX 而来）也是 gcc、clang
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

`#ifndef ... #define ... #endif`：只 `#include` 一次防止重复声明，= `#pragma once`（后者支持已较广泛）

---

# C 语言开发

编译步骤

- 预处理（preprocessing）、编译（compilation）、汇编（assembly）、链接（linking）
- cpp、cc1、ar、ld，整合在 gcc 套装下
- 处理 # 开头的预编译指令、将源码编译为汇编代码、将汇编代码编译为二进制代码、组合众多二进制代码生成可执行文件
- gcc -E、gcc -S、gcc -c、gcc
- main.c -> main.i -> main.s -> main.o -> main

不像直接 gcc 解决，一步步调用 cpp、cc1、ar、ld 需要额外的参数配置

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

CMake：构建工具的构建工具

Modern CMake

ninja：更好的多线程编译支持

vcpkg

---

# C++ 语言开发

与 C 相似

- gcc -> g++，clang -> clang++
- 复用 Makefile、CMake

---

# Python 语言开发

综述

不着力于具体代码，而是分析一些外围架构

语法：官方文档、《Python 学习手册（第五版）》，注意 3.5-3.8 的新语法

---

# Python 语言开发

解释器 python

解释器（Interpreter）：源代码 -> 字节码 -> PVM

python2、python3

---

# Python 语言开发

包管理器 pip

```shell
# 安装 Python 3 和 Python 3 的 pip。对于 Python 2 和 3 间的纠纠缠缠，我们将在之后讲解。
$ sudo apt install python3 python3-pip

# 测试一下看看，是否能够正常使用它们。
# 请保证在 `python` 和 `pip` 后有 3 这个数字。这也是历史遗留问题。
$ python3 -V
$ pip3 -V

# 暂时忽略以下两条指令，我们会在之后讲解。
$ python3 -m venv venv
$ source venv/bin/activate
(venv)$ ls
venv

# 安装一个 Python 包 a、b，以及 a、b 依赖的 Python 包。
(venv)$ pip3 install a b

# 卸载一个 Python 包 b。注意：这不会删除之前一起安装的包 b 的依赖。
(venv)$ pip3 uninstall b
```

---

# Python 语言开发

Python 依赖管理

pip、setuptools 功能太基础

---

# Python 语言开发

requirements.txt

```# requirements.txt
django
pytest>=3.0.0
pytest-cov==1.0.0
```

```bash
pip3 install -r requirements.txt
```

可以生成（`pip freeze` 及其他依赖管理实现生成）也可以手写

---

# Python 语言开发

setuptools：setup.py

打包（packaging）

---

# Python 语言开发

其他的：pip-tools、pipenv……

pip-tools：

- 增加 requirements.dev，用 requirements.dev 生成 requirements.txt
- 简单直接
- 基于 requirements.txt，依然稍显简陋

pipenv：

- 类 npm lock，更健全
- 完成度和工业中的稳定性尚有待证明

pyenv：

- 管理 python 解释器版本
- 可配合不管理解释器版本的依赖管理实现使用

Poetry：

- 类 npm lock

pdm：

- 非 virtualenv-based、全新的依赖管理体系
- 支持有限，但已可使用

---

# Python 语言开发

Virtualenv

pip / Python 依赖体系（不像 npm）不允许同时安装不同版本的同一个包

局部包

```bash
python3 -m venv venv
source venv/bin/activate
deactivate
```

关键功能通过环境变量实现

---

# Python 语言开发

Python 的版本

Python 2 已在 2020 年初正式宣告停止维护

Python 2 与 3：最好看做两种不同的编程语言（语法层面、设计层面、API 层面）

推荐：>=3.8（Ubuntu 20.04 默认）

Python 3.x 兼容性

“Modern” Python

---

# Python 语言开发

Python 的其他实现

官方：CPython

- JPython：将 Python 编译到 Java 字节码，由 JVM 来运行
- PyPy：相较于 CPython，实现了 JIT（just in time）编译器，性能有极大地提升
- Cython：引入了额外的语法和严密的类型系统，性能也有很大提升
- Numba：将 Python 编译到机器码，从而直接运行，性能也不错

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
