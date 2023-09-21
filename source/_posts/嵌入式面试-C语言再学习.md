---
title: 嵌入式面试-C语言再学习
index_img:  https://lonly-hexo-img.oss-cn-shanghai.aliyuncs.com/hexo_images/嵌入式面试-知识点复习/1694780455921.png
date: 2022-05-08 19:23
updated: 2023-02-22 23:23
hide: false
# sticky: 100 #置顶，数字越大越靠前
# banner_img: #/img/post_banner.jpg
# comment: false
categories: 00-项目
---

# 字符串和字符串函数

## 一、字符串

### 1、字符串介绍

字符串是以空字符(\0)结尾的char数组，例如：
char ar[20] = "hello world";     

### 2、定义字符串

字符常量，又称字符串文字，是指位于一对双引号中的任何字符。双引号里的字符加上编译器自动提供的结束标志(\0)字符，作为一个字符串被存储在内存里。
```
#include <stdio.h>
int main (void)
{
	char ar1[50] = "hello" " " "world""!";
	char ar2[50] = "hello world!";
	printf ("%s\n", ar1);
	printf ("%s\n", ar2);
	printf ("\"We are family!\"\n");
	return 0;
}
输出结果：
hello world!
hello world!
"We are family!"
```
### 3、字符串初始化

定义一个字符串数组时，必须让编译器知道它需要多大空间。

**方法一，指定一个足够大的数组来容字符串**
```
char ar[20] = "hello world";
char ar[2+5];   /*数组大小也可以是常量表达式*/
```
指定数组大小时，一定要确保数组元素数比字符串长度至少多1（多出来的1个元素用于容纳空字符），未被使用的元素均被自动初始化为0。这里的**0是char形式的空字符\0**，不是数字字符0.

**方法二，让编译器决定数组大小**
```
char ar[] = "hello world";
char ar[] = {'h' ,'e' ,'l' ,'l' ,'o' ,' ' ,'w' ,'o' ,'r' ,'l' ,'d' ,'\0'};
```

注意：标示结束的空字符。如果没有它，得到的就只是一个字符数组而不是一个字符串。
注意区分：数字字符 0，空字符 \0，空格 ' '，空指针 NULL。

**方法三，指针初始化字符串**
```
char * str = "hello world";
```

**说明：数组和指针初始化字符串的区别**
- 数组初始化是从静态存储区把一个字符串复制给数组，而指针初始化只是复制字符串的地址。其主要区别在于数组名ar是一个常量，而指针str则是一个变量。如，只有指针变量可以进行 str\++。数组的元素是变量（除非声明数组时带有关键字const），但是数组名不是变量。如，*(ar + 2) = 'm';  是可以的。

**扩展，区分单个字符和字符串**

用单引号引起的一个字符实际上代表一个整数，整数值对应于该字符的编译器采用的字符集中的序列值。因此，对于采用 ASCII 字符集的编译器而言，'a' 的含义与 0141（八进制）或者97（十进制）严格一致。

用双引号引起的字符串，代表的却是一个指向无名数组起始字符的指针，该数组被双引号之间的字符以及一个额外的二进制值为零的字符 '\0' 初始化。
```
#include <stdio.h>  
#include <string.h>  
#include <stdlib.h>  
  
int main (void)  
{  
    char *str1 = "abcde";  
    char str2[] = "abcde";  
    char str3[8] = "abcde";  
    char str4[] = {'a', 'b', 'c', 'd', 'e'};  
    char *p1 = malloc (20);  
      
    printf ("sizeof (str1) = %d, strlen (str1) = %d\n", sizeof (str1), strlen (str1));  
    printf ("sizeof (*str1) = %d, strlen (str1) = %d\n", sizeof (*str1), strlen (str1));  
    printf ("sizeof (str2) = %d, strlen (str2) = %d\n", sizeof (str2), strlen (str2));  
    printf ("sizeof (str3) = %d, strlen (str3) = %d\n", sizeof (str3), strlen (str3));  
    printf ("sizeof (str4) = %d, strlen (str4) = %d\n", sizeof (str4), strlen (str4));  
    printf ("sizeof (p1) = %d, sizeof (*p1) = %d\n", sizeof (p1), sizeof (*p1));  
    printf ("sizeof (malloc(20)) = %d\n", sizeof (malloc (20)));  
    return 0;  
}  
输出结果：  
sizeof (str1) = 4, strlen (str1) = 5  
sizeof (*str1) = 1, strlen (str1) = 5  // *strl指向第一个字符
sizeof (str2) = 6, strlen (str2) = 5  
sizeof (str3) = 8, strlen (str3) = 5  
sizeof (str4) = 5, strlen (str4) = 5  
sizeof (p1) = 4, sizeof (*p1) = 1    // sizeof不能求得动态分配的内存的大小!
sizeof (malloc(20)) = 4  
```
根据上面的例子，可知 str1、str2以空字符(\0)结尾是字符串。而 str4 不是字符串。

如果两者混用，那么编译器的类型检查功能将会检测到这样的错误：

 
```
#include <stdio.h>
#include <string.h>
 
int main (void)
{
	char *str = '2';
	return 0;
}
输出结果：
警告： 初始化时将整数赋给指针，未作类型转换 [默认启用]
```
```
#include <stdio.h>
#include <string.h>
 
int main (void)
{
	printf ("1111111\n");
	printf ('\n');  // 该行错误
	printf ("2222222\n");
 
	return 0;
}
输出结果：
main.c: In function ‘main’:
main.c:8:10: warning: passing argument 1 of ‘printf’ makes pointer from integer without a cast [-Wint-conversion]
    8 |  printf ('\n');
      |          ^~~~
      |          |
      |          int
In file included from main.c:1:
/usr/include/stdio.h:332:43: note: expected ‘const char * restrict’ but argument is of type ‘int’
  332 | extern int printf (const char *__restrict __format, ...);
      |                    ~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~

run: line 1:     3 Segmentation fault      (core dumped) ./a.out

Exited with error status 139
```
还有需要注意的，在用双引号括起来的字符串中，注释符 /* 属于字符串的一部分，而在注释中出现的双引号 "" 又属于注释的一部分。
```
#include <stdio.h>
#include <string.h>
 
int main (void)
{
	char *str = "123/*ddddd*/456";
 
	printf ("%s\n", str);
	/*"123456"*/
	return 0;
}
输出结果：
123/*ddddd*/456
```
再再有注意宏定义，用于定义字符串，尤其是路径
```
A),#define ENG_PATH_1 E:\English\listen_to_this\listen_to_this_3               
B),#define ENG_PATH_2 “ E:\English\listen_to_this\listen_to_this_3”
```
A 为 定义路径， B  为定义字符串

### 4、字符串输入/输出

参看：输入/输出

## 二、字符串函数

字符串函数，其原型在/kernel/include/linux/string.h有声明
这类字符串库函数的功能实现函数的答案在内核源码/kernel/lib/string.c

扩展：size_t类型为unsigned int类型，占位符使用%u或%lu，C99用%zd。

下面来介绍一些最有用和最常用的函数：

### 1、strlen() 函数

#include <string.h>
size_t strlen(const char *s);

函数功能：用来统计字符串中有效字符的个数

功能实现函数：
```
size_t strlen (const char *s)
{
	const char *sc;
	for (sc = s; *sc != '\0'; ++sc)
	/"nothing"/
	return sc - s;
}
```

strlen()函数被用作改变字符串长度，例如：
```
#include <stdio.h>
#include <string.h>
void fit (char *, unsigned int);
int main (void)
{
	char str[] = "hello world";
	fit (str, 7);
	puts (str);
	puts (str + 8);
	return 0;
}
 
void fit (char *string, unsigned int size)
{
	if (strlen (string) > size)
		*(string + size) = '\0';
}
输出结果：
hello w
rld
```
可以看出：fit()函数在数组的第8个元素中放置了一个'\0'字符来代替原有的o字符。puts()函数输出停在o字符处，忽略了数组的其他元素。然而，数组的其他元素仍然存在。

### 2、strcat()函数
```
#include <string.h>
char *strcat(char *dest, const char *src);
```
函数功能：合并两个字符串
缺点：超出字符串存储区范围的话，有可能修改数组以外的存储区这会导致严重错误

功能实现函数：
```
char *strcat(char *dest, const char *src)
{
	char *tmp = dest;
 
	while (*dest)
		dest++;
	while ((*dest++ = *src++) != '\0')
		;
	return tmp;
}
```
strcat()函数它将第二个字符串的一份拷贝添加到第一个字符串的结尾，从而使第一个字符串成为一个新的组合字符串，第二个字符串并没有改变，例如：
```
#include <stdio.h>
#include <string.h>
#define SIZE 80
int main (void)
{
	char flower[SIZE];
	char addon[]= " is beautiful";
	gets (flower);
	strcat (flower, addon);
	puts (flower);
	return 0;
}
输出结果：
rose
rose is beautiful
```
### 3、strncat()函数

上面有提到strcat()函数的缺点，它并不能检查第一个数组是否能够容纳第二个字符串。如果没有为第一个数组分配足够大的空间，多出来的字符溢出到相邻存储单元时就会出现问题。
```
#include <string.h>
 char *strncat(char *dest, const char *src, size_t n);
```
函数功能：合并两个字符串

功能实现函数：
```
char *strncat(char *dest, const char *src, size_t count)
{
	char *tmp = dest;
 
	if (count) {
		while (*dest)
			dest++;
		while ((*dest++ = *src++) != 0) {
			if (--count == 0) {
				*dest = '\0';
				break;
			}
		}
	}
	return tmp;
}
```
例如:
```
#include <stdio.h>
#include <string.h>
#define SIZE 30
#define BUGSIZE 13
int main (void)
{
	char flower[SIZE];
	char addon[]= " is beautiful";
	char bug[BUGSIZE];
	int arg;
 
	puts ("what is your favorite flower");
	gets (flower);
	if ((strlen (addon) + strlen (flower) + 1) <= SIZE)
		strcat (flower, addon);
	puts (flower);
 
	puts ("what is your favorite flower");
	gets (bug);
	arg = BUGSIZE - strlen (bug) -1;
	strncat (bug, addon, arg);
	puts (bug);
	return 0;
}
输出结果：
what is your favorite flower
Rose
Rose is beautiful
what is your favorite flower
MuDan
MuDan is bea
```
### 4、strcmp()函数
```
#include <string.h>
int strcmp(const char *s1, const char *s2);
```
函数功能：比较两个字符串的大小，比较依据的是ASCII码

返回值：依次比较字符串每一个字符的ASCII码，如果两个字符串参数相同，返回0；如果第一个字符串参数较大，则返回1，如果第二个字符串较大，则返回-1。

功能实现函数：
```
int strcmp(const char *cs, const char *ct)
{
	unsigned char c1, c2;
 
	while (1) {
		c1 = *cs++;
		c2 = *ct++;
		if (c1 != c2)
			return c1 < c2 ? -1 : 1;
		if (!c1)
			break;
	}
	return 0;
}
```
需要注意的是：

strcmp()函数比较的是字符串，而不是数组和字符。它可以用来比较存放在不同大小数组里的字符串。
```
#include <stdio.h>
#include <string.h>
#define SIZE 30
#define STOP "q"
int main (void)
{
	char str[SIZE];
	while (gets (str) != NULL && strcmp (str, STOP))
	{
		printf ("hello\n");
		sleep (1);
	}
	return 0;
}
```
这里提一下：
- strcmp比较区分字母大小写 相当是比较的时候纯粹按照ascii码值来比较从头到尾。
- 而 stricmp 是不区分字母的大小写的。


### 5、strncmp()函数
```
#include <string.h>
int strncmp(const char *s1, const char *s2, size_t n);
```
函数功能：只比较两个字符串里前n个字符

功能实现函数：
```
int strncmp(const char *cs, const char *ct, size_t count)
{
	unsigned char c1, c2;
 
	while (count) {
		c1 = *cs++;
		c2 = *ct++;
		if (c1 != c2)
			return c1 < c2 ? -1 : 1;
		if (!c1)
			break;
		count--;
	}
	return 0;
}
```
### 6、strcpy()函数
```
#include <string.h>
char *strcpy(char *dest, const char *src);
```
函数功能：把一个字符串复制到另外一个字符数组里
缺点：有可能修改不属于数组的存储区，这会导致错误

功能实现函数：
```
char *strcpy(char *dest, const char *src)
{
	char *tmp = dest;
 
 
	while ((*dest++ = *src++) != '\0')
		/* nothing */;
	return tmp;
}
```
例如：
```
char str1[30];

