概述#
编译器介绍
gcc 是 GCC 中的 GUN C Compiler（C 编译器）

g++ 是 GCC 中的 GUN C++ Compiler（C++ 编译器）

gcc、g++ 编译常用选项说明：

选项	含义
-o file	指定生成的输出文件名为 file
-E	只进行预处理
-S (大写)	只进行预处理和编译
-c (小写)	只进行预处理、编译和汇编
`gcc hello.c -o hello`

没有指定可执行程序名字，默认生成 a.exe

指定可执行程序名字 hello，系统自动加.exe 后缀名

#include<> 与 #include “” 的区别：

< > 表示系统直接按系统指定的目录检索

“” 表示系统先在 “” 指定的路径查找头文件，如果找不到，再按系统指定的目录检索

system 函数
#include <stdlib.h>

int system(const char *command);

功能：在已经运行的程序中执行另外一个外部程序

参数：外部可执行程序名字

返回值：

成功：不同系统返回值不一样

失败：通常是 - 1

#include <stdio.h>
#include <stdlib.h>

int main()
{
    //system("calc"); //windows平台
    system("ls"); //Linux平台, 需要头文件#include <stdlib.h>
    return 0;
}
C 程序编译步骤
C 代码编译成可执行程序经过 4 步：

1）预处理：宏定义展开、头文件展开、条件编译等，同时将代码中的注释删除，这里并不会检查语法

2）编译：检查语法，将预处理后文件编译生成汇编文件

3）汇编：将汇编文件生成目标文件 (二进制文件)

4）链接：C 语言写的程序是需要依赖各种库的，所以编译之后还需要把库链接到最终的可执行程序中去

Linux平台下，ldd(“l”为字母) 找可执行程序查所依赖的动态库

C 语言基础
数据类型
数据类型的作用：编译器预算对象（变量）分配的内存空间大小。



常量
在程序运行过程中，其值不能被改变的量

整型常量	100，200，-100，0
实型常量	3.14 ， 0.125，-3.123
字符型常量	‘a’,‘b’,‘1’,‘\n’
字符串常量	“a”,“ab”，“12356”
变量
在程序运行过程中，其值可以改变

变量在使用前必须先定义，定义变量前必须有相应的数据类型

标识符命名规则：

标识符不能是关键字

标识符只能由字母、数字、下划线组成

第一个字符必须为字母或下划线

标识符中字母区分大小写

声明和定义区别

声明变量不需要建立存储空间，如：extern int a;

定义变量需要建立存储空间，如：int b;

进制
数据在计算机中主要是以补码的形式存储的。

术语	含义
bit (比特)	一个二进制代表一位，一个位只能表示 0 或 1 两种状态。
Byte (字节)	一个字节为 8 位，计算机中存储的最小单位是字节。习惯以 “字节”（Byte）为单位。
十进制转化二进制的方法



十进制的小数转换成二进制

将小数部分乘以 2，取结果的整数部分为二进制的一位。 然后继续取结果的小数部分乘 2 重复，一直到小数部分全部为 0 结束.



C 语言表示相应进制数

十进制	以正常数字 1-9 开头，如 123
八进制	以数字 0 开头，如 0123
十六进制	以 0x 开头，如 0x123
二进制	C 语言不能直接书写二进制数
数值存储方式
原码
最高位做为符号位，0 表示正，为 1 表示负

其它数值部分就是数值本身绝对值的二进制数

十进制数	原码
+15	0000 1111
-15	1000 1111
+0	0000 0000
-0	1000 0000
反码
对于正数，反码与原码相同

对于负数，符号位不变，其它部分取反 (1 变 0,0 变 1)

十进制数	反码
+15	0000 1111
-15	1111 0000
+0	0000 0000
-0	1111 1111
补码
在计算机系统中，数值一律用补码来存储。

对于正数，原码、反码、补码相同

对于负数，其补码为它的反码加 1

补码符号位不动，其他位求反，最后整个数加1，得到原码

十进制数	补码
+15	0000 1111
-15	1111 0001
+0	0000 0000
-0	0000 0000
#include <stdio.h>

int main()
{
    int  a = -15;
    printf("%x\n", a);
    //结果为 fffffff1
    //fffffff1对应的二进制：1111 1111 1111 1111 1111 1111 1111 0001
    //符号位不变，其它取反：1000 0000 0000 0000 0000 0000 0000 1110
    //上面加1：1000 0000 0000 0000 0000 0000 0000 1111  最高位1代表负数，就是-15
    return 0;
}
补码的意义

在计算机系统中，数值一律用补码来存储，主要原因是：

统一了零的编码

将符号位和其它位统一处理

将减法运算转变为加法运算

两个用补码表示的数相加时，如果最高位 (符号位) 有进位，则进位被舍弃

数据类型
整型：int
打印格式	含义
%d	输出一个有符号的 10 进制 int 类型
% o (字母 o)	输出 8 进制的 int 类型
%x	输出 16 进制的 int 类型，字母以小写输出
%X	输出 16 进制的 int 类型，字母以大写写输出
%u	输出一个 10 进制的无符号数
数据类型	占用空间
short (短整型)	2 字节
int (整型)	4 字节
long (长整形)	Windows 为 4 字节，Linux 为 4 字节 (32 位)，8 字节 (64 位)
long long (长长整形)	8 字节
数据类型	占用空间	取值范围
short	2 字节	-32768 到 32767 (-2^15 ~ 2^15-1)
int	4 字节	-2147483648 到 2147483647 (-2^31 ~ 2^31-1)
long	4 字节	-2147483648 到 2147483647 (-2^31 ~ 2^31-1)
unsigned short	2 字节	0 到 65535 (0 ~ 2^16-1)
unsigned int	4 字节	0 到 4294967295 (0 ~ 2^32-1)
unsigned long	4 字节	0 到 4294967295 (0 ~ 2^32-1)
字符型：char
字符型变量用于存储一个单一字符，在 C 语言中用 char 表示，其中每个字符变量都会占用 1 个字节。

char 的本质就是一个 1 字节大小的整型。

ASCII 码大致由以下两部分组成：

ASCII 非打印控制字符： ASCII 表上的数字 0-31 分配给了控制字符，用于控制像打印机等一些外围设备。

ASCII 打印字符：数字 32-126 分配给了能在键盘上找到的字符。数字 127 代表 Del 命令

转义字符

转义字符	含义	ASCII** 码值（十进制）**
\a	警报	007
\b	退格 (BS) ，将当前位置移到前一列	008
\f	换页 (FF)，将当前位置移到下页开头	012
\n	换行 (LF) ，将当前位置移到下一行开头	010
\r	回车 (CR) ，将当前位置移到本行开头	013
\t	水平制表 (HT) （跳到下一个 TAB 位置）	009
\v	垂直制表 (VT)	011
\ \	代表一个反斜线字符”"	092
\ '	代表一个单引号（撇号）字符	039
\“	代表一个双引号字符	034
?	代表一个问号	063
\0	数字 0	000
实型 (浮点型)：float、double
实型变量也可以称为浮点型变量，浮点型变量是用来存储小数数值的。

