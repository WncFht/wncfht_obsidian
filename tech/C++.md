# C++

## 1 动态内存

### 1.1 new 和 delete 运算符

- 栈: 声明的所有变量将占用栈内存
- 堆: 未使用的内存

```C++
new data-type;

double* pvalue = NULL;
pvalue = new double;

if (!(pvalue = new double)) {
	cout << "Error: out of memory." << endl;
	exit(1);
}

delete pvalue;
```

- new 不仅分配的内存还创建了对象
- malloc () 只分配的内存

### 1.2 数组的动态内存分配

```C++
int ***array;
// 假定数组第一维为 m， 第二维为 n， 第三维为h
// 动态分配空间
array = new int **[m];
for( int i=0; i<m; i++ )
{
    array[i] = new int *[n];
    for( int j=0; j<n; j++ )
    {
        array[i][j] = new int [h];
    }
}
//释放
for( int i=0; i<m; i++ )
{
    for( int j=0; j<n; j++ )
    {
        delete [] array[i][j];
    }
    delete [] array[i];
}
delete [] array;
```

### 1.3 对象的动态内存分配

```C++
#include <iostream>
using namespace std;
 
class Box
{
   public:
      Box() { 
         cout << "调用构造函数！" <<endl; 
      }
      ~Box() { 
         cout << "调用析构函数！" <<endl; 
      }
};
 
int main( )
{
   Box* myBoxArray = new Box[4]; // 一个包含 4 个 Box 对象的数组
 
   delete [] myBoxArray; // 删除数组
   return 0;
}
```

```out
调用构造函数！
调用构造函数！
调用构造函数！
调用构造函数！
调用析构函数！
调用析构函数！
调用析构函数！
调用析构函数！
```

## 2 命名空间

- 编译器为了区别同名函数引入了命名空间

### 2.1 定义命名空间

```C++
namespace namespace_name {
 // code
}

namespace_name::code;
```

### 2.2 using 指令

### 2.3 不连续的命名空间

### 2.4 嵌套的命名空间

```C++
namespace namespace_name1 {
   // 代码声明
   namespace namespace_name2 {
      // 代码声明
   }
}

// 访问 namespace_name2 中的成员
using namespace namespace_name1::namespace_name2;
 
// 访问 namespace_name1 中的成员
using namespace namespace_name1;
```

## 3 模板

- 泛型编程: 独立于任何特定类型的方式写代码

### 3.1 函数模板

```C++
template <typename type> ret-type func-name(parameter list)
{
   // 函数的主体
}
```

```C++
#include <iostream>
#include <string>
 
using namespace std;
 
template <typename T>
inline T const& Max (T const& a, T const& b) 
{ 
    return a < b ? b:a; 
} 

int main ()
{
 
    int i = 39;
    int j = 20;
    cout << "Max(i, j): " << Max(i, j) << endl; 
 
    double f1 = 13.5; 
    double f2 = 20.7; 
    cout << "Max(f1, f2): " << Max(f1, f2) << endl; 
 
    string s1 = "Hello"; 
    string s2 = "World"; 
    cout << "Max(s1, s2): " << Max(s1, s2) << endl; 
 
    return 0;
}
```

### 3.2 类模板

```C++
template <class type> class class-name {
// code
}
```

