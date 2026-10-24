# C++ 自顶向下

> 从经典面试题出发，理解 C++ 背后的语言设计、对象模型、编译系统、操作系统与机器模型。

---

## 1. 项目定位

本项目不是一套传统的 C++ 语法教程，也不是单纯的大厂 C++ 八股题整理。

项目希望通过一种 **“经典面试题驱动 + 自顶向下理解”** 的方式，重新建立对 C++ 的完整认识。

传统学习 C++ 时，我们经常按照下面的顺序学习：

```text
变量
→ 指针
→ 引用
→ 类
→ 继承
→ 模板
→ STL
→ 多线程
```

这种方式能够教会我们“怎么写 C++”，但很多知识最终容易变成彼此孤立的规则：

- 为什么有构造和析构？
- 为什么需要虚函数？
- 为什么模板通常写在头文件？
- 为什么 `vector` 扩容后迭代器会失效？
- 为什么 `volatile` 不能保证线程安全？
- 为什么 Release 模式下变量会消失？
- 为什么有 Undefined Behavior？
- 为什么 `std::move` 本身并不真正移动任何东西？

本项目希望换一个角度：

> **先理解 C++ 试图解决什么问题，再理解它为什么设计出这些语言机制。**

最终建立的不只是语法知识，而是一套能够解释 C++ 的统一模型。

---

# 2. 为什么要做这个项目

AI 辅助编码会逐渐成为软件开发的常态。

未来，很多过去依赖程序员手写的工作：

```text
STL 使用
模板代码
RAII 封装
线程池
序列化
常见数据结构
工程样板代码
```

都可以由 AI 快速完成。

但 AI 能生成代码，并不意味着这些代码一定：

- 生命周期正确；
- 所有权合理；
- 没有 Undefined Behavior；
- 没有悬空引用；
- 没有数据竞争；
- 接口设计合理；
- 内存布局高效；
- cache 友好；
- ABI 安全；
- 满足真实工程约束。

因此，在 AI 编码时代，人对 C++ 的理解仍然决定着能够驾驭多复杂的软件系统。

人的核心能力会逐渐从：

> “这段代码我会不会写？”

转向：

> “我能不能判断这段代码为什么应该这样设计？”

本项目最终希望培养的是这种能力。

---

# 3. 核心学习理念

## 3.1 不背孤立八股，而是用八股题寻找知识入口

经典面试题仍然非常有价值。

例如：

> `new/delete` 和 `malloc/free` 有什么区别？

如果只是准备面试，可以背：

- `new` 是运算符，`malloc` 是函数；
- `new` 返回具体类型；
- `new` 调用构造函数；
- `delete` 调用析构函数。

但本项目会继续追问：

```text
为什么 new 需要构造对象？
        ↓
因为 memory 和 object 不是一个概念
        ↓
什么是 Object Lifetime？
        ↓
为什么对象需要析构？
        ↓
资源如何绑定对象生命周期？
        ↓
RAII
        ↓
Ownership
        ↓
unique_ptr
        ↓
move semantics
```

于是，一道面试题最终可以串起现代 C++ 的一整条设计主线。

---

## 3.2 每个问题尽量追到设计层

学习一个 C++ 机制时，依次回答：

```text
是什么？
↓
面试应该怎么回答？

为什么存在？
↓
它解决了什么问题？

如果没有它怎么办？
↓
C 语言通常如何解决？

编译器如何实现？
↓
对象、符号、机器代码发生了什么？

机器真正执行什么？
↓
内存、寄存器、CPU 的成本是什么？

工程上什么时候应该使用？
↓
设计选择与 trade-off 是什么？
```

目标是从：

> 会使用语言机制

逐渐进入：

> 能解释语言设计。

---

## 3.3 C 与 C++ 对照理解

C 是理解 C++ 非常好的参照系。

例如：

```text
C

数据
+
函数
+
地址
```

很多事情依赖程序员约定：

```text
malloc
init
使用
destroy
free
```

而 C++ 尝试把其中一些约定提升为语言机制：

```text
对象
生命周期
构造
析构
RAII
类型系统
模板
```

因此学习过程中经常问：

> 如果只有 C，这件事情会怎么实现？

能够帮助理解 C++ 为什么需要某个机制。

