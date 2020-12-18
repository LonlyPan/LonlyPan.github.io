---
layout: post
show_title: "2020-12-18-C_C++-应用笔记"
title: "2020-12-18-C_C++-应用笔记"
date: 2020-12-18
last_modified_at: 2020-12-18
categories: c&c++
---


<!--more-->
---
layout: post
show_title: "数组_array_vector"
title: "2019-5-27-数组_array_vector"
data: 2019-5-27 16:47
categories:  c&c++
---

# 数组_array_vector- 2019-5-27

C++ 中有三种数组：C风格的数组、std::array、std::vector。

编译软件：Visual Studio 2019

<!--more-->

## C风格的数组

这个风格不够现代，且不够安全，示例如下：

```c
#include <iostream>

void print_array(int *arr){
    for(int i=0;i<10;i++)
        printf("%d ",arr[i]);
    printf("\n");
}

int main()
{
    int carr[10]; // 内容随机，
    print_array(carr);

    int carr1[10] = {1}; // 第一个元素为1，其余0
    print_array(carr1);

    int carr2[10] = {1,2}; // 第一个元素为1，第二个2，其余0
    print_array(carr2);

}
```

可能的输出如下：

0 0 -1572448 32766 -1460174784 21991 -1460174775 21991 1327354688 10961   
1 0 0 0 0 0 0 0 0 0   
1 2 0 0 0 0 0 0 0 0   

## std::array

std::array是C++11引入的一个模板类，封装了C风格的数组，因此是固定大小的数组容器。所谓固定大小，是指编译时就确定了大小。通常情况下是占用栈（stack）上的空间，且性能更好（相比std::vector）。

需要头文件：

`#include <array>`

示例如下：

```cpp
#include <iostream>
#include <array>

void print_array(std::array<int, 10> arr) {
	for (int i = 0; i < 10; i++)
		printf("%d ", arr[i]);
	printf("\n");
}

int main()
{
	std::array<int, 10> stdarr; // 表示int类型的大小为10的数组。内容随机，有的编译器会报错
	print_array(stdarr);
	
	std::array<int, 10> stdarr = {1}; // 第一个元素为1，其余0
	print_array(stdarr);

	std::array<int, 10> stdarr2 = { 1,2 }; // 第一个元素为1，第二个2，其余0
	print_array(stdarr2);
}
```

输出结果：

------ 第一个编译直接报错，显示未赋初值  
1 0 0 0 0 0 0 0 0 0  
1 2 0 0 0 0 0 0 0 0  

