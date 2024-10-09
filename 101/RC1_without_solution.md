---
title: ENGR1010J Recitation Class 1
author: [Zhang Jinbin]
date: October 2024
theme: default
---

# ENGR1010J Recitation Class 1 
![MATLAB Heart](heart.jpg)
## 1 Introduction
Welcome to JI, and our programming world. I hope a series of programming related course will make you fall in love with computer(Not only video games).

And before we start, I would like to introduce a little bit about <span style="color: blue;">ENGR1010J, Introduction to Computer and Programming</span>  itself.

This course is not difficult, but requires much more energy and time than you think. You'are strongly recommended to <span style="color: red;">Learn by Yourself</span>! Lectures and RCs are not enough to make you a strong programmer, or even qualified programmer.

Then how to become a strong programmer? Here are some tips:
1. ***Be Passionate*** : Try to discover the beauty of programming. (like the beautiful heart generated with MATLAB above).
2. ***Keep Practicing*** : Our workload is pretty low, not enough for you to get familiar with either language or algorithm. You may find extra exercises useful. LeetCode is a good option if you have enough knowledge.
3. ***Learn from Everywhere*** : You do not only have 1 lecturer and 2 TAs. You have the whole internet and friends from SJTU, JI or even high school classmates. Do not be shy to ask questions.
4. ***Think Independently*** : Do not just copy and paste. Try to understand the code and think about how to improve it.
5. ***Ask Question Politely*** : Ask question is a very very important symbol if you become a part of programmer community. We do not hate "stupid" questions, but we do hate impolite questions without any thinking.

I will explain more in the latter part, especially the last point.

## 2 Honor Code and Ask For Help
Honor code is complex and simple. If you follow the common rules, it does not exist. But if you break it, you may be in trouble. Break HC may lead to very serious consequences, like 0 points for that assignment, 1 letter deduction for the final grade, or even F for the whole course. What's more, you are not able to apply postgraduate recommendation without examination, and may not be able to apply for exchange program such as DD.

How to avoid breaking HC? Let's see some examples(for this course only):
1. **Student A cheat in the exam.**Violate HC, may lead to F grade. You should notice the announcement of the exam (you can check it in syllabus also). Some devices are allowed in one exam but not the other.
2. **Student B submit his/her Lab but cannot explain the logic behind it.** Violate HC, may lead to 0 points for that lab. Copy from other students, online resources and AI directly is not allowed. General discuss is allowed, but you should write your own code and understand the logic behind it.
3. **Student C, D, E.. submit the same work, even they all know the logic clearly.** Violate HC, may lead to 0 points for that lab. Even if you talk with each other how to solve it, you should write your own code. You can share the logic, but not the code.
4. **Student F ask his/her relatives/friends/other institute/previous JI students/TA to finish the lab for him/her.** Violate HC, may lead t o letter grade deduction. You can not leave your work to other people, current TA can only help you.
5. **Student G, H, I, J, K belongs to the same project group. And G copy other peoples work.** All the 5 students violate HC, you need to warn your group member if you find someone violate HC. If you do not warn, you will be punished as well.

Once you are familiar with HC, you could learn to ask questions. The following procedure is recommended:
1. ***Dig out the problem and think by yourself.*** Always think about the problem before asking. You may find the answer by yourself, and you will be much happier than just get the answer.
2. ***Search similar problem on Internet/Ask AI tools.*** Most of time, you could get answer at this stage. Do not just copy! You should learn something from the answer.
3. ***Talk with friends.*** Even when you get the answer, you can discuss it with your friends. You would get a deeper understanding, and your friends may benefit from your explanation as well.
4. ***Ask TA.*** You could ask TA through Feishu/Email/Office Hour/WeChat(Feishu is recommended). Your TA is also a busy student, so give them some personal time as well. Thanks.
5. ***Ask Professor.*** You could ask professor through Feishu/Email/Office Hour. But you should ask TA first. Professor is also a busy person, and he may not reply to you immediately. 

Every time you ask other people for help, please explain the problem clearly and attach your code (screenshots, source code or just a photo). You should also explain what you have tried and what you do not understand.