char str2[ ] = "hello";

char *pts1 = str1;

char *pts2 = str2;

pts1 = pts2 (错误)

strcpy (pts1， pts2); (正确)
```
因为pts1和pts2都是指向字符串的指针，上面的表达式只复制字符串的地址而不是字符串本身。

字符串之间的复制应使用strcpy ()函数，它在字符串运算中的作用等价于赋值运算符。

总之，strcpy()接受两个字符串指针参数。指向最初字符串的第二个指针可以是一个已声明的指针、数组名或字符串常量。指向复制字符串的第一个指针应指向空间大到足够容纳该字符串的数据对象，比如一个数组。记住，声明一个数组将为数据分配存储空间，而声明一个指针只为一个地方分配存储空间。
```
#include <stdio.h>
#include <string.h>
#define WORDS "best"
#define SIZE 40
 
int main (void)
{
	char *orig = WORDS;
	char copy[SIZE] = "Be the best that you can be.";
	char *ps;
 
	puts (orig);
	puts (copy);
	ps = strcpy (copy + 7 , orig);
	puts (copy);
	puts (ps);
	return 0;
}
输出结果：
best
Be the best that you can be.
Be the best
best
```
上述例子，运用了strcpy()函数的两个属性。首先，它是char*类型，它返回的是第一个参数的值，即一个字符的地址；其次，第一个参数不需要指向数组的开始，这样就可以只复制数组的一部分。


### 7、strncpy()函数
```
#include <string.h>
char *strncpy(char *dest, const char *src, size_t n);
```
函数功能：把一个字符串复制到另外一个字符数组里

功能实现函数：
```
har *strncpy(char *dest, const char *src, size_t count)
{
	char *tmp = dest;
 
	while (count) {
		if ((*tmp = *src) != 0)
			src++;
		tmp++;
		count--;
	}
	return dest;
}
```
上面有讲，strcpy()函数的缺点，就是不能检查目标字符串是否容纳得下源字符串。使用strncpy()函数，它的第三个参数可指明最大可复制的字符数。
```
#include <stdio.h>
#include <string.h>
int main()
{
   char src[40];
   char dest[12];
  
   memset(dest, '\0', sizeof(dest));
   strcpy(src, "This is w3cschool.cc");
   strncpy(dest, src, 10);
   printf("最终的目标字符串： %s\n", dest);
   return(0);
}
输出结果：
最终的目标字符串： This is w3
```
### 8、strstr()函数
```
#include <string.h>
char *strstr(const char *haystack, const char *needle);
```
函数功能：在一个字符串中查找另外一个字符串所在位置

功能实现函数：
```
char *strstr(const char *s1, const char *s2)
{
	size_t l1, l2;
 
	l2 = strlen(s2);
	if (!l2)
		return (char *)s1;
	l1 = strlen(s1);
	while (l1 >= l2) {
		l1--;
		if (!memcmp(s1, s2, l2))
			return (char *)s1;
		s1++;
	}
	return NULL;
}
```
例如：
```
#include <stdio.h>
#include <string.h>
int main()
{
   const char haystack[20] = "W3CSchool";
   const char needle[10] = "3";
   char *ret;
   ret = strstr(haystack, needle);
   printf("子字符串是： %s\n", ret);
   return(0);
}
输出结果：
子字符串是： 3CSchool
```
如果needle字符串不是haystack的一部分，则会出现警告：
`assignment discards ‘const’ qualifier from pointer target type`
 

### 9、memset()函数
```
#include <string.h>
void *memset(void *s, int c, size_t n);
```
函数功能：可以把字符数组中所有的字符存储区填充同一个字符数据

功能实现函数：
```
void *memset(void *s, int c, size_t count)
{
	char *xs = s;
 
	while (count--)
		*xs++ = c;
	return s;
}
```
举例：
```
#include <stdio.h>
#include <string.h>
int main ()
{
   char str[50];
   strcpy(str,"This is string.h library function");
   puts(str);
   memset(str,'$',7);
   puts(str);
   return(0);
}
输出结果:
This is string.h library function
$$$$$$$ string.h library function
```
### 10、sprintf()函数
```
#include <stdio.h>
int sprintf(char *str, const char *format, ...);
```
函数功能：按照格式把数据打印在字符数组中，形成一个字符串
```
#include <stdio.h>
 
#define SIZE 30
int main (void)
{
	char str[SIZE];
	sprintf (str, "%s %s %d\n", "I","love",512 );
	puts (str);
	return 0;
}
输出结果：
I love 512
```
注意：使用sprintf()和使用printf()的方法一样，只是结果字符串被存放在数组fornal中，而不是被显示在屏幕上。

再有如果想将2转化为字符串“02”，该怎么办呢？
```

#include <stdio.h>
 
int main (void)
{
    char str[2];
    sprintf (str, "%02d", 2);
    printf ("str = %s\n", str);
    return 0;
}
 
输出结果：
 
str = 02
```

### 11、memcpy函数
```
#include <string.h>
void *memcpy(void *dest, const void *src, size_t n);
```
函数功能：从存储区src 复制 n 个字符到存储区dest。

参数：
dest -- 指向用于存储复制内容的目标数组，类型强制转换为 void* 指针。
src   -- 指向要复制的数据源，类型强制转换为 void* 指针。
n      -- 要被复制的字节数。

返回值：该函数返回一个指向目标存储区 dest 的指针。

功能实现函数：
```
void *memcpy(void *dest, const void *src, size_t count)
{
     char *tmp = dest;
     const char *s = src;
 
  	 while (count--)
     *tmp++ = *s++;
     return dest;
}
```
//示例一
```
#include <stdio.h>
#include <string.h>
 
int main ()
{
   const char src[50] = "hello world!";
   char dest[50];
 
   printf("Before memcpy dest = %s\n", dest);
   memcpy(dest, src, strlen(src)+1);
   printf("After memcpy dest = %s\n", dest);
   
   return(0);
}
输出结果：
Before memcpy dest = 
After memcpy dest = hello world!
```

//示例二
```
#include <stdio.h>
#include <string.h>
 
int main ()
{
   const int src[50] = {1, 2, 3, 4, 5, 6, 7, 8, 9};
   int dest[50];
 
   memcpy(dest, src, 9*4);
   
   int i;
   for (i = 0; i < 9 ; i++)
   {
   		printf ("%d ", dest[i]);
   }
   printf ("\n");
   return(0);
}
输出结果：
1 2 3 4 5 6 7 8 9 
```
#### strcpy 和 memcpy 的区别：

（1）strcpy 和 memcpy 都是标准 C 库函数

（2）strcpy 提供了字符串的复制。即 strcpy 只用于字符串复制，并且它不仅复制字符串内容之外，还会复制字符串的结束符。

（3）strcpy函数的原型是：char* strcpy(char* dest, const char* src);

（4）memcpy提供了一般内存的复制。即memcpy对于需要复制的内容没有限制，因此用途更广。

（5）memcpy函数的原型是：void *memcpy( void *dest, const void *src, size_t count );

 
strcpy 和 memcpy 主要有以下3方面的区别：

（1）复制的内容不同。strcpy 只能复制字符串，而memcpy可以复制任何内容，例如字符串数组、整型、结构体、类等。

（2）复制的方法不同。strcpy不需要指定长度，它遇到被复制字符串结束符'\0'才结束，所以容易溢出。memcpy则是根据其第3个参数决定复制的长度。

（3）用途不同。通常在复制字符串时用strcpy，而需要复制其他类型数据时则一般用memcpy。

 

### 12、memcmp函数
```
 #include <string.h>

int memcmp(const void *s1, const void *s2, size_t n);
```
函数功能：

把存储区 str1 和存储区 str2 的前 n 个字节进行比较。

参数：
str1 -- 指向内存块的指针。
str2 -- 指向内存块的指针。
n -- 要被比较的字节数。
返回值：
如果返回值 < 0，则表示 str1 小于 str2。
如果返回值 > 0，则表示 str2 小于 str1。
如果返回值 = 0，则表示 str1 等于 str2。

示例：
```
#include <stdio.h>
#include <string.h>
 
int main (void)
{
	int ret = 0;
 
	char str1[5] = {'1','2','3','4','5'};
	char str2[5] = {'1','A','B','4','5'};
	
	ret = memcmp (str1, str2, 5);
	
   if(ret > 0)
   {
	   printf("str2 灏忎簬 str1\n");
   }
   else if(ret < 0) 
   {
	   printf("str1 灏忎簬 str2\n");
   }
   else 
   {
	   printf("str1 绛変簬 str2\n");
   }
	return 0;
}
 
输出结果：
str1 小于 str2
```
 

## 三、将字符串转换成数字

atoi()函数、atol()函数、atof()函数
```
#include <stdlib.h>
int atoi(const char *nptr);
long atol(const char *nptr);

double atof(const char *nptr);
```
函数功能: 分别把数字的字符串表示转换为 int、long和double形式。如果没有执行有效的转换返回0.

atoi()功能实现函数：
```
int my_atoi(const char *str)
{
    int value = 0;
    int flag = 1; //判断符号
    while (*str == ' ')  //跳过字符串前面的空格
    {
        str++;
    }
    if (*str == '-')  //第一个字符若是‘-’，说明可能是负数
    {
        flag = 0;
        str++;
    }
    else if (*str == '+') //第一个字符若是‘+’，说明可能是正数
    {
        flag = 1;
        str++;
    }//第一个字符若不是‘+’‘-’也不是数字字符，直接返回0
    else if (*str >= '9' || *str <= '0') 
    {
        return 0;    
    }
    //当遇到非数字字符或遇到‘\0’时，结束转化
    while (*str != '\0' && *str <= '9' && *str >= '0')
    {
        value = value * 10 + *str - '0'; //将数字字符转为对应的整形数
        str++;
    }
    if (flag == 0) //负数的情况
    {
        value = -value;
    }
    return value;
}
```
测试：
```
#include <stdio.h>
int my_atoi(const char *str)
{
    int value = 0;
    int flag = 1; //判断符号
    while (*str == ' ')  //跳过字符串前面的空格
    {
        str++;
    }
    if (*str == '-')  //第一个字符若是‘-’，说明可能是负数
    {
        flag = 0;
        str++;
    }
    else if (*str == '+') //第一个字符若是‘+’，说明可能是正数
    {
        flag = 1;
        str++;
    }//第一个字符若不是‘+’‘-’也不是数字字符，直接返回0
    else if (*str >= '9' || *str <= '0') 
    {
        return 0;    
    }
    //当遇到非数字字符或遇到‘\0’时，结束转化
    while ((*str != '\0')&& (*str <= '9') && (*str >= '0'))
    {
        value = value * 10 + *str - '0'; //将数字字符转为对应的整形数
        str++;
    }
    if (flag == 0) //负数的情况
    {
        value = -value;
    }
    return value;
}
 
