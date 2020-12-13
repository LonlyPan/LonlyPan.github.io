---
layout: post
show_title: "new和malloc "
title: "2020-04-11-new和malloc "
data: 2020-04-11 15:8
categories:  c&c++
---

# malloc

`void * malloc（size_t大小）;`

**简介：**  
分配一个内存大小为 size 字节的块，并返回一个指向该块开头的指针。
新分配的内存块的内容未初始化，剩余的值不确定。
如果size为零，则返回值取决于特定的库实现（它可以是null指针，也可以不是null指针），但不应取消对返回的指针的引用。

<!--more-->
**参数：**  
size -- 内存块的大小，以字节为单位。
size_t 是无符号整数类型。

**返回值：**  
成功时，指向函数分配的内存块的指针。
该指针的类型始终为void*，可以将其强制转换为所需的数据指针类型，以便将其取消引用。
如果函数未能分配所请求的内存块，则返回空指针。


## 实例

```c
/* malloc example: random string generator*/
#include <stdio.h>      /* printf, scanf, NULL */
#include <stdlib.h>     /* malloc, free, rand */
#include <string.h>    /* strcpy, strcat */

int main ()
{
   char *str;
 
   /* 最初的内存分配 */
   str = (char *) malloc(15);
   strcpy(str, "runoob");
   printf("String = %s,  Address = %u\n", str, str);
 
   /* 重新分配内存 */
   str = (char *) realloc(str, 25);  /* 尝试重新调整之前调用 malloc 所分配的 str 所指向的内存块的大小 */
   strcat(str, ".com");  /* 把 src 所指向的字符串追加到 dest 所指向的字符串的结尾 */
   printf("String = %s,  Address = %u\n", str, str);
 
   free(str);  /* 释放内存 */
 
   return(0);
}
```
运行结果：

```c
String = runoob,  Address = 57917504
String = runoob.com,  Address = 57917504
```

**注意：**  
` str = (char *) malloc(15);`  
 这里的15 必须必须要大于要存储的字符宽度，包含结束符。  
 如果小于所要存储的字符大小(比如将上文中 15 改为 1)，实际上运行结果还是一样的，但问题在于多出的字符空间是会随时被其它变量覆盖，不安全。  
 malloc 的作用就是开辟一个专用空间，不会被其它程序和变量占用。
 