std::array相比C风格数组的另一个优势是：可以使用STL的各种算法函数。 [更多方法见这里 ](https://en.cppreference.com/w/cpp/container/array)

例如这里的示例：

```cpp
#include <iostream>
#include <array>

void print_array(std::array<int, 3> arr,int size) {
	for (int i = 0; i < size; i++)
		printf("%d ", arr[i]);
	printf("\n");
}

int main()
{
	std::array<int, 3> stdarr3 = { 3, 2, 1 };
	std::sort(stdarr3.begin(), stdarr3.end());  // 从低到高排序
	print_array(stdarr3, 3);
}
```

输出结果：

1 2 3

## std::vector

std::vector是一个老牌的C++模板类，提供了动态大小的数组。std::vector的数据是存储在堆（heap）上的，因此性能上不如std::array（堆的访问速度小于栈的访问速度）。

首先包含头文件：

示例如下：

```cpp
#include <iostream>
#include <vector>

void print_array(std::vector<int> arr,int size) {
	for (int i = 0; i < size; i++)
		printf("%d ", arr[i]);
	printf("\n");
}

int main()
{
	std::vector<int> v = { 7, 5, 16, 8 };  //定义一个可变大小的数组，并赋初值
	v.push_back(25);  // 向数组末尾添加数据
	v.push_back(13);
	print_array(v, v.size());
}
```

输出结果：

7 5 16 8 25 13

[更多方法见这里：](https://en.cppreference.com/w/cpp/container/vector)

由于std::vector是动态大小的数组，那必然存在已有空间不够的情况，那空间大小是如何再次分配的呢？
通过跟进push_back的代码，可看到如下：

![enter description here](https://LonlyPan.github.io/images/Posts/2019-5-27-数组_array_vector/enter_description_here.png)

可知当最终空间不足时，会不断的把容量\*2。容量是capacity，而不是size。size是已经占用数据的大小，capacity是vector为了实现动态数组，提前申请出的空间。因此vector当发生容量乘以2时，会带来数据的复制。从老的地址空间复制数据到扩大后的地址空间。这也是可能带来性能问题的地方。因此有时可以提前一次申请足够的空间，例如：std::vector<int> v(100)。

参考：

* [stdvector-versus-stdarray-in-c](https://stackoverflow.com/questions/4424579/stdvector-versus-stdarray-in-c)


## 扩展：二维数组，可变数组

### 1、array <array<int, 5>, 3 >array_t  二维数组

```cpp
#include <iostream>
#include <vector>
#include <array>

using namespace std;

int main() {
	array<int, 5> array1 = { 1, 2, 3, 4, 5 };  //定义一个一维的array
	array<int, 5> array2 = { 6, 7, 8, 9, 10 };
	array<int, 5> array3 = { 11, 12, 13, 14, 15 };
	array <array<int, 5>, 3 >array_t = { array1, array2, array3 };  //定义一个二维数组 3行5列


	for (int i = 0; i < array_t.size(); i++)  // 这里我们在实现数组的遍历的时候，可以使用array.size()求出数组的大小了
	{
		for (int j = 0; j < array1.size(); j++)
		{
			cout << array_t[i][j] << " ";
		}
		cout << endl;
	}
}
```

输出结果：

1 2 3 4 5  
6 7 8 9 10  
11 12 13 14 15  

### 2、vector<vector<int>> array_t   可变数组

```
#include <iostream>
#include <vector>
#include <array>

using namespace std;

int main() {

	//vector<vector<int>> array_t;
	vector<int> array1 = { 1, 2, 3, 4, 5 };  //定义一个一维的可变array数组
	vector<int> array2 = { 6, 7, 8, 9, 10, 11 };  
	vector<int> array3 = { 11, 12, 13, 14, 15, 16 };
	vector<vector<int>> array_t = { array1, array2, array3 };  //定义一个行列都可变的二维数组

	//array_t.push_back(array1);
	int i;
	for (i = 0; i <array_t.size(); i++)  // 这里我们在实现数组的遍历的时候，可以使用array.size()求出数组的大小了
	{
		for (int j = 0; j < array_t[i].size(); j++)
		{
			cout << array_t[i][j] << " ";
		}
		cout << endl;
	}
}
```

输出结果：注意这里数组大小是变化的

1 2 3 4 5  
6 7 8 9 10 11  
11 12 13 14 15 16  

### 3、array<vector<int>, 3> array_t  局部可变二维数组

```cpp
#include <iostream>
#include <vector>
#include <array>

using namespace std;

int main() {
	vector<int> array1 = { 1, 2, 3, 4, 5 };  //定义一个一维的可变array
	vector<int> array2 = { 6, 7, 8, 9, 10, 11 };
	vector<int> array3 = { 11, 12, 13, 14, 15, 16 };
	array<vector<int>, 3>array_t = { array1 ,array2 ,array3 };   // 声明 3行 但 不定列（类型为int）的可变二维数组

	int i;
	for (i = 0; i <array_t.size(); i++)  // 这里我们在实现数组的遍历的时候，可以使用array.size()求出数组的大小了
	{
		for (int j = 0; j < array_t[i].size(); j++)
		{
			cout << array_t[i][j] << " ";
		}
		cout << endl;
	}
}
```
这里和上一个很相似，注意程序区别。
输出结果： 

1 2 3 4 5  
6 7 8 9 10 11  
11 12 13 14 15 16  

### 4、vector<array<int,5>> array_t  

这个留着大家自己探索吧
注意这里行是可变的，列示固定的。

## 注意事项

array <array<int, 5>, 3 >array_t = { array1, array2, array3 };  //定义一个二维数组 3行5列

在声明以上数组时，初始化的一维数组 array1 类型必须和 array<int, 5>是统一的，即不能按照 C写法（`int array1[] = {......}`）声明，否则会报错。当然你可以挨个赋值，就不用提前声明array1了。

# 程序在内存中的分布_2020-04-13
 
**函数执行：** 从函数的  `{` 开始，到 `}` 结束，这之后继续执行下一个函数。  
**程序执行：** 从`main()` 函数的 `{`开始，到 `}` 结束，这之后启动执行下一个程序。但其实对于大多数没有操作系统的单片机来说，只有一个 `main()` 函数，程序执行结束则意味着重启或停机。

## 内存分布

![enter description here](https://LonlyPan.github.io/images/Posts/2020-4-11-程序在内存中的分布/程序内存分布.png)

**1. stack栈区**
1. 存储自动变量、每次函数调用时需要保存的信息（函数参数，函数内局部变量，函数返回值，函数调用时的返回地址）
2. 函数返回时（执行结束），系统自动回收内存
3. 先入后出：最先存入栈中的数据最后取出。

**2. heap堆区**
1. 动态分配的内存，由程序员申请（使用malloc()、new()等内存分配函数实现）
2. 若不手动释放内存，则一直存在。对于 new()，会在程序执行结束后由系统回收。

**3. 数据段**

编译器编译时就已经分配好内存

- **bbs段**

  未初始化的变量，在执行时由系统初始化为 0。例如：对于全局数组，会在程序执行（main函数开始）之前由系统全部初始化为0。

- **data段**

  具有明确初始值的变量

**4.  text代码段**

1. CPU执行的机器指令，即程序代码。
2. 只读数据（常量）

>**总结如下：**  
堆栈和数据区存放的都是变量，但堆栈存放程序运行时的数据（由系统分配）。数据区则是在编译器编译期就已经分配好的  
代码段存放代码和常量，即变量之外的数据

**5. 命令行参数和环境变量**

这个不是太明白，目前只是粗略了解，可能理解会有很多错误：

- 命令行参数

  只要有操作系统就会有**命令行**这个概念，是操作系统的一个最基本的对外交互操作方式，使用者可以在命令行界面输入各种**参数**指令来执行比如读取文件，写入文件，运行程序等等操作。  这样使用者就可以正常运行整个系统程序。如果没有了**命令行**，就相当于整个操作系统（程序）对外封闭，别人无法获取任何信息，那么系统也就没有了意义。

  对于 windows系统，我们也可以理解我们每次双击打开文件，读写文件时其实是后台在调用命令行操作，这样用户不需要了解任何程序也可以正常使用这个系统。

- 环境变量

  可以理解为系统运行时的用到的各种 **用户可修改的变量**，比如 "Path" 变量，里面存储了一些常用命令所存放的目录路径， 当用户将自定义文件目录添加到 "Path" 下之后，运行某些程序除了在当前文件夹查找，还会到设置的 Path 路径中区查找。还比如 **系统语言** 等变量。

参考资料：
[1. main函数——命令行参数与环境变量](https://blog.51cto.com/muhuizz/1767467)  

## 堆和栈的区别

**1.分配和管理方式不同** 
- 堆是动态分配的，其空间的分配和释放都由程序员控制。
- 栈由编译器自动管理。栈有两种分配方式：静态分配和动态分配。
  - 静态分配由编译器完成，比如局部变量的分配。
  - 动态分配由`malloc()`函数进行分配，但是栈的动态分配和堆是不同的，它的动态分配是由编译器进行释放，无须手工控制。

**2.产生碎片不同**
- 对堆来说，频繁的new/delete或者malloc/free势必会造成内存空间的不连续，造成大量的碎片，使程序效率降低。
- 对栈而言，则不存在碎片问题，因为栈是先进后出的队列，永远不可能有一个内存块从栈中间弹出。

**3.生长方向不同**
- 堆是从内存的低地址向高地址方向增长。
- 栈是从内存的高地址向低地址方向增长。

## 程序举例

### C程序
```c 
int a = 0;      // a 在 data
char *p1;       // p1 在 bss

main()            /* 函数体在代码段 */
{
    int b;                      // b 在 stack
    char s[] = "abc";           // s 在 stack, abc\0 在text代码段
    char *p2;                   // p2 在 stack
    char *p3 = "123456";        // p3 在 stack, 123456\0 在text代码段
    static int c = 0;           // c 在 data
    p1 = (char *)malloc(10);    // 申请的10字节内存在 heap, bss 中的指针 p1 指向 heap 中的内存
    p2 = (char *)malloc(20);    // 申请的20字节内存在 heap, stack 中的指针 p2 指向heap中的内存
    strcpy(p1, "123456");       // 123456\0 在text代码段，编译器可能会将它与 p3 所指向的 "123456" 优化成一块
}

```

### C++程序


```cpp

/* 类本身即其内部成员存储在代码区，不占用任何其它内存，只是一段代码，必须实例化才会具有意义 */
class MyObject{
    int a;
    
    void func1();
};

MyObject obj1;   /* 语句1 */
MyObject *obj2 = new MyObject;   /* 语句2 */

void MyObject::func1(){
    MyObject obj3;   /* 语句3 */
    MyObject *obj4 = new MyObject;   /* 语句 4*/
}
```

**函数外部：**  
语句1实例化的对象及其一般数据成员存放在全局数据段  
语句2实例化的对象及其一般数据成员存放在堆区，因为使用 new 分配。  

**函数外部：**  
语句3 实例化的对象及其一般数据成员存放在栈中，随着函数执行结束而结束  
语句4 实例化的对象及其一般数据成员存放在堆中，随着函数执行结束而结束。但此时销毁的只是指针 obj4，其指向的对象并没有，也就是还在占用内容，会造成内存泄露，所以必须要手动释放。

## 参考资料

[1. 程序在内存中的分布](https://www.cnblogs.com/Lynn-Zhang/p/5449199.html)  
[2. 程序在内存中的分布](https://www.jianshu.com/p/155e83ba18b8)  
[3. C 程序的内存空间布局](https://blog.csdn.net/why19911024/article/details/53033426)  
[4. C\++：在堆上创建对象，还是在栈上？](https://www.devbean.net/2014/02/cpp-create-object-on-heap-or-stack/#comment-17679)  
[5. c\++类的成员变量的存储位置？](https://segmentfault.com/q/1010000004149330)  
[6. c\++中的类和实例分别存储在什么地方](https://zhidao.baidu.com/question/1737185158839349467.html)  
[7. C\++成员函数在内存中的存储方式](https://blog.csdn.net/fuzhongmin05/article/details/59112081)  
[8. Class Members on Stack Or Heap?](https://www.daniweb.com/programming/software-development/threads/450593/class-members-on-stack-or-heap)  
[9. Memory Layout of C Programs](https://www.geeksforgeeks.org/memory-layout-of-c-program/)  
[10. In what address do the variables get stored when declared in C or C++?](https://www.quora.com/In-what-address-do-the-variables-get-stored-when-declared-in-C-or-C++https://www.quora.com/In-what-address-do-the-variables-get-stored-when-declared-in-C-or-C++)  
