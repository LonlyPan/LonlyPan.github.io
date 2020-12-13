---
layout: post
show_title: "数组_array_vector"
title: "2019-5-27-数组_array_vector"
data: 2019-5-27 16:47
categories:  c&c++
---

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