```C++
#include <iostream>
#include <vector>
#include <cstdlib>
#include <string>
#include <stdexcept>
 
using namespace std;
 
template <class T>
class Stack { 
  private: 
    vector<T> elems;     // 元素 
 
  public: 
    void push(T const&);  // 入栈
    void pop();               // 出栈
    T top() const;            // 返回栈顶元素
    bool empty() const{       // 如果为空则返回真。
        return elems.empty(); 
    } 
}; 
 
template <class T>
void Stack<T>::push (T const& elem) 
{ 
    // 追加传入元素的副本
    elems.push_back(elem);    
} 
 
template <class T>
void Stack<T>::pop () 
{ 
    if (elems.empty()) { 
        throw out_of_range("Stack<>::pop(): empty stack"); 
    }
    // 删除最后一个元素
    elems.pop_back();         
} 
 
template <class T>
T Stack<T>::top () const 
{ 
    if (elems.empty()) { 
        throw out_of_range("Stack<>::top(): empty stack"); 
    }
    // 返回最后一个元素的副本 
    return elems.back();      
} 
 
int main() 
{ 
    try { 
        Stack<int>       intStack;    // int 类型的栈 
        Stack<string> stringStack;    // string 类型的栈 
 
        // 操作 int 类型的栈 
        intStack.push(7); 
        cout << intStack.top() <<endl; 
 
        // 操作 string 类型的栈 
        stringStack.push("hello"); 
        cout << stringStack.top() << std::endl; 
        stringStack.pop(); 
        stringStack.pop(); 
    } 
    catch (exception const& ex) { 
        cerr << "Exception: " << ex.what() <<endl; 
        return -1;
    } 
}
```

```out
7
hello
Exception: Stack<>::pop(): empty stack
```

## 4 预处理器

- 不是 C++语句，不会以分号结尾

### 4.1 #define 预处理

```C++
#define macro-name replacement-text 
```

### 4.2 参数宏

```C++
#include <iostream>
using namespace std;
 
#define MIN(a,b) (a<b ? a : b)
 
int main ()
{
   int i, j;
   i = 100;
   j = 30;
   cout <<"较小的值为：" << MIN(i, j) << endl;
 
    return 0;
}
```

### 4.3 条件编译

```C++
#ifdef NULL
   #define NULL 0
#endif

#ifdef DEBUG
   cerr <<"Variable x = " << x << endl;
#endif

#if 0
   不进行编译的代码
#endif
```

### 4.4 # 和 ## 运算符

```C++
#include <iostream>
using namespace std;
 
#define MKSTR( x ) #x
 
int main ()
{
    cout << MKSTR(HELLO C++) << endl;
 
    return 0;
}
```

```C++
#include <iostream>
using namespace std;
 
#define concat(a, b) a ## b
int main()
{
   int xy = 100;
   
   cout << concat(x, y);
   return 0;
}
```

### 4.5 预定义宏

