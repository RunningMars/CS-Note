## 标识符

标识符区分大小写。

单行注释：//注释内容

多行注释：/* 注释内容 */

## 数据类型

<img src="C note rdg.assets/164326dlzlrmeyddbu7ukm.png" alt="img" style="zoom: 67%;" />

<img src="C note rdg.assets/165849up9oyaog0p7q6666.png" alt="img" style="zoom:67%;" />

char 字符型，占用一个字节
int 整型，通常反映了所用机器中整数的最自然长度
float 单精度浮点型
double 双精度浮点型 

这里 C 语言并没有限制 int 的坑具体要挖多大，short int 或 long int 的坑又要挖多大。标准只是要求：short int <= int <= long int <= long long int。

sizeof 运算符

sizeof 用于获得数据类型或表达式的长度，它有三种使用方式：

- sizeof(type_name); //sizeof(类型);
- sizeof(object); //sizeof(对象);
- sizeof object; //sizeof 对象;

signed 和 unsigned

还有一对类型限定符是 signed 和 unsigned，它们用于限定 char 类型和任何整型变量的取值范围。

signed 表示该变量是带符号位的，而 unsigned 表示该变量是不带符号位的。带符号位的变量可以表示负数，而不带符号位的变量只能表示正数，它的存储空间也就相应扩大一倍。默认所有的整型变量都是 signed 的，也就是带符号位的。

因此加上 signed 和 unsigned 限定符，四种整型就变成了八种：

- [signed] short [int]
- unsigned short [int]
- [signed] int
- unsigned int
- [signed] long [int]
- unsigned long [int]
- [signed] long long [int]
- unsigned long long [int]

## 常量

```c
#define URL "http://www.iappsasia.com"
#define YEAR 2022
```

其中这个 #define 是一条预处理命令（预处理命令都以"#"开头），我们也称为宏定义命令。它的功能就是把程序中所有出现的标识符都替换为随后的常量。

`#` 预处理命令

字符串常量

C 语言用一个特殊的转义字符来表示字符串的结束位置。这样当操作系统读取到这个转移字符的时候，就知道该字符串到此为止了。

这个转义字符就是空字符：'\0'



