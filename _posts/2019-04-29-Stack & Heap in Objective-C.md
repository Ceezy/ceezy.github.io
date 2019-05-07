---
title: "Objective-C 里的栈和堆"
categories:
  - iOS
tags:
  - iOS
  - Objective-C
  - Memory
  - Stack
  - Heap
---



## 程序的内存布局（Program Memory Layout）

下图展示的是一个简单的计算机程序的内存结构：

![](/assets/images/stack-and-heap-in-objective-c/15565177982518.jpg)

上图 Stack 位于高位地址，text 位于地位地址。

* **Text**：文本区，也称代码区（Code Segment），存放着可执行指令。这个区域的数据一般是只读的。
* **Data**：数据区，也称初始化数据区，存放着一些已经初始化了的静态变量，全局变量或者一些文字常量等。
* **BSS**：BSS（Block Started by Symbol）区，也称未初始化数据区，存放着所有未初始化的全局变量和静态变量。
* **Heap**：堆区，其数据结构类似于链表，其大小是不固定的。堆区的大小由程序员通过 malloc，calloc，realloc，free 等操作来管理。
* **Stack**：栈区，其数据结构类似于数组，并且是后进先出（LIFO）的结构。栈区的内存是由系统管理的，当一个函数被调用，一个栈帧（局部变量，参数和返回地址）会被压入栈区，但函数返回时，这个栈帧会从栈区弹出，所以太深的递归调用会导致栈溢出。

为了更好的了解那些数据存储在哪个区域，大家可以看看下面这段代码：

```c++
int a = 0; // Data 区
char *p1; // BSS 区
void main() {
    int b; // 栈区
    char s[] = "abc"; // 栈区
    char *p2; // 栈
    char *p3 = "123456"; // "123456\0" 在 Data 区，p3 在栈区
    static int c = 0; // Data 区
    
    // 分配得来的10和20字节的区域就在堆区
    p1 = (char *)malloc(10);
    p2 = (char *)malloc(20);
    
    strcpy(p1, "123456"); // "123456\0" 在 Data 区，编译器可能会将它与 p3 所指向的"123456"优化成同一个地方
    
    return;
}
```

## 栈对象 vs. 堆对象

```
NSObject *obj = [[NSObject alloc] init];
```

上面这段代码由 `NSObject` alloc 出来的内存在堆区，即 `obj` 指向的是一个堆对象。

Objective-C 不可以直接创建栈对象，但是可以通过以下的代码来实现：

```
struct {
    Class isa;
} fakeNSObject;
fakeNSObject.isa = [NSObject class];
    
NSObject *obj = (NSObject *)&fakeNSObject;
NSLog(@"%@", [obj description]);
```

C++ 是支持创建栈对象和堆对象的，例如下面代码：

```c++
string stackString;
string *heapString = new string;
```

栈对像拥有以下优点：
* **速度**：在栈区分配空间是非常快的，因为它在编译过程中就已将创建好了。
* **简单**：栈对象的内存由系统管理，你不需要手动管理其内存，也不用担心内存泄漏。

栈对象的缺点：
* **不安全**：由于栈对象的内存是系统管理的，栈对象的唯一拥有者是函数，一旦函数返回了，函数内的栈对象都会被销毁，此时如果有某一指针指向这个栈对象，那这个指针则会变成野指针，再次使用会导致奔溃。
* **不灵活**：栈对象没有堆对象那么灵活，堆对象可以随时创建和销毁。

## Objective-C 中实际的栈对象

Objective-C 中的 Block 有三种类型：Stack，Global 和 Malloc。

一般我们创建的 Block 大部分都会在栈上，如果你需要持续使用它，需要将其 copy 到堆，这也是为什么当 Block 作为属性时，常常要用 copy 修饰。

## 参考

* [Data segment](https://en.wikipedia.org/wiki/Data_segment)
* [Stack and Heap Objects in Objective-C](https://www.mikeash.com/pyblog/friday-qa-2010-01-15-stack-and-heap-objects-in-objective-c.html)