---

# 4. 最终希望建立的 C++ 世界观

整个课程最终围绕几个核心模型展开：

```text
程序构建模型
对象模型
生命周期模型
资源模型
所有权模型
类型模型
多态模型
泛型模型
编译模型
内存模型
并发模型
机器成本模型
```

看到一行 C++：

```cpp
std::vector<std::unique_ptr<Base>> objects;
```

最终希望能够同时看到：

```text
vector
→ 连续存储 unique_ptr

unique_ptr
→ 独占所有权

Base
→ 可能存在运行时多态

对象本体
→ 通常独立位于其他内存位置

遍历
→ pointer chasing

virtual
→ runtime dispatch

析构
→ vector
→ unique_ptr
→ 派生对象
→ 资源释放
```

也就是说：

> 从“看到语法”逐渐进入“看到整个系统”。

---

# 5. 课程总体路线

```text
第 0 阶段
源码 → 可执行程序 → 进程 → CPU

        ↓

第 1 阶段
对象、资源与生命周期

        ↓

第 2 阶段
对象模型与动态多态

        ↓

第 3 阶段
类型系统与接口设计

        ↓

第 4 阶段
模板、泛型与 STL

        ↓

第 5 阶段
编译器、链接器与语言底层深化

        ↓

第 6 阶段
并发与 C++ 内存模型

        ↓

第 7 阶段
现代 C++ 工程设计与性能

        ↓

综合阶段
从语言使用者到系统设计者
```

课程不是严格的一次性线性过程。

部分主题会采用：

> **第一次建立地图 → 后续带着更多知识重新深入**

的螺旋式学习方式。

---

# 6. 第 0 阶段：从源码到真正运行的程序

> 核心问题：**一段 `.cpp` 最终为什么能够变成 CPU 正在执行的程序？**

这一阶段偏向：

```text
Compiler
Linker
OS
Runtime
ABI
CPU
```

虽然不完全属于 C++ 语言本身，却是理解高级 C++ 必不可少的基础。

## 核心路线

```text
.cpp
↓
Preprocessor
↓
Translation Unit
↓
Compiler Frontend
↓
IR
↓
Optimizer
↓
Assembly / Machine Code
↓
.o / .obj
↓
Linker
↓
Executable
↓
OS Loader
↓
Virtual Address Space
↓
Runtime Initialization
↓
main()
↓
CPU
```

## 典型面试问题

- C++ 从源码到可执行文件经历哪些阶段？
- 预处理到底做了什么？
- `#include` 的本质是什么？
- 什么是 Translation Unit？
- `.o/.obj` 文件里面有什么？
- 编译和链接有什么区别？
- 什么是 symbol？
- 什么是 relocation？
- 什么是 undefined reference？
- 静态链接与动态链接有什么区别？
- `.dll/.so/.dylib` 是怎样被加载的？
- OS 如何把 executable 变成 process？
- 一个进程的虚拟地址空间是什么样的？
- `main()` 是程序执行的第一条指令吗？
- 全局对象什么时候构造？
- 函数调用时发生了什么？
- 什么是 Calling Convention？
- stack frame 是什么？
- 局部变量一定在栈上吗？
- heap 与 OS 虚拟内存是什么关系？
- Debug Symbol / PDB 是什么？
- Release 为什么会出现 `<optimized away>`？
- ABI 是什么？
- Name Mangling 为什么存在？
- `extern "C"` 到底改变了什么？

## 阶段目标

建立：

```text
源码
→ 编译器
→ 链接器
→ 可执行文件
→ OS
→ 进程
→ CPU
```

的完整系统地图。

---

# 7. 第 1 阶段：对象、资源与生命周期

> 核心问题：**C++ 中一个对象究竟是什么，它什么时候存在，又由谁负责它的生命周期？**

## 典型面试问题

- `new/delete` 与 `malloc/free` 有什么区别？
- memory 和 object 有什么区别？
- 构造函数为什么存在？
- 析构函数为什么存在？
- 什么是 RAII？
- 什么是浅拷贝与深拷贝？
- 拷贝构造什么时候调用？
- 什么是 Rule of Three？
- 什么是 Rule of Five？
- 什么是 Rule of Zero？
- 左值和右值是什么？
- 为什么需要右值引用？
- 为什么需要移动构造？
- `std::move` 到底做了什么？
- `unique_ptr`、`shared_ptr`、`weak_ptr` 有什么区别？
- `shared_ptr` 的引用计数是什么？
- 为什么会出现循环引用？
- 为什么现代 C++ 不推荐裸 `new/delete`？