## 3 MATLAB Basics
![MATLAB Interface](MATLAB_Window.png)
```matlab
>> 1e3 % 1 thousand
ans =
        1000
>> 1e-3 % 1 thousandth
ans =
    1.0000e-03
```
![Change Directory](cd.png){width=80%}
```matlab
>> pwd % print working directory
ans =
    '/Users/moranxuege/SJTU_JI/2024FA/ENGR1010J/RCs/RC1/Demo'
>> cd .. % change directory to parent directory
>> pwd
ans = 
    '/Users/moranxuege/SJTU_JI/2024FA/ENGR1010J/RCs'
>> cd Demo % change directory to Demo
>> pwd
ans = 
    '/Users/moranxuege/SJTU_JI/2024FA/ENGR1010J/RCs/RC1/Demo'
```

### 3.1 MATLAB Expressions and Variables
Simple operations: +, -, *, /, ^, sqrt(), sin().
```matlab
>> 1+2
ans =
    3
>> 2^3
ans =
    8
>> sqrt(2)
ans =
    1.4142
>> a = sin(pi/6)
a =
    0.5000
>> b = sin(pi/3)
b =
    0.8660
>> a + b
ans =
    1.3660
```

### 3.2 MATLAB Functions
First of all, you should know the difference between script and function. Script is a series of commands that you can run all at once. Function is a block of code that you can call from other scripts or functions. 

You could create pure function m file, or a script file contains local function. In the previous case, you should save the file with the same name as the function name. In the latter case, you could save the file with any name except the local function name.
```matlab
function sum = f(a,b)
    sum = a + b;
end
```
You could use this function in the command window or in another script(function).
```matlab
>> f(1,2)
ans =
    3
```
Here is an example of `script.m` which contains local function.
```matlab
%print sum and product
sum = f(2,3);
disp(sum);

product = p(2,3);
disp(product)

function product = p(a,b)
    product = a * b;
end
```
```matlab
>> script
     5
     6
```

### 3.3 MATLAB Matrix
MATLAB performs well when doing calculation with matrix. You could create matrix with `[]` or `zeros()`, `ones()`, `eye()`, `rand()`, `randn()`.
```matlab
>> A = [1,2,3;4,5,6;7,8,9] 
% semicolon separate rows, comma or space separate columns
A =
     1     2     3
     4     5     6
     7     8     9
>> A(2,3) % A(row_index, column_index)
ans =
     6
```
We could use `[]` to concatenate matrix.
```matlab
>> B = [1,2;3,4]
B =
     1     2
     3     4
>> C = [B, B; B, B]
C =
     1     2     1     2
     3     4     3     4
     1     2     1     2
     3     4     3     4
```
***Ex1.1*** Create a matrix of the following form:
```matlab
1 2 3 4 5
2 3 4 5 6
3 4 5 6 7
4 5 6 7 8
5 6 7 8 9
```
***Ex1.2*** Create a matrix of the following form:
Hint: Try `fliplr` or `flipud`. You could also use loop.
```matlab
1 0 0 0 1
0 1 0 1 0
0 0 1 0 0
0 1 0 1 0
1 0 0 0 1
```
***Ex1.3*** Generalize 1.1 and 1.2, create a MATLAB function `Ex1(n)` and `Ex2(n)` to generate the matrix of size n.

### 3.4 Characters and Strings

Characters: `'a'` `'b'` `'c'`.
Strings: `"Hello World"` `"JI"`.
They are very interesting. You will meet them over and over again in future courses.
![Strings](string.png)

***Ex1.4*** Create a password input function `password()` which behaves like below(You could set your own password in the function):
```matlab
>> password()
Please input your password: 123456 % 123456 is user input
Your password is wrong, please try again.
Please input your password: 12345678 % 12345678 is user input
Correct, welcome!
```

### 3.5 Control Flow
![Control Flow](relation.png)
Please remember `~=` means not equal to! 
Some important operators:
1. `&` : and
2. `|` : or
3. `~` : not
4. `xor` : exclusive or. (`xor(A,b)`)
5. `&&` : short-circuit and
6. `||` : short-circuit or

Please notice the difference between `&` and `&&`.

Then we come to the control flow. We have `if`, `else`, `elseif`, `for`, `while`, `switch`, `case`, `break`, `continue`, `return`. You should be familiar with these concept.

***Ex1.5*** Create a function `isPrime(n)` to determine whether n is a prime number.

Hint: You could use `for` loop to check whether n is divisible by any number from 2 to sqrt(n).

In fact, MATLAB has a default function `isprime(n)` to check whether n is a prime number. You could try it by yourself and compare the result.

## 4 Summary
MATLAB itself is not hard to learn, and some of you may seldom use this language in the future. But the basic concept of programming is very important. You should be familiar with the basic syntax and control flow of a language. You should also know how to ask questions and how to avoid breaking HC.