在 C 语言中， 浮点型变量分为两种： 单精度浮点数 (float)、 双精度浮点数

数据类型	占用空间	有效数字范围
float	4 字节	7 位有效数字
double	8 字节	15～16 位有效数字
//不以f结尾的常量是double类型，以f结尾的常量(如3.14f)是float类型
float a = 3.14f; //或3.14F
double b = 3.14;
//科学法赋值
a = 3.2e3f;
类型限定符
限定符	含义
extern	声明一个变量，extern 声明的变量没有建立存储空间。 extern int a;
const	定义一个常量，常量的值不能修改。 const int a = 10;
volatile	防止编译器优化代码
register	定义寄存器变量，提高效率。CPU 有空闲寄存器，那么 register 就生效，如果没有那么 register 无效。
字符串格式化输出和输入
字符串常量

字符串是内存中一段连续的 char 空间，以’\0’结尾。

字符串常量是由双引号括起来的字符序列，如 “china”

每个字符串的结尾，编译器会自动的添加一个结束标志位’\0’，即 “a” 包含两个字符’a’和’\0’。

printf 函数和 putchar 函数
printf 是输出一个字符串，putchar 输出一个 char。

printf 格式字符：

打印格式	对应数据类型	含义
%d	int	接受整数值并将它表示为有符号的十进制整数
%hd	short int	短整数
%hu	unsigned short	无符号短整数
%o	unsigned int	无符号 8 进制整数
%u	unsigned int	无符号 10 进制整数
%x,%X	unsigned int	无符号 16 进制整数，x 对应的是 abcdef，X 对应的是 ABCDEF
%f	float	单精度浮点数
%lf	double	双精度浮点数
%e,%E	double	科学计数法表示的数，此处”e” 的大小写代表在输出时用的”e” 的大小写
%c	char	字符型。可以把输入的数字按照 ASCII 码相应转换为对应的字符
%s	char *	字符串。输出字符串中的字符直至字符串中的空字符（’\0’即空字符）
%p	void *	以 16 进制形式输出指针
%%	%	输出一个百分号
scanf 函数与 getchar 函数
getchar 是从标准输入设备读取一个 char。

scanf 通过 % 转义的方式可以得到用户通过标准输入设备输入的数据。

#include <stdio.h>

int main()
{
    char ch1;
    char ch3;

```c
printf("请输入ch1的字符：");
ch1 = getchar();
printf("ch1 = %c\n", ch1);

printf("请输入ch3的字符：");
scanf("%c", &ch3);//这里第二个参数一定是变量的地址，而不是变量名
printf("ch3 = %c\n", ch3);

return 0;
```
}
运算符
运算符类型	作用
算术运算符	用于处理四则运算
赋值运算符	用于将表达式的值赋给变量
比较运算符	用于表达式的比较，并返回一个真值或假值
逻辑运算符	用于根据表达式的值返回真值或假值
位运算符	用于处理数据的位运算
sizeof 运算符	用于求字节数长度
逻辑运算符
c 语言中的逻辑运算符都是短路运算符，一旦确定这个式子是真的就不再判断了

运算符	术语	示例	结果
!	非	!a	如果 a 为假，则！a 为真； 如果 a 为真，则！a 为假。
&&	与	a && b	如果 a 和 b 都为真，则结果为真，否则为假。
||	或	a || b	如果 a 和 b 有一个为真，则结果为真，二者都为假时，结果为假。
流程结构
if…else
#include <stdio.h>
int main()
{
    int a = 1;
    int b = 2;
    if (a > b)
    {
        printf("%d\n", a);
    }else{
        printf("%d\n", b);
    }
    return 0;
}
三目运算符
#include <stdio.h>

int main()
{
    int a = 10;
    int b = 20;
    int c;
    a = 1;
    b = 2;
    c = ( a > b ? a : b );
    printf("c2 = %d\n", c);
    return 0;
}
switch 语句
#include <stdio.h>

int main()
{
    char c;
    c = getchar();
    switch (c) //参数只能是整型变量
    {
    case '1':
        printf("OK\n");
        break;//switch遇到break就中断了
    case '2':
        printf("not OK\n");
        break;
    default://如果上面的条件都不满足，那么执行default
        printf("are u ok?\n");
    }
    return 0;
}
while 语句
#include <stdio.h>

int main()
{
    int a = 20;
    while (a > 10)
    {
        scanf("%d", &a);
        printf("a = %d\n", a);
    }
    return 0;
}
do…while
#include <stdio.h>

int main()
{
    int a = 1;
    do
    {
        a++;
        printf("a = %d\n", a);
    } while (a < 10);

```c
return 0;
```
}
for 语句
#include <stdio.h>

int main()
{
    int i;
    int sum = 0;
    for (i = 0; i <= 100; i++)
    {
        sum += i;
    }
    printf("sum = %d\n", sum);
    return 0;
}
break、continue、goto
break 语句

在 switch 条件语句和循环语句中都可以使用 break 语句：

当它出现在 switch 条件语句中时，作用是终止某个 case 并跳出 switch 结构。

当它出现在循环语句中，作用是跳出当前内循环语句，执行后面的代码。

continue 语句

在循环语句中，如果希望立即终止本次循环，并执行下一次循环。

  int main()
  {
      int sum = 0;           //定义变量sum
      for (int i = 1; i <= 100; i++)
      {
          if (i % 2 == 0)   //如果i是一个偶数，执行if语句中的代码
          {
              continue;      //结束本次循环
          }
          sum += i;          //实现sum和i的累加
      }

```c
  printf("sum = %d\n", sum);

  return 0;
```
  }
goto 语句

#include <stdio.h>

int main()
{
    goto End; //无条件跳转到End的标识
    printf("aaaaaaaaa\n");

End:
    printf("bbbbbbbb\n");
    return 0;
}
数组和字符串
数组
数组就是在内存中连续的相同类型的变量空间。同一个数组所有的成员都是相同的数据类型，同时所有的成员在内存中的地址是连续的。

按数组元素类型的不同，数组可分为：数值数组、字符数组、指针数组、结构数组等类别。

int a[10];
char s[10];
char *p[10];
struct Stu boy[10];
一维数组
int a [3] 表示数组 a 有 3 个元素，其下标从 0 开始计算，因此 3 个元素分别为 a [0],a [1],a [2]
在定义数组的同时进行赋值，称为初始化。

全局数组若不初始化，编译器将其初始化为零。

局部数组若不初始化，内容为随机值。

