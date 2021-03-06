---
title: 深入理解计算机系统-计算机系统的漫游
date: 2019-03-01 14:46:21
categories:
- 读书笔记
tags:
- 读书笔记
- 深入理解计算机系统
- 操作系统
---

> 计算机系统漫游
> **计算机系统**是由*硬件*和*软件*组成的

```c
#include <stdio.h>

int main()
{
  print("hello, world\n");
  return 0;
}
```

## 信息就是位+上下文

> 程序是由源程序（hello.c）文件开始，源程序是由值0和1组成的位（又称为比特）序列，8个位被组成一组，成为字节，每个字节表示程序中的某些特殊文本字符。

## 程序被其他程序翻译成不同格式

> 程序的生命周期是从一个高级语言程序开始的，因此能被人读懂。在系统上运行程序时，每天语句都必须被其他程序转化为一系列的低级*机器语言指令*。然后这些指令按照一种称为*可执行目标程序*的格式打包好，并以二进制磁盘文件的行驶存放起来。目标程序也被称为*可执行目标文件*。
> 从源文件到目标文件的转化是由*编译器驱动程序*完成的
> 编译过程可分为四个阶段完成，程序是（预处理器，编译器，汇编器，链接器）构成了编译系统

- 预处理阶段
> 预处理器（cpp）以字符#开头的命令，修改原始C程序。比如include引入，把它插入程序文本中，结果得到了新的程序，通常以.i作为扩展名
- 编译阶段
> 编译器（ccl）将.i文本翻译成.s文本，他包含一个汇编语言程序。
- 汇编阶段
> 汇编器（as）降.s文件翻译成机器语言指令，并将这些指令打包成叫做*可重定位目标程序*，并将结果保存在.o文件中，他是一个二进制文件。
- 链接阶段
> 连接器（ld）负责将程序中调用的标准库合并到.o文件中得到*可执行目标文件*，这个文件可以被系统执行。

## 了解编译系统如何工作的益处

- 优化程序性能
- 理解链接时出现的错误
- 避免安全漏洞

## 处理器读兵解释存储唉内存中的指令

> shell是一个命令解析器，他输出一个提示符，等待输入一个命令，然后执行这个命令。

### 系统的硬件组成

- 总线
- I/O设备
- 主存
- 处理器

## 高速缓存的重要性

> 高速缓存存储器存在的应用程序员能利用高速缓存将程序的性能提高到一个数量级

## 存储设备形成的层次结构

![存储器的层次结构](/assets/images/csapp-1-9.png)

## 操作系统管理硬件

> 所有程序对硬件的操作尝试都应该通过操作系统

操作系统的两个功能
> 防止硬件被失控的应用程序滥用
> 面向应用程序提供单一的机制来控制复杂而又通常大不相同的低级硬件设备。通过（进程、虚拟内存和文件）来实现这两个功能

### 进程

> 进程是操作系统对一个正在运行的程序的抽象，在一个操作系统中可以同时运行多个进程，每个进程好像都是在独自占用硬件。而并发运行是说一个进程的指令和另一个进程的指令是交错执行的。
> 传统系统在一个时刻只能执行一个程序，而多核处理器能同时执行多个程序，无论在单核还是多核系统中，一个cpu看上去都是在执行多个进程，这是通过处理器在进程间切换来实现的，操作系统实现这种机制成为*上下文切换*

### 线程

> 通常我们认为一个进程只有单一的控制流，但是在现代操作系统中，一个进程实际上是由多个成为*线程*的执行单元组成，每个线程都运行在进程的上下文中，并共享同样的代码和全局数据。

### 虚拟内存

> 虚拟内存是一个抽象的概念，他为每个进程提供了一个假象，即每个进程都在独占的使用主存，每个进程看到的内存都是一致的，成为*虚拟空间地址*
> 虚拟内存运作需要硬件和操作系统软件之间精密复杂的交互，包括处理器生成的每个地址的硬件翻译

### 文件

> 文件就是字节序列。每一个I/O设备都可以看作成文件，包括键盘、磁盘、显示器、网络。

## 系统间利用网络通讯

> 从一个单独的系统来看，网络了以视为一个I/O设备

## 重要主题

### Amdahl定律

> 对提升某一部分性能所带来的效果做出来简单却有见地的观察，这个观察被称为Amdahl定律。
> 当我们对系统的某个部分加速时，其对系统整体新能的影响取决于该部分的重要性与加速程度。

![Amdahl定律](/assets/images/csapp-1-9-1.png)

### 并发和并行

> 数字计算机始终有两个需求是驱动进步的持续动力，一个是我们想要计算机做的更多，另一个是我们想要计算机运行的更快。并发指一个同时具有多个活动的系统，并行指的是用并发来使一个系统更快

#### 线程级并发
> 构建在进程这个抽象之上，我们设计出了同时有多个程序执行的系统，这就导致了并发，使用线程，我们甚至能够在一个进程中执行多个控制流。

#### 指令级并行
 
> 在较低的抽象层次上，现代处理器可以同时执行多条指令的属性称为指令级并行。

#### 单指令、多数据并行

> 在最低层次上，许多现代处理器拥有特殊硬件，语序一条指令产生多个可以并行执行的操作，这种方式称为单指令、多数据，即SIMD并行。

### 抽象的重要性

 - 文件是对I/O设备的抽象
 - 虚拟内存是对程序存储器的抽象
 - 进程是对正在运行的程序的抽象
 - 虚拟机是对整个计算机的抽象
 
## 小结

 1. 了解计算机系统基本构成
 2. 了解程序运算方式
 3. 了解抽象对于计算机的重要性
 