int main(void)
{
    int i  = my_atoi(" -1234dd");
    printf("%d\n", i);
    return 0;
}
输出结果：
-1234
```
测试：
```
#include <stdio.h>
#include <stdlib.h>
int main (void)
{
	int val1, val2;
	long val3, val4;
	double val5, val6;
	val1 = atoi ("512you");
	val2 = atoi ("you512");
	val3 = atol ("1000000you");
	val4 = atol ("you1000000");
	val5 = atof ("3.14you");
	val6 = atof ("you3.14");
	printf ("val1 = %d\n", val1);
	printf ("val2 = %d\n", val2);
	printf ("val3 = %lu\n", val3);
	printf ("val4 = %lu\n", val4);
	printf ("val5 = %lg\n", val5);
	printf ("val6 = %lg\n", val6);
	return 0;
}
输出结果：
val1 = 512
val2 = 0
val3 = 1000000
val4 = 0
val5 = 3.14
val6 = 0
```
itoa函数功能实现：
```
void itoa(int num,char str[] )
{
int sign = num,i = 0,j = 0;
char temp[11];
if(sign<0)//判断是否是一个负数
{
num = -num;
};
do
{
temp[i] = num%10+'0';        
num/=10;
i++;
}while(num>0);
if(sign<0)
{
temp[i++] = '-';//对于负数,要加以负号
}
temp[i] = '\0';
i--;
while(i>=0)//反向操作
{
str[j] = temp[i];
j++;
i--;
}
str[j] = '\0';
}
```
测试：
```
#include <stdio.h>
void my_itoa(int num,char str[] )
{
int sign = num,i = 0,j = 0;
char temp[11];
if(sign<0)//判断是否是一个负数
{
num = -num;
};
do
{
temp[i] = num%10+'0';        
num/=10;
i++;
}while(num>0);
if(sign<0)
{
temp[i++] = '-';//对于负数,要加以负号
}
temp[i] = '\0';
i--;
while(i>=0)//反向操作
{
str[j] = temp[i];
j++;
i--;
}
str[j] = '\0';
}
 
int main (void)
{
	char arr[10]= {0};
	my_itoa (-123,arr);
	printf ("%s\n", arr);
	
}
输出结果：
-123
```
## 四、字符转十六进制
```
void StrToHex(char *pbDest, char *pbSrc, int nLen)
{
  char h1,h2;
  char s1,s2;
  int i;
 
	for (i=0; i<nLen/2; i++)
	{
		h1 = pbSrc[2*i];
		h2 = pbSrc[2*i+1];
 
		s1 = toupper(h1) - 0x30;
		if (s1 > 9)
			s1 -= 7;
		s2 = toupper(h2) - 0x30;
		if (s2 > 9)
			s2 -= 7;
 
		pbDest[i] = s1*16 + s2;
	}
}
```
# 值传递，址传递，引用传递

## 一、值传递

当函数被调用的时候，形参被创建，调用时带的参数被拷贝到刚创建好的形参，函数结束时，形参被摧毁。由于是参数的一个副本被传递到被调用的函数。所以，原始的参数不会被函数修改。

值传递的优点： 通过值来传递的参数可以是数字，变量，表达式。参数的值不会被“被调用的函数”修改。

值传递的缺点： 当函数被多次调用，值传递结构体和类会带来性能上的损害（耗时），给调用者返回值只能通过被调用函数的返回值。

事实上，关于 C 函数的参数传递规则可以表述如下：
> **所有传递给函数的参数都是按值传递的。**
```
#include <stdio.h>
 
void swap (int x, int y)
{
	int temp;
	temp = x;
	x = y;
	y = temp;
	printf ("x = %d, y = %d\n", x, y);
}
 
int main (void)
{
	int a = 4, b = 9;
	swap (a, b);
	printf ("a = %d, b = %d\n", a, b);
	return 0;
}
输出结果：
x = 9, y = 4
a = 4, b = 9
```
看调用swap函数的代码：

swap (a, b) 时所完成的操作代码如下所示：
```
int x=a;  //←
int y=b;  //←注意这里，头两行是调用函数时的隐含操作
int tmp;
tmp=x;
x=y;
y=tmp;
```
请注意在调用执行 swap 函数的操作中我人为地加上了头两句：
```
int x=a;
int y=b;
```
这是调用函数时的两个隐含动作。它确实存在，现在我只不过把它显式地写了出来而已。问题一下就清晰起来啦（看到这里，现在你认为函数里面交换操作的是a,b变量或者只是x,y变量呢？）

原来 ，其实函数在调用时是隐含地把实参a,b 的值分别赋值给了x,y，之后在你写的 swap 函数体内再也没有对a,b进行任何的操作了。交换的只是x,y变量。并不是a,b。当然a,b的值没有改变啦！函数只是把a,b的值通过赋值传递给了x,y，函数里头操作的只是x,y的值并不是a,b的值。这就是所谓的参数的值传递了。

哈哈，终于明白了，正是因为它隐含了那两个的赋值操作，才让我们产生了前述的迷惑（以为a,b已经代替了x,y，对x,y的操作就是对a,b的操作了，这是一个错误的观点啊！）。

参看：存储类、链接

上面的解释虽然便于理解，但是不对。按照值传递的特点，我们可以很清楚的看到，虽然在 swap 函数中暂时使得运行结果显示了交换后的数据，即达到了交换的目的，但实际情况却是随着 swap 函数的结束，被作为局部参数的形参  x，y 以及 swap 函数本身的局部参数 temp 都将结束期生存期，在内存中的存储空间释放，因此实参 a , b 并未受到影响，依然保持原值。



## 二、址传递

址传递又可以理解为指针传递，可以改变指针指向内容的值，但是不能改变指针本身，无需复制开销。如果需要改变指针本身，可以使用二重指针或者指针引用。

```
#include <stdio.h>
void swap (int *px, int *py)
{
	int temp=*px;
	*px=*py;
	*py=temp;
	printf("*px = %d, *py = %d\n", *px, *py);
}
 
int main(void)
{
	int a=4;
	int b=6;
	swap (&a,&b);
	printf("a = %d,b = %d\n", a, b);
	return 0;
}
输出结果：
*px = 6, *py = 4
a = 6,b = 4
```
看函数的接口部分：swap (int *px, int *py)，请注意参数px，py都是指针

再看调用处：swap (&a, &b); 它将 a 的地址 &a 代入到 px，b 的地址 &b 代入到 py。同上面的值传递一样，函数调用时做了两个隐含的操作，将 &a ，&b 的值赋给了px，py。
整个 swap 函数调用执行如下：
```
px=&a;   
py=&b;  //请注意这两行，它是调用 swap 的隐含动作。
int temp=*px;
*px=*py;
*py=temp;
printf("*px=%d,*py=%d\n",*px, *py);
```
这样，有了头两行的隐含赋值操作，我们现在已经可以看出，指针 px，py 的值已经分别是 a，b 变量的地址值了。接下来，对 *px，*py 的操作当然也就是对 a，b 变量本身的操作了。所以函数里头的交换就是对 a，b 值的交换了，这就是所谓的址传递（传递 a，b 的地址给 px，py）

总结：

在这个程序中用指针变量作参数，虽然传送的是变量的地址，但实参和形参之间的数据传递依然是单向的“值传递”，即调用函数不可能改变实参指针变量的值。但它不同于一般值传递的是，它可以通过指针间接访问的特点来改变指针变量所指变量的值，即最终达到了改变实参的目的。



问题：如何实现C语言返回多个值？？

数组的地址作为函数的形参，以址传递方式传递数组参数

```
#include <stdio.h>
void foo (int arr[])
{
	int i = 0;
	for (i = 0; i < 6; i++)
	{
		arr[i] = i + 1;
	}
}
 
int main (void)
{
	int arr[5] = {0};
	int i = 0;
	foo (arr);
	for (i = 0; i < 5; i++)
	{
		printf ("%d ", arr[i]);
	}
	printf ("\n");
	return 0;
}
输出结果：
1 2 3 4 5
```
## 三、引用传递

为了通过引用来传递一个变量，把函数的参数声明为引用，当函数被调用时，所声明的引用形参，就会为其的引用。

引用传递的优点：改不改变 argument 的值是可以控制的，传递结构体、类时速度快，因为不用做一系列copy的工作；使用引用传递可以返回多个值；引用必须被初始化。

引用传递的缺点：使用引用传递时，北调函数会改变argument的值，向“调用者”保证，argument不会被“被调用函数”改变。
```

#include <stdio.h>
void swap (int &x, int &y)
{
	int temp = x;
	x = y;
	y = temp;
	printf ("x = %d, y = %d\n", x, y);
}
 
int main (void)
{
	int a = 4;
	int b = 6;
	swap (a, b);
	printf ("a = %d, b = %d\n", a, b);
	return 0;
}
 
g++ swap.cpp 
输出结果：
x = 6, y = 4
a = 6, b = 4
```
看函数的 接口部分：swap (int &x, int &y) ，参数 x，y 是int的变量，调用时我们可以像值传递一样调用函数，但是 x，y 前都有一个取地址符号 &。有了这个，调用 swap 时函数会将 a，b 分别代替了 x，y。我们称 x，y 分别引用了 a，b 变量。这样函数里头操作的其实就是实参 a，b 本身了，也就是说函数里可以直接修改到 a，b 的值了。


## 面试题：动态分配内存问题

### 问题一：

```
void GetMemory( char *p )
{
 p = (char *) malloc( 100 );
}
void Test( void )
{
 char *str = NULL;
 GetMemory( str );
 strcpy( str, "hello world" );
 printf( str );
}
```
请问运行 Test 函数会有什么样的结果？ 
答：程序崩溃。
因为 GetMemory 并不能传递动态内存，Test 函数中的 str 一直都是 NULL。strcpy(str, "hello world");将使程序崩溃。
在函数内部修改形参并不能真正的改变传入形参的值,执行完
char *str = NULL;
GetMemory( str );
后的str仍然为NULL;


二级指针作为函数的形式参数可以让被调用函数使用其他函数的指针类型存储区
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
void fa(char** p)  //主要还是指针的问题
{
	*p=(char* )malloc(100);
	if(*p)
	{
		return;	
	}
}
int main()
{
	char* str=NULL;//这块没问题的
	fa(&str);  // 传递的时指针的地址
	strcpy(str,"hello");
	printf("%s\n",str);
	free(str);
	str=NULL;
	return 0;
}
输出结果：
hello
```

第二种方法：使用返回值

```
#include <stdio.h>  
#include <stdlib.h>  
#include <string.h>  
char* fa(char* p)  //主要还是指针的问题  
{  
    p=(char* )malloc(100);  
   	return p;
}  
int main()  
{  
    char* str=NULL;//这块没问题的  
    str = fa(str);  // 函数返回了指针地址
    strcpy(str,"hello");  
    printf("%s\n",str);  
    free(str);  
    str=NULL;  
    return 0;  
}  
输出结果：
hello
```

### 问题二：

```
char *GetMemory( void )
{
 char p[] = "hello world"; // 局部变量，调用结束后消失
 return p;
}
void Test( void )
{
 char *str = NULL;
 str = GetMemory();
 printf( str );
}
```
请问运行 Test 函数会有什么样的结果？ 
答：可能是乱码。

因为 GetMemory 返回的是指向“栈内存”的指针，该指针的地址不是 NULL，但其原现的内容已经被清除，新内容不可知。
char p[] = "hello world";
return p;
p[]数组为函数内的局部自动变量,在函数返回后,内存已经被释放。这是许多程序员常犯的错误,其根源在于不理解变量的生存期。

```
#include <stdio.h>
char* fa(char* p_str)//指针做形参可以使用调用函数的存储区
{
	char* p=p_str;
	p="hello world";  // p指向形参p_str地址，返回的而是p_str地址
	return p;
}
 
int main()
{
	char* str=NULL;
	printf("%s\n",fa(str));
	return 0;
}
输出结果：
hello world
```
# 关键字typedef

# 声明与定义

# 内存管理

# 存储类型关键字

定义： 是对声明的实现或者实例化。连接器(linker)需要定义来引用内存实体。与上面的声明相应的定义如下：参看：C语言再学习 -- 存储类、链接

C语言中有 5 个作为存储类说明符的关键字。关键字typedef 与内存存储无关，由于语法原因被归入此类。

现在简单了解一下这五个存储类说明符的关键字：

**说明符 auto**  表明一个变量具有自动存储时期。该说明符只能用于在具有代码块作用域的变量声明中，而这样的变量已经拥有自动存储时期，因此它主要用来明确指出意图，使程序更易读。

**说明符 register**  也只能用于具有代码块作用域的变量。它将一个变量归入寄存器存储类，这相当于请求将该变量存储在一个寄存器内，以更快地存取。它的使用也使得不能获得变量的地址。