```c
int a[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };//定义一个数组，同时初始化所有成员变量
int a[10] = { 1, 2, 3 };//初始化前三个成员，后面所有元素都设置为0
int a[10] = { 0 };//所有的成员都设置为0
 //[]中不定义元素个数，定义时必须初始化
int a[] = { 1, 2, 3, 4, 5 };//定义了一个数组，有5个成员
```
一维数组的逆置

#include <stdio.h>

int main()
{
    int a[] = {  1, -2, 3,- 4, 5, -6, 7, -8, -9, 10 };//定义一个数组，同时初始化所有成员变量
    int i = 0;
    int j = sizeof(a) / sizeof(a[0]) -1;
    int tmp;
    while (i < j)
    {
        tmp = a[i];
        a[i] = a[j];
        a[j] = tmp;
        i++;
        j--;
    }
    for (i = 0; i < sizeof(a) / sizeof(a[0]); i++)
    {
        printf("%d ", a[i]);
    }
    printf("\n");
    return 0;
}
二维数组
int a[3][4]; 定义了一个三行四列的数组。二维数组是按行进行存放的，先存放 a [0] 行，再存放 a [1] 行、a [2] 行，并且每行有四个元素，也是依次存放的。l 二维数组实际的硬件存储器是连续编址的，也就是说内存中只有一维数组，即放完一行之后顺次放入第二行，和一维数组存放方式是一样的。

二维数组的初始化

```c
//分段赋值     int a[3][4] = {{ 1, 2, 3, 4 },{ 5, 6, 7, 8, },{ 9, 10, 11, 12 }};
int a[3][4] = 
{ 
    { 1, 2, 3, 4 },
    { 5, 6, 7, 8, },
    { 9, 10, 11, 12 }
};
//连续赋值
int a[3][4] = { 1, 2, 3, 4 , 5, 6, 7, 8, 9, 10, 11, 12  };
//可以只给部分元素赋初值，未初始化则为0
int a[3][4] = { 1, 2, 3, 4  };
//所有的成员都设置为0
int a[3][4] = {0};
//[]中不定义元素个数，定义时必须初始化
int a[][4] = { 1, 2, 3, 4, 5, 6, 7, 8};
```
数组名
数组名是一个地址的常量，代表数组中首元素的地址。

#include <stdio.h>

int main()
{
    //定义了一个二维数组，名字叫a
    int a[3][4] = { 1, 2, 3, 4 , 5, 6, 7, 8, 9, 10, 11, 12  };

```c
//测二维数组所占内存空间，有3个一维数组，每个一维数组的空间为4*4
//sizeof(a) = 3 * 4 * 4 = 48
printf("sizeof(a) = %d\n", sizeof(a));

//求二维数组行数
printf("i = %d\n", sizeof(a) / sizeof(a[0]));

// 求二维数组列数
printf("j = %d\n", sizeof(a[0]) / sizeof(a[0][0]));

//求二维数组行*列总数
printf("n = %d\n", sizeof(a) / sizeof(a[0][0]));

return 0;
```
}
多维数组
多维数组的定义与二维数组类似，其语法格式具体如下：int a[3][4][5];

字符串
字符数组与字符串
C 语言中没有字符串这种数据类型，可以通过 char 的数组来替代；

数字0(和字符‘\0’等价)结尾的char数组就是一个字符串，但如果 char 数组没有以数字 0 结尾，那么就不是一个字符串，只是普通字符数组，所以字符串是一种特殊的char的数组。

#include <stdio.h>

int main()
{
    char c1[] = { 'c', ' ', 'p', 'r', 'o', 'g' }; //普通字符数组
    printf("c1 = %s\n", c1); //乱码，因为没有’\0’结束符

```c
//以‘\0’(‘\0’就是数字0)结尾的字符数组是字符串
char c2[] = { 'c', ' ', 'p', 'r', 'o', 'g', '\0'}; 
printf("c2 = %s\n", c2);

//字符串处理以‘\0’(数字0)作为结束符，后面的'h', 'l', 'l', 'e', 'o'不会输出
char c3[] = { 'c', ' ', 'p', 'r', 'o', 'g', '\0', 'h', 'l', 'l', 'e', 'o', '\0'};
printf("c3 = %s\n", c3);

return 0;
```
}
字符串处理函数
gets()
#include <stdio.h>
char *gets(char *s);
功能：从标准输入读入字符，并保存到s指定的内存空间，直到出现换行符或读到文件结尾为止。
参数：
    s：字符串首地址
返回值：
    成功：读入的字符串
    失败：NULL
fgets()
#include <stdio.h>
char *fgets(char *s, int size, FILE *stream);
功能：从stream指定的文件内读入字符，保存到s所指定的内存空间，直到出现换行字符、读到文件结尾或是已读了size - 1个字符为止，最后会自动加上字符 '\0' 作为字符串结束。
参数：
    s：字符串
    size：指定最大读取字符串的长度（size - 1）
    stream：文件指针，如果读键盘输入的字符串，固定写为stdin
返回值：
    成功：成功读取的字符串
    读到文件尾或出错： NULL

char str[100];
printf("请输入str: ");
fgets(str, sizeof(str), stdin);
printf("str = \"%s\"\n", str);
通过 scanf 和 gets 输入一个字符串的时候，不包含结尾的 “\n”，但通过fgets结尾多了“\n”。fgets () 函数是安全的，不存在缓冲区溢出的问题。

puts()
#include <stdio.h>
int puts(const char *s);
功能：标准设备输出s字符串，在输出完成后自动输出一个'\n'。
参数：
    s：字符串首地址
返回值：
    成功：非负数
    失败：-1
fputs()
#include <stdio.h>
int fputs(const char * str, FILE * stream);
功能：将str所指定的字符串写入到stream指定的文件中， 字符串结束符 '\0'  不写入文件。 
参数：
    str：字符串
    stream：文件指针，如果把字符串输出到屏幕，固定写为stdout
返回值：
    成功：0
    失败：-1

fputs("hello world", stdout);
strlen()
#include <string.h>
size_t strlen(const char *s);
功能：计算指定指定字符串s的长度，不包含字符串结束符‘\0’
参数：
s：字符串首地址
返回值：字符串s的长度，size_t为unsigned int类型

char str[] = "abcdefg";
int n = strlen(str);
printf("n = %d\n", n);
strcpy()
#include <string.h>
char *strcpy(char *dest, const char *src);
功能：把src所指向的字符串复制到dest所指向的空间中，'\0'也会拷贝过去
参数：
    dest：目的字符串首地址
    src：源字符首地址
返回值：
    成功：返回dest字符串的首地址
    失败：NULL
strncpy()
#include <string.h>
char *strncpy(char *dest, const char *src, size_t n);
功能：把src指向字符串的前n个字符复制到dest所指向的空间中，是否拷贝结束符看指定的长度是否包含'\0'。
参数：
    dest：目的字符串首地址
    src：源字符首地址
    n：指定需要拷贝字符串个数