![QQ_1726539879740.png](https://raw.githubusercontent.com/WncFht/picture/main/picture/QQ_1726539879740.png)

## 5 信号处理

![QQ_1726539936441.png](https://raw.githubusercontent.com/WncFht/picture/main/picture/QQ_1726539936441.png)

### 5.1 signal () 函数

```C++
void (*signal (int sig, void (*func)(int)))(int); 

signal(registered signal, signal handler)
```

```C++
#include <iostream>
#include <csignal>
#include <unistd.h>
 
using namespace std;
 
void signalHandler( int signum )
{
    cout << "Interrupt signal (" << signum << ") received.\n";
 
    // 清理并关闭
    // 终止程序  
 
   exit(signum);  
 
}
 
int main ()
{
    // 注册信号 SIGINT 和信号处理程序
    signal(SIGINT, signalHandler);  
 
    while(1){
       cout << "Going to sleep...." << endl;
       sleep(1);
    }
 
    return 0;
}
```

按 Ctrl + C 中断程序:

```out
Going to sleep....
Going to sleep....
Going to sleep....
Interrupt signal (2) received.
```

### 5.2 raise () 函数

```C++
int raise (signal sig);
```

```C++
#include <iostream>
#include <csignal>
#include <unistd.h>
 
using namespace std;
 
void signalHandler( int signum )
{
    cout << "Interrupt signal (" << signum << ") received.\n";
 
    // 清理并关闭
    // 终止程序 
 
   exit(signum);  
 
}
 
int main ()
{
    int i = 0;
    // 注册信号 SIGINT 和信号处理程序
    signal(SIGINT, signalHandler);  
 
    while(++i){
       cout << "Going to sleep...." << endl;
       if( i == 3 ){
          raise(SIGINT);
       }
       sleep(1);
    }
 
    return 0;
}
```

### 5.3 多线程

- 两种类型的多任务处理
	- 基于进程是程序的并发执行
	- 基于线程是同一程序的片段的并发执行

### 5.4 概念说明

#### 5.4.1 线程（Thread）

- 线程是程序执行中的单一顺序控制流，多个线程可以在同一个进程中独立运行。
- 线程共享进程的地址空间、文件描述符、堆和全局变量等资源，但每个线程有自己的栈、寄存器和程序计数器。

#### 5.4.2 并发（Concurrency）与并行 （Parallelism）

- **并发**：多个任务在时间片段内交替执行，表现出同时进行的效果。
- **并行**：多个任务在多个处理器或处理器核上同时执行。  
C++11 以后有多线程支持:
- [std::thread](https://www.runoob.com/cplusplus/cpp-libs-thread.html)：用于创建和管理线程。
- [std::mutex](https://www.runoob.com/cplusplus/cpp-libs-mutex.html)：用于线程之间的互斥，防止多个线程同时访问共享资源。
- std::lock_guard 和 std::unique_lock：用于管理锁的获取和释放。
- [std::condition_variable](https://www.runoob.com/cplusplus/cpp-libs-condition_variable.html)：用于线程间的条件变量，协调线程间的等待和通知。
- [std::future](https://www.runoob.com/cplusplus/cpp-libs-future.html) 和 std::promise：用于实现线程间的值传递和任务同步。

### 5.5 创建线程

```C++
#include<thread>
std::thread thread_object(callable, args...);
```

#### 5.5.1 使用函数指针

```C++
#include <iostream>
#include <thread>

void printMessage(int count) {
    for (int i = 0; i < count; ++i) {
        std::cout << "Hello from thread (function pointer)!\n";
    }
}

int main() {
    std::thread t1(printMessage, 5); // 创建线程，传递函数指针和参数
    t1.join(); // 等待线程完成
    return 0;
}
```

#### 5.5.2 使用函数对象

```C++
#include <iostream>
#include <thread>

class PrintTask {
public:
    void operator()(int count) const {
        for (int i = 0; i < count; ++i) {
            std::cout << "Hello from thread (function object)!\n";
        }
    }
};

int main() {
    std::thread t2(PrintTask(), 5); // 创建线程，传递函数对象和参数
    t2.join(); // 等待线程完成
    return 0;
}
```

#### 5.5.3 使用 Lambda 表达式

```C++
#include <iostream>
#include <thread>

int main() {
    std::thread t3([](int count) {
        for (int i = 0; i < count; ++i) {
            std::cout << "Hello from thread (lambda)!\n";
        }
    }, 5); // 创建线程，传递 Lambda 表达式和参数
    t3.join(); // 等待线程完成
    return 0;
}
```

### 5.6 线程管理

```C++
t.join();
t.detach();
```

### 5.7 线程的传参

#### 5.7.1 值传递

```C++
std::thread t(funx, arg1, arg2);
```

#### 5.7.2 引用传递

```C++
#include <iostream>
#include <thread>

void increment(int& x) {
    ++x;
}

int main() {
    int num = 0;
    std::thread t(increment, std::ref(num)); // 使用 std::ref 传递引用
    t.join();
    std::cout << "Value after increment: " << num << std::endl;
    return 0;
}
```

### 5.8 线程同步与互斥

#### 5.8.1 互斥量（Mutex）

```C++
std::mutex mtx;
mtx.lock();   // 锁定互斥锁
// 访问共享资源
mtx.unlock();// 释放互斥锁

std::lock_guard<std::mutex> lock(mtx); // 自动锁定和解锁
// 访问共享资源
```

#### 5.8.2 锁（Locks）

- `std::lock_guard` ：作用域锁，当构造时自动锁定互斥量，当析构时自动解锁。
- `std::unique_lock` ：与 `std::lock_guard` 类似，但提供了更多的灵活性，例如可以转移所有权和手动解锁。

```C++ 
#include <mutex>

std::mutex mtx;

void safeFunctionWithLockGuard() {
    std::lock_guard<std::mutex> lk(mtx);
    // 访问或修改共享资源
}

void safeFunctionWithUniqueLock() {
    std::unique_lock<std::mutex> ul(mtx);
    // 访问或修改共享资源
    // ul.unlock(); // 可选：手动解锁
    // ...
}
```

#### 5.8.3 条件变量（Condition Variable）

```C++ 
std::condition_variable cv;
std::mutex mtx;
bool ready = false;

std::unique_lock<std::mutex> lock(mtx);
cv.wait(lock, []{ return ready; }); // 等待条件满足
// 条件满足后执行
```

```C++ 
#include <mutex>
#include <condition_variable>

std::mutex mtx;
std::condition_variable cv;
bool ready = false;

void workerThread() {
    std::unique_lock<std::mutex> lk(mtx);
    cv.wait(lk, []{ return ready; }); // 等待条件
    // 当条件满足时执行工作
}

void mainThread() {
    {
        std::lock_guard<std::mutex> lk(mtx);
        // 准备数据
        ready = true;
    } // 离开作用域时解锁
    cv.notify_one(); // 通知一个等待的线程
}
```

TODO

#### 5.8.4 原子操作（Atomic Operations）

- 对共享数据的访问不可分割

```C++
#include <atomic>
#include <thread>

std::atomic<int> count(0);

void increment() {
    count.fetch_add(1, std::memory_order_relaxed);
}

int main() {
    std::thread t1(increment);
    std::thread t2(increment);
    t1.join();
    t2.join();
    return count; // 应返回2
}
```

#### 5.8.5 线程局部存储（Thread Local Storage，TLS）

- 允许每个线程有自己的数据副本

```C++
#include <iostream>
#include <thread>

thread_local int threadData = 0;

void threadFunction() {
    threadData = 42; // 每个线程都有自己的threadData副本
    std::cout << "Thread data: " << threadData << std::endl;
}

int main() {
    std::thread t1(threadFunction);
    std::thread t2(threadFunction);
    t1.join();
    t2.join();
    return 0;
}
```

#### 5.8.6 死锁（Deadlock）和避免策略

- 死锁即多个线程互相等待对方释放资源
	- 总是以相同的顺序请求资源。
	- 使用超时来尝试获取资源。
	- 使用死锁检测算法。

### 5.9 线程间通信

```C++
std::promise<int> p;
std::future<int> f = p.get_future();

std::thread t([&p] {
    p.set_value(10); // 设置值，触发 future
});

int result = f.get(); // 获取值
```

C++17 引入了并行算法库

```C++
#include <algorithm>
#include <vector>
#include <execution>

std::vector<int> vec = {1, 2, 3, 4, 5};
std::for_each(std::execution::par, vec.begin(), vec.end(), [](int &n) {
    n *= 2;
});
```

TODO  
[多线程实例](https://www.runoob.com/w3cnote/cpp-multithread-demo.html)

## 6 Web 编程

### 6.1 什么是 CGI？

- 公共网关接口（CGI），是一套标准，定义了信息是如何在 Web 服务器和客户端脚本之间进行交换的。
- CGI 规范目前是由 NCSA 维护的，NCSA 定义 CGI 如下：
	- 公共网关接口（CGI），是一种用于外部网关程序与信息服务器（如 HTTP 服务器）对接的接口标准。

### 6.2 Web 浏览

- 您的浏览器联系上 HTTP Web 服务器，并请求 URL，即文件名。
- Web 服务器将解析 URL，并查找文件名。如果找到请求的文件，Web 服务器会把文件发送回浏览器，否则发送一条错误消息，表明您请求了一个错误的文件。
- Web 浏览器从 Web 服务器获取响应，并根据接收到的响应来显示文件或错误消息。

### 6.3 CGI 架构图

![image.png](https://raw.githubusercontent.com/WncFht/picture/main/picture/20240917120115.png)

### 6.4 Web 服务器配置

TODO