**说明符 static**  在用于具有代码块的作用域的变量的声明时，使该变量具有静态存储时期，从而得以在程序运行期间（即使在包含该变量的代码块没有运行时）存在并保留其值。变量仍具有代码块作用域和空链接。static 用于具有文件作用域的变量的声明时，表明该变量具有内部链接。

**说明符 extern**  表明在声明一个已经在别处定义了的变量。如果包含 extern 的声明具有文件作用域，所指向的变量必须具有外部链接。如果包含 extern 的声明具有代码块作用域，所指向的变量可能具有外部链接也可能具有内部链接，这取决于该变量的定义声明。

**关键字 typedef** 参看：C语言再学习 -- 关键字typedef

>**注意，这 5 个作为存储类说明符的关键字，不可以同时出现的。**

例如：  typedef static int int32  是错误的。

下面来一一详细介绍：

##	1、auto关键字

auto 关键字在C语言中只有一个作用，那就是修饰局部变量。 
auto 修饰局部变量，表示这个局部变量是自动局部变量，自动局部变量分配在栈上。（既然在栈上，说明它如果不初始化那么值就是随机的） 
平时定义局部变量时就是定义的auto的，只是省略了auto关键字而已。可见，auto的局部变量其实就是默认定义的普通的局部变量。 即 int a = 10; 等价于 auto int a = 10;

auto 修饰局部变量，若省去数据类型，变量默认为 int 类型
```
#include <stdio.h>
//auto int d;   修饰全局变量 错误： 文件作用域声明‘d’指定了‘auto’
int main (void)
{
	auto int a = 10;  //等价于 int a = 10;
	auto b = 9;       //默认数据类型 为 int
	auto c;           //不初始化，值为随机的
 
	printf ("sizeof (b) = %d\n", sizeof (b));
	printf ("c = %d\n", c);
	return 0;
}
输出结果：
sizeof (b) = 4
c = -1217310732
```
 

## 2、register关键字

在 C 语言中的 register 修饰的变量表示将此变量存储在CPU的寄存器中，由于CPU访问寄存器比访问内存快很多，可以大大提高运算速度。但在使用register时有几点需要注意。

1. 用register修饰的变量只能是局部变量，不能是全局变量。CPU的寄存器资源有限，因此不可能让一个变量一直占着CPU寄存器。
2. register变量一定要是CPU可以接受的值。
3. 不可以用&运算符对register变量进行取址。比如 int i；(自动为auto)int \*p=&i;是对的， 但register int j; int \*p = &j; 是错的，因为无法对寄存器取址。
4. register只是请求寄存器变量，不一定能够成功。
5. 随着编译程序设计技术的进步，在决定那些变量应该被存到寄存器中时，现在的C编译环境能比程序员做出更好的决定。实际上，许多编译程序都会忽略register修饰符，因为尽管它完全合法，但它仅仅是暗示而不是命令。
```
#include <stdio.h>
//register int n; 修饰全局变量 错误： ‘n’的寄存器名无效
 
int main (void)
{
	register int i;
	//int *p = &i;  对i取地址 错误： 要求寄存器变量‘i’的地址。
	int tmp = 0;
	for (i = 1; i < 100; i++)
		tmp += i;
	printf ("tmp = %d\n", tmp);
	return 0;
}
输出结果：
tmp = 4950
```
 

寄存器变量(register): 寄存器变量会尽量把变量放到寄存器(而不是栈或堆), 从而加快存取速度。下面的例子可以很好的看出：
```
#include <stdio.h>
#include <sys/timeb.h>
long long getSystemTime() {
    struct timeb t;
    ftime(&t);
    return 1000 * t.time + t.millitm;
}
 
#define TIME 1000000000
 
int m, n = TIME; /* 全局变量 */
int main(void)
{   
    
    register int a, b = TIME; /* 寄存器变量 */
    int x, y = TIME;          /* 一般变量   */
    long long start = 0, end = 0;
    
    start=getSystemTime();
    for (a = 0; a < b; a++);
    end=getSystemTime();
    printf("寄存器变量用时: %lld ms\n", end - start);
    
    start=getSystemTime();
    for (x = 0; x < y; x++);
    end=getSystemTime();
    printf("一般变量用时: %lld ms\n", end - start);
    
    start=getSystemTime();
    for (m = 0; m < n; m++);
    end=getSystemTime();
    printf("全局变量用时: %lld ms\n", end - start);
 
    return 0;
}
输出结果：
寄存器变量用时: 533 ms
一般变量用时: 3513 ms
全局变量用时: 3587 ms
```
 

## 3、static关键字

参看：C语言再学习 -- 内存管理
参看：C语言再学习 -- 存储类、链接

首先了解下，进程中的内存区域划分
（1）代码区 存放程序的功能代码的区域，比如：函数名
（2）只读常量区 主要存放字符串常量和const修饰的全局变量
（3）全局区 主要存放 已经初始化的全局变量 和 static修饰的全局变量
（4）BSS段 主要存放 没有初始化的全局变量 和 static修饰的局部变量，BSS段会在main函数执行之前自动清零
（5）堆区 主要表示使用malloc/calloc/realloc等手动申请的动态内存空间，内存由程序员手动申请和手动释放
（6）栈区 主要存放局部变量（包括函数的形参），const修饰的局部变量，以及块变量，该内存区域由操作系统自动管理

下面详细介绍 static 关键字，主要有三类用法：

### 1）static 修饰全局变量

static 修饰的全局变量也叫静态全局变量，和已经初始化的全局变量同 在全局区。

该类具有静态存储时期、文件作用域和内部链接，仅在编译时初始化一次。如未明确初始化，它的字节都被设定为0。static全局变量只初使化一次，是为了防止在其他文件单元中被引用；利用这一特性可以在不同的文件中定义同名函数和同名变量，而不必担心命名冲突。

示例说明：
```
file.h

//头文件 .h
#ifndef __FILE_H__
#define __FILE_H__
void foo ();
#endif


file1.c

#include <stdio.h>
#include "file.h"
 
int n = 5;  //已初始化的全局变量
static int m = 10;  //已初始化的静态全局变量
 
int x;  //未初始化的全局变量，自动初始化为 0
static int y;  //未初始化的静态全局变量， 自动初始化为 0
 
void foo ()  //静态全局变量，具有文件作用域，静态定义，内部链接
{
	printf ("x = %d, y = %d\n", x, y);
	printf ("n = %d, m = %d\n", n, m);
}


file2.c

#include <stdio.h>
#include "file.h"
int main (void)
{
	extern int n;    
	extern int m;  
	foo ();
        printf ("n = %d\n", n);  //全局变量，可被其他文件使用
        //printf ("m = %d\n", m);  //静态全局变量， 不可被其他文件使用
        //出现错误 file2.c:(.text+0x27): undefined reference to `m'
 return 0;
}
输出结果：
x = 0, y = 0
n = 5, m = 10
n = 5
```

### 2）static 修饰局部变量


static 修饰的局部变量也叫静态局部变量，和没有初始化的全局变量同 在BBS段。而非静态局部变量是被分配在栈上面的。非静态局部变量，函数调用结束后存储空间释放；静态局部变量，具有静态存储时期。只在程序开始时执行一次，函数调用结束后存储区空间并不释放，保留其当前值。

该类具有静态存储时期、代码作用域和空链接，仅在编译时初始化一次。如未明确初始化，它的字节都被设定为0。
```
file.h

//头文件
#ifndef __FILE_H__
#define __FILE_H__
void foo ();
#endif


file1.c

#include <stdio.h>
#include "file.h"
void foo ()
{
	int n = 5;     //已初始化，局部变量
	static m = 10; //已初始化，静态局部变量
 
	printf ("n = %d, m = %d\n", n, m);	
 
	n++;
	m++;
}
file2.c

#include <stdio.h>
#include "file.h"
int main (void)
{
	foo ();
	foo ();
	foo ();  //自动局部变量，函数调用结束后存储空间释放
	foo ();  //静态局部变量，具有静态存储时期。只在程序开始时执行一次，函数调用结束后存储区空间并不释放，保留其当前值。
 
	extern int n;
	extern int m;
//	printf ("n = %d\n", n);  
//	printf ("n = %d\n", m);  //静态局部变量，为空链接，不可以被其他文件使用,出现错误
//      file2.c:(.text+0x1f): undefined reference to `n'
//      file2.c:(.text+0x36): undefined reference to `m'
 
        int x;  //未初始化，局部变量，初始化为随机数  https://blog.csdn.net/wz947324/article/details/79867250
        static int y; //未初始化，静态局部变量，自动初始化为 0
        printf ("x = %d, y = %d\n", x, y);
 
        return 0;
}