## 核心模型

```text
storage
↓
object construction
↓
object lifetime
↓
resource ownership
↓
copy / move
↓
destruction
↓
storage release
```

最终建立：

> **生命周期 + 所有权**

这条现代 C++ 最重要的主线之一。

---

# 8. 第 2 阶段：对象模型与动态多态

> 核心问题：**C++ 的 class 最终在内存中是什么？运行时多态是怎样实现的？**

## 典型面试问题

- `struct` 和 `class` 有什么区别？
- C++ 对象在内存中如何布局？
- 空类大小为什么通常不是 0？
- 成员函数存在哪里？
- `this` 指针是什么？
- 什么是对象对齐？
- 什么是 padding？
- 继承后对象如何布局？
- 虚函数是如何实现的？
- vptr 和 vtable 是什么？
- 一个对象有几个 vptr？
- 为什么析构函数经常需要声明为 virtual？
- 构造函数为什么不能是 virtual？
- static 成员函数为什么不能是 virtual？
- 什么是纯虚函数？
- 什么是抽象类？
- 多继承如何实现？
- 菱形继承有什么问题？
- virtual inheritance 做了什么？
- `dynamic_cast` 如何工作？

## 核心模型

```text
class abstraction
↓
object layout
↓
inheritance
↓
vptr / vtable
↓
runtime dispatch
↓
ABI
```

---

# 9. 第 3 阶段：类型系统与接口设计

> 核心问题：**如何让函数签名和类型本身表达程序设计意图？**

## 典型面试问题

- pointer 和 reference 有什么区别？
- `const` 有哪些用法？
- `const T*` 与 `T* const` 有什么区别？
- 为什么函数参数经常使用 `const T&`？
- 值传递什么时候反而更好？
- `T&`、`const T&`、`T&&` 分别表达什么？
- `explicit` 是干什么的？
- `auto` 如何进行类型推导？
- `decltype` 是什么？
- `enum` 与 `enum class` 有什么区别？
- 什么是 implicit conversion？
- 什么是 type safety？

## 核心思想

类型不是单纯告诉编译器：

> 这块内存是什么格式。

更应该表达：

```text
是否拥有
是否可修改
是否可以为空
是否允许隐式转换
生命周期关系
接口契约
```

最终进入：

> **让类型系统承担设计约束。**

---

# 10. 第 4 阶段：模板、泛型与 STL

> 核心问题：**C++ 如何在不明显牺牲运行时性能的情况下实现高级抽象？**

## 典型面试问题

- function template 与 class template 是什么？
- template 是如何实例化的？
- 模板为什么通常写在头文件？
- 什么是 specialization？
- 什么是 SFINAE？
- Concept 解决了什么问题？
- static polymorphism 是什么？
- static polymorphism 和 virtual 有什么区别？
- STL 为什么把 Container 和 Algorithm 分离？
- Iterator 为什么存在？
- `vector` 的底层结构是什么？
- `vector` 如何扩容？
- 为什么扩容会导致 iterator 失效？
- `vector/list/deque` 有什么区别？
- `map/unordered_map` 有什么区别？
- `push_back` 与 `emplace_back` 有什么区别？
- `sort` 为什么不能直接作用于 `list`？
- Range 与 Iterator 的关系是什么？

## 核心路线

```text
template
↓
generic programming
↓
static polymorphism
↓
iterator
↓
STL
↓
concept
↓
zero-overhead abstraction
```

---

# 11. 第 5 阶段：编译器、链接器与语言底层深化

> 核心问题：**C++ 的语言规则如何与编译器优化、链接系统以及 ABI 相互作用？**

这一阶段会重新回到第 0 阶段，但这一次带着前面已经建立起来的 C++ 对象与类型知识深入分析。

## 典型面试问题