返回值：
    成功：返回dest字符串的首地址
    失败：NULL

```c
char dest[20] ;
char src[] = "hello world";
strncpy(dest, src, 5);
dest[5] = '\0';
printf("%s\n", dest);
```
strcat()
#include <string.h>
char *strcat(char *dest, const char *src);
功能：将src字符串连接到dest的尾部，‘\0’也会追加过去
参数：
    dest：目的字符串首地址
    src：源字符首地址
返回值：
    成功：返回dest字符串的首地址
    失败：NULL

char str[20] = "123";
char *src = "hello world";
printf("%s\n", strcat(str, src));
strncat()
#include <string.h>
char *strncat(char *dest, const char *src, size_t n);
功能：将src字符串前n个字符连接到dest的尾部，‘\0’也会追加过去
参数：
    dest：目的字符串首地址
    src：源字符首地址
    n：指定需要追加字符串个数
返回值：
    成功：返回dest字符串的首地址
    失败：NULL

char str[20] = "123";
char *src = "hello world";
printf("%s\n", strncat(str, src, 5));
strcmp()
#include <string.h>
int strcmp(const char *s1, const char *s2);
功能：比较 s1 和 s2 的大小，比较的是字符ASCII码大小。
参数：
    s1：字符串1首地址
    s2：字符串2首地址
返回值：
    相等：0
    大于：>0
    小于：<0

char *str1 = "hello world";
char *str2 = "hello mike";
if (strcmp(str1, str2) == 0)
{
    printf("str1==str2\n");
}
strncmp()
#include <string.h>
int strncmp(const char *s1, const char *s2, size_t n);
功能：比较 s1 和 s2 前n个字符的大小，比较的是字符ASCII码大小。
参数：
    s1：字符串1首地址
    s2：字符串2首地址
    n：指定比较字符串的数量
返回值：
    相等：0
    大于： > 0
    小于： < 0
sprintf()
#include <stdio.h>
int sprintf(char *str, const char *format, ...);
功能：根据参数format字符串来转换并格式化数据，然后将结果输出到str指定的空间中，直到出现字符串结束符 '\0'  为止。
参数：
    str：字符串首地址
    format：字符串格式，用法和printf()一样
返回值：
    成功：实际格式化的字符个数
    失败：- 1

char dst[100] = { 0 };
int a = 10;
char src[] = "hello world";
int len = sprintf(dst, "a = %d, src = %s", a, src);
printf("dst = \" %s\"\n", dst);
printf("len = %d\n", len);

结果：
dst = "a = 10, src = hello world"
len = 26
strchr()
#include <string.h>
char *strchr(const char *s, int c);
功能：在字符串s中查找字母c出现的位置
参数：
    s：字符串首地址
    c：匹配字母(字符)
返回值：
    成功：返回第一次出现的c地址
    失败：NULL

char src[] = "ddda123abcd";
char *p = strchr(src, 'a');
printf("p = %s\n", p);
结果：
p = a123abcd
strstr()
#include <string.h>
char *strstr(const char *haystack, const char *needle);
功能：在字符串haystack中查找字符串needle出现的位置
参数：
    haystack：源字符串首地址
    needle：匹配字符串首地址
返回值：
    成功：返回第一次出现的needle地址
    失败：NULL

char src[] = "ddddabcd123abcd333abcd";
char *p = strstr(src, "abcd");
printf("p = %s\n", p);
strtok()
#include <string.h>
char *strtok(char *str, const char *delim);
功能：来将字符串分割成一个个片段。当strtok()在参数s的字符串中发现参数delim中包含的分割字符时, 则会将该字符改为\0 字符，当连续出现多个时只替换第一个为\0。
参数：
    str：指向欲分割的字符串
    delim：为分割字符串中包含的所有字符
返回值：
    成功：分割后字符串首地址
    失败：NULL
在第一次调用时：strtok()必需给予参数s字符串,往后的调用则将参数s设置成NULL，每次调用成功则返回指向被分割出片段的指针

```c
char a[100] = "adc*fvcv*ebcy*hghbdfg*casdert";
char *s = strtok(a, "*");//将"*"分割的子串取出
while (s != NULL)
{
    printf("%s\n", s);
    s = strtok(NULL, "*");
}
```
结果：
adc
fvcv
ebcy
hghbdfg
casdert
atoi()
#include <stdlib.h>
int atoi(const char *nptr);
功能：atoi()会扫描nptr字符串，跳过前面的空格字符，直到遇到数字或正负号才开始做转换，而遇到非数字或字符串结束符('\0')才结束转换，并将结果返回返回值。
参数：
    nptr：待转换的字符串
返回值：成功转换后整数

类似的函数有：
    atof()：把一个小数形式的字符串转化为一个浮点数。
    atol()：将一个字符串转化为long类型
函数
函数定义的一般形式：

返回类型 函数名 (形式参数列表)

{

 执行语句部分；

}

在定义函数时指定的形参，在未出现函数调用时，它们并不占内存中的存储单元，因此称它们是形式参数。

```c
void max(int a = 10, int b = 20) // error, 形参不能赋值
{
}
```


函数的声明
如果使用用户自己定义的函数，而该函数与调用它的函数（即主调函数）不在同一文件中，或者函数定义的位置在主调函数之后，则必须在调用此函数之前对被调用的函数作声明。

注意：一个函数只能被定义一次，但可以声明多次。

多文件 (分文件) 编程
把函数声明放在头文件 xxx.h 中，在主函数中包含相应头文件

在头文件对应的 xxx.c 中实现 xxx.h 声明的函数

防止头文件重复包含
方法一：

```c
#ifndef __SOMEFILE_H__
#define __SOMEFILE_H__
```

// 声明语句
#endif
方法二：

#pragma once

// 声明语句
指针
指针的实质就是内存 “地址”。指针是内存单元的编号，指针变量是存放地址的变量。

指针也是一种数据类型，指针变量也是一种变量

指针变量指向谁，就把谁的地址赋值给指针变量

* 操作符操作的是指针变量指向的内存空间

#include <stdio.h>

int main()
{
    int a = 0;
    char b = 100;
    printf("%p, %p\n", &a, &b); //打印a, b的地址

```c
//int*指针类型,一种数据类型，p才是变量名
int *p;
p = &a;//将a的地址赋值给变量p，p也是一个变量，值是一个内存地址编号
printf("%d\n", *p);//p指向了a的地址，*p就是a的值
return 0;
```
}
注意：& 可以取得一个变量在内存中的地址。但是，不能取寄存器变量，因为寄存器变量不在内存里，而在 CPU 里面，所以是没有地址的。

使用 sizeof () 测量指针的大小，得到的总是：4 或 8

在 32 位平台，所有的指针（地址）都是 32 位 (4 字节)

在 64 位平台，所有的指针（地址）都是 64 位 (8 字节)