输出结果：
n = 5, m = 10
n = 5, m = 11
n = 5, m = 12
n = 5, m = 13
x = -1216741388, y = 0
```

### 3）static 修饰函数

外部函数可被其他文件中的函数调用，而静态函数只可以在定义它的文件中使用。例如，考虑一个包含如下函数声明的文件:
```
double gamma (); //默认为外部的  
static double beta (); //静态函数  
extern double delta ();  
```

函数gamma ()和delta ()可被程序的其他文件中的函数使用，而beta ()则不可以，因为beta ()被限定在一个文件内，故可在其他文件中使用相同名称的不同函数。使用 static 存储类的原因之一就是创建为一个特定模块所私有的函数，从而避免可能的名字冲突。

通常使用关键字 extern 来声明在其他文件中定义的函数。这一习惯做法主要是为了程序更清晰，因为除非函数声明使用了关键字 static ，否则认为就是extern 的。


## 4、extern 关键字

首先，再讲解之前先要了解下，声明和定义的区别。

参看：C语言再学习 -- 声明与定义

举个例子： 
A) int i； 
B) extern int i； 
哪个是定义？哪个是声明？或者都是定义或者都是声明？

什么是定义：所谓的定义就是(编译器)创建一个对象，为这个对象分配一块内存并给它取上一个名字，这个名字就是我们经常所说的变量名或对象名。但注意，这个名字一旦和这块内存匹配起来，它们就同生共死，终生不离不弃。并且这块内存的位置也不能被改变。一个变量或对象在一定的区域内（比如函数内，全局等）只能被定义一次，如果定义多次，编译器会提示你重复定义同一个变量或对象。

什么是声明：有两重含义，如下：
第一重含义：告诉编译器，这个名字已经匹配到一块内存上了，下面的代码用到变量或对象是在别的地方定义的。声明可以出现多次。
第二重含义：告诉编译器，我这个名字我先预定了，别的地方再也不能用它来作为变量名或对象名。比如你在图书馆自习室的某个座位上放了一本书，表明这个座位已经有人预订，别人再也不允许使用这个座位。其实这个时候你本人并没有坐在这个座位上。这种声明最典型的例子就是函数参数的声明，例如：
void fun(int i, char c);
好，这样一解释，我们可以很清楚的判断: A)是定义； B)是声明。
记住， 定义声明最重要的区别：定义创建了对象并为这个对象分配了内存，声明没有分配内存。

声明： 指定了一个变量的标识符，用来描述变量的类型，是类型还是对象，或者函数等。声明，用于编译器(compiler)识别变量名所引用的实体。以下这些就是声明：

extern int bar;
extern int g(int, int);
double f(int, double); // 对于函数声明，extern关键字是可以省略的。
class foo; // 类的声明，前面是不能加class的。
定义： 是对声明的实现或者实例化。连接器(linker)需要它（定义）来引用内存实体。与上面的声明相应的定义如下：

int bar;
int g(int lhs, int rhs) {return lhs*rhs;} 
double f(int i, double d) {return i+d;} 
class foo {};// foo 这里已经拥有自己的内存了，对照上面两个函数，你就应该明白{}的用处了吧？
无论如何，定义 操作是只能做一次的。如果你忘了定义一些你已经声明过的变量，或者在某些地方被引用到的变量，那么，连接器linker是不知道这些引用该连接到那块内存上的。然后就会报missing symbols 这样的错误。如果你定义变量超过一次，连接器是不知道把引用和哪块内存连接，然后就会报 duplicated symbols这样的错误了。以上的symbols其实就是指定义后的变量名，也就是其标识的内存块。
 
总结：
如果只是为了给编译器提供引用标识，让编译器能够知道有这个引用，能用这个引用来引用某个实体（但没有为实体分配具体内存块的过程）是为声明。如果该操作能够为引用指定一块特定的内存，使得该引用能够在link阶段唯一正确地对应一块内存，这样的操作是为定义。
声明是为了让编译器正确处理对声明变量和函数的引用。定义是一个给变量分配内存的过程，或者是说明一个函数具体干什么用。
通过上述对声明和定义的解释可以看出，在C语言中，修饰符 extern 用在变量或者函数的声明前，用来说明“此变量/函数是在别处定义的，要在此处引用”。extern 是 C/C++ 语言中表明函数和全局变量作用范围（可见性）的关键字，该关键字告诉编译器，其声明的函数和变量可以在本模块或其它模块中使用。
 
1）extern 修饰变量的声明
具有外部链接的静态变量具有文件作用域，外部链接和静态存储时期。这一类型有时被称为外部存储类，这一类型的变量被称为外部变量。把变量的定义声明放在所有函数之外，即创建了一个外部变量。为了使程序更加清晰，可以在使用外部变量的函数中通过使用 extern 关键字来再次声明它。如果变量是在别的文件中定义，使用 extern 来声明该变量就是必须的。
 
int n; /*外部定义的变量*/ double Up[100]; /*外部定义的数组*/ extern char Coal; /*必须的声明，因为Coal在其他文件中定义*/ void next (void); int main (void) { extern double Up[]; /*可选的声明，此处不必指明数组大小*/ extern int n; /*可选的声明，如果将extern漏掉，就会建立一个独立的自动变量*/ } void next (void) { ... } 
 
下列 3 个例子展示了外部变量和自动变量的 4 种可能组合：
 
/*例1*/ int H; int magic (); int main (void) { extern int H; /*声明H为外部变量*/ ... } int magic () { extern int H; /*与上面的H是同一变量*/ } /*例2*/ int H; int magic (); int main (void) { extern int H; /*声明H为外部变量*/ ... } int magic () { ... /*未声明H，但知道该变量*/ } /*例3*/ int H; /*对main()和magic()不可见，但是对文件中其他不单独拥有局部H的函数可见*/ int magic (); int main (void) { int H; /*声明H， 默认为自动变量，main（）的局部变量*/ ... } int P；/*对magic()可见，对main()不可见，因为P声明子啊main()之后*/ int magic () { auto int H; /*把局部变量H显式地声明为自动变量*/ } 
这些例子说明了外部变量的作用域：从声明的位置开始到文件结尾为止。它们也说明了外部变量的生存期。
外部变量H和P存在的时间与程序运行时间一样，并且它们不局限于任一函数，在一个特定函数返回时并不消失。

多文件的程序中声明外部变量，使用 extern 来声明该变量就是必须的。注意能够被其他模块以extern修饰符引用到的变量通常是全局变量,可以放在file2.c文件的任何位置

//file1.c
int n = 10, m = 5; //n, m 为全局变量，只能定义在一处
//file2.c
#include <stdio.h>
//extern int n, m;  //声明 全局变量
//int n= 2, m = 3;  
/*
若果再次定义n,m。会出现错误
/tmp/cc4R2MbY.o:(.data+0x0): multiple definition of `n'
/tmp/ccwV9hWd.o:(.data+0x0): first defined here
/tmp/cc4R2MbY.o:(.data+0x4): multiple definition of `m'
/tmp/ccwV9hWd.o:(.data+0x4): first defined here
collect2: ld 返回 1
*/
 
void max (void);
 
int main (void)
{
	//printf ("n = %d, m = %d\n", n, m);
	max ();
	return 0;
}
 
void max (void)
{
	extern int n, m; //n, m为全局变量
	printf ("n = %d, m = %d\n", n, m);
}

编译：
gcc file1.c file2.c -o file
输出结果：
n = 10, m = 5
 

2）extern 修饰函数的声明

外部函数可被其他文件中的函数调用，而静态函数只可以在定义它的文件中使用。例如，考虑一个包含如下函数声明的文件:

double gamma (); //默认为外部的  
static double beta (); //静态函数  
extern double delta ();  
函数gamma ()和delta ()可被程序的其他文件中的函数使用，而beta ()则不可以，因为beta ()被限定在一个文件内，故可在其他文件中使用相同名称的不同函数。使用 static 存储类的原因之一就是创建为一个特定模块所私有的函数，从而避免可能的名字冲突。

通常使用关键字 extern 来声明在其他文件中定义的函数。这一习惯做法主要是为了程序更清晰，因为除非函数声明使用了关键字 static ，否则认为就是extern 的。换句话说，在定义（函数）的时候，这个extern居然可以被省略。

如果函数的声明中带有关键字 extern，仅仅是暗示这个函数可能再别的源文件里定义，没有其它作用。即下述这两个函数声明没有明显的区别：extern int foo (); 和 int foo (); 函数定义和声明时 extern 可有可无。

 
//file.c #include <stdio.h> void foo (void) { printf ("hello world!\n"); }
//file2.c
#include <stdio.h>
 
extern void foo ( );  //该函数声明可以放在 file2.c的任何位置
//等同于 void foo ();
int main (void)
{
	foo ();
	return 0;
}
编译：
gcc file1.c file2.c -o file
输出结果：
hello world!
 
一般把所有的全局变量和全局函数都放在一个 *.c 文件中，然后用一个同名的 *.h 文件包含所有的函数和变量的声明.
 
//main.c #include <stdio.h> #include "read.h" int main (void) { read (); printf ("num = %d\n", num); return 0; } 
//read.c
#include <stdio.h>
#include "read.h"
int num; //全局变量  定义
 
void read (void)
{
	printf ("请输入一个数字：");
	scanf ("%d", &num);
}
//read.h
//头文件卫士，防止头文件被重复定义
#ifndef __READ_H__
#define __READ_H__
extern int num;
void read (void);  //等价于 extern void read (void);
#endif
编译：
gcc main.c read.c -o read
输出结果：
请输入一个数字：12
num = 12
注意：
（1） extern int num = 10;  没有这种形式，不是定义。如果在 read.h中如此写的话会出现：
 
read.h:3:12: 警告： ‘num’已初始化，却又被声明为‘extern’ [默认启用] In file included from read.c:2:0: read.h:3:12: 警告： ‘num’已初始化，却又被声明为‘extern’ [默认启用] /tmp/ccQ3Jzzm.o:(.data+0x0): multiple definition of `num' /tmp/cceLUBvB.o:(.data+0x0): first defined here collect2: ld 返回 1
（2）再有，在使用 extern 时候要严格对应声明时的格式，例如:
声明的函数为： extern void read (void);
定义的时候 返回值、形参类型、函数名 需要一致，为 void read (void) {...}
C 程序中，不允许出现类型不同的同名变量。
（3）定义数组，修饰指针
在一个源文件里定义了一个数组：char a[100];
在另外一个文件里用下列语句进行了声明：extern char *a；
这样是不可以的，程序运行时会告诉你非法访问。原因在于，指向类型T的指针并不等价于类型T的数组。extern char *a声明的是一个指针变量而不是字符数组，因此与实际的定义不同，从而造成运行时非法访问。应该将声明改为extern char a[ ]。
但是，extern char a[]与 extern char a[100]等价。
因为这只是声明，不分配空间，所以编译器无需知道这个数组有多少个元素。
 
 
3）extern "C"

上面讲到了，C 程序中，不允许出现类型不同的同名变量。例如：
 
#include <stdio.h> void foo(void); int foo (int ,int); int main (void) { return 0; } 输出结果： test.c:5:5: 错误： 与‘foo’类型冲突 test.c:3:6: 附注： ‘foo’的上一个声明在此 
而C++程序中 却允许出现重载。重载的定义：同一个作用域，函数名相同，参数表不同的函数构成重载关系，例如：

 

 
 

//renam.cpp #include <iostream> using namespace std; void foo (int i) { cout << i << endl; } void foo (int i, double d) { cout << i << ' ' << d << endl; } int main (void) { foo (1); //_Z3fooi (1); foo (1,2);//_Z3fooid (1, 2.); return 0; }
gcc -c rename.cpp  //生成 rename.o 
 
nm rename.o //查看
=============================
000000f3 t _GLOBAL__I__Z3fooi
00000000 T _Z3fooi
0000002b T _Z3fooid
000000b3 t _Z41__static_initialization_and_destruction_0ii
         U _ZNSolsEPFRSoS_E
         U _ZNSolsEd
         U _ZNSolsEi
         U _ZNSt8ios_base4InitC1Ev
         U _ZNSt8ios_base4InitD1Ev
         U _ZSt4cout
         U _ZSt4endlIcSt11char_traitsIcEERSt13basic_ostreamIT_T0_ES6_
00000000 b _ZStL8__ioinit
         U _ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_c
         U __cxa_atexit
         U __dso_handle
         U __gxx_personality_v0
00000081 T main

可以看到：函数被 C++编译后在库中的名字与 C 语言的不同。
函数void foo (int i); 的库名为 _Z3fooi
函数void foo (int i, double d);  的库名为 _Z3fooid

通过库名，可以看出来包含了函数名、函数参数数量及类型信息，C++就是靠这种机制来实现函数重载的。而 C 语言则不会，因此会造成链接时找不到对应函数的情况，此时C函数就需要用extern “C”进行链接指定，来解决名字匹配问题，这告诉编译器，请保持我的名称，不要给我生成用于链接的中间函数名。
未加 extern "C" 声明的，在C++中因为重载，库名是 _Z3fooid，加上 extern "C" 会采用 C语言的方式 编译生成 foo。extern “C”这个声明的真实目的是为了实现C++与C及其它语言的混合编程。
 
 
 
参看：c/c++ 混合编程的 extern “C” 参看：extern "c"用法之一
参看：extern "c"用法解析
参看：extern ”C"的使用
C++中 extern "C" 的两种用法：
 
1）用C++语言写的一个函数，如果想让这个函数可以被其他C语言程序所用，则用extern "C" 来告诉C++编译器，请用C语言习惯来编译此函数。如：
 
//add.h #ifndef _ADD_H #define _ADD_H #ifdef __cplusplus extern "C" { #endif int add (int ,int ); #ifdef __cplusplus } #endif #endif 
//add.cpp
#include "add.h"
int add (int x, int y) {
	return x + y;
}
//main.c
#include <stdio.h>
#include "add.h"
int main (void) {
	int x=13,y=6;
	printf("%d+%d=%d\n",x,y,add(x,y));
	return 0;
}
编译：
gcc add.cpp main.c -o add -lstdc++
输出结果：
13+6=19
__cplusplus是cpp中自定义的一个宏，告诉编译器，这部分代码按C语言的格式进行编译，而不是C++的。
源文件为*.c，__cplusplus没有被定义，extern "C" {}这时没有生效对于C他看到只是 extern int add(int, int); 
add 函数编译符号成 add
gcc -c main.c nm main.o U add 00000000 T main U printf
源文件为*.cpp(或*.cc,*.C,*.cpp,*.cxx,*.c++), __cplusplus被定义 ,对于C++他看到的是 extern "C"  { extern  int add( int ,int);}编译器就会知道 add(13, 6);调用的C风格的函数，就会知道去找add符号而不是_Z3addii ；因此编译正常通过。

注：-lstdc++ 申明用c++库
如果将，add.h 如下改写，不使用 extern "C"：

#ifndef _ADD_H #define _ADD_H /* #ifdef __cplusplus extern "C" { #endif int add (int ,int ); #ifdef __cplusplus } #endif */ extern int add (int, int); #endif
编译：gcc add.cpp main.c -o add -lstdc++  出现错误
/tmp/ccBSzdDa.o: In function `main': main.c:(.text+0x29): undefined reference to `add' collect2: ld 返回 1 
但是，编译：g++ add.cpp main.c -o add 是OK的