- `static` 在不同位置分别是什么意思？
- `extern` 是什么？
- internal linkage / external linkage 是什么？
- `inline` 真正意味着什么？
- inline function 一定会内联吗？
- 什么是 ODR？
- 为什么头文件定义变量容易重复定义？
- template 与 Translation Unit 有什么关系？
- 什么是 Name Mangling？
- 什么是 ABI？
- `volatile` 到底保证什么？
- `volatile` 能保证线程安全吗？
- 什么是 Undefined Behavior？
- 为什么 signed integer overflow 是 UB？
- 为什么 UB 会导致 Release 出现奇怪结果？
- 编译器为什么可以删除变量？
- 什么是 dead code elimination？
- 什么是 constant folding？
- 什么是 inlining？
- 什么是 register allocation？
- 什么是 auto-vectorization？

## 核心模型

```text
C++ Source
↓
C++ Abstract Machine
↓
Compiler IR
↓
Optimization
↓
Machine Code
↓
CPU
```

---

# 12. 第 6 阶段：并发与 C++ 内存模型

> 核心问题：**多个 CPU 核心同时操作内存时，“读写一个变量”究竟意味着什么？**

## 典型面试问题

- process 和 thread 有什么区别？
- mutex 是如何工作的？
- mutex 和 spinlock 有什么区别？
- 什么是 deadlock？
- 死锁的四个必要条件是什么？
- `condition_variable` 为什么需要 mutex？
- 为什么 `wait` 通常需要配合循环？
- 什么是 spurious wakeup？
- atomic 为什么是原子的？
- CAS 是什么？
- 什么是 data race？
- data race 与 race condition 有什么区别？
- volatile 为什么不能实现线程同步？
- 什么是 happens-before？
- 什么是 memory ordering？
- `memory_order_relaxed` 到底放松了什么？
- compiler reordering 与 CPU reordering 有什么区别？
- cache coherence 与 C++ memory model 有什么关系？

## 核心路线

```text
thread
↓
shared state
↓
data race
↓
synchronization
↓
atomic
↓
happens-before
↓
memory ordering
↓
compiler / CPU
```

---

# 13. 第 7 阶段：现代 C++ 工程设计与性能

> 核心问题：**当语言机制全部掌握之后，怎样设计真正可靠、高效、可维护的 C++ 系统？**

重点逐渐从：

> 这个语法怎么写？

转向：

> 这个接口应该怎样设计？

主要主题包括：

- Ownership Design
- Lifetime Design
- Value Semantics
- API Design
- Exception Safety
- Error Handling
- Dependency Management
- Data-Oriented Design
- Cache Locality
- Allocation Strategy
- Memory Pool
- ABI Stability
- Static vs Dynamic Polymorphism
- Compile-time vs Runtime
- Profiling
- Performance Engineering

典型问题例如：

```cpp
void process(Data* data);
```

看到这样的 API，要能够主动追问：

```text
data 可以为空吗？

process 会修改它吗？

谁拥有 data？

生命周期由谁保证？

有多少个元素？

数据是否连续？
```

然后判断是否应该设计成：

```cpp
void process(std::span<const Data> data);
```

此时 C++ 学习已经从：

> 语言使用

进入：

> 软件设计。

---

# 14. 综合阶段：从 C++ 程序员到系统工程师

最终把前面所有知识放进真实问题中：

```text
需求
↓
数据模型
↓
Ownership Model
↓
Lifetime Model
↓
Concurrency Model
↓
API
↓
Memory Layout
↓
Compiler
↓
Runtime
↓
Hardware Cost
```

此时判断一个方案时，不再只问：

> 能不能写？

而是问：

```text
语义是否正确？
所有权是否清晰？
生命周期是否安全？
接口是否表达约束？
是否存在 UB？
线程安全吗？
内存布局是否合理？
cache 行为如何？
有没有多余 allocation？
抽象成本是多少？
未来是否容易演化？
```

---

# 15. 每一节课程的固定学习方式

为了避免重新退化成“八股背诵”，每一道经典题尽量按照同一结构展开。

## 1. 面试题

先明确：

> 面试官通常会怎样问？

---

## 2. 30 秒回答

整理出适合面试现场的简洁回答。

目标是：

> 先能答出来。

---

## 3. 继续追问

模拟面试官常见追问。

例如：

