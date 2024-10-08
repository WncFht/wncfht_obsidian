---
url: https://blog.csdn.net/weixin_45031801/article/details/134399664
title: 【Linux】GDB 保姆级调试指南（什么是 GDB？GDB 如何使用？）_gdb 教程 - CSDN 博客
date: 2024-10-08 22:22:47
tag: 
summary: 文章浏览阅读3.6w次，点赞234次，收藏768次。GDB是Linux下非常好用且强大的调试工具。GDB可以调试C、C++、Go、java、 objective-c、PHP等语言。对于以后想称为一个Linux下工作的c/c++程序员，GDB是必不可少的工具，所以本篇来从零讲解GDB在LInux的调试。    对于GDB调试器来说，不像VS编译器中那样的图形化界面形式，而是采用纯命令行的形式进行调试。so 在开始学习的时候，大家可能会感觉晦涩难懂，但是这是C/C++程序员必须要掌握的技能，所以我将手把手进行零基础的讲解，本篇以C语言来讲解和调试。_gdb教程
---

![](https://i-blog.csdnimg.cn/blog_migrate/615489c69d098bb46dc8e3f30b31b4f2.png)

![](https://i-blog.csdnimg.cn/blog_migrate/57694fee9599109c66d749f9849988d3.gif)

**目录**

[一、前言](#t0) 

[二、什么是 GDB](#t1) 

 [💦何为调试](#t2)

 [💦GDB 调试工具 --- 提供的帮助](#t3)

 [三、GDB 的安装教程](#t4)

 [💦检查机器上是否安装了 gdb](#t5)

 [💦gdb 的安装](#t6)

 [四、GDB 在那个开发版本（debug / release）中进行应用呢？](#t7)

 [💦看看 gdb 如何使用](#t8)

 [💦【Debug 版本】与【Release 版本】的区别](#t9)

 [💦Linux 中开发环境的转换](#t10)

 [💦总结](#t11)

[五、使用 GDB 调试代码 ---- 指令学习](#t12)

[💦 指令集汇总](#t13) 

[💦指令演示](#t14)

[✨行号显示](#t15) 

 [✨断点设置](#t16)

 [✨查看断点信息](#t17) 

 [✨删除断点](#t18) 

 [✨开启 / 禁用断点](#t19)

 [✨运行 / 调试](#t20)

 [✨逐过程和逐语句](#t21) 

 [✨ 打印 / 追踪变量](#t22)

 [✨ 查看函数调用](#t23)

 [✨ 修改变量的值](#t24)

 [💦最常用指令（指令三剑客）](#t25)

 [✨指定行号跳转](#t26)

 [✨强制执行函数](#t27)

 [✨跳转到下一断点](#t28) 

 [六、GDB 调试的实战演练](#t29)

 [七、总结](#t30)

 [八、共勉](#t31)

## **一、前言** 

 **GDB** 是 **Linux** 下非常好用且强大的**调试工具**。GDB 可以调试 C、C++、Go、java、 objective-c、PHP 等语言。对于以后想称为一个 **Linux 下工作的 c/c++ 程序员****，GDB 是必不可少的工具**，**所以本篇来从零讲解 GDB 在 LInux 的调试**。

        对于 **GDB 调试器**来说，**不像 VS 编译器中那样的图形化界面形式**，而是采用**纯命令行的形式**进行调试。so 在开始学习的时候，大家可能会感觉晦涩难懂，但是这是 **C/C++ 程序员必须要掌握的技能**，所以我将手把手进行零基础的讲解，本篇以 **C 语言来讲解和调试**。

## 二、什么是 GDB 

 **GDB** 是由 GUN 软件系统社区提供的**调试工具**，同 GCC 配套组成了一套完整的开发环境，GDB 是 Linux 和许多 类 Unix 系统的标准开发环境。

###  **💦何为调试**

 **⭐调试： 就是让代码一步一步慢慢执行，跟踪程序的运行过程。比如，可以让程序停在某个地方，查看当前所有变量的值，或者内存中的数据；也可以让程序一次只执行一条或者几条语句，看看程序到底执行了哪些代码。帮助我们发现代码中的错误，改进代码。**

###  **💦GDB 调试工具 --- 提供的帮助**

 一般来说，**GDB** 主要能够提供以下**四个方面**的帮助：

*   程序启动时，**可以按照自定义的要求运行程序**，例如设置参数和环境变量；
*   可以让被调试的程序在所指定的代码处暂停运行，并查看当前运行状态 （例如当前变量的值，函数的执行结果），即**支持断点调试**
*   当程序被停住时，可以检查当前程序的中的变量的状态；
*   在程序执行过程中，可以改变某个变量的值，还可以改变代码的执行顺序，**从而尝试修改程序中出现的逻辑错误**

##  **三、GDB 的安装教程**

 **在 CentOS7 下安装 GDB**

###  **💦检查机器上是否安装了 gdb**

```
rpm -qa | grep gdb

```

![](https://i-blog.csdnimg.cn/blog_migrate/d9b577c01494816c85b26a6eae447aea.png)

若没有反应，则没有安装，进行下一步

###  **💦gdb 的安装**

```
sudo yum install -y gdb

``` 

##  **四、GDB 在那个开发版本（debug / release）中进行应用呢？**

###  💦看看 gdb 如何使用

 **下面是本次调试所要使用到的代码：**

```
  1 #include <stdio.h>
  2 
  3 int AddToTop(int top)
  4 {
  5     printf("Enter AddToTop\n");
  6 
  7     int count = 0;
  8     for(int i = 1;i <= top; ++i)
  9     {
 10        count += i;
 11     }
 12 
 13     printf("Quit AddToTop\n");                                                                         
 14     return count;
 15 }
 16 
 17 int main(void)
 18 {
 19     int top = 10;
 20     int ret = AddToTop(top);
 21 
 22     printf("ret = %d\n", ret);
 23     return 0;
 24 }
```

**下面是 Makefile 中的内容，用于自动化编译：**

```
  1 mytest:test.c
  2     gcc -o mytest test.c -std=c99                                                                      
  3 .PHONY:clean
  4 clean:
  5     rm -rf mytest
```

![](https://i-blog.csdnimg.cn/blog_migrate/ead3b9f9cc76a1aca7ac08bed352444b.png)

**注：`-std=c99`表示以 c99 的标准来编译代码**

* * *

*   如果要进入 gdb 开始调试，那直接**`gdb + 可执行程序`**即可

*   不过进去之后发现似乎有一些奇怪的内容，【no debugging symbols found】，翻译过来就是**没有调试信息**。那这是为何呢？是 gdb 出问题了吗？

![](https://i-blog.csdnimg.cn/blog_migrate/a7c9184b0ae13ec25918d60c33187343.png)

*   先不要着急，如果有经常调试的同学就可以知道只有在【DeBug】的环境下才会有我们想要的调试信息，所以可以初步推断这可能不是一个【DeBug】版本的可执行程序

*   先使用**`q(quit)`**退出 gdb

  
**让我们先看下去，了解一下其他的知识再来解决这个问题** 

###  **💦【Debug 版本】与【Release 版本】的区别**

接下去我们就来说说有关【DeBug】和【Release】版本的不同之处  
📚**【Debug】**—— **调试版本**  
📚**【Release】**—— **发布版本**

*   在使用 VS 的时候我们可以直接使用鼠标来进行操作，**当前程序以 DeBug 或者是 Release 的形式进行运行，**那么运行出来的可执行程序版本也是不同的**，我们程序员在编写代码后运行一般是使用【DeBug】环境进行运行。因为在企业里写软件项目，将代码写完后程序员自己要做简单的测试，保证代码没有问题**

![](https://i-blog.csdnimg.cn/blog_migrate/a48c23aa5438317d8fc12df8c266d9c1.png)

*   当程序员自己测试完没有问题之后，就会将这个可执行程序给到`测试人员`进行测试，而且会给出自己的单元测试报告。对**于测试人员来说所处的模式是【Release】**，也就是将来客户要使用的这款软件的发布版本
*   当测试在测的过程中，一定会发现一些问题。此时测试人员就会把报告再打回研发部。研发部做修改重新生成 Release 版本的可行性程序给到测试人员继续测试

*   最后只有当测试通过了，再将生成的【单元测试报告】与产品经理进行核对之后没有问题，那这个软件才可以真正地面向市场👉

###  **💦Linux 中开发环境的转换**

 其实对于我们刚才直接 **make** 自动化生成的可执行程序是通过 **gcc** 直接编译产生得到的，它是一个**【Release】版本**的可执行程序，因此无法进行调试。

![](https://i-blog.csdnimg.cn/blog_migrate/d580a3d3ec7bd279ed7047e0a5de2b06.png)

*   若是我们想要使用**`gcc/g++`**去生成一个可执行程序时，**默认是【Release】版本的**，**而不是【DeBug】**

*   但若是我们想要去生成一个**【DeBug】版本**的可执行程序也是可以的，只需要修改一下我们的 Makefile 即可，给 gcc 后面带上一个**`-g`**的命令选项，此时再去 **make** 一下的话生成的就是【DeBug】版本的了

* * *

*   为了之前的【Release】版本不被覆盖，我们将其重命名一下为**`mytest-release`**

*   在生成【DeBug】版本后一样对其进行一个重命名为**`mytest-debug`**

![](https://i-blog.csdnimg.cn/blog_migrate/2d18e41dfaba25b10497bbdf5d9d96b1.png)

*   通过观察上图中两个可执行文件的大小便可以发现虽然它们都是可执行程序，但是容量大小却不一样，这是为什么呢❓
    

⚡ ：因为以 Release 版本发布的软件是给客户的，**客户是不需要调试信息**的  
⚡ ：往可执行程序里添加很多的调试信息意味着**软件的体积会变大**

*   一方面，用户下载需要时间了
*   另一方面，用户下载好之后将软件启动、运行都需要更多的时间，体验不好。一般能不加就不加

⚡ ：**但是对于 DeBug 来说会自动加调试信息，容量体积比 Release 大** 

###  **💦总结**

⚡ ：程序的发布方式有两种，**debug 模式和 release 模式**  
⚡ ：**Linux gcc/g++ 出来的二进制程序，默认是 release 模式**  
⚡ ：**要使用 gdb 调试，必须在源代码生成二进制程序的时候, 加上`-g`选项**

## **五、使用 GDB 调试代码 ---- 指令学习**

### 💦 指令集汇总 

因为这个调试器是在 Linux 环境下的，是纯[命令行](https://so.csdn.net/so/search?q=%E5%91%BD%E4%BB%A4%E8%A1%8C&spm=1001.2101.3001.7020)模式，所以会有很多的指令，做好心里准备😢

注：**（）括号**里面是该指令的全称   
💜  **`l(list) 行号/函数名`** —— 显示对应的 code，每次 10 行  
💜**`r(run)` —— F5**【无断点直接运行、有断点从第一个断点处开始运行】  
💜**`b(breakpoint) + 行号`** —— 在那一行打断点  
💜**`b 源文件：函数名`** —— 在该函数的第一行打上断点  
💜**`b 源文件：行号`** —— 在该源文件中的这行加上一个断点吧  
💜**`info b`** —— 查看断点的信息  
breakpoint already hit 1 time【此断点被命中一次】  
💜**`d(delete) + 当前要删除断点的编号`** —— 删除一个断点【不可以 d + 行号】

*   若当前没有跳出过 gdb，则断点的编号会持续累加

💜**`d + breakpoints`** —— 删除所有的断点  
💜**`disable b(breakpoints)`** —— 使所有断点无效【默认缺省】  
💜**`enable b(breakpoints)`** —— 使所有断点有效【默认缺省】  
💜**`disable b(breakpoint) + 编号`** —— 使一个断点无效【禁用断点】  
💜**`enable b(breakpoint) + 编号`** —— 使一个断点有效【开启断点】

*   相当于 VS 中的空断点

💜**`enable breakpount`** —— 使一个断点有效【开启断电】  
💜**`n(next)`** —— 逐过程【相当于 F10，为了查找是哪个函数出错了】  
💜**`s(step)`** —— 逐语句【相当于 F11，】  
💜**`bt`** —— 看到底层函数调用的过程【函数压栈】  
💜**`set var`** —— 修改变量的值  
💜**`p(print) 变量名`** —— 打印变量值  
💜**`display`** —— 跟踪查看一个变量，每次停下来都显示它的值【变量 / 结构体…】  
💜**`undisplay + 变量名编号`** —— 取消对先前设置的那些变量的跟踪

排查问题三剑客🗡  
💜**`until + 行号`** —— 进行指定位置跳转，执行完区间代码  
💜**`finish`** —— 在一个函数内部，执行到当前函数返回，然后停下来等待命令  
💜**`c(continue)`** —— 从一个断点处，直接运行至下一个断点处【VS 下不断按 F5】

### **💦指令演示**

 看了上面的这些命令后，相信你一定回到了刚开始学习 **Linux 指令的时候那种恐惧感，不过没关系，我会一一地演示这些指令，让你在看完本文后有一个基本的调试能力💪**

 

*   首先我们进入到 gdb，然后它会等待我们输入指令

![](https://i-blog.csdnimg.cn/blog_migrate/ff201e377b5b3edc062342e864bb75a2.png) 

#### **✨行号显示** 

 **`l(list) 行号/函数名` —— 显示对应的 code，每次 10 行**

*   首先若是直接【L】的话便会随机显示出该源文件中的随机 10 行内容，这不是我们想要的

![](https://i-blog.csdnimg.cn/blog_migrate/50a65171f19bd958326f0cf8d8ba5738.png)

*   若是【L 0】或者是【L 1】的话那就是从第一行开始往下列 10 行的内容
    *   注意这里的 L 是小写，而且与数字之间要有一个空格

![](https://i-blog.csdnimg.cn/blog_migrate/5e0a54e651c8e3cde2c6821266cb42d9.png)

*   接下去若是想要看到我们所写的全部代码，**只需要多`Enter`几次就可以了，gdb 会自动记忆你上次敲入的指令**

![](https://i-blog.csdnimg.cn/blog_migrate/db411f429eee375fed303dc8019d2256.png)

####  **✨断点设置**

 **`b + 行号` —— 在那一行打断点**

![](https://i-blog.csdnimg.cn/blog_migrate/15019303997fa6f8353f8e96d515c997.png)

**`b 源文件：函数名` —— 在该函数的第一行打上断点**

![](https://i-blog.csdnimg.cn/blog_migrate/cce226bc48aa2d4af23f4fc2cb185ab0.png)

**`b 源文件：行号` —— 在该源文件中的这行加上一个断点**

![](https://i-blog.csdnimg.cn/blog_migrate/2ac32094fa628833fac2b7a75f0b86c9.png)

####  ✨查看断点信息 

 ****`info b`** —— 查看断点的信息**

*   若是直接执行【info】的话，出来的就是所有的调试信息

![](https://i-blog.csdnimg.cn/blog_migrate/6da6c5332f968159af666fdd09621bb6.png)

*   但若是我们只想查看一下所打的断点的信息，那就在后面加个**`b/breakpoint`**

![](https://i-blog.csdnimg.cn/blog_migrate/31125eecea592d13b142bb124e6349a4.png)

*   接下来简要介绍一下断点的一些字段信息

*   **Num** —— 编号
*    Type —— 类型
*    Disp —— 状态
*    Enb —— 是否可用
*    Address —— 地址
*    What —— 在此文件的哪个函数的第几行

![](https://i-blog.csdnimg.cn/blog_migrate/f74ddb5c9a37330aa30ed57effef33f8.png)

*   最后的话就是每个断点信息的下面这块**`breakpoint already hit 1 time`**即此断点被命中 1 次

#### **✨删除断点** 

**`d + 当前要删除断点的编号` —— 删除一个断点【不可以 d + 行号】**

![](https://i-blog.csdnimg.cn/blog_migrate/50fa05bfe2c2fb5e86250d50a28f6453.png)

**`d + breakpoints` —— 删除所有的断点**

![](https://i-blog.csdnimg.cn/blog_migrate/c96ba6f6fd384b04734ab084ba9d1210.png)

*   此时若继续将这个 20 行的断点打上时，就可以发现其编号为【4】，而并不是从 1 开始，这是因为我们没有退出过 gdb，所以会持续上一次的编号继续往下

![](https://i-blog.csdnimg.cn/blog_migrate/6d29d82a398c8cd0574ac1f7c958b9cb.png)

####  **✨开启 / 禁用断点**

 **`disable b(breakpoints)` —— 使所有断点无效【默认缺省】**

![](https://i-blog.csdnimg.cn/blog_migrate/38a0b7fc881cf9e748b1844e67dedc6c.png)

**`enable b(breakpoints)` —— 使所有断点有效【默认缺省】**

![](https://i-blog.csdnimg.cn/blog_migrate/cd0172b729a7eed8d00fd9ab408416f8.png)

**`disable b(breakpoint) + 编号` —— 使一个断点无效【禁用断点】**

![](https://i-blog.csdnimg.cn/blog_migrate/3bb0fb1b5e4ddaab41cbb5b33cf83c14.png)

**`enable b(breakpoint) + 编号` —— 使一个断点有效【开启断点】**

![](https://i-blog.csdnimg.cn/blog_migrate/715dbf8c57b50aefbc04bfae6dd03af8.png)

####  **✨运行 / 调试**

**`r(run)` —— F5【无断点直接运行、有断点从第一个断点处开始运行】**

*   首先若是将断点删除掉，使用【r】指令运行的话就会直接运行到程序结束

![](https://i-blog.csdnimg.cn/blog_migrate/d9be4ca1e1e5ded6ca58e1b9916d99da.png)

*   再加上断点去运行的话就会在打的断点处停下来

![](https://i-blog.csdnimg.cn/blog_migrate/15f27bb3dc3cfc1172dbe7d286938d5d.png)

#### **✨逐过程和逐语句** 

 ****`n(next)`** —— 逐过程【相当于 F10，为了查找是哪个函数出错了】**

*   可以看到，我从第一个断点处也就是 20 行的位置开始执行，按下【n】之后因为在其后即 22 行有一个断点，此时就会直接运行到断点处

![](https://i-blog.csdnimg.cn/blog_migrate/8717761203b17650ee9ca593c7f391fd.png)

**`s(step)` —— 逐语句【相当于 F11，一次走一条代码，可进入函数，同样的库函数也会进入】**

*   此时我们按下【s】，也就相当于是【step】，让程序一步一步地走，继而进入了**`Addtop`**这个函数，若是你在 printf() 语句要执行时按下【s】的话 gdb 就会进入 **printf() 库函数**内部去执行，这里就不展示了

![](https://i-blog.csdnimg.cn/blog_migrate/0b430886ed82108ddaba626b356bc911.png)

*   接下去我们可以就继续【n】，然后进行**逐过程**调试，来到 for 循环中，**那么逐过程也就是变量 i 的累加和计数器 count 的累加**，所以会反复执行（**通过图中最左侧可以看出是第 8 行和第 10 行在反复执行**）

*   可以看到后面我没有再按【n】了，但是依旧会执行上面的步骤，这点上面也有提到过，因为 gdb 会自动化记忆你上一次执行过的命令，所以若是不想再敲了，直接**`Enter`**就可以了

![](https://i-blog.csdnimg.cn/blog_migrate/877974b6c952308871521244c963d2b3.png)

#### **✨** 打印 / 追踪变量

**`p(print) 变量名` —— 打印变量值**

*   都执行了那么多次了，不知道【i】和【count】发生了怎样的变化，将它们打印出来看看吧💻

![](https://i-blog.csdnimg.cn/blog_migrate/2f365f5c56ce72add14c5a1354c41b7a.png)

*   通过继续执行【n】，然后再去打印就可以发现**`i`**的值和**`count`**的值发生了变化

![](https://i-blog.csdnimg.cn/blog_migrate/e1d471ac9bd999465371efa75bd7f062.png)

  
**但是你不觉得这样每次去打印会显得很繁琐吗，那一定会的，所以我们有更好的办法💡**  
**`display` —— 跟踪查看一个变量，每次停下来都显示它的值【变量 / 结构体…】**

![](https://i-blog.csdnimg.cn/blog_migrate/fd93f1614020e7d45fbc8de9686e75cf.png)

*   我们也可以去追踪一下这两个变量的地址，不过可以看到对于地址来说是不会发生改变的

![](https://i-blog.csdnimg.cn/blog_migrate/261ee30b7da8abfa5e812f08679c14d1.png)

  
**`undisplay + 变量名编号` —— 取消对先前设置的那些变量的跟踪**

*   但是呢，每次都追踪打印这么多内容又太多了，我想把它们取消了可以吗？答：**当然是可以的**

*   既然有**`display`，**那就有**`undisplay`**

![](https://i-blog.csdnimg.cn/blog_migrate/b5eb90f87d5cd21274fadadee2da3e7e.png)

#### ✨ 查看函数调用

**`bt` —— 看到底层函数调用的过程【函数压栈】**

*   通过仔细观察刚才追踪的 4 个变量最左侧的编号，就可以看到它们的排列的顺序是倒着的。因为变量 i 和变量 count 是我们先追踪的，它们的地址是我们后追踪的，所以可以看出这很像是**一个压栈的过程**

![](https://i-blog.csdnimg.cn/blog_migrate/e1363aa471d31d18d0d886fbe4e45c22.png)

*   其实不仅是对于它们，**`Addtop`**函数和**`main`**函数也呈现这样的关系。此时我们就可以通过【bt】这个指令来查看函数压栈的过程，此时便可以看到因为

![](https://i-blog.csdnimg.cn/blog_migrate/fe2d0db1801ee8f6f6f4dab120f14a0f.png)

#### **✨ 修改变量的值**

**`set var` —— 修改变量的值**

*   对于这个修改变量的值，很像是在 VS 里调试之前设置的那种条件断点，可以使调试开始后直接运行到此断点处。不过对于【set var】而言是在调试过程中进行设置

![](https://i-blog.csdnimg.cn/blog_migrate/96c363d2c82ee429e4a320d755efc0d4.png)

###  **💦最常用指令（指令三剑客）**

掌握了上面的这些，你就可以在 Linux 下调一些简单的代码了，不过想做到高效地进行调试，就需要学习一下**【三剑客】** 

####  **✨指定行号跳转**

**`until + 行号` —— 进行指定位置跳转，执行完区间代码**

*   可以看到，当前在 for 循环内容执行累加的逻辑，但若是我们一直这么执行下去，就没有时间排错了，除了上面的哪一种【set var】之外，还有一种方法其实起到直接结束当前循环的作用，那就是进行指定行号跳转

*   通过观察下图可以看到，当我们运行了**`until 13`**之后，程序直接就给出了我们最终的结果 count，而且即将要执行最后的打印语句，说明我们跳转成功了

![](https://i-blog.csdnimg.cn/blog_migrate/93aa9651f78d9a771a67a83484ee83f4.png)

####  **✨强制执行函数**

**`finish` —— 在一个函数内部，执行到当前函数返回，然后停下来等待命令**

*   有时候我们会有这样的需求，在初步排查的时候推断可能是某个函数内部的逻辑出了问题，但是呢又不想一步步地进到函数内部进行调试，在 VS 中其实很简单，**只需要在函数下方设个断点，然后 F5 直接运行到断点处即可**

*   但是在 Linux 下的 gdb 中，我们可以使用【finsh】指令来直接使一个函数执行完毕。从下图我们可以看到，首先【s】进到函数内部，接下去我直接使用**`finish`**，可以看到它直接回到了调用函数的位置，returned 了一个返回值

![](https://i-blog.csdnimg.cn/blog_migrate/6a184149b9c1e42dfb743a947ab83c4c.png)

*   然后可以看到，在获取到返回值后，也就直接进行了 printf 打印

![](https://i-blog.csdnimg.cn/blog_migrate/e671d1d31c16fdf13205ec78eb2f27b7.png)

####  ✨跳转到下一断点 

**`c(continue)` —— 从一个断点处，直接运行至下一个断点处【VS 下不断按 F5】**

*   这点也是我刚才在上面有提到过的，在 VS 中，我们要直接跳转到下一个断点处只修要按下 F5 即可，那在 gdb 中该如何操作呢，你需要敲个【c】就可以了

*   从下图我们可以看出，对于这个指令的用处可谓是非常大，当我处于第一个断点也就是 20 行的时候，直接敲下【c】，就可以运行到第二个断点处也就是第 10 行。之后若反复敲【c】，因为这是一个单语句的循环，所以循环的下一次还是会执行到此处。**上面的这两个功能就和我们在 VS 中用的 F5 是一个道理**

![](https://i-blog.csdnimg.cn/blog_migrate/c07fd292da40750cccb7593eca48a083.png)

##  **六、GDB 调试的实战演练**

 📖**纸上得来终觉浅，绝知此事要躬行**🔨

以下是本次调试所要使用到的代码，相信你一定非常熟悉了，也就是使用指针交换两数

```
  1 #include <stdio.h>
  2 
  3 void swap(int* x, int* y)
  4 {
  5     int t = *x;
  6     *x = *y;
  7     *y = t;                                                                                            
  8 }
  9 int main(void)
 10 {
 11     int a = 10;
 12     int b = 20;
 13 
 14     printf("a = %d, b = %d\n", a, b);
 15 
 16     swap(&a, &b);
 17 
 18     printf("a = %d, b = %d\n", a, b);
 19     return 0;
 20 }
 
```

**首先先来看一下运行后的结果：**

![](https://i-blog.csdnimg.cn/blog_migrate/61b7cf908f3b05a8fbad0b1f025ea993.png)

**接下里，我们进入 GDB 调试模式，看看它们是如何交换的。**

*   首先我们在程序第 16 行设置上一个断点，然后【r】从第 15 行开始运行

![](https://i-blog.csdnimg.cn/blog_migrate/1069ea2684ebdd1c7442ca5f3b9f0fd7.png)

*   然后我们使用【s】进入到 swap 函数中，因为我首先不想调试，想先立马看看运行结果，但是此时又已经进入调试了，那么我们就可以使用到【finish】来立马执行完这个函数，然后观察一下结果

*   可以看到，最后打印出结果的时候 a 和 b 的值确实发生了交换

![](https://i-blog.csdnimg.cn/blog_migrate/fa88e657f94398dc6df1c36ca8032573.png)

*   既然清楚了二者会进行一个交换，接下去我们就逐语句【n】进行一个单步追踪吧

*   因为提前看了执行结果，所以我们要重新开始调试，按下【r】即可，它会询问你是否需要重新开始调试，选择 y 之后就可以重新从 16 行开始进行调试

![](https://i-blog.csdnimg.cn/blog_migrate/c1a2f2940f318d2bd457160cdb8e3f47.png)

*   首先通过【display】记录一下两个变量的值和地址

![](https://i-blog.csdnimg.cn/blog_migrate/3cfc638ad263c4d4a0aa922531192650.png)

*   接着按【s】进入到 swap 函数里，追踪一下指针 x 和指针 y 的内容，也就是它们所存放的地址，就可以看到，函数内部已经接受到了这两个变量的地址

![](https://i-blog.csdnimg.cn/blog_migrate/9f4e1716896f140a08b6a574bb33de51.png)

*   然后对我们要观察的值变化继续做一个追踪

*   并且在执行完第一个语句**`t = *x`**时，临时变量 t 中已经存放了变量 a 的值，也就是指针所指向的那块空间中的值

![](https://i-blog.csdnimg.cn/blog_migrate/75c072eecf61ddafb40a82d644f75ecb.png)

*   接下去执行**`*x = *y`**，此时 * x 中的值就发生了变化，因为指针 x 可以直接找到变量 a 的地址，所以可以对其中的内容做修改，就变为了 20

![](https://i-blog.csdnimg.cn/blog_migrate/3c112533920340488a14229dd9ea3008.png)

*   接下去执行**`*y = t`**，同理，指针 y 可以直接找到变量 b 的地址，所以可以对其中的内容做修改，将原本保存在临时变量 t 中的 10 赋值给到 * y，也就修改了其中的内容

![](https://i-blog.csdnimg.cn/blog_migrate/87c08909996fedbbe4ffa64027c977c7.png)

*   再按【n】的话这个 swap 函数就执行结束了，回到了 main 函数，就可以清楚地看到函数内部的修改带动了函数外部值的变化，真正地通过【传址调用】交换了两个数

![](https://i-blog.csdnimg.cn/blog_migrate/31359d0888335889254511e3412219c0.png)

**整体的调试结束！！！！**

##  **七、总结**

最后来总结一下本文所学习的内容📖

*    在本文的开始，我们在初见 GDB 的时候发现了**【no debugging symbols】**的报错信息，于是便谈到了一个可执行程序的**【DeBug】版本和【Release】版本**，知道了对于前者来说是我们程序员使用的环境，是存在调试信息的；对于后者来说是测试人员所处的环境，是不存在调试信息的。而对于 gcc/g++ 而言默认生成的可执行程序就是【Release】版本的，**因此我们要加上一个 - g 命令选项使其在 make 之后生成一个【DeBug】版本的可执行程序，这样就可以进行调试了**
*   接着我们正式进入到了**调试器 GDB 的使用**，介绍了很多相关的指令，这些都是我整理出来的常见指令，其实对于 GDB 来说还有很多指令，但是真正常用的也就这些，只要你认真地看下来，将它们都使用熟练了，相信你的调试功底会大幅度提升，对你在 VS 中的调试也是有所帮助的
*   最后，在学习了 GDB 的许多调试指令后，我们便进行了**【Swap Two Numbers】的实战演练**，不仅巩固了 C 语言中有关**传址调用以及函数栈帧**的相关指知识，而且使我们对于**调试器 GDB 的使用更上一层楼↑**

## **八、共勉**

   **以下就是我对【Linux 基础】GDB 调试器保姆级的理解，如果有不懂和发现问题的小伙伴，请在评论区说出来哦，同时我还会继续更新对【Linux 基础】Linux 环境开发 的理解，请持续关注我哦！！！**  

![](https://i-blog.csdnimg.cn/blog_migrate/5addf3c66b04e5eb36efe7ecb59ef90d.png)