因为g++会自动将c的模块中的符号表转换为 _Z3addii 这也是GNU compiler的强大之处，可是别的编译器也许就不这么智能了。所以在c/c++混合编程时还是最好加上extern “C”。
2）如果要在C++程序中调用C语言写的函数， 在C++程序里边用 extern "C" 修饰要被调用的这个C程序，告诉C++编译器此函数是C语言写的，是C语言编译器生成的，调用他的时候请按照C语言习惯传递参数等。

//sub.h #ifndef _SUB_H #define _SUB_H int sub(int ,int); #endif
//sub.c
#include "sub.h"
int sub(int x,int y) {
	return x + y;
}
//main.cpp
#include <iostream>
using namespace std;
extern "C" {
#include "sub.h"
}
int main (void) {
	int x=5,y=6;
	cout << x << "+" << y << "="
		<< sub(x, y) << endl;
	return 0;
}
编译：
gcc sub.c main.cpp -o sub -lstdc++
5+6=11
————————————————
版权声明：本文为CSDN博主「聚优致成」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_29350001/article/details/53895693

# 存储类、链接

这一章是我看的时间最长的一章了，表面上是有很多关键字和几个函数需要学习，其实我知道是自己最近不在状态了，做项目没进展，看书看不下去，两头都放不下，最后两头都没有做好。不由的想起一句话，你不快乐是因为：你可以像只猪一样懒，却无法像只猪一样，懒得心安理得。好了，言归正传，进入正题：

一、存储类

C为变量提供了5种不同的存储类型，或称存储类。

下面是C的5种存储类：

自动 —— 在一个代码块内（或在一个函数头部作为参量）声明的变量，无论有没有存储类修饰符 auto，都属于自动存储类。该类具有自动存储时期、代码块作用域和空链接。如未经初始化，它的值是不定的。

寄存器 —— 在一个代码块内（或在一个函数头部作为变量）使用存储类修饰符 register 声明的变量属于寄存器存储类。该类具有自动存储时期、代码块作用域和空链接，并且您无法获得其地址。把一个变量声明为寄存器变量可以指示编译器提供可用的最快访问。如未经初始化，它的值是不定的。

静态、空链接 —— 在一个代码块内使用存储类修饰符 static 声明的变量属于静态空链接存储类。该类具有静态存储时期、代码作用域和空链接，仅在编译时初始化一次。如未明确初始化，它的字节都被设定为0。

静态、外部链接 —— 在所有函数外部定义、未使用存储类修饰符 static 的变量属于静态、外部链接存储类。该类具有静态存储时期、文件作用域和外部链接，仅在编译时初始化一次。如未明确初始化，它的字节都被设定为0。

静态、内部链接 —— 在所有函数外部定义、使用存储类修饰符 static 的变量属于静态、内部链接存储类。该类具有静态存储时期、文件作用域和内部链接，仅在编译时初始化一次。如未明确初始化，它的字节都被设定为0。



这5种存储类有一个共同之处：在决定了使用哪一个存储类之后，就自动决定了作用域和存储时期。

其实还有一种选择就是使用 malloc() 函数来分配和管理内存，该函数返回一个指向具有所请求字节数的内存块的指针。将这一内存的地址作为参数来条用函数free()，可以使该内存块重新可用。



存储类

时期

作用域

链接    

声明方式

自动       

自动         

代码块

空

代码块内

寄存器

自动

代码块    

空

代码块内，使用关键字register       

具有外部链接的静态    

静态

文件

外部

所有函数之外

具有内部链接的静态

静态

文件

内部

所有函数之外，使用关键字static

空链接的静态

静态

代码块

空

代码块内，使用关键字static


上面提到的一些术语如：作用域、链接和存储时期。我们来做一下了解。

1、作用域

作用域描述了程序中可以访问一个标识符的一个或多个区域。一个C变量的作用域可以是代码块作用域、函数原型作用域，或者文件作用域。



代码块作用域：


double blocky (double cleo)
{
	double patcrick = 0.0;
	int i;
	for (i = 0; i < 10; i++)
	{
		double q = cleo * i;   /*q作用域的开始*/
		...
		patrick * = q;         
	}                        /*q作用域的结束*/
	return patrick;
}
上面代码中的变量 cleo 和 patrick 都有直到结束花括号的代码块作用域，q的作用域被限制在内部代码块内，只有该代码块内的代码可以访问q。

由上可知，代码块中定义的变量，从该变量被定义的地方到包含该定义的代码块的末尾该变量可见。另外，函数的形式参量尽管在函数的开始花括号前进行定义，同样也具有代码块作用域，隶属于包含函数体的代码块。



函数原型作用域：

适用于函数原型中使用的变量名，如下所示：


int mighty (int mouse, double large);
int mighty (int , double );  （同上）
函数原型作用域从变量定义处一直到原型声明的末尾，这意味着编译器在处理一个函数原型的参数是，它所关心的只是该参数的类型。



文件作用域：

一个在所有函数之外定义的变量具有文件作用域，具有文件作用域的变量从它定义处到包含该定义的文件结尾都是可见的。


#include <stdio.h>
int unite = 0;
void critic (void);
int main (void)
{
	...
	return 0;
}
 
void critic (void)
{
	...
}

这里，变量 units 具有文件作用域，在 main () 和 critic () 中都可以使用它。因为它们可以在不止一个函数中使用，文件作用域变量也被称为全局变量。


2、链接

一个C变量具有下列链接之一：外部链接、内部链接、空链接。



外部链接：

一个具有外部链接的变量可以在一个多文件程序的任何地方使用。


int n = 5;  /*文件作用域，外部链接，未使用 static */
int main (void)
{
    ...
    return 0;
}

内部链接：

一个具有内部链接的变量可以在一个文件的任何地方使用。


static int dodgers = 3; /*文件作用域，内部链接，使用了 static ，该文件所私有*/
int main (void)
{
	...
	return 0;
}


空链接：
具有代码块作用域或者函数原型作用域的变量有空链接，意味着它们是由其定义所在的代码块或函数原型所私有的。


double blocky (double cleo)  
{
	double patcrick = 0.0;    /*代码块作用域，空链接，该代码块所私有*/
	int i;
	for (i = 0; i < 10; i++)
	{
		double q = cleo * i;   /*q作用域的开始*/
		...
		patrick * = q;         
	}                        /*q作用域的结束*/
	return patrick;
}

3、存储时期

一个C变量有以下两种存储时期之一：静态存储时期和自动存储时期。



静态存储时期：

如果一个变量具有静态存储时期，它在程序执行期间将一直存在。具有文件作用域的变量具有静态存储时期。

注意对于具有文件作用域的变量，关键字 static 表明链接类型，并非存储时期。一个使用 static 声明了的文件作用域变量具有内部链接，而所有的文件作用域变量，无论它具有内部链接，还是具有外部链接，都具有静态存储时期。

总结上面这句话就是, static 修饰的静态全局变量，具有内部链接，不能被其他文件使用。



自动存储时期：

具有代码块作用域的变量，一般情况下具有自动存储时期。

在程序进入定义这些变量的代码块时，将为这些变量分配内存；当退出这个代码块时，分配的内存将被释放。



4、自动变量

属于自动存储类的变量具有自动存储时期、代码块作用域和空链接。默认情况下，在代码块或函数的头部定义的任意变量都属于自动存储类。



关键字auto：

关键字auto称为存储类说明符。

int main (void)
{
    auto int p;
}
为了表明有意覆盖一个外部函数定义时，或者为了表明不能把变量改变为其他存储类这一点很重要时，可以这样做。


代码块作用域和空链接意味着只有变量定义所在的代码块才可以通过名字访问变量（当然，可以用参数想其他函数传送该变量的值和地址，但那是以间接的方式知道的）。另一个函数可以使用具有同样名字的变量，但那将是存储在不同内存位置中的一个独立变量。


int loop (int n)
{
	int m;  // m的作用域
	scanf ("%d, &m");
	{
		int i; //m 和 i 的作用域
		for (i = m; i < n; i++)  
			put ("i is local to a sub-block\n");
	}
	return m;  //m的作用域， i 已经消失 
}

如上嵌套代码块中，只有定义变量的代码块及其内部的任何代码块可以访问的变量，变量 i 仅在内层花括号中是可见的。如果试图在内层代码块之前或之后使用该变量，将得到一个编译错误。


如果在内层代码块定义了一个具有和外层代码块变量同一个名字的变量，我们称之为内层定义覆盖了外部定义，但当运行离开内层代码块时，外部变量重新恢复作用。


#include <stdio.h>
int main (void)
{
	int x = 30;
	printf ("x in outer block : %d\n", x);
	{
		int x = 77; 
		printf ("x int inner block : %d\n", x);
	}
	printf ("x in outer block : %d\n", x);
	while (x++ < 33)
	{
		int x = 100;
		x++;
		printf ("x in while loop : %d\n", x);
	}
	printf ("x in outer block : %d\n", x);
	return 0;
}
输出结果：
x in outer block : 30
x int inner block : 77
x in outer block : 30
x in while loop : 101
x in while loop : 101
x in while loop : 101
x in outer block : 34


1）不带 {} 的代码块

语句若为循环或者 if 语句的一部分，即使没有使用 { }，也认为是一个代码块。更完整地说，整个循环是该循环所在代码块的子代码块，而循环体是整个循环代码块的子代码块。


#include <stdio.h>
int main (void)
{
	int n = 10;
	printf ("Initially, n = %d\n", n);
	for (int n = 1; n < 3; n++)
		printf ("loop 1: n = %d\n", n);
	printf ("After loop 1, n = %d\n", n);
	for (int n = 1; n < 3; n++)
	{
		printf ("loop 2 index n = %d\n", n);
		int n = 30;
		printf ("loop 2 n = %d\n", n);
		n++;
	}
	printf ("After loop 2, n = %d\n", n);
	return 0;
}
输出结果：
Initially, n = 10
loop 1: n = 1
loop 1: n = 2
After loop 1, n = 10
loop 2 index n = 1
loop 2 n = 30
loop 2 index n = 2
loop 2 n = 30
After loop 2, n = 10


注意，有些编译器可能不支持这些新的C99作用域规则，需要使用 -std=c99选项来激活这些规则：

例如： gcc -std=c99 forc99.c



2）自动变量的初始化

除非显式的初始化自动变量，否则它不会被自动初始化。

int main (void)
{
    int n;
    int m = 5;
}
变量 m 的初始化为5，而变量 n 的初值则是先前占用分配给它的空间任意值。不要指望这个值是 0 。除非你对自动变量进行显示的初始化，否则当自动变量创建时，它们的值总是垃圾值。


#include <stdio.h>
int main (void)
{
	int a;
	int b = a + 3;
	printf ("a = %d, b = %d\n", a, b);
	return 0;
}
 
输出结果：
a = 134513705, b = 134513708
倘若一个非常量表达式中所用到的变量先前定义过的话，可将自动变量初始化为该表达式：

int main (void)
{
    int n = 1;
    int m = 5 * n; //使用先前定义过的变量
}

5、寄存器变量

寄存器变量多是存放在一个寄存器而非内存中，所以无法获得寄存器变量的地址。具有代码块作用域、空链接以及自动存储时期。通过使用存储类说明符 register 可以声明寄存器变量：


int main (void)
{
    register int n;
}
可以把一个形式参量请求为寄存器变量，只需在函数头部使用 register 关键字：
void macho (register int n)
可以使用 register 声明的类型是有限的。例如，处理器可能没有足够大的寄存器来容纳 double 类型。


6、具有代码块作用域的静态变量

“静态”是指变量的位置固定不动，而非变量不可变。具有文件作用域的变量自动(也是必须的)具有静态存储时期。也可以创建具有代码块作用域，兼具静态存储的局部变量。这些变量和自动变量具有相同的作用域，但当包含这些变量的函数完成工作时，它们并不消失。也就是说，这些变量具有代码块作用域、空链接，却有静态存储时期。从一次函数调用到下一次调用，计算机都记录着它们的值。这样的变量通过使用存储类说明符 static （这提供了静态存储时期）在代码块内声明（这提供了代码块作用域和空链接）创建。