```text
malloc 和 new 有什么区别？

↓ 追问

operator new 又是什么？

↓ 追问

placement new 呢？

↓ 追问

只有 storage 时对象存在了吗？

↓
Object Lifetime
```

---

## 4. 为什么这样设计

进入语言设计层：

> 它究竟解决了什么问题？

---

## 5. C 语言对照

分析：

> 如果没有这个 C++ 机制，用 C 会怎样实现？

---

## 6. 编译器 / OS / CPU 视角

必要时继续向下：

```text
源码
↓
IR
↓
机器代码
↓
内存
↓
CPU
```

---

## 7. 工程实践

最后回答：

> 实际项目中应该怎样选择？

并讨论对应 trade-off。

---

## 8. 一句话总结

最终留下一个能够长期记住的核心模型，而不是十几条孤立结论。

---

# 16. 面试准备目标

本项目仍然服务于真实的大厂 C++ 面试。

但目标不是准备一本更厚的“八股文”。

而是希望达到三个层级。

## Level 1：能回答

面对经典问题：

> 知道标准答案。

---

## Level 2：能解释

能够继续回答：

> 为什么？

并解释背后的语言机制。

---

## Level 3：能设计

面对没有标准答案的问题，可以根据：

```text
lifetime
ownership
type
memory
performance
concurrency
ABI
```

做出合理工程选择。

这才是整个项目真正追求的能力上限。

---

# 17. 项目文档组织

后续课程文档建议按照阶段建立。

```text
README.md

00-从源码到可执行程序/
    00-阶段总览.md
    01-CPP程序是如何运行起来的.md
    02-预处理与Translation-Unit.md
    03-Object-File与Symbol.md
    ...

01-对象资源与生命周期/
    00-阶段总览.md
    01-new与malloc.md
    02-对象生命周期.md
    03-RAII.md
    ...

02-对象模型与动态多态/
03-类型系统与接口设计/
04-模板泛型与STL/
05-编译链接与语言底层/
06-并发与内存模型/
07-现代C++工程设计/
```

README 作为项目入口。

每个阶段设置一个：

```text
00-阶段总览.md
```

记录：

- 本阶段目标；
- 核心知识模型；
- 面试题目录；
- 已完成内容；
- 与其他阶段的联系。

所有专题文档通过 Obsidian 双链互相连接。

---

# 18. 当前学习进度

## 总体进度

- [x] 确立项目学习理念
- [x] 确立自顶向下课程框架
- [x] 确立“经典面试题驱动”的学习方式
- [x] 完成第 0～7 阶段总体设计
- [ ] 第 0 阶段：从源码到可执行程序
- [ ] 第 1 阶段：对象、资源与生命周期
- [ ] 第 2 阶段：对象模型与动态多态
- [ ] 第 3 阶段：类型系统与接口设计
- [ ] 第 4 阶段：模板、泛型与 STL
- [ ] 第 5 阶段：编译器、链接器与语言底层深化
- [ ] 第 6 阶段：并发与 C++ 内存模型
- [ ] 第 7 阶段：现代 C++ 工程设计与性能
- [ ] 综合阶段

当前准备进入：

> **第 0 阶段：从源码到真正运行的程序**

第一道核心问题：

> **一个 C++ 程序，从 `.cpp` 源代码到 CPU 开始执行 `main()`，完整经历了什么？**

---

# 19. 项目的最终目标

完成整个项目以后，希望面对一段 C++ 代码时，能够同时从多个层次理解它：

```text
源码层
它表达了什么？

语言层
对象和类型的语义是什么？

设计层
所有权和生命周期是什么？

编译层
编译器会怎样理解？

链接层
最终有哪些 symbol？

运行时
对象位于哪里？

OS 层
进程和虚拟内存发生了什么？

硬件层
CPU 和 cache 真正付出了什么成本？
```

最终形成一种稳定的 C++ 思维方式：

> **语法只是表象，背后是对象、生命周期、类型、编译器、操作系统与机器共同组成的系统。**

而在 AI 辅助编码成为常态之后：

> **AI 负责快速产生代码，人负责建立模型、制定约束、判断设计，并理解机器最终真正执行了什么。**

这就是本项目所希望达到的 C++ 理解上限。
