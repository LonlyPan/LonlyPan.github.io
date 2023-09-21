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
    str = fa(str);  // 函数返回了地址
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
 char p[] = "hello world";
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
	p="hello world";
	return p;
}
 
int main()
{
	char* str=NULL;
	printf("%s\n",fa(str));
	return 0;
}
```

# 存储类、链接
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