#include <stdio.h>
void trystat (void);
int main (void)
{
	int count;
	for (count = 1; count <= 3; count++)
	{
		printf ("Hello World %d\n", count);
		trystat ();
	}
	return 0;
}
 
void trystat (void)
{
	int fade = 1;
	static int stay = 1;
	printf ("fade = %d and stay = %d\n", fade++, stay++);
}
输出结果：
Hello World 1
fade = 1 and stay = 1
Hello World 2
fade = 1 and stay = 2
Hello World 3
fade = 1 and stay = 3



在每次调用trystat（）时fade都被初始化，而stay只在编译trystat（）时被初始化一次。如果不显式地对静态变量进行初始化，它们将被初始化为0.


7、具有外部链接的静态变量
具有外部链接的静态变量具有文件作用域，外部链接和静态存储时期。这一类型有时被称为外部存储类，这一类型的变量被称为外部变量。把变量的定义声明放在所有函数之外，即创建了一个外部变量。为了使程序更加清晰，可以在使用外部变量的函数中通过使用 extern 关键字来再次声明它。如果变量是在别的文件中定义，使用 extern 来声明该变量就是必须的。


int n;   /*外部定义的变量*/
double Up[100];  /*外部定义的数组*/
extern char Coal;  /*必须的声明，因为Coal在其他文件中定义*/
 
void next (void); 
 
int main (void)
{
	extern double Up[]; /*可选的声明，此处不必指明数组大小*/
	extern int n;  /*可选的声明，如果将extern漏掉，就会建立一个独立的自动变量*/
}
 
void next (void)
{
	...
}


下列 3 个例子展示了外部变量和自动变量的 4 种可能组合：

/*例1*/
int H;
int magic ();
int main (void)
{
	extern int H;  /*声明H为外部变量*/
	...
}
 
int magic ()
{
	extern int H;  /*与上面的H是同一变量*/
}
 
/*例2*/
int H;
int magic ();
int main (void)
{
	extern int H;  /*声明H为外部变量*/
	...
}
 
int magic ()
{
	...            /*未声明H，但知道该变量*/
}
 
 
/*例3*/
int H; /*对main()和magic()不可见，但是对文件中其他不单独拥有局部H的函数可见*/
int magic ();
int main (void)
{
	int H;          /*声明H， 默认为自动变量，main（）的局部变量*/
	...
}
 
int P；/*对magic()可见，对main()不可见，因为P声明子啊main()之后*/
int magic ()
{
	auto int H;     /*把局部变量H显式地声明为自动变量*/
}



这些例子说明了外部变量的作用域：从声明的位置开始到文件结尾为止。它们也说明了外部变量的生存期。

外部变量H和P存在的时间与程序运行时间一样，并且它们不局限于任一函数，在一个特定函数返回时并不消失。



1）外部变量初始化

外部变量可以被显式地初始化，如果不对外部变量初始化，则它们将自动被赋初值 0. 这一原则也适用于外部定义的数组元素。不同于自动变量，只可以用常量表达式来初始化文件作用域变量。


int x = 10; //可以， 10是常量
int y = 3 + 20;  //可以，一个常量表达式
size_t = sizeof (int);  //可以，一个常量表达式
int x2 = 2 * x;  //不可以， x是一个变量
（只要类型不是一个变长数组，sizeof表达式就被认为是常量表达式）


2）外部变量的使用

#include <stdio.h>
int units = 0;   //一个外部变量
void critic (void);
int main (void)
{
	extern int units;  //可选的二次声明
	printf ("How many pounds to a firkin of butter?\n");
	scanf ("%d", &units);
	while (units != 56)
		critic ();
	printf ("You must have looked it up!\n");
	return 0;
}
 
void critic (void)
{       //这里省略了可选的二次声明
	printf ("No luck, chummy. Try again\n");
	scanf ("%d", &units);
}
输出结果：
How many pounds to a firkin of butter?
23
No luck, chummy. Try again
56
You must have looked it up!



两个函数main() 和 critic() 都是标识符 units 来访问同一个变量。在 C 的术语中，称 units 具有文件作用域、外部链接以及静态存储时期。
在上述例子中，声明主要是使程序的可读性更好。存储类说明符 extern 告诉编译器在该函数中用到的 units 都是指同一个在函数外部（甚至在文件之外）定义的变量。再次，main（）和 critic （）都使用了外部定义的 units。



3）定义和声明

可参看：C语言再学习 2--声明与定义

这里需要注意的是，关键字 extern 用于声明，而非定义。



8、具有内部链接的静态变量

具有内部链接的静态变量，具有静态存储时期、文件作用域以及内部链接。通过使用存储类说明符 static 在所有函数外部进行定义（正如定义外部变量那样）来创建一个这样的变量。


int n = 1;     //外部链接
static int m = 1； //具有内部链接的静态变量
int main （void）
{
    extern int n； //使用全局变量 n
    extern int m;  //使用全局变量 m
}


可以在函数中使用存储类说明符 extern 来再次声明任何具有文件作用域的变量。这样的声明并不改变链接。



9、存储类说明符

C语言中有 5 个作为存储类说明符的关键字，分别是 auto、register、static、extern 以及 typedef。关键字typedef 与内存存储无关，由于语法原因被归入此类。

在文章：C语言再学习 18--关键字    里面我们将详细介绍各个关键字。



现在简单了解一下这五个存储类说明符的关键字：

说明符 auto 表明一个变量具有自动存储时期。该说明符只能用于在具有代码块作用域的变量声明中，而这样的变量已经拥有自动存储时期，因此它主要用来明确指出意图，使程序更易读。

说明符 register也只能用于具有代码块作用域的变量。它将一个变量归入寄存器存储类，这相当于请求将该变量存储在一个寄存器内，以更快地存取。它的使用也使得不能获得变量的地址。

说明符 static在用于具有代码块的作用域的变量的声明时，使该变量具有静态存储时期，从而得以在程序运行期间（即使在包含该变量的代码块没有运行时）存在并保留其值。变量仍具有代码块作用域和空链接。static 用于具有文件作用域的变量的声明时，表明该变量具有内部链接。

说明符 extern表明在声明一个已经在别处定义了的变量。如果包含 extern 的声明具有文件作用域，所指向的变量必须具有外部链接。如果包含 extern 的声明具有代码块作用域，所指向的变量可能具有外部链接也可能具有内部链接，这取决于该变量的定义声明。


//parta.c
#include <stdio.h>
void report_count ();
void accumulate (int k);
int count = 0;  //文件作用域，外部链接
int main (void)
{
	int value; //自动变量
	register int i;  //寄存器变量
 
	printf ("Enter a positive integer (0 to quit): ");
	while (scanf ("%d", &value) == 1 && value > 0)
	{
		++count;  //使用文件作用域变量
		for (i = value; i >= 0; i--)
			accumulate (i);
		printf ("Enter a posotove integer (0 to quit):");
	}
	report_count ();
	return 0;
}
 
void report_count ()
{
	printf ("Loop executed %d times\n", count);
}
 
 

//partb.c
#include <stdio.h>
extern int count;  //引用声明，外部链接
static int total = 0;  //文件作用域、静态定义，内部链接
void accumulate (int k); //原型
void accumulate (int k) //k 具有代码块作用域，空链接
{
	static int subtotal = 0;  //静态、空链接
	if (k <= 0)
	{
		printf ("Loop cycle: %d\n", count);
		printf ("subtotal: %d; total: %d\n", subtotal, total);
		subtotal = 0;
	}
	else
	{
		subtotal += k;
		total += k;
	}
}
 

gcc parta.c partb.c -o part
输出结果：
Enter a positive integer (0 to quit): 5
Loop cycle: 1
subtotal: 15; total: 15
Enter a posotove integer (0 to quit):10
Loop cycle: 2
subtotal: 55; total: 70
Enter a posotove integer (0 to quit):2
Loop cycle: 3
subtotal: 3; total: 73
Enter a posotove integer (0 to quit):0
Loop executed 3 times

10、存储类和函数

外部函数可被其他文件中的函数调用，而静态函数只可以在定义它的文件中使用。例如，考虑一个包含如下函数声明的文件:


double gamma (); //默认为外部的
static double beta (); //静态函数
extern double delta ();
函数gamma ()和delta ()可被程序的其他文件中的函数使用，而beta ()则不可以，因为beta ()被限定在一个文件内，故可在其他文件中使用相同名称的不同函数。使用 static 存储类的原因之一就是创建为一个特定模块所私有的函数，从而避免可能的名字冲突。
通常使用关键字 extern 来声明在其他文件中定义的函数。这一习惯做法主要是为了程序更清晰，因为除非函数声明使用了关键字 static ，否则认为就是extern 的。


# 输入/输出

https://blog.csdn.net/qq_29350001/article/details/52316708

# 关键字sizeof与strlen

## sizeof 

### 一、简单介绍

sizeof 是 C 语言的一种单目操作符，如 C 语言的其他操作符\++、--等。它并不是函数。C 规定 sizeof 返回 sieze_t 类型的值。这是一个无符号整数类型。C99更进一步，把%zd 作为用来显示 size_t 类型值的 printf() 说明符。如果你的系统没有实现 %zd，你可以试着使用 %u 或者 %lu 代替它。

sizeof 操作符以字节形式给出了其操作数的存储大小。操作数可以是一个表达式或括在括号内的类型名。操作数的存储大小由操作数的类型决定。


### 二、使用方法

1、用于数据类型

sizeof 使用形式： sizeof (type)
数据类型必须用圆括号括住。如：sizeof (int)

2、用于变量
sizeof 使用形式：sizeof (var_name) 或 sizeof var_name
变量名可以不用圆括号括住。如：sizeof (6.08) 或 sizeof 6.08 等都是正确的形式。带括号的用法更普遍，大多数程序员采用这种形式。

注意：sizeof 操作符不能用于函数类型，不完全类型和位字段。不完全类型指具有未知存储大小的数据类型，如未知存储大小的数组类型、未知内容的结构或联合类型、void类型等。如：

sizeof (max) 若此时变量 max 定义为 int max ( )，
sizeof (char_v) 若此时 char_v 定义为 char char_v [MAX] 且 MAX 未知
sizeof (void)

上述例子，都是不正确的形式。


3、sizeof 的结果

sizeof 操作符的结果类型是 size_t，它在头文件中 typedef 为 unsigned int 类型。该类型保证能容纳实现所建立的最大对象的字节大小。
1. 在windows，32位系统中
char         1个字节
short        2个字节
int            4个字节
long         4个字节
double    8个字节
float         4个字节

	看看这三个分别是什么?
1，‘ 1‘，“ 1”。
	第一个是整形常数， 32 位系统下占 4 个 byte；
	第二个是字符常量，占 1 个 byte；
	第三个是字符串常量，占 2 个 byte。

三者表示的意义完全不一样，所占的内存大小也不一样，初学者往往弄错。


2. 当操作数为指针时，sizeof 依赖于编译器。一般unix的指针为 4个字节
```
#include <stdio.h>
 
int main (void)
{
	char ptr[] = "hello";
	char *str = ptr;
	printf ("sizeof (str) = %d\n", sizeof (str));
	return 0;
}
```
输出结果：
`sizeof (str) = 4`
在 32 位系统下，不管什么样的指针类型，其大小都为 4 byte。可以测试一下 sizeof（ void *）。

3. 当操作数具有数组类型时，其结果是数组的总字节数
```
#include <stdio.h>
 
int main (void)
{
	int ptr[20] = {1,2,3,4,5};
	printf ("sizeof (ptr) = %d\n", sizeof (ptr));
	return 0;
}
```
输出结果：
`sizeof (ptr) = 80`


4. 联合类型操作数的 sizeof 是其最大字节成员的字节数，结构体类型操作数的sizeof，需要考虑内存对齐补齐。

```
view plain  copy    
#include <stdio.h>  
  
typedef union {  
    char ch;  
    int num;  
}UN;  
  
int main (void)  
{  
    printf ("sizeof (UN) is %d\n", sizeof (UN));  
    return 0;  
}  
```
输出结果：  
`sizeof (UN) is 4  `