野指针和空指针
任意数值赋值给指针变量没有意义，因为这样的指针就成了野指针，野指针不会直接引发错误，操作野指针指向的内存区域才会出问题。

```c
int a = 100;
int *p;
p = 0x12345678; //给指针变量p赋值，p为野指针
*p = 1000;  //操作野指针指向未知区域，内存出问题，err
```
C 语言中，可以把 NULL 赋值给此指针，这样就标志此指针为空指针。

NULL 是一个值为 0 的宏常量：#define NULL ((void *)0)

万能指针 void *
void * 指针可以指向任意变量的内存空间

```c
void *p = NULL;
int a = 10;
p = (void *)&a; //指向变量时，最好转换为void *
//使用指针变量指向的内存时，转换为int *
*( (int *)p ) = 11;
printf("a = %d\n", a);

结果：a = 11
```
const 修饰的指针
修饰 *，指针指向内存区域不能修改，指针指向可以变

const int *p1 = &a; //等价于int const *p1 = &a;
//*p1 = 111; //err
p1 = &b; //ok
修饰 p1，指针指向不能变，指针指向的内存可以修改

int * const p2 = &a;
//p2 = &b; //err
*p2 = 222; //ok
指针操作数组
数组名字是数组的首元素地址，它是一个常量

#include <stdio.h>