**参考链接：**  
[1. malloc](http://www.cplusplus.com/reference/cstdlib/malloc/)  
[2. C 库函数 - malloc()](https://www.runoob.com/cprogramming/c-function-malloc.html)  

# new

`new data-type;`

data-type 可以是包含数组在内的任意内置的数据内省，也可以包括类或结构在内的用户自定义的任何数据类型。

## 内置数据类型分配

实例：

```c 
#include <iostream>
using namespace std;
 
int main ()
{
   double* pvalue  = NULL; /* 声明初始化一个为 null 的指针 */
                           /* 因为用 new 分配返回的是内存地址，所以变量也必须为指针类型 */
   pvalue  = new double;   /* 为变量请求内存，分配失败，会返回 NULL指针 */
 
   *pvalue = 29494.99;     /* 在分配的地址存储值 */
   cout << "Value of pvalue : " << *pvalue << endl;  /* 输出打印值 */
 
   delete pvalue;         /* 释放 pvalue 所指向的内存 */
 
   return 0;
}
```
运行结果：

```c
Value of pvalue : 29495
```

## 数组内存分配

**一维数组**

``` cpp
// 动态分配,数组长度为 m
int *array=new int [m];
 
//释放内存
delete [] array;
```
**二维数组**

``` cpp
int **array
// 假定数组第一维长度为 m， 第二维长度为 n
// 动态分配空间
array = new int *[m];
for( int i=0; i<m; i++ )
{
    array[i] = new int [n]  ;
}
//释放
for( int i=0; i<m; i++ )
{
    delete [] arrary[i];
}
delete [] array;
```

## 类对象实例内存分配

对象与简单的数据类型没有什么不同。例如，请看下面的代码，我们将使用一个对象数组来理清这一概念：

```cpp
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
   Box* myBoxArray = new Box[4];
 
   delete [] myBoxArray; // 删除数组
   return 0;
}
```

如果要为一个包含四个 Box 对象的数组分配内存，构造函数将被调用 4 次，同样地，当删除这些对象时，析构函数也将被调用相同的次数（4次）。

当上面的代码被编译和执行时，它会产生下列结果：

```
调用构造函数！
调用构造函数！
调用构造函数！
调用构造函数！
调用析构函数！
调用析构函数！
调用析构函数！
调用析构函数！
```

# delete 与 delete[] 区别：

1、针对简单类型 使用 new 分配后的不管是数组还是非数组形式内存空间用两种方式均可 如：

```cpp
int *a = new int[10];   
delete a;   
delete [] a;
```
此种情况中的释放效果相同，原因在于：分配简单类型内存时，内存大小已经确定，系统可以记忆并且进行管理，在析构时，系统并不会调用析构函数， 它直接通过指针可以获取实际分配的内存空间，哪怕是一个数组内存空间(在分配过程中 系统会记录分配内存的大小等信息，此信息保存在结构体_CrtMemBlockHeader中， 具体情况可参看VC安装目录下CRT\SRC\DBGDEL.cpp)

2、针对类Class，两种方式体现出具体差异

当你通过下列方式分配一个类对象数组：

```cpp
class A
{
    private:
        char *m_cBuffer;
        int m_nLen;
    public:
        A(){ m_cBuffer = new char[m_nLen]; }
        ~A() { delete [] m_cBuffer; }
};
A *a = new A[10];

// 仅释放了a指针指向的全部内存空间 但是只调用了a[0]对象的析构函数 剩下的从a[1]到a[9]这9个用户自行分配的m_cBuffer对应内存空间将不能释放 从而造成内存泄漏
delete a;

// 调用使用类对象的析构函数释放用户自己分配内存空间并且   释放了a指针指向的全部内存空间
delete [] a;
```

所以总结下就是，如果ptr代表一个用new申请的内存返回的内存空间地址，即所谓的指针，那么：

- **delete ptr** -- 代表用来释放内存，且只用来释放ptr指向的内存。
- **delete[] rg** -- 用来释放rg指向的内存，！！还逐一调用数组中每个对象的 destructor！！

对于像 int/char/long/int*/struct 等等简单数据类型，由于对象没有 destructor，所以用 delete 和 delete [] 是一样的！但是如果是C++ 对象数组就不同了！

# new 和 malloc 内部的实现方式区别

new 的功能是在堆区新建一个对象，并返回该对象的指针。
所谓的【新建对象】的意思就是，将调用该类的构造函数，因为如果不构造的话，就不能称之为一个对象。

而 malloc 只是机械的分配一块内存，如果用 mallco 在堆区创建一个对象的话，是不会调用构造函数的。
严格说来用 malloc 不能算是新建了一个对象，只能说是分配了一块与该类对象匹配的内存而已，然后强行把它解释为【这是一个对象】，按这个逻辑来，也不存在构造函数什么事。

同样的，用 delete 去释放一个堆区的对象，会调用该对象的析构函数。
用 free 去释放一个堆区的对象，不会调用该对象的析构函数。

做个简单的实验即可明了:

```cpp
#include <iostream>
#include <malloc.h>

class TEST
{
private:
    int num1;
    int num2;
public:
    TEST()
    {
        num1 = 10;
        num2 = 20;
    }
    void Print()
    {
        std::cout << num1 << " " << num2 << std::endl;
    }
};

int main(void)
{
    // 用malloc()函数在堆区分配一块内存空间，然后用强制类型转换将该块内存空间
    // 解释为是一个TEST类对象，这不会调用TEST的默认构造函数
    TEST * pObj1 = (TEST *)malloc(sizeof(TEST));
    pObj1->Print();

    // 用new在堆区创建一个TEST类的对象，这会调用TEST类的默认构造函数
    TEST * pObj2 = new TEST;
    pObj2->Print();

    return 0;
}

/*
运行结果：

-----------------------------
-842150451 -842150451       |  pObj1所指的对象中，字段num1与num2都是垃圾值  
10 20                       |  pObj2所指的对象中，字段num1与num2显然是经过了构造后的值
-----------------------------
*/
```

# 参考资料

[C++ 动态内存](https://www.runoob.com/cplusplus/cpp-dynamic-memory.html)