结构体内存对齐与补齐
- 一个存储区的地址一定是它自身大小的整数倍（双精度浮点类型的地址只需要4的整数倍就行了)，这个规则也叫数据对齐，结构体内部的每个存储区通常也需要遵守这个规则。数据对齐可能造成结构体内部存储区之间有浪费的字节。
结构体的大小一定是内部最大基本类型存储区大小的整数倍，这个规则叫数据补齐。
```
#include <stdio.h>  
typedef struct  
{  
    char ch;  
    int num;  
    char ch1;  
}str;  
  
int main (void)  
{  
    printf ("sizeof (str) is %d\n", sizeof (str));  
    return 0;  
}  
输出结果：  
sizeof (str) is 12  
```

5. 如果操作数是函数中的数组形参或函数类型的形参，sizeof给出其指针的大小
```
#include <stdio.h>
 
char ca[10];
 
void foo (char ca[100])
{
	printf ("sizeof (ca) = %d\n", sizeof (ca));
}
int main (void)
{
	char ca [25];
	foo (ca);
	return 0;
}
输出结果：
sizeof (ca) = 4
```

### 三、主要用途

1、sizeof操作符的一个主要用途是与存储分配和I/O系统那样的例程进行通信。例如：
void *malloc（size_t size）, size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream)　 

2、sizeof的另一个的主要用途是计算数组中元素的个数。例如：void　*memset（void *s,int c,sizeof(s)）

3.在动态分配一对象时,可以让系统知道要分配多少内存。如：int *p=(int *)malloc(sizeof(int)*10);

4.由于操作数的字节数在实现时可能出现变化，建议在涉及到操作数字节大小时用sizeof来代替常量计算。

5.如果操作数是函数中的数组形参或函数类型的形参，sizeof给出其指针的大小。



### 四、注意的地方

1、在混合类型的算术运算的情况下，较小的类型被转换成较大的类型。反之，可能会丢失数据。
```
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
 
int main (void)
{
	int a = 10;
	printf ("sizeof ((a > 5) ? 4 : 8.0) = %d\n", sizeof ((a > 5) ? 4 : 8.0));
	return 0;
}
输出结果：
sizeof ((a > 5) ? 4 : 8.0) = 8  // 4被转成double，在编译中，小数默认时double型
```
2、判断表达式的长度并不需要对表达式进行求值
```
#include <stdio.h>
int main (void)
{
	int a = 10;
	int b = 0;
	int c = sizeof (b = a + 12);
	printf ("a = %d, b = %d, c = %d\n", a, b, c);
	return 0;
}
输出结果：
a = 10, b = 0, c = 4
```
所以sizeof (b = a + 12)并没有向 a 赋任何值。


## strlen

strlen首先是一个函数，只能以char * 做参数，返回的是字符的实际长度，不是类型占内存的大小。其结果是运行的时候才计算出来的。
```
#include <string.h>
size_t strlen(const char *s);
```
函数功能：用来统计字符串中有效字符的个数

功能实现函数：
```
size_t strlen (const char *s)  
{  
    const char *sc;  
    for (sc = s; *sc != '\0'; ++sc)  
    /"nothing"/  
    return sc - s;  
}  
```
strlen()函数被用作改变字符串长度，例如：

```
#include <stdio.h>  
#include <string.h>  
void fit (char *, unsigned int);  
int main (void)  
{  
    char str[] = "hello world";  
    fit (str, 7);  
    puts (str);  
    puts (str + 8);  
    return 0;  
}  
  
void fit (char *string, unsigned int size)  
{  
    if (strlen (string) > size)  
        *(string + size) = '\0';  
}  
输出结果：  
hello w  
rld  
```
可以看出：fit()函数在数组的第8个元素中放置了一个'\0'字符来代替原有的o字符。puts()函数输出停在o字符处，忽略了数组的其他元素。然而，数组的其他元素仍然存在。

## sizeof 与 strlen 的区别

参看：C语言再学习 -- 字符串和字符串函数

谈两者的区别之前，先要讲一下什么是字符串，字符串就是一串零个或多个字符，并且以一个位模式为全 0 的 '\0' 字节结尾。如在代码中写 "abc"，那么编译器帮你存储的是 "abc\0"。

siezeof运算符提供给你的数目比strlen大1，这是因为它把用来标志字符串结束的不可见的空字符（'\0'）也计算在内。

#include <stdio.h>
#include <string.h>
#include <stdlib.h>
 
int main (void)
{
	char *str1 = "abcde";
	char str2[] = "abcde";
	char str3[8] = "abcde";
	char str4[] = {'a', 'b', 'c', 'd', 'e'};
	char *p1 = malloc (20);
    
	printf ("sizeof (str1) = %d, strlen (str1) = %d\n", sizeof (str1), strlen (str1));
	printf ("sizeof (*str1) = %d, strlen (str1) = %d\n", sizeof (*str1), strlen (str1));
	printf ("sizeof (str2) = %d, strlen (str2) = %d\n", sizeof (str2), strlen (str2));
	printf ("sizeof (str3) = %d, strlen (str3) = %d\n", sizeof (str3), strlen (str3));
	printf ("sizeof (str4) = %d, strlen (str4) = %d\n", sizeof (str4), strlen (str4));
	printf ("sizeof (p1) = %d, sizeof (*p1) = %d\n", sizeof (p1), sizeof (*p1));
	printf ("sizeof (malloc(20)) = %d\n", sizeof (malloc (20)));
	return 0;
}
输出结果：
sizeof (str1) = 4, strlen (str1) = 5
sizeof (*str1) = 1, strlen (str1) = 5
sizeof (str2) = 6, strlen (str2) = 5
sizeof (str3) = 8, strlen (str3) = 5
sizeof (str4) = 5, strlen (str4) = 5
sizeof (p1) = 4, sizeof (*p1) = 1
sizeof (malloc(20)) = 4


总结：

1. sizeof 操作符的结果类型是 size_t，它在头文件中typedef为 unsigned int 类型。该类型保证能容纳实现所建立的最大对象的字节大小。

2. sizeof 是算符，strlen 是函数。

3. sizeof可以用类型做参数，strlen只能用char*做参数，且必须是以''\0''结尾的。sizeof 还可以用函数做参数，比如：

short f();
printf("%d\n",sizeof(f()));
输出的结果是sizeof(short)，即2。

4.数组做 sizeof 的参数不退化，传递给 strlen 就退化为指针了。

5.大部分编译程序 在编译的时候就把 sizeof 计算过了 是类型或是变量的长度这就是 sizeof(x) 可以用来定义数组维数的原因
charstr[20]="0123456789";
int a=strlen(str);//a=10;
int b=sizeof(str);//而b=20;

6. strlen 的结果要在运行的时候才能计算出来，是用来计算字符串的长度，不是类型占内存的大小。

7. sizeof 后如果是类型必须加括弧，如果是变量名可以不加括弧。这是因为sizeof是个操作符不是个函数。

8. 当适用了于一个结构类型时或变量， sizeof 返回实际的大小，当适用一静态地空间数组， sizeof 归还全部数组的尺寸。sizeof 操作符不能返回动态地被分派了的数组或外部的数组的尺寸。

9. 数组作为参数传给函数时传的是指针而不是数组，传递的是数组的首地址，如：
fun(char [8])
fun(char [])
都等价于 fun(char *)
在C++里参数传递数组永远都是传递指向数组首元素的指针，编译器不知道数组的大小如果想在函数内知道数组的大小， 需要这样做：进入函数后用memcpy拷贝出来，长度由另一个形参传进去
fun(unsiged char*p1, int len)
{
    unsigned char* buf= new unsigned char[len+1]
    memcpy(buf, p1,len);
}


我们能常在用到 sizeof 和 strlen 的时候，通常是计算字符串数组的长度看了上面的详细解释，发现两者的使用还是有区别的，从这个例子可以看得很清楚：

charstr[20]="0123456789";
int a=strlen(str);//a=10; >>>> strlen 计算字符串的长度，以结束符 0x00 为字符串结束。
int b=sizeof(str);//而b=20; >>>> sizeof 计算的则是分配的数组 str[20] 所占的内存空间的大小，不受里面存储的内容改变。
上面是对静态数组处理的结果，如果是对指针，结果就不一样了

char* ss ="0123456789";
sizeof(ss) 结果4 ＝＝＝》ss是指向字符串常量的字符指针，sizeof 获得的是一个指针的之所占的空间,应该是 长整型的，所以是4
sizeof(*ss) 结果1 ＝＝＝》*ss是第一个字符 其实就是获得了字符串的第一位'0' 所占的内存空间，是char类 型的，占了 1 位
strlen(ss)= 10 >>>> strlen 计算字符串的长度，以结束符 0x00 为字符串结束。



面试题：

1、sizeof（ int） *p 表示什么意思？

需要明白 sizeof 后跟数据类型，必须要用圆括号括住的，强制类型转换也应该是 (int*) p，所以这句话所表达的意思是 sizeof (int) 乘以 p


#include <stdio.h>
 
int main (void)
{
	int p = 1;
	printf ("%d\n", sizeof (int)*p); 
	return 0;
}
输出结果：
4
#include <stdio.h>
 
int main (void)
{
	int p = 1;
	printf ("%d\n", sizeof ((int *)p));  //强制类型转换
	return 0;
}
输出结果：
4
2、在 32 位系统下：
int *p = NULL;
sizeof(p)的值是多少？
sizeof(*p)呢？

sizeof (p) = 4;  因为 p为指针，32位系统 指针所占字节为 4个字节

sizeof (*p) = 4;  因为 *p 为 指针所指向的变量为int类型，整型为 4个字节

#include <stdio.h>
 
int main (void)
{
	short *p = NULL;
	int i = sizeof (p);
	int j = sizeof (*p);
	printf ("i = %d, j = %d\n", i, j);
	return 0;
}
输出结果：
i = 4, j = 2
3、int a[100];
sizeof (a) 的值是多少？
sizeof(a[100])呢？ //请尤其注意本例。
sizeof(&a)呢？
sizeof(&a[0])呢？

sizeof (a) = 400;  因为 a是类型为整型、有100个元素的数组，所占内存为400个字节

sizeof (a[100]) = 4;  因为 a[100] 为数组的第100元素的值该值为 int 类型，所占内存为4个字节。

sizeof (&a) = 4;  因为 &a 为数组的地址即指针，32位系统 指针所占字节为 4个字节

sizeof (&a[0]) = 4; 因为&a[0] 为数组的首元素的地址即指针，32位系统 指针所占字节为 4个字节


#include <stdio.h>
 
int main (void)
{
	int a[100];
	printf ("sizeof (a) = %d\n", sizeof (a));
	printf ("sizeof (a[100] = %d\n", sizeof (a[100]));
	printf ("sizoef (&a) = %d\n", sizeof (&a));
	printf ("sizeof (&a[0] = %d\n)", sizeof (&a[0]));
	return 0;
}
输出结果：
sizeof (a) = 400
sizeof (a[100] = 4
sizoef (&a) = 4
sizeof (&a[0] = 4

4、int b[100];
void fun(int b[100])
{
sizeof(b);// sizeof (b) 的值是多少？
}

sizeof (b) = 4;  因为函数中的数组形参或函数类型的形参，sizeof给出其指针的大小。参数传递数组永远都是传递指向数组首元素的指针。


#include <stdio.h>
 
void fun (int b[100]) //指针做形参
{
	printf ("sizeof (b) = %d\n", sizeof (b));
}
 
int main (void)
{
	int a[10];
	fun (a);
	return 0;
}
输出结果：
sizeof (b) = 4

#include <stdio.h>
void foo (void)
{
	printf ("111\n");
}
 
void fun (foo) //函数做形参
{
	printf ("sizeof (foo) = %d\n", sizeof (foo));
}
int main (void)
{
	fun ();
	return 0;
}
输出结果：
sizeof (foo) = 4
————————————————
版权声明：本文为CSDN博主「聚优致成」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_29350001/article/details/52277578
<!--more-->