int  main()
{
    int a[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
    int i = 0;
    int n = sizeof(a) / sizeof(a[0]);
    //数组遍历操作
    for (i = 0; i < n; i++)
    {
        printf("%d, ", *(a+i));
    }
    printf("\n");
    //数组赋值操作
    int *p = a; //定义一个指针变量保存a的地址
    for (i = 0; i < n; i++)
    {
        p[i] = 2 * i;
    }
    return 0;
}
指针运算
指针计算不是简单的整数相加

如果是一个 int *，+1 的结果是增加一个 int 的大小

如果是一个 char *，+1 的结果是增加一个 char 大小

指针数组
指针数组，它是数组，数组的每个元素都是指针类型。

#include <stdio.h>

int main()
{
    int *p[3];//指针数组
    int a = 1;
    int b = 2;
    int c = 3;
    p[0] = &a;
    p[1] = &b;
    p[2] = &c;
    for (int i = 0; i < sizeof(p) / sizeof(p[0]); i++ )
    {
        printf("%d, ", *(p[i]));
    }
    printf("\n");

```c
return 0;
```
}
多级指针
C 语言允许有多级指针存在，在实际的程序中一级指针最常用，其次是二级指针。

二级指针就是指向一个一级指针变量地址的指针。

```c
int a = 10;
int *p = &a; //一级指针
*p = 100; //*p就是a

int **q = &p;
//*q就是p
//**q就是a

int ***t = &q;
//*t就是q
//**t就是p
//***t就是a
```
指针和函数

```c
#include <stdio.h>

void swap2(int *x, int *y)
{
    int tmp;
    tmp = *x;
    *x = *y;
    *y = tmp;
}
int main()
{
    int a = 3;
    int b = 5;
    swap2(&a, &b); //地址传递
    printf("a2 = %d, b2 = %d\n", a, b);
    return 0;
}
```


数组名做函数参数，函数的形参会退化为指针

```c
#include <stdio.h>
//void printArrary(int a[10], int n)
//void printArrary(int a[], int n)
void printArrary(int *a, int n)
{
    int i = 0;
    for (i = 0; i < n; i++)
    {
        printf("%d, ", a[i]);
    }
    printf("\n");
}

int main()
{
    int a[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
    int n = sizeof(a) / sizeof(a[0]);
    //数组名做函数参数
    printArrary(a, n); 
    return 0;
}
指针做为函数的返回值

#include <stdio.h>
int a = 10;
int *getA()
{
    return &a;
}

int main()
{
    *( getA() ) = 111;
    printf("a = %d\n", a);
    return 0;
}
指针和字符串
#include <stdio.h>

int main()
{
    char str[] = "hello world";
    char *p = str;
    *p = 'm';
    p++;
    *p = 'i';
    printf("%s\n", str);//millo world
    return 0;
}
```


main 函数的形参
argv 是命令行参数的字符串数组，argv 数组的每个成员都是 char * 类型

argc 代表命令行参数的数量，程序名字本身算一个参数



```c
#include <stdio.h>

//argc: 传参数的个数（包含可执行程序）
//argv：指针数组，指向输入的参数
int main(int argc, char *argv[])
{
    //指针数组，它是数组，每个元素都是指针
    char *a[] = { "aaaaaaa", "bbbbbbbbbb", "ccccccc" };
    int i = 0;
  printf("argc = %d\n", argc);
  for (i = 0; i < argc; i++)
  {
      printf("%s\n", argv[i]);
  }
  return 0;
}
```

内存管理
局部变量和全局变量
局部变量
局部变量也叫auto自动变量 (auto 可写可不写)，一般情况下代码块 {} 内部定义的变量都是自动变量.

它有如下特点：

在一个函数内定义，只在函数范围内有效

在复合语句中定义，只在复合语句中有效

随着函数调用的结束或复合语句的结束局部变量的声明声明周期也结束

如果没有赋初值，内容为随机

静态 (static) 局部变量

static 局部变量的作用域也是在定义的函数内有效

static 局部变量的生命周期和程序运行周期一样，同时 staitc局部变量的值只初始化一次，但可以赋值多次

static 局部变量若未赋以初值，则由系统自动赋值：数值型变量自动赋初值 0，字符型变量赋空字符

全局变量

在函数外定义，可被本文件及其它文件中的函数所共用，若其它文件中的函数调用此变量，须用 extern 声明

全局变量的生命周期和程序运行周期一样

不同文件的全局变量不可重名

静态 (static) 全局变量

在函数外定义，作用范围被限制在所定义的文件中

不同文件静态全局变量可以重名，但作用域不冲突

static 全局变量的生命周期和程序运行周期一样，同时 staitc 全局变量的值只初始化一次

全局函数和静态函数
函数默认都是全局的，使用关键字 static 可以将函数声明为静态，函数定义为 static就意味着这个函数只能在定义这个函数的文件中使用，在其他文件中不能调用，即使在其他文件中声明这个函数都没用。不同的文件static函数名是可以相同的。

类型	作用域	生命周期
auto 变量	一对 {} 内	当前函数
static 局部变量	一对 {} 内	整个程序运行期
extern 变量	整个程序	整个程序运行期
static 全局变量	当前文件	整个程序运行期
extern 函数	整个程序	整个程序运行期
static 函数	当前文件	整个程序运行期
register 变量	一对 {} 内	当前函数
内存布局
程序没有加载到内存前，可执行程序内部已经分为代码区（text）、数据区（data）和未初始化数据区（bss）3 个部分（有些人直接把 data 和 bss 合起来叫做静态区或全局区）。

代码区

存放 CPU 执行的机器指令。通常代码区是可共享的（即另外的执行程序可以调用它），使其可共享的目的是对于频繁被执行的程序，只需要在内存中有一份代码即可。代码区通常是只读的，使其只读的原因是防止程序意外地修改了它的指令。另外，代码区还规划了局部变量的相关信息。

全局初始化数据区 / 静态数据区（data 段）

该区包含了在程序中明确被初始化的全局变量、已经初始化的静态变量（包括全局静态变量和局部静态变量）和常量数据（如字符串常量）。

未初始化数据区（ bss 区）

存入的是全局未初始化变量和未初始化静态变量。未初始化数据区的数据在程序开始执行之前被内核初始化为 0 或者空（NULL）。

代码区和全局区 (data 和 bss) 的大小就是固定的

系统把程序加载到内存，除了根据可执行程序的信息分出代码区（text）、数据区（data）和未初始化数据区（bss）之外，还额外增加了栈区、堆区。

栈区（stack）

栈是一种先进后出的内存结构，由编译器自动分配释放，存放函数的参数值、返回值、局部变量等。在程序运行过程中实时加载和释放，因此，局部变量的生存周期为申请到释放该段栈空间。

堆区（heap）

堆是一个大容器，它的容量要远远大于栈，但没有栈那样先进后出的顺序。用于动态内存分配。堆在内存中位于BSS区和栈区之间。一般由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收。

内存操作函数
memset()
#include <string.h>
void *memset(void *s, int c, size_t n);
功能：将s的内存区域的前n个字节以参数c填入
参数：
    s：需要操作内存s的首地址
    c：填充的字符，c虽然参数为int，但必须是unsigned char , 范围为0~255
    n：指定需要设置的大小
返回值：s的首地址

int a[10];
memset(a, 97, sizeof(a));
int i = 0;
for (i = 0; i < 10; i++)
{
    printf("%c\n", a[i]);
}
memcpy()
#include <string.h>
void *memcpy(void *dest, const void *src, size_t n);
功能：拷贝src所指的内存内容的前n个字节到dest所值的内存地址上。
参数：
    dest：目的内存首地址
    src：源内存首地址，注意：dest和src所指的内存空间不可重叠
    n：需要拷贝的字节数
返回值：dest的首地址

    int a[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
    int b[10];
    
    memcpy(b, a, sizeof(a));
    int i = 0;
    for (i = 0; i < 10; i++)
    {
        printf("%d, ", b[i]);
    }
    printf("\n");
memmove () 功能用法和 memcpy () 一样，区别在于：dest 和 src 所指的内存空间重叠时，memmove () 仍然能处理，不过执行效率比 memcpy () 低些。

memcmp()
#include <string.h>
int memcmp(const void *s1, const void *s2, size_t n);
功能：比较s1和s2所指向内存区域的前n个字节
参数：
    s1：内存首地址1
    s2：内存首地址2
    n：需比较的前n个字节
返回值：
    相等：=0
    大于：>0
    小于：<0

    int a[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
    int b[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
    
    int flag = memcmp(a, b, sizeof(a));
    printf("flag = %d\n", flag);
堆区内存分配和释放
malloc()
#include <stdlib.h>
void *malloc(size_t size);
功能：在内存的动态存储区(堆区)中分配一块长度为size字节的连续区域，用来存放类型说明符指定的类型。分配的内存空间内容不确定，一般使用memset初始化。
参数：
    size：需要分配内存大小(单位：字节)
返回值：
    成功：分配空间的起始地址
    失败：NULL

#include <stdlib.h> 
#include <stdio.h>
#include <string.h>

int main()
{
    int count, *array, n;
    printf("请输入要申请数组的个数:\n");
    scanf("%d", &n);

    array = (int *)malloc(n * sizeof (int));
    //将申请到空间清0
    memset(array, 0, sizeof(int)*n);
    for (count = 0; count < n; count++) /*给数组赋值*/
        array[count] = count;
    
    for (count = 0; count < n; count++) /*打印数组元素*/
        printf("%2d", array[count]);
    //释放array所指向的一块内存空间，对同一内存空间多次释放会出错。
    free(array);
    return 0;
}
复合类型
结构体
默认对齐值：

Linux 默认#pragma pack(4)

window 默认#pragma pack(8)

结构体内存对齐

数组成员对齐规则。第一个数组成员应该放在offset为0的地方，以后每个数组成员应该放在 offset 为 min（当前成员的大小，#pargama pack (n)）整数倍的地方开始。

结构体总的大小 (sizeof 的结果)，必须是 min（结构体内部最大成员，#pargama pack (n)）的整数倍。

如果一个结构体 B 里嵌套另一个结构体 A, 还是以最大成员类型的大小对齐，但是结构体 A 的起点为 A 内部最大成员的整数倍的地方。

struct stu2   {      
    char c;  //1    
    short s;  //2
    int i;   //4
    double d;  //8
}


struct str
{
    char ch1;    //1字节
    char ch2;    //1字节
    char ch3;    //1字节
    char ch4;    //1字节

    int i;       //int占4字节，已经是4的倍数，从偏移4字节开始，到这里一共使用4+4=8字节
    double d;   //double占8字节，从偏移8字节开始，一共使用8+8=16字节。
}
结构体类型和结构体变量关系：

结构体类型：指定了一个结构体类型，相当于一个模型，但其中并无具体数据，系统不分配实际内存单元。

结构体变量：系统根据结构体类型（内部成员状况）为之分配空间。


//结构体类型的定义
struct stu
{
    char name[50];
    int age;
};
//定义类型同时定义变量
struct stu2
{
    char name[50];
    int age;
}s2 = { "lily", 22 };

//先定义类型，再定义变量（常用）struct stu s1 = { "mike", 18 };

int main()
{
    struct stu s1;

    //如果是普通变量，通过点运算符操作结构体成员
    strcpy(s1.name, "abc");
    s1.age = 18;
    printf("s1.name = %s, s1.age = %d\n", s1.name, s1.age);
    
    //如果是指针变量，通过->操作结构体成员
    strcpy((&s1)->name, "test");
    (&s1)->age = 22;
    printf("(&s1)->name = %s, (&s1)->age = %d\n", (&s1)->name, (&s1)->age);
    
    return 0;
}
结构体套结构体

struct person
{
    char name[20];
    char sex;
};

struct stu
{
    int id;
    struct person info;
};
int main()
{
    struct stu s[2] = { 1, "lily", 'F', 2, "yuri", 'M' };

    int i = 0;
    for (i = 0; i < 2; i++)
    {
        printf("id = %d\tinfo.name=%s\tinfo.sex=%c\n", s[i].id, s[i].info.name, s[i].info.sex);
    }
    return 0;
}
注意：相同类型的两个结构体变量，可以相互赋值

结构体和指针

#include<stdio.h>

//结构体类型的定义
struct stu
{
    char name[50];
    int age;
};

int main()
{
    struct stu s1 = { "lily", 18 };
    //如果是指针变量，通过->操作结构体成员
    struct stu *p = &s1;
    printf("p->name = %s, p->age=%d\n", p->name, p->age);
    printf("(*p).name = %s, (*p).age=%d\n",  (*p).name,  (*p).age);
    //堆区结构体变量
    struct stu *p2 = NULL;
    p2 = (struct stu *)malloc(sizeof(struct  stu));
    strcpy(p2->name, "test");
    free(p2);
    return 0;
}
结构体做函数参数

结构体默认为值传递。

#include<stdio.h>
#include <string.h>

//结构体类型的定义
struct stu
{
    char name[50];
    int age;
};
//函数参数为结构体指针变量,
void set_stu_pro(struct stu *tmp)
{
    strcpy(tmp->name, "mike");
    tmp->age = 18;
}
int main()
{
    struct stu s = { 0 };
    set_stu_pro(&s); //地址传递
    printf("s.name = %s, s.age = %d\n", s.name, s.age);
    return 0;
}
共用体 (联合体)
联合 union 是一个能在同一个存储空间存储不同类型数据的类型；

联合体所占的内存长度等于其最长成员的长度，也有叫做共用体；

同一内存段可以用来存放几种不同类型的成员，但每一瞬时只有一种起作用；

共用体变量的地址和它的各成员的地址都是同一地址。

#include <stdio.h>

//共用体也叫联合体 
union Test
{
    unsigned char a;
    unsigned int b;
    unsigned short c;
};

int main()
{
    //定义共用体变量
    union Test tmp;

    //1、所有成员的首地址是一样的
    printf("%p, %p, %p\n", &(tmp.a), &(tmp.b), &(tmp.c));
    
    //2、共用体大小为最大成员类型的大小
    printf("%lu\n", sizeof(union Test));
    
    //3、一个成员赋值，会影响另外的成员
    //左边是高位，右边是低位
    //低位放低地址，高位放高地址
    tmp.b = 0x44332211;
    
    printf("%x\n", tmp.a); //11
    printf("%x\n", tmp.c); //2211
    return 0;
}
枚举
枚举值是常量，不能在程序中用赋值语句再对它赋值。

举元素本身由系统定义了一个表示序号的数值从 0 开始顺序定义为 0，1，2 …

#include <stdio.h>
enum weekday
{
    sun = 2, mon, tue, wed, thu, fri, sat
};

enum bool
{
    flase, true
};

int main()
{
    enum weekday a, b, c;
    a = sun;
    b = mon;
    c = tue;
    printf("%d,%d,%d\n", a, b, c);//2,3,4


    enum bool flag;
    flag = true;
    
    if (flag == 1)
    {
        printf("flag为真\n");//flag为真
    }
    return 0;
}

typedef
typedef 为 C 语言的关键字，作用是为一种数据类型 (基本类型或自定义数据类型) 定义一个新名字，不能创建新类型。

#include <stdio.h>

typedef int INT;
typedef char T_BYTE;

typedef struct type
{
    INT a;
    T_BYTE c;
}TYPE, *PTYPE;

int main()
{
    TYPE t;
    t.a = 254;
    t.c = 'c';
    PTYPE p = &t;
    printf("%u,%c\n", p->a, p->c);//254,c
    return 0;
}
文件操作
从用户或者操作系统使用的角度（逻辑上）把文件分为：

文本文件

基于字符编码，常见编码有 ASCII、UNICODE 等

一般可以使用文本编辑器直接打开

二进制文件

基于值编码，自己根据具体应用，指定某个值是什么意思

把内存中的数据按其在内存中的存储形式原样输出到磁盘上

在 C 语言中用一个指针变量指向一个文件，这个指针称为文件指针。

C 语言中有三个特殊的文件指针由系统默认打开，用户无需定义即可直接使用:

stdin： 标准输入，默认为当前终端（键盘），我们使用的 scanf、getchar 函数默认从此终端获得数据。

stdout：标准输出，默认为当前终端（屏幕），我们使用的 printf、puts 函数默认输出信息到此终端。

stderr：标准出错，默认为当前终端（屏幕），我们使用的 perror 函数默认输出信息到此终端。

打开文件
#include <stdio.h>
FILE * fopen(const char * filename, const char * mode);
功能：打开文件
参数：
    filename：需要打开的文件名，根据需要加上路径
    mode：打开文件的模式设置
返回值：
    成功：文件指针
    失败：NULL
    FILE *fp_passwd = NULL;

    //打开当前目录passdw文件：源文件(源程序)所在目录
    FILE *fp_passwd = fopen("passwd.txt", "r");
    
    //绝对路径：
    //打开C盘test目录下一个叫passwd.txt文件
    FILE *fp_passwd  = fopen("c://test//passwd.txt","r");
打开模式	含义
r 或 rb	以只读方式打开一个文本文件（不创建文件，若文件不存在则报错）
w 或 wb	以写方式打开文件 (如果文件存在则清空文件，文件不存在则创建一个文件)
a 或 ab	以追加方式打开文件，在末尾添加内容，若文件不存在则创建文件
r + 或 rb+	以可读、可写的方式打开文件 (不创建新文件)
w + 或 wb+	以可读、可写的方式打开文件 (如果文件存在则清空文件，文件不存在则创建一个文件)
a + 或 ab+	以添加方式打开文件，打开文件并在末尾更改文件，若文件不存在则创建文件
b 是二进制模式的意思，b 只是在 Windows 有效，在 Linux 用 r 和 rb 的结果是一样的

Unix 和 Linux 下所有的文本文件行都是 \n 结尾，而 Windows 所有的文本文件行都是 \r\n 结尾

文件的关闭
#include <stdio.h>
int fclose(FILE * stream);
功能：关闭先前fopen()打开的文件。此动作让缓冲区的数据写入文件中，并释放系统所提供的文件资源。
参数：
    stream：文件指针
返回值：
    成功：0
    失败：-1

    FILE * fp = NULL;
    fp = fopen("abc.txt", "r");
    fclose(fp);
文件的顺序读写
按照字符读写文件 fgetc、fputc
#include <stdio.h>
#include <string.h>

int main()
{
    char buf[] = "this is a test for fputc";
    int i = 0;
    int n = strlen(buf);
    FILE *fp = NULL;
    fp = fopen("abc.txt", "w");
    for (i = 0; i < n; i++)
    {
        //往文件fp写入字符buf[i]
        int ch = fputc(buf[i], fp);
        printf("ch = %c\n", ch);
    }
    fclose(fp);
}
在 C 语言中，EOF 表示文件结束符 (end of file)。

#define EOF (-1)

在文本文件中，数据都是以字符的 ASCII 代码值的形式存放。我们知道，ASCII 代码值的范围是 0~127，不可能出现 - 1，因此可以用 EOF 作为文件结束标志。

#include <stdio.h>
#include <string.h>

int main()
{
    char ch;
    FILE *fp = NULL;
    fp = fopen("abc.txt", "r");
    while (!feof(fp)) //文件没有结束，则执行循环
    {
        ch = fgetc(fp);
        printf("%c", ch);
    }
    printf("\n");
    fclose(fp);
}
按照行读写文件 fgets、fputs
char * fgets(char * str, int size, FILE * stream);
功能：从stream指定的文件内读入字符，保存到str所指定的内存空间，直到出现换行字符、读到文件结尾或是已读了size - 1个字符为止，最后会自动加上字符 '\0' 作为字符串结束。
参数：
    str：字符串
    size：指定最大读取字符串的长度（size - 1）
    stream：文件指针
返回值：
    成功：成功读取的字符串

#include <stdio.h>
int fputs(const char * str, FILE * stream);
功能：将str所指定的字符串写入到stream指定的文件中，字符串结束符'\0' 不写入文件。 
参数：
    str：字符串
    stream：文件指针
返回值：
    成功：0
    失败：-1
读写例子：

#include <stdio.h>
#include <string.h>

int main()
{
    char buf[100] = {0};
    FILE *read = fopen("abc.txt", "r");
    FILE *write = fopen("abcd.txt", "w");
    while (!feof(read)) //文件没有结束
    {
        memset(buf, 0, sizeof(buf));
        char *p = fgets(buf, sizeof(buf), read);
        if (p != NULL)
        {
            fputs(buf, write);
        }
    }
}
按照格式化文件 fprintf、fscanf
//写文件,指定出现字符串结束符 '\0' 为止
fprintf(fp, "%d %d %d\n", 1, 2, 3);
//读文件,从stream指定的文件读取字符串，并根据参数format字符串来转换并格式化数据
fscanf(fp, "%d %d %d\n", &a, &b, &c);
按照块读写文件 fread、fwrite
1）写文件
#include <stdio.h>
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
功能：以数据块的方式给文件写入内容
参数：
    ptr：准备写入文件数据的地址
    size： size_t 为 unsigned int类型，此参数指定写入文件内容的块数据大小
    nmemb：写入文件的块数，写入文件数据总大小为：size * nmemb
    stream：已经打开的文件指针
返回值：
    成功：实际成功写入文件数据的块数目，此值和nmemb相等
    失败：0


typedef struct Stu
{
    char name[50];
    int id;
}Stu;

Stu s[3];
int i = 0;
for (i = 0; i < 3; i++)
{
    sprintf(s[i].name, "stu%d%d%d", i, i, i);
    s[i].id = i + 1;
}

int ret = fwrite(s, sizeof(Stu), 3, fp);
printf("ret = %d\n", ret);

2）读文件
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
功能：以数据块的方式从文件中读取内容
参数：
    ptr：存放读取出来数据的内存空间
    size： size_t 为 unsigned int类型，此参数指定读取文件内容的块数据大小
    nmemb：读取文件的块数，读取文件数据总大小为：size * nmemb
    stream：已经打开的文件指针
返回值：
    成功：实际成功读取到内容的块数，如果此值比nmemb小，但大于0，说明读到文件的结尾。
    失败：0

typedef struct Stu
{
    char name[50];
    int id;
}Stu;

Stu s[3];
int ret = fread(s, sizeof(Stu), 3, fp);
printf("ret = %d\n", ret);

int i = 0;
for (i = 0; i < 3; i++)
{
    printf("s = %s, %d\n", s[i].name, s[i].id)
}
文件的随机读写
int fseek(FILE *stream, long offset, int whence);
功能：移动文件流（文件光标）的读写位置。
参数：
    stream：已经打开的文件指针
    offset：根据whence来移动的位移数（偏移量），可以是正数，也可以负数，如果正数，则相对于whence往右移动，如果是负数，则相对于whence往左移动。如果向后移动的字节数超过了文件末尾，再次写入时将增大文件尺寸。
    whence：其取值如下：
        SEEK_SET：从文件开头移动offset个字节
        SEEK_CUR：从当前位置移动offset个字节
        SEEK_END：从文件末尾移动offset个字节
返回值：
    成功：0
    失败：-1
long ftell(FILE *stream);
功能：获取文件流（文件光标）的读写位置。
参数：
    stream：已经打开的文件指针
返回值：
    成功：当前文件流（文件光标）的读写位置
    失败：-1
void rewind(FILE *stream);
功能：把文件流（文件光标）的读写位置移动到文件开头。
参数：
    stream：已经打开的文件指针
返回值：
    无返回值
typedef struct Stu
{
    char name[50];
    int id;
}Stu;

Stu s[3];
Stu tmp; 
int ret = 0;

//假如已经往文件写入3个结构体
//fwrite(s, sizeof(Stu), 3, fp);

//文件光标读写位置从开头往右移动2个结构体的位置
fseek(fp, 2 * sizeof(Stu), SEEK_SET);

//读第3个结构体
ret = fread(&tmp, sizeof(Stu), 1, fp);
if (ret == 1)
{
    printf("[tmp]%s, %d\n", tmp.name, tmp.id);
}

//把文件光标移动到文件开头,等同于fseek(fp, 0, SEEK_SET);
rewind(fp);

ret = fread(s, sizeof(Stu), 3, fp);
printf("ret = %d\n", ret);

int i = 0;
for (i = 0; i < 3; i++)
{
    printf("s === %s, %d\n", s[i].name, s[i].id);
}
其他函数
int remove(const char *pathname);
功能：删除文件
参数：
    pathname：文件名
返回值：
    成功：0
    失败：-1


int rename(const char *oldpath, const char *newpath);
功能：把oldpath的文件名改为newpath
参数：
    oldpath：旧文件名
    newpath：新文件名
返回值：
    成功：0
    失败： - 1


#include <sys/types.h>
#include <sys/stat.h>
int stat(const char *path, struct stat *buf);
功能：获取文件状态信息
参数：
    path：文件名
    buf：保存文件信息的结构体
返回值：
    成功：0
    失败-1

文件缓冲区
所谓缓冲文件系统是指系统自动地在内存区为程序中每一个正在使用的文件开辟一个文件缓冲区从内存向磁盘输出数据必须先送到内存中的缓冲区，装满缓冲区后才一起送到磁盘去。

#include <stdio.h>
int fflush(FILE *stream);
功能：更新缓冲区，让缓冲区的数据立马写到文件中。
参数：
    stream：文件指针
返回值：
    成功：0
    失败：-1

————————————————
原文作者：rufo
转自链接：https://learnku.com/articles/45953
版权声明：著作权归作者所有。商业转载请联系作者获得授权，非商业转载请保留以上作者信息和原文链接。