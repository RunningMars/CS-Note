



### 总览

所有的函数参数传递,都是值传递, 拷贝副本;  

Go 推荐使用驼峰命名

如果传递的是指针,拷贝一个指针的值, 指针指向相同的地址, 同样可以达到引用修改的效果;

Go 在语法上有着类 C 的感觉。如果你希望将两个（或更多）语句放在一行书写，它们必须用分号 (’;’) 分隔。一般情况下，你不需要分号。

Go 同其他语言不同的地方在于变量的类型在变量名的后面。不是：int a，而是 a int。

变量声明了没有使用, 也会发生编译错误

函数传参都是值拷贝 , 所以大多情况需要传递指针

Go 语言提供了, channel 通信通道 , 可以保障goroutine协程之间间通信(妙哉妙哉)

Go 语言提供了, 互斥锁和读写锁 , 可以保障goroutine协程运行安全 (数据竟态问题)

Go 语言提供了, 可直接使用的协程安全的 map , 和 Java (HasMap 线程安全版)类似 , 而基本类型（int32、uint32、int64、uint64）,提供了原子操作

go 语言中,map 是一个使用 make 创建的数据结构的引用, 因为当需要扩容时, 底层的地址会发生变化 , 所以 map 本身是一个引用, 用来隐藏底层地址的变化;

<img src="GoNote2.assets/image-20250213下午24801772.png" alt="image-20250213下午24801772" style="zoom:50%;" />





#### 格式化占位符

<img src="GoNote2.assets/image-20250213下午14046182.png" alt="image-20250213下午14046182" style="zoom:50%;" />

在 Go 语言的 `fmt.Printf` 中，格式化占位符（如 `%d`、`%s`）的设计**直接继承自 C 语言的 `printf` 函数**，这些简写的命名规则源于早期编程语言的历史约定。以下是具体解释：

------

**一、核心简写的词源**

| 占位符 | 全称/来源                 | 示例                    | 设计逻辑                  |
| ------ | ------------------------- | ----------------------- | ------------------------- |
| `%d`   | **D**ecimal integer       | `%d` → 十进制整数       | "decimal" 的首字母 `d`    |
| `%s`   | **S**tring                | `%s` → 字符串           | "string" 的首字母 `s`     |
| `%f`   | **F**loating-point        | `%f` → 浮点数           | "float" 的首字母 `f`      |
| `%o`   | **O**ctal integer         | `%o` → 八进制整数       | "octal" 的首字母 `o`      |
| `%x`   | He**x**adecimal integer   | `%x` → 十六进制         | "hex" 的末尾字母 `x`      |
| `%c`   | **C**haracter             | `%c` → 单个字符         | "char" 的首字母 `c`       |
| `%p`   | **P**ointer address       | `%p` → 指针地址         | "pointer" 的首字母 `p`    |
| `%t`   | **T**ruth value (boolean) | `%t` → 布尔值           | "truth" 的首字母 `t`      |
| `%e`   | **E**xponential notation  | `%e` → 科学计数法       | "exponent" 的首字母 `e`   |
| `%g`   | **G**eneral float         | `%g` → 自动选择浮点格式 | 可能是 "general" 的首字母 |

---



### 变量

在函数内部，可以使用更简略的 `:=` 方式声明并初始化变量。

在外部, 例如 main.go 中 无法使用短变量声明 x:=1

变量声明以关键字`var`开头，变量类型放在变量的后面，行尾无需分号。 举个例子：

```go
var name string
var age int
var isOk bool
```

##### 短变量声明:

左边也必须声明新的变量声明才能使用短变量声明,;

左边可以声明多个变量,如果在局部变量中,已存在同名的变量,则只赋值,不声明新的同名变量;

```go
var user User
user,error := getUser()

// 短变量声明 := 左边可以声明多个变量,如果在局部变量中,已存在同名的变量,则只赋值,不声明新的同名变量;
// 但是 := 左边也必须声明新的变量声明才能使用短变量声明


```

---



#### 多重赋值:

<img src="GoNote2.assets/image-20250214下午42525031.png" alt="image-20250214下午42525031" style="zoom:50%;" />

互换两个变量的值



可赋值性

<img src="GoNote2.assets/image-20250214下午42709620.png" alt="image-20250214下午42709620" style="zoom: 50%;" />



---



### 常量

相对于变量，常量是恒定不变的值，多用于定义程序运行期间不会改变的那些值。 常量的声明和变量声明非常类似，只是把`var`换成了`const`，常量在定义的时候必须赋值。



`iota`是go语言的常量计数器，只能在常量的表达式中使用。

`iota`在const关键字出现时将被重置为0。const中每新增一行常量声明将使`iota`计数一次(iota可理解为const语句块中的行索引)。 使用iota能简化定义，在定义枚举时很有用。

举个例子：

```go
const (
		n1 = iota //0
		n2        //1
		n3        //2
		n4        //3
	)
```

几个常见的`iota`示例:

使用`_`跳过某些值

```go
const (
		n1 = iota //0
		n2        //1
		_
		n4        //3
	)
```

`iota`声明中间插队

```go
const (
		n1 = iota //0
		n2 = 100  //100
		n3 = iota //2
		n4        //3
	)
	const n5 = iota //0
```

定义数量级 （这里的`<<`表示左移操作，`1<<10`表示将1的二进制表示向左移10位，也就是由`1`变成了`10000000000`，也就是十进制的1024。同理`2<<2`表示将2的二进制表示向左移2位，也就是由`10`变成了`1000`，也就是十进制的8。）

```go
const (
		_  = iota
		KB = 1 << (10 * iota)
		MB = 1 << (10 * iota)
		GB = 1 << (10 * iota)
		TB = 1 << (10 * iota)
		PB = 1 << (10 * iota)
	)
```

多个`iota`定义在一行

```go
const (
		a, b = iota + 1, iota + 2 //1,2
		c, d                      //2,3
		e, f                      //3,4
	)
```







#### 类型的声明:

<img src="../../../../go_program_note.assets/image-20240715下午24056011.png" alt="image-20240715下午24056011" style="zoom:50%;" />

<img src="GoNote2.assets/image-20240715下午24217468.png" alt="image-20240715下午24217468" style="zoom:50%;" />

##### 要比较两个变量 , 需要它俩都是相同的底层类型, 

命名类型 与 非命名类型比较 需要底层是相同的数据类型

命名类型 与 命名类型比较 智能相同的命名类型之间比较, 即使是底层数据类型相同也会编译报错: 类型不匹配 

---



#### 作用域



<img src="GoNote2.assets/image-20240717上午102626186.png" alt="image-20240717上午102626186" style="zoom:50%;" />

##### 代码块中声明的变量,它的作用域 ,仅在代码块中; 





<img src="GoNote2.assets/image-20240717上午111539756.png" alt="image-20240717上午111539756" style="zoom:50%;" />

<img src="../../../../go_program_note.assets/image-20240717上午111556392.png" alt="image-20240717上午111556392" style="zoom:50%;" />

##### 	局部声明一个变量 , 可以与全局存在的变量同名, 但会使局部优先使用局部自身的变量 ,而不会去使用全局变量;

##### 	 如果要使用全局变量 , 在局部中不使用声明 , 直接赋值;







---

### 流程控制

for 循环

switch 流程



break 只跳出一层

continue 跳过当次循环

return 跳出所有,直接结束当前方法



for循环的初始语句可以被忽略，但是初始语句后的分号必须要写，例如：

```go
func forDemo2() {
	i := 0
	for ; i < 10; i++ {
		fmt.Println(i)
	}
}
```

for循环的初始语句和结束语句都可以省略，例如：

```go
func forDemo3() {
	i := 0
	for i < 10 {
		fmt.Println(i)
		i++
	}
}
```

这种写法类似于其他编程语言中的`while`，在`while`后添加一个条件表达式，满足条件表达式时持续循环，否则结束循环。

#### 无限循环

```go
for {
    循环体语句
}
```

for循环可以通过`break`、`goto`、`return`、`panic`语句强制退出循环。

#### for range(键值循环)

Go语言中可以使用`for range`遍历数组、切片、字符串、map 及通道（channel）。 通过`for range`遍历的返回值有以下规律：

1. 数组、切片、字符串返回索引和值。
2. map返回键和值。
3. 通道（channel）只返回通道内的值。

Go1.22版本开始支持 for range 整数。👉 [Go 1.22 Release Notes](https://go.dev/doc/go1.22)。

```go
for i := range 5 {
	fmt.Println(i)
}

for range 2 {
	fmt.Println("《Go语言之路》上市啦！")
}
```

输出：

```bash
0
1
2
3
4
《Go语言之路》上市啦！
《Go语言之路》上市啦！
```



#### switch

`fallthrough`语法可以执行满足条件的case的下一个case，是为了兼容C语言中的case设计的。

```go
func switchDemo5() {
	s := "a"
	switch {
	case s == "a":
		fmt.Println("a")
		fallthrough
	case s == "b":
		fmt.Println("b")
	case s == "c":
		fmt.Println("c")
	default:
		fmt.Println("...")
	}
}
```

输出：

```bash
a
b
```

---



### 基本类型

Go语言中有丰富的数据类型，除了基本的整型、浮点型、布尔型、字符串外， 还有数组、切片、结构体、函数、map、通道（channel）等。Go 语言的基本类型和其他语言大同小异。

#### 整型:

int8    int16    int32    int64
uint8  uint16  uint32  uint64

1个字节  到  8 个字节

go 中的方法入参变量是否可以设置为 int 类型 , 如果设置为 int 类型, 那么传入的变量类型可以是哪些? 

在 Go 语言中，方法的入参变量可以设置为 int 类型。int 是一个平台相关的类型，其大小依赖于当前操作系统架构：

​	•	在 32 位系统上，int 是 int32。

​	•	在 64 位系统上，int 是 int64。

**传入的变量类型**

当你定义一个方法的入参为 int 时，传入的变量必须是 int 类型（或者是一个可以隐式转换为 int 的类型）。如果你试图传递其他整数类型（例如 int32 或 int64），Go 会尝试进行类型转换。如果类型不匹配且无法自动转换，编译器会报错。

1. **`int` 参数**：
   - 可接受 `int` 类型变量
   - 其他类型需显式转换
   - 编译时通过 `GOARCH` 控制位数
2. **短声明变量**：
   - 默认为 `int`
   - 传递到不同位数函数需显式转换
3. **工程建议**：
   - 关键路径避免依赖 `int` 的位数
   - 协议/存储相关场景强制使用 `int32`/`int64`

通过遵循这些规则，可以确保代码在不同平台下的类型安全性和一致性。



#### 浮点型:

float32    float64



#### map

go 语言中,map 是一个使用 make 创建的数据结构的引用, 因为当需要扩容时, 底层的地址会发生变化 , 所以 map 本身是一个引用, 用来隐藏底层地址的变化;



#### byte和rune类型

组成每个字符串的元素叫做“字符”，可以通过遍历或者单个获取字符串元素获得字符。 字符用单引号（’）包裹起来，如：

```go
var a = '中'
var b = 'x'
```

Go 语言的字符有以下两种：

1. `uint8`类型，或者叫 byte 型，代表一个`ASCII码`字符。
2. `rune`类型，代表一个 `UTF-8字符`。

当需要处理中文、日文或者其他复合字符时，则需要用到`rune`类型。`rune`类型实际是一个`int32`。

Go 使用了特殊的 rune 类型来处理 Unicode，让基于 Unicode 的文本处理更为方便，也可以使用 byte 型进行默认字符串处理，性能和扩展性都有照顾。

```go
// 遍历字符串
func traversalString() {
	s := "hello沙河"
	for i := 0; i < len(s); i++ { //byte
		fmt.Printf("%v(%c) ", s[i], s[i])
	}
	fmt.Println()
	for _, r := range s { //rune
		fmt.Printf("%v(%c) ", r, r)
	}
	fmt.Println()
}
```

输出：

```bash
104(h) 101(e) 108(l) 108(l) 111(o) 230(æ) 178(²) 153() 230(æ) 178(²) 179(³) 
104(h) 101(e) 108(l) 108(l) 111(o) 27801(沙) 27827(河) 
```

因为UTF8编码下一个中文汉字由3~4个字节组成，所以我们不能简单的按照字节去遍历一个包含中文的字符串，否则就会出现上面输出中第一行的结果。

字符串底层是一个byte数组，所以可以和`[]byte`类型相互转换。字符串是不能修改的 字符串是由byte字节组成，所以字符串的长度是byte字节的长度。 rune类型用来表示utf8字符，一个rune字符由一个或多个byte组成。

底层:

```go
// rune is an alias for int32 and is equivalent to int32 in all ways. It is
// used, by convention, to distinguish character values from integer values.
type rune = int32
```



#### 修改字符串

要修改字符串，需要先将其转换成`[]rune`或`[]byte`，完成后再转换为`string`。无论哪种转换，都会重新分配内存，并复制字节数组。

```go
func changeString() {
	s1 := "big"
	// 强制类型转换
	byteS1 := []byte(s1)
	byteS1[0] = 'p'
	fmt.Println(string(byteS1))

	s2 := "白萝卜"
	runeS2 := []rune(s2)
	runeS2[0] = '红'
	fmt.Println(string(runeS2))
}
```

---





### 数组

GO的数组是基本类型，不是引用类型；

数组是同一种数据类型元素的集合。 在Go语言中，数组从声明时就确定，使用时可以修改数组成员，但是数组大小不可变化。

#### **数组的定义**

比如：`var a [5]int`， 数组的长度必须是常量，并且长度是数组类型的一部分。一旦定义，长度不能变。 `[5]int`和`[10]int`是不同的类型。

#### **声明数组和声明切片要注意**：

```go
//声明切片，无需指定长度
var s []int
//声明数组，必须指明长度
var a [3]int 
fmt.Printf("%T",s)
fmt.Println("")
fmt.Printf("%T",a)
```

```go
var a [3]int
fmt.println(a)
```



<img src="GoNote2.assets/image-20250224上午104307430.png" alt="image-20250224上午104307430" style="zoom:50%;" />

##### 如果一个数组的元素是可比较的,那这个数组也是可比较的

<img src="GoNote2.assets/image-20250224上午104539636.png" alt="image-20250224上午104539636" style="zoom:50%;" />

***

### 切片（Slice）

切片是一个拥有相同类型元素的可变长度的序列。它是基于数组类型做的一层封装。它非常灵活，支持自动扩容。
切片是一个引用类型，它的内部结构包含`地址`、`长度`和`容量`。切片一般用于快速地操作一块数据集合。

#### 切片的赋值拷贝

下面的代码中演示了拷贝前后两个变量共享底层数组，对一个切片的修改会影响另一个切片的内容，这点需要特别注意。

```go
func main() {
	s1 := make([]int, 3) //[0 0 0]
	s2 := s1             //将s1直接赋值给s2，s1和s2共用一个底层数组
	s2[0] = 100
	fmt.Println(s1) //[100 0 0]
	fmt.Println(s2) //[100 0 0]
}
```

#### 切片的扩容策略

可以通过查看`$GOROOT/src/runtime/slice.go`源码，其中扩容相关代码如下：

```go
newcap := old.cap
doublecap := newcap + newcap
if cap > doublecap {
	newcap = cap
} else {
	if old.len < 1024 {
		newcap = doublecap
	} else {
		// Check 0 < newcap to detect overflow
		// and prevent an infinite loop.
		for 0 < newcap && newcap < cap {
			newcap += newcap / 4
		}
		// Set newcap to the requested cap when
		// the newcap calculation overflowed.
		if newcap <= 0 {
			newcap = cap
		}
	}
}
```

从上面的代码可以看出以下内容：

- 首先判断，如果新申请容量（cap）大于2倍的旧容量（old.cap），最终容量（newcap）就是新申请的容量（cap）。
- 否则判断，如果旧切片的长度小于1024，则最终容量(newcap)就是旧容量(old.cap)的两倍，即（newcap=doublecap），
- 否则判断，如果旧切片长度大于等于1024，则最终容量（newcap）从旧容量（old.cap）开始循环增加原来的1/4，即（newcap=old.cap,for {newcap += newcap/4}）直到最终容量（newcap）大于等于新申请的容量(cap)，即（newcap >= cap）
- 如果最终容量（cap）计算值溢出，则最终容量（cap）就是新申请容量（cap）。

需要注意的是，切片扩容还会根据切片中元素的类型不同而做不同的处理，比如`int`和`string`类型的处理方式就不一样。使用copy()函数复制切片

首先我们来看一个问题：

```go
func main() {
	a := []int{1, 2, 3, 4, 5}
	b := a
	fmt.Println(a) //[1 2 3 4 5]
	fmt.Println(b) //[1 2 3 4 5]
	b[0] = 1000
	fmt.Println(a) //[1000 2 3 4 5]
	fmt.Println(b) //[1000 2 3 4 5]
}
```

由于切片是引用类型，所以a和b其实都指向了同一块内存地址。修改b的同时a的值也会发生变化。

Go语言内建的`copy()`函数可以迅速地将一个切片的数据复制到另外一个切片空间中，`copy()`函数的使用格式如下：

```bash
copy(destSlice, srcSlice []T)
```

其中：

- srcSlice: 数据来源切片
- destSlice: 目标切片

举个例子：

```go
func main() {
	// copy()复制切片
	a := []int{1, 2, 3, 4, 5}
	c := make([]int, 5, 5)
	copy(c, a)     //使用copy()函数将切片a中的元素复制到切片c
	fmt.Println(a) //[1 2 3 4 5]
	fmt.Println(c) //[1 2 3 4 5]
	c[0] = 1000
	fmt.Println(a) //[1 2 3 4 5]
	fmt.Println(c) //[1000 2 3 4 5]
}
```



左边包含 , 右边不包含  , 取切片[n:m]

![image-20250219下午32707485](GoNote2.assets/image-20250219下午32707485.png)



##### 切片底层的数组不足时, 动态扩容



##### 和 Java ArrayList 很相似的动态扩容机制



#### **slice 切片不能相互作对比**

<img src="GoNote2.assets/image-20250224上午105705193.png" alt="image-20250224上午105705193" style="zoom:50%;" />

<img src="GoNote2.assets/image-20250224上午105442184.png" alt="image-20250224上午105442184" style="zoom:50%;" />

#### append 函数

<img src="GoNote2.assets/image-20250224上午105833005.png" alt="image-20250224上午105833005" style="zoom:50%;" />

---



### map



go 语言中,map 是一个使用 make 创建的数据结构的引用

<img src="GoNote2.assets/image-20240714上午105151317.png" alt="image-20240714上午105151317" style="zoom:50%;" />



map是一种无序的基于`key-value`的数据结构，Go语言中的map是引用类型，必须初始化才能使用。
map类型的变量默认初始值为nil，需要使用make()函数来分配内存。语法为：

```go
make(map[KeyType]ValueType, [cap])
```



当 map 为 nil 时 , 可以访问它的键但得到的是对应类型的零值;

当 map 为 nil 时 , 设置键值对会报错 , 因为还没有初始化内存;



#### 判断某个键是否存在

Go语言中有个判断map中键是否存在的特殊写法，格式如下:

```go
value, ok := map[key]
```

举个例子：

```go
func main() {
	scoreMap := make(map[string]int)
	scoreMap["张三"] = 90
	scoreMap["小明"] = 100
	// 如果key存在ok为true,v为对应的值；不存在ok为false,v为值类型的零值
	v, ok := scoreMap["张三"]
	if ok {
		fmt.Println(v)
	} else {
		fmt.Println("查无此人")
	}
}
```



**map 的 key 必须是可比较的 , 在 map 中是唯一的**

<img src="GoNote2.assets/image-20250224上午110949350.png" alt="image-20250224上午110949350" style="zoom:50%;" />

<img src="GoNote2.assets/image-20250224上午110938607.png" alt="image-20250224上午110938607" style="zoom:50%;" />



**循环 map 中的 key , 每一次都是无序随机的**

<img src="GoNote2.assets/image-20250224上午111333443.png" alt="image-20250224上午111333443" style="zoom:50%;" />



**map 和 slice 一样不可以相互比较, map 可以和 nil 做比较**

<img src="GoNote2.assets/image-20250224上午111416932.png" alt="image-20250224上午111416932" style="zoom:50%;" />

---



### 结构体

Go语言中没有“类”的概念，也不支持“类”的继承等面向对象的概念。Go语言中通过结构体的内嵌再配合接口比面向对象具有更高的扩展性和灵活性。

#### 自定义类型

在Go语言中有一些基本的数据类型，如`string`、`整型`、`浮点型`、`布尔`等数据类型， Go语言中可以使用`type`关键字来定义自定义类型。

自定义类型是定义了一个全新的类型。我们可以基于内置的基本类型定义，也可以通过struct定义。例如：

```go
//将MyInt定义为int类型
type MyInt int
```

通过`type`关键字的定义，`MyInt`就是一种新的类型，它具有`int`的特性。

#### 类型定义和类型别名的区别

类型别名与类型定义表面上看只有一个等号的差异，我们通过下面的这段代码来理解它们之间的区别。

```go
//类型定义
type NewInt int

//类型别名
type MyInt = int

func main() {
	var a NewInt
	var b MyInt
	
	fmt.Printf("type of a:%T\n", a) //type of a:main.NewInt
	fmt.Printf("type of b:%T\n", b) //type of b:int
}
```

结果显示a的类型是`main.NewInt`，表示main包下定义的`NewInt`类型。b的类型是`int`。`MyInt`类型只会在代码中存在，编译完成时并不会有`MyInt`类型。

#### 结构体

Go语言中的基础数据类型可以表示一些事物的基本属性，但是当我们想表达一个事物的全部或部分属性时，这时候再用单一的基本数据类型明显就无法满足需求了，Go语言提供了一种自定义数据类型，可以封装多个基本数据类型，这种数据类型叫结构体，英文名称`struct`。 也就是我们可以通过`struct`来定义自己的类型了。

Go语言中通过`struct`来实现面向对象。

#### 结构体的定义

使用`type`和`struct`关键字来定义结构体，具体代码格式如下：

```go
type 类型名 struct {
    字段名 字段类型
    字段名 字段类型
    …
}
```

其中：

- 类型名：标识自定义结构体的名称，在同一个包内不能重复。
- 字段名：表示结构体字段名。结构体中的字段名必须唯一。
- 字段类型：表示结构体字段的具体类型。

举个例子，我们定义一个`Person`（人）结构体，代码如下：

```go
type person struct {
	name string
	city string
	age  int8
}
```

同样类型的字段也可以写在一行，

```go
type person1 struct {
	name, city string
	age        int8
}
```

#### 创建指针类型结构体

我们还可以通过使用`new`关键字对结构体进行实例化，得到的是结构体的地址。 格式如下：

```go
var p2 = new(person)
fmt.Printf("%T\n", p2)     //*main.person
fmt.Printf("p2=%#v\n", p2) //p2=&main.person{name:"", city:"", age:0}
```

从打印的结果中我们可以看出`p2`是一个结构体指针。



#### 结构体成员变量不能是它自己, 但可以是它自己类型的指针

<img src="GoNote2.assets/image-20250224上午113814767.png" alt="image-20250224上午113814767" style="zoom:50%;" />



#### 方法和接收者

Go语言中的`方法（Method）`是一种作用于特定类型变量的函数。这种特定类型变量叫做`接收者（Receiver）`。接收者的概念就类似于其他语言中的`this`或者 `self`。

方法的定义格式如下：

```go
func (接收者变量 接收者类型) 方法名(参数列表) (返回参数) {
    函数体
}
```

其中，

- 接收者变量：接收者中的参数变量名在命名时，官方建议使用接收者类型名称首字母的小写，而不是`self`、`this`之类的命名。例如，`Person`类型的接收者变量应该命名为 `p`，`Connector`类型的接收者变量应该命名为`c`等。
- 接收者类型：接收者类型和参数类似，可以是指针类型和非指针类型。
- 方法名、参数列表、返回参数：具体格式与函数定义相同。

举个例子：

```go
//Person 结构体
type Person struct {
	name string
	age  int8
}

//NewPerson 构造函数
func NewPerson(name string, age int8) *Person {
	return &Person{
		name: name,
		age:  age,
	}
}

//Dream Person做梦的方法
func (p Person) Dream() {
	fmt.Printf("%s的梦想是学好Go语言！\n", p.name)
}

func main() {
	p1 := NewPerson("小王子", 25)
	p1.Dream()
}
```

方法与函数的区别是，函数不属于任何类型，方法属于特定的类型。

#### 指针类型的接收者

指针类型的接收者由一个结构体的指针组成，由于指针的特性，调用方法时修改接收者指针的任意成员变量，在方法结束后，修改都是有效的。这种方式就十分接近于其他语言中面向对象中的`this`或者`self`。 例如我们为`Person`添加一个`SetAge`方法，来修改实例变量的年龄。

```go
// SetAge 设置p的年龄
// 使用指针接收者
func (p *Person) SetAge(newAge int8) {
	p.age = newAge
}
```

调用该方法：

```go
func main() {
	p1 := NewPerson("小王子", 25)
	fmt.Println(p1.age) // 25
	p1.SetAge(30)
	fmt.Println(p1.age) // 30
}
```

#### 值类型的接收者

当方法作用于值类型接收者时，Go语言会在代码运行时将接收者的值复制一份。在值类型接收者的方法中可以获取接收者的成员值，但修改操作只是针对副本，无法修改接收者变量本身。

```go
// SetAge2 设置p的年龄
// 使用值接收者
func (p Person) SetAge2(newAge int8) {
	p.age = newAge
}

func main() {
	p1 := NewPerson("小王子", 25)
	p1.Dream()
	fmt.Println(p1.age) // 25
	p1.SetAge2(30) // (*p1).SetAge2(30)
	fmt.Println(p1.age) // 25
}
```

#### 什么时候应该使用指针类型接收者

1. 需要修改接收者中的值
2. 接收者是拷贝代价比较大的大对象
3. 保证一致性，如果有某个方法使用了指针接收者，那么其他的方法也应该使用指针接收者。

#### 任意类型添加方法

在Go语言中，接收者的类型可以是任何类型，不仅仅是结构体，任何类型都可以拥有方法。 举个例子，我们基于内置的`int`类型使用type关键字可以定义新的自定义类型，然后为我们的自定义类型添加方法。

```go
//MyInt 将int定义为自定义MyInt类型
type MyInt int

//SayHello 为MyInt添加一个SayHello的方法
func (m MyInt) SayHello() {
	fmt.Println("Hello, 我是一个int。")
}
func main() {
	var m1 MyInt
	m1.SayHello() //Hello, 我是一个int。
	m1 = 100
	fmt.Printf("%#v  %T\n", m1, m1) //100  main.MyInt
}
```

**注意事项：** 非本地类型不能定义方法，也就是说我们不能给别的包的类型定义方法。

#### 嵌套结构体

一个结构体中可以嵌套包含另一个结构体或结构体指针，就像下面的示例代码那样。

```go
//Address 地址结构体
type Address struct {
	Province string
	City     string
}

//User 用户结构体
type User struct {
	Name    string
	Gender  string
	Address Address
}

func main() {
	user1 := User{
		Name:   "小王子",
		Gender: "男",
		Address: Address{
			Province: "山东",
			City:     "威海",
		},
	}
	fmt.Printf("user1=%#v\n", user1)//user1=main.User{Name:"小王子", Gender:"男", Address:main.Address{Province:"山东", City:"威海"}}
}
```

#### 嵌套匿名字段

上面user结构体中嵌套的`Address`结构体也可以采用匿名字段的方式，例如：

```go
//Address 地址结构体
type Address struct {
	Province string
	City     string
}

//User 用户结构体
type User struct {
	Name    string
	Gender  string
	Address //匿名字段
}

func main() {
	var user2 User
	user2.Name = "小王子"
	user2.Gender = "男"
	user2.Address.Province = "山东"    // 匿名字段默认使用类型名作为字段名
	user2.City = "威海"                // 匿名字段可以省略
	fmt.Printf("user2=%#v\n", user2) //user2=main.User{Name:"小王子", Gender:"男", Address:main.Address{Province:"山东", City:"威海"}}
}
```

#### 结构体的“继承”

Go语言中使用结构体也可以实现其他编程语言中面向对象的继承。

```go
//Animal 动物
type Animal struct {
	name string
}

func (a *Animal) move() {
	fmt.Printf("%s会动！\n", a.name)
}

//Dog 狗
type Dog struct {
	Feet    int8
	*Animal //通过嵌套匿名结构体实现继承
}

func (d *Dog) wang() {
	fmt.Printf("%s会汪汪汪~\n", d.name)
}

func main() {
	d1 := &Dog{
		Feet: 4,
		Animal: &Animal{ //注意嵌套的是结构体指针
			name: "乐乐",
		},
	}
	d1.wang() //乐乐会汪汪汪~
	d1.move() //乐乐会动！
}
```

#### 结构体字段的可见性

**结构体中字段大写开头表示可公开访问，小写表示私有（仅在定义当前结构体的包中可访问）。**



#### 结构体的比较

<img src="GoNote2.assets/image-20250224上午112111600.png" alt="image-20250224上午112111600" style="zoom:50%;" />

```go
	p1 := Point{1, 2}
	p2 := Point{1, 2}

	fmt.Println(p1.X == p2.X && p1.Y == p2.Y)  //true
	fmt.Println(p1 == p2)    //true
```



```go
  p1 := Person{"1", 2}
	p2 := Person{"1", 2}

	fmt.Println(p1.Name == p2.Name && p1.Age == p2.Age) //true
	fmt.Println(p1 == p2)                               //true

	fmt.Printf("%v \n", p1)  // {1 2}
	// %#v  按照 go 语法输出变量的值
	fmt.Printf("%#v \n", p1) // main.Person{Name:"1", Age:2} 

	fmt.Printf("%p  %p \n", &p1, &p2) // 0xc0000125b8  0xc0000125d0
	fmt.Println(&p1 == &p2)           // false
```



#### **和其他可比较的类型一样, 可比较的结构体类型都可以作为 map 的键类型**

deepseek:

go 语言中, struct 结构体实体是否可以作为 map 的键来使用,请举例?

好的，用户问的是在Go语言中，结构体是否可以作为map的键，并且要举例说明。我需要先回忆一下Go语言中map键的要求。记得在Go中，map的键必须是可比较的类型，也就是说这个类型必须支持==和!=操作符。结构体如果所有字段都是可比较的，那么结构体本身也是可比较的，因此可以作为键。

接下来我要确认结构体作为键的条件。例如，如果结构体包含切片、map或者函数类型的字段，这些类型是不可比较的，所以这样的结构体不能作为键。用户可能需要一个例子说明可用和不可用的情况。

然后需要举例子。比如，一个包含基本类型的结构体，如Name和Age，这样的结构体可以作为键。另一个例子包含切片，这样会导致编译错误。代码示例要清晰，展示两种情况。

还要注意用户可能想了解如何使用结构体作为键，比如如何初始化、赋值和访问。需要说明注意事项，比如如果结构体中有指针字段，虽然指针本身可比较，但指向的内容变化会影响键的比较结果，这可能不是用户期望的。

另外，用户可能在实际使用中遇到结构体作为键的情况，比如缓存配置、复合主键等场景。可能还需要提醒结构体作为键时的不可变性，确保键在使用过程中不会被修改，否则会导致查找失败。

现在要组织语言，分步骤解释，先说明条件，再举正反例子，最后总结注意事项和应用场景。确保回答准确，代码正确，没有语法错误。

在 Go 语言中，**结构体（struct）可以作为 map 的键**，但需要满足一个关键条件：**结构体的所有字段必须是可比较（comparable）的类型**。Go 要求 map 的键必须支持 `==` 和 `!=` 操作符，因此结构体的字段不能包含不可比较的类型（如切片、map、函数等）。

------

##### 示例 1：合法（可比较的结构体作为键）

```go
package main

import "fmt"

// 结构体的所有字段均为可比较类型（string、int）
type Person struct {
    Name string
    Age  int
}

func main() {
    // 创建一个以 Person 结构体为键的 map
    m := make(map[Person]string)

    // 定义结构体键
    key1 := Person{Name: "Alice", Age: 30}
    key2 := Person{Name: "Bob", Age: 25}

    // 向 map 中插入值
    m[key1] = "Engineer"
    m[key2] = "Designer"

    // 访问值
    fmt.Println(m[key1]) // 输出: Engineer
    fmt.Println(m[key2]) // 输出: Designer
}
```

------

##### 示例 2：非法（包含不可比较字段的结构体）

如果结构体包含不可比较的字段（如切片），则无法作为 map 的键：

```go
type InvalidKey struct {
    Name  string
    Scores []int // 切片是不可比较类型！
}

func main() {
    // 编译错误：invalid map key type InvalidKey
    m := make(map[InvalidKey]string)
}
```

------

##### 关键注意事项

1. **可比较性规则**：

   - 基本类型（`int`, `string`, `bool` 等）和指针是可比较的。
   - 数组（如果元素可比较）是可比较的。
   - **切片、map、函数、通道**是不可比较的。

2. **结构体作为键的行为**：

   - 结构体是值类型，作为键时会复制整个结构体的值。
   - 如果结构体包含指针字段，指针的值（内存地址）会被比较，而不是指针指向的内容。

3. **修改结构体键的风险**：

   - 如果修改了已作为键的结构体的字段值，会导致 map 无法正确检索到该键对应的值。例如：

     ```go
     p := Person{Name: "Alice", Age: 30}
     m := map[Person]string{p: "Engineer"}
     p.Age = 31 // 修改结构体字段
     fmt.Println(m[p]) // 输出空字符串（找不到键）
     ```

------

##### 总结

- **合法**：如果结构体的所有字段均为可比较类型，则可以作为 map 的键。
- **典型用途**：缓存复合配置、唯一标识多字段组合等场景。
- **推荐实践**：确保结构体不可变（字段为不可修改类型，或通过封装避免修改）。



---

### 函数

Go语言中支持函数、匿名函数和闭包，并且函数在Go语言中属于“一等公民”。

**defer**  延迟执行
go语言函数的return，分为两个步骤1，将返回值付给ret ； 2，return ret 。defer 执行的实际是在1和2之间

**函数类型**：根据入参和返回值分为不同的类型

**匿名函数**

#### **闭包**

闭包指的是一个函数和与其相关的引用环境组合而成的实体。简单来说，`闭包=函数+引用环境`。 首先我们来看一个例子：

```go
func adder() func(int) int {
	var x int
	return func(y int) int {
		x += y
		return x
	}
}
func main() {
	var f = adder()
	fmt.Println(f(10)) //10
	fmt.Println(f(20)) //30
	fmt.Println(f(30)) //60

	f1 := adder()
	fmt.Println(f1(40)) //40
	fmt.Println(f1(50)) //90
}
```

变量`f`是一个函数并且它引用了其外部作用域中的`x`变量，此时`f`就是一个闭包。 在`f`的生命周期内，变量`x`也一直有效。 闭包进阶示例1：

```go
func adder2(x int) func(int) int {
	return func(y int) int {
		x += y
		return x
	}
}
func main() {
	var f = adder2(10)
	fmt.Println(f(10)) //20
	fmt.Println(f(20)) //40
	fmt.Println(f(30)) //70	

	f1 := adder2(20)
	fmt.Println(f1(40)) //60
	fmt.Println(f1(50)) //110
}
```

---

### **指针**

指针本身是一个变量，它也有自己用于存放值的内存地址，而它的值为指向目标的内存地址值

区别于C/C++中的指针，Go语言中的指针不能进行偏移和运算，是安全指针。
要搞明白Go语言中的指针需要先知道3个概念：指针地址、指针类型和指针取值。

```go
func main() {
   var a int
   a = 100
   b := &a
   fmt.Printf("type a:%T type b:%T\n", a, b)
   // 将a的十六进制内存地址打印出来
   fmt.Printf("%p\n", &a)
   fmt.Printf("%p\n", b) // b的值
   fmt.Printf("%v\n", b)
   fmt.Printf("%p\n", &b) // b的内存地址
}
```

#### go语言中函数传参数永远传的是拷贝

```go
//go语言中函数传参数永远传的是拷贝
func f(x person) {
	x.gender = "女" // 修改的是副本的gender
}
func f2(x *person) {
	// (*x).gender = "女" // 根据内存地址找到那个原变量,修改的就是原来的变量
	x.gender = "女" // 语法糖,自动根据指针找对应的变量
}
func main() {
	var p person
	p.name = "周林"
	p.gender = "男"
	f(p)
	fmt.Println(p.gender) // 男
	f2(&p)                // ox1241ac3
	fmt.Println(p.gender) // 女
}
```



#### new

new是一个内置的函数，它的函数签名如下：

```go
func new(Type) *Type
```

其中，

- Type表示类型，new函数只接受一个参数，这个参数是一个类型
- *Type表示类型指针，new函数返回一个指向该类型内存地址的指针。

new函数不太常用，使用new函数得到的是一个类型的指针，并且该指针对应的值为该类型的零值。举个例子：

```go
func main() {
	a := new(int)
	b := new(bool)
	fmt.Printf("%T\n", a) // *int
	fmt.Printf("%T\n", b) // *bool
	fmt.Println(*a)       // 0
	fmt.Println(*b)       // false
}	
```

本节开始的示例代码中`var a *int`只是声明了一个指针变量a但是没有初始化，指针作为引用类型需要初始化后才会拥有内存空间，才可以给它赋值。应该按照如下方式使用内置的new函数对a进行初始化之后就可以正常对其赋值了：

```go
func main() {
	var a *int
	a = new(int)
	*a = 10
	fmt.Println(*a)
}
```



以下是关于 Go 语言中 `new()` 函数的使用总结，以 Markdown 格式呈现：

```go
# Go 语言中的 `new()` 函数

## 概述
`new()` 是 Go 语言中的一个内置函数，用于分配内存并返回指向该内存的指针。其函数签名如下：

func new(Type) *Type
```

- `Type`：要分配内存的类型。
- 返回值：指向该类型零值的指针。

------

##### `new()` 的用途

`new()` 可以用于创建以下类型的指针：

1. **基本类型**：如 `int`、`float64`、`bool` 等。
2. **结构体**：可以创建结构体的指针。
3. **数组**：可以创建数组的指针。
4. **自定义类型**：可以创建自定义类型的指针。

------

##### 使用 `new()` 创建结构体

`new()` 可以用于创建结构体的指针。例如：

go

复制

```go
type Person struct {
    Name string
    Age  int
}

func main() {
    p := new(Person)  // 创建一个指向 Person 结构体的指针
    p.Name = "Alice"  // 通过指针访问结构体字段
    p.Age = 30
    fmt.Println(p)    // 输出: &{Alice 30}
}
```

------

##### `new()` 与 `&Type{}` 的区别

- **`new(Type)`**：
  - 返回一个指向 `Type` 零值的指针。
  - 适用于不需要立即初始化的场景。
- **`&Type{}`**：
  - 返回一个指向已初始化的 `Type` 的指针。
  - 适用于需要立即初始化的场景。

##### 示例

go

复制

```go
// 使用 new()
p1 := new(Person)  // p1 是一个指向零值 Person 的指针

// 使用 &Type{}
p2 := &Person{Name: "Alice", Age: 30}  // p2 是一个指向已初始化 Person 的指针
```

------

##### 何时使用 `new()`

- 需要分配内存但不需要立即初始化。
- 创建基本类型的指针。

------

##### 示例代码

go

复制

```go
package main

import "fmt"

type Person struct {
    Name string
    Age  int
}

func main() {
    // 使用 new() 创建基本类型的指针
    i := new(int)
    fmt.Println(*i) // 输出: 0

    // 使用 new() 创建结构体的指针
    p := new(Person)
    p.Name = "Bob"
    p.Age = 25
    fmt.Println(p) // 输出: &{Bob 25}

    // 使用 &Type{} 创建结构体的指针
    p2 := &Person{Name: "Alice", Age: 30}
    fmt.Println(p2) // 输出: &{Alice 30}
}
```

------

##### 总结

- `new()` 用于分配内存并返回指向该内存的指针。
- 适用于基本类型、结构体、数组和自定义类型。
- 对于结构体，`new()` 返回一个指向零值结构体的指针。
- 如果需要立即初始化结构体，可以使用 `&Type{}` 语法。









#### make

make也是用于内存分配的，区别于new，它只用于slice、map以及channel的内存创建，而且它返回的类型就是这三个类型本身，而不是他们的指针类型，因为这三种类型就是引用类型，所以就没有必要返回他们的指针了。make函数的函数签名如下：

```go
func make(t Type, size ...IntegerType) Type
```

make函数是无可替代的，我们在使用slice、map以及channel的时候，都需要使用make进行初始化，然后才可以对它们进行操作。这个我们在上一章中都有说明，关于channel我们会在后续的章节详细说明。

本节开始的示例中`var b map[string]int`只是声明变量b是一个map类型的变量，需要像下面的示例代码一样使用make函数进行初始化操作之后，才能对其进行键值对赋值：

```go
func main() {
	var b map[string]int
	b = make(map[string]int, 10)
	b["沙河娜扎"] = 100
	fmt.Println(b)
}
```

#### new与make的区别

1. 二者都是用来做内存分配的。

2. make只用于slice、map以及channel的初始化，返回的还是这三个引用类型本身；

3. 而new用于类型的内存分配，并且内存对应的值为类型零值，返回的是指向类型的指针。

   

---



### 接口

接口（interface）定义了一个对象的行为规范，只定义规范不实现，由具体的对象来实现规范的细节。

#### 接口组合

接口与接口之间可以通过互相嵌套形成新的接口类型，例如Go标准库`io`源码中就有很多接口之间互相组合的示例。

```go
// src/io/io.go

type Reader interface {
	Read(p []byte) (n int, err error)
}

type Writer interface {
	Write(p []byte) (n int, err error)
}

type Closer interface {
	Close() error
}

// ReadWriter 是组合Reader接口和Writer接口形成的新接口类型
type ReadWriter interface {
	Reader
	Writer
}

// ReadCloser 是组合Reader接口和Closer接口形成的新接口类型
type ReadCloser interface {
	Reader
	Closer
}

// WriteCloser 是组合Writer接口和Closer接口形成的新接口类型
type WriteCloser interface {
	Writer
	Closer
}
```

对于这种由多个接口类型组合形成的新接口类型，同样只需要实现新接口类型中规定的所有方法就算实现了该接口类型。

#### 面向接口编程

PHP、Java等语言中也有接口的概念，不过在PHP和Java语言中需要显式声明一个类实现了哪些接口，在Go语言中使用隐式声明的方式实现接口。只要一个类型实现了接口中规定的所有方法，那么它就实现了这个接口。

​       由于接口类型的值可以是任意一个实现了该接口的类型值，所以接口值除了需要记录具体**值**之外，还需要记录这个值属于的**类型**。也就是说接口值由“类型”和“值”组成，鉴于这两部分会根据存入值的不同而发生变化，我们称之为接口的`动态类型`和`动态值`。

```go
// 定义接口
type speaker interface {
	speak() 
}
func main(){
  var ss speaker // 定义一个接口类型:speaker 的变量:ss
	fmt.Println(ss)  // <nil>
	fmt.Printf("%T",ss)   // <nil>
}
```

让我们来看一种特例：(*interface{})(nil)。将 nil 转成 interface 类型的指针，其实得到的结果仅仅是空接口类型指针并且它指向无效的地址。注意是空接口类型指针而不是空指针，这两者的区别蛮大的，学过 C 的童鞋都知道空指针是什么概念。

#### 空接口

**空接口的定义**

空接口是指没有定义任何方法的接口类型。因此任何类型都可以视为实现了空接口。也正是因为空接口类型的这个特性，空接口类型的变量可以存储任意类型的值。

#### 接口值

由于接口类型的值可以是任意一个实现了该接口的类型值，所以接口值除了需要记录具体**值**之外，还需要记录这个值属于的**类型**。也就是说接口值由“类型”和“值”组成，鉴于这两部分会根据存入值的不同而发生变化，我们称之为接口的`动态类型`和`动态值`。

我们接下来通过一个示例来加深对接口值的理解。

下面的示例代码中，定义了一个`Mover`接口类型和两个实现了该接口的`Dog`和`Car`结构体类型。

```go
type Mover interface {
	Move()
}

type Dog struct {
	Name string
}

func (d *Dog) Move() {
	fmt.Println("狗在跑~")
}

type Car struct {
	Brand string
}

func (c *Car) Move() {
	fmt.Println("汽车在跑~")
}
```

首先，我们创建一个`Mover`接口类型的变量`m`。

```go
var m Mover
```

此时，接口变量`m`是接口类型的零值，也就是它的类型和值部分都是`nil`，就如下图所示。

我们可以使用`m == nil`来判断此时的接口值是否为空。

```go
fmt.Println(m == nil)  // true
```

**注意：**我们不能对一个空接口值调用任何方法，否则会产生panic。

```go
m.Move() // panic: runtime error: invalid memory address or nil pointer dereference
```

接下来，我们将一个`*Dog`结构体指针赋值给变量`m`。

```go
m = &Dog{Name: "旺财"}
```

此时，接口值`m`的动态类型会被设置为`*Dog`，动态值为结构体变量的拷贝。

然后，我们给接口变量`m`赋值为一个`*Car`类型的值。

```go
var c *Car
m = c
```

这一次，接口值`m`的动态类型为`*Car`，动态值为`nil`。

**注意：**此时接口变量`m`与`nil`并不相等，因为它只是动态值的部分为`nil`，而动态类型部分保存着对应值的类型。

```go
fmt.Println(m == nil) // false
```

接口值是支持相互比较的，当且仅当接口值的动态类型和动态值都相等时才相等。

```go
var (
	x Mover = new(Dog)
	y Mover = new(Car)
)
fmt.Println(x == y) // false
```

但是有一种特殊情况需要特别注意，如果接口值保存的动态类型相同，但是这个动态类型不支持互相比较（比如切片），那么对它们相互比较时就会引发panic。

```go
var z interface{} = []int{1, 2, 3}
fmt.Println(z == z) // panic: runtime error: comparing uncomparable type []int
```

#### 类型断言

接口值可能赋值为任意类型的值，那我们如何从接口值获取其存储的具体数据呢？

我们可以借助标准库`fmt`包的格式化打印获取到接口值的动态类型。

```go
var m Mover

m = &Dog{Name: "旺财"}
fmt.Printf("%T\n", m) // *main.Dog

m = new(Car)
fmt.Printf("%T\n", m) // *main.Car
```

而`fmt`包内部其实是使用反射的机制在程序运行时获取到动态类型的名称。关于反射的内容我们会在后续章节详细介绍。

而想要从接口值中获取到对应的实际值需要使用类型断言，其语法格式如下。

```go
x.(T)
```

其中：

- x：表示接口类型的变量
- T：表示断言`x`可能是的类型。

该语法返回两个参数，第一个参数是`x`转化为`T`类型后的变量，第二个值是一个布尔值，若为`true`则表示断言成功，为`false`则表示断言失败。

举个例子：

```go
var n Mover = &Dog{Name: "旺财"}
v, ok := n.(*Dog)
if ok {
	fmt.Println("类型断言成功")
	v.Name = "富贵" // 变量v是*Dog类型
} else {
	fmt.Println("类型断言失败")
}
```

如果对一个接口值有多个实际类型需要判断，推荐使用`switch`语句来实现。

```go
// justifyType 对传入的空接口类型变量x进行类型断言
func justifyType(x interface{}) {
	switch v := x.(type) {
	case string:
		fmt.Printf("x is a string，value is %v\n", v)
	case int:
		fmt.Printf("x is a int is %v\n", v)
	case bool:
		fmt.Printf("x is a bool is %v\n", v)
	default:
		fmt.Println("unsupport type！")
	}
}
```

由于接口类型变量能够动态存储不同类型值的特点，所以很多初学者会滥用接口类型（特别是空接口）来实现编码过程中的便捷。只有当有两个或两个以上的具体类型必须以相同的方式进行处理时才需要定义接口。切记不要为了使用接口类型而增加不必要的抽象，导致不必要的运行时损耗。

在 Go 语言中接口是一个非常重要的概念和特性，使用接口类型能够实现代码的抽象和解耦，也可以隐藏某个功能的内部实现，但是缺点就是在查看源码的时候，不太方便查找到具体实现接口的类型。

相信很多读者在刚接触到接口类型时都会有很多疑惑，请牢记接口是一种类型，一种抽象的类型。区别于我们在之前章节提到的那些具体类型（整型、数组、结构体类型等），它是一个只要求实现特定方法的抽象类型。

**小技巧：** 下面的代码可以在程序编译阶段验证某一结构体是否满足特定的接口类型。

```go
// 摘自gin框架routergroup.go
type IRouter interface{ ... }

type RouterGroup struct { ... }

var _ IRouter = &RouterGroup{}  // 确保RouterGroup实现了接口IRouter
```

上面的代码中也可以使用`var _ IRouter = (*RouterGroup)(nil)`进行验证。

##### go 语言中实现接口是隐式的





```go
	var in io.Writer

	fmt.Printf("type= %T", in) //type= <nil>

	fmt.Printf("value= %v", in) //value= <nil>

```



<img src="GoNote2.assets/image-20240725下午25649944.png" alt="image-20240725下午25649944" style="zoom:50%;" />

##### 接口的值是可比较的,是比较它的动态类型和动态值



<img src="GoNote2.assets/image-20240725下午25755695.png" alt="image-20240725下午25755695" style="zoom:50%;" />



<img src="GoNote2.assets/image-20240725下午40447645.png" alt="image-20240725下午40447645" style="zoom:50%;" />

<img src="GoNote2.assets/image-20240725下午40506679.png" alt="image-20240725下午40506679" style="zoom:50%;" />

#### error 接口

<img src="GoNote2.assets/image-20240725下午55758921.png" alt="image-20240725下午55758921" style="zoom:50%;" />

#####  error 是一个接口类型, 有一个 Error() string 方法

<img src="GoNote2.assets/image-20240725下午100724176.png" alt="image-20240725下午100724176" style="zoom:50%;" />

##### 接口类型才能进行类型断言

因为除接口类型外的其他类型都是已经确定的类型

##### 将一个接口类型的值, 断言为另一个接口类型 T ,  如果断言检测成功 , 结果的类型为 接口类型T, (通常方法数量是增多)

<img src="GoNote2.assets/image-20240726上午95339277.png" alt="image-20240726上午95339277" style="zoom:50%;" />

<img src="GoNote2.assets/image-20240726上午103244306.png" alt="image-20240726上午103244306" style="zoom:50%;" />

<img src="GoNote2.assets/image-20240726上午104244235.png" alt="image-20240726上午104244235" style="zoom:50%;" />

<img src="GoNote2.assets/image-20250303下午31316515.png" alt="image-20250303下午31316515" style="zoom:50%;" />







### goroutine 和通道

<img src="GoNote2.assets/image-20240726下午43742230.png" alt="image-20240726下午43742230" style="zoom:50%;" />

<img src="GoNote2.assets/image-20240726下午35911908.png" alt="image-20240726下午35911908" style="zoom:50%;" />

##### main 函数 return 时, 所有的 goroutine 都暴力的直接终结

##### 没有办法让一个 goroutine 来停止另一个 goroutine, 除了让 main 函数返回

killall java  终结所有的指定名字的进程(UNIX 系统)



#### 通道



<img src="GoNote2.assets/image-20240726下午43931421.png" alt="image-20240726下午43931421" style="zoom:50%;" />



<img src="GoNote2.assets/image-20240726下午44104481.png" alt="image-20240726下午44104481" style="zoom:50%;" />

##### 通道已被关闭 , 向它发送值会崩溃

##### 通道已被关闭 , 用它接收值, 将会获取到通道中值 , 当通道中没有值时 , 接收操作将得到通道类型对应的零值

```go
//1. 未初始化的通道
var ch chan int // 未初始化的通道，值为 nil

//2. 显式将通道设置为 nil
ch := make(chan int)
ch = nil // 通道显式设置为 nil

//1.	在 select 语句中禁用通道
//在 select 语句中，设置某个通道为 nil 可以有效地禁用与该通道相关的 case 分支，因为对 nil 通道的发送和接收操作会阻塞。这样可以动态地控制 select 语句的行为。
var ch1, ch2 chan int
ch1 = make(chan int)
ch2 = nil

select {
case val := <-ch1:
    fmt.Println("Received from ch1:", val)
case val := <-ch2: // 这个 case 永远不会执行
    fmt.Println("Received from ch2:", val)
default:
    fmt.Println("No activity")
}
```

##### 通道是 nil 的时候不能关闭通道 

##### 通道中有值和无值都可以关闭通道 

#### 无缓冲通道

<img src="GoNote2.assets/image-20240726下午44833925.png" alt="image-20240726下午44833925" style="zoom:50%;" />

##### 使用无缓冲通道 , 等于将操作发送和接收的两个 goroutine 同步化了, 他们会等待彼此

<img src="GoNote2.assets/image-20240726下午50834966.png" alt="image-20240726下午50834966" style="zoom:50%;" />

#### 有缓冲通道

<img src="GoNote2.assets/image-20240726下午53129866.png" alt="image-20240726下午53129866" style="zoom:50%;" />

##### 缓冲通道可用空间被放满的时候 , 发送操作将被阻塞等待, 直到通道中有可用空间的时候才会被唤醒; 

##### 相反,如果缓冲通道是空的 , 接收操作将被阻塞等待, 直到通道中有可用数据的时候才会被唤醒; 

<img src="GoNote2.assets/image-20240726下午54520101.png" alt="image-20240726下午54520101" style="zoom:50%;" />

##### goroutine 泄漏:  

##### goroutine 被阻塞在同步接收后者同步发送无缓冲通道, 此时主 goroutine 结束 , 导致阻塞的 goroutine 被卡死;



<img src="GoNote2.assets/image-20240726下午62233290.png" alt="image-20240726下午62233290" style="zoom:50%;" />



<img src="GoNote2.assets/image-20240726下午94704992.png" alt="image-20240726下午94704992" style="zoom:50%;" />



<img src="GoNote2.assets/image-20240726下午94723124.png" alt="image-20240726下午94723124" style="zoom:50%;" />

<img src="GoNote2.assets/image-20240726下午94853511.png" alt="image-20240726下午94853511" style="zoom:50%;" />





<img src="GoNote2.assets/image-20240729上午94101405.png" alt="image-20240729上午94101405" style="zoom:50%;" />

##### 计数信号量

##### 为限制并发执行的 goroutine 的数量, 在指定容量的缓冲通道中, 通过发送数据来将令牌写入占位, 执行完后接收掉占位令牌,

#### select 多路复用

<img src="GoNote2.assets/image-20240729上午103710498.png" alt="image-20240729上午103710498" style="zoom:50%;" />

##### Select多路复用同时命中 , 会随机选择一个进入执行

<img src="GoNote2.assets/image-20240729上午103904860.png" alt="image-20240729上午103904860" style="zoom:50%;" />

<img src="GoNote2.assets/image-20240730下午12023095.png" alt="image-20240730下午12023095" style="zoom:50%;" />

##### 使用select多路复用接收通道, 可以避免被阻塞

<img src="GoNote2.assets/image-20240730上午105021095.png" alt="image-20240730上午105021095" style="zoom:50%;" />



##### 多路复用 select

1,如果没有 case , 比如 select{} 将一直阻塞;

2,多个 case 的情况, 唯一执行成功的,被命中; 如果有多个执行成功的,则随机命中其中一个 case;

3, 可以设置 default : , 当没有执行成功的 case, 将可以执行默认操作或者灵活的退出

4,如果多路都阻塞 , 并且没有 default 的话, select 将阻塞等待

多路复用, 是同时使用多个通道, 避免单个通道阻塞整个流程的好方法



##### 使用 range 从通道中取值,如果通道未关闭就会阻塞; 如果通道已被关闭,自动退出range循环

##### 使用显式的 value,ok :=  <-chan , 可以使用 ok 值判断通道是否关闭



通道的关闭:



#### 计数信号量

<img src="GoNote2.assets/image-20240730下午30503872.png" alt="image-20240730下午30503872" style="zoom:50%;" />





#### 如何取消 goroutine 

<img src="GoNote2.assets/image-20240730下午32141471.png" alt="image-20240730下午32141471" style="zoom:50%;" />

<img src="GoNote2.assets/image-20240730下午32323792.png" alt="image-20240730下午32323792" style="zoom:50%;" />



<img src="GoNote2.assets/image-20250304上午114824219.png" alt="image-20250304上午114824219" style="zoom:50%;" />

### 使用共享变量实现并发



#### 数据竞态





<img src="GoNote2.assets/image-20240731下午84405216.png" alt="image-20240731下午84405216" style="zoom:50%;" />



#### 互斥锁 sync.Mutex

<img src="GoNote2.assets/image-20240731下午92132119.png" alt="image-20240731下午92132119" style="zoom:50%;" />



<img src="GoNote2.assets/image-20240731下午92224700.png" alt="image-20240731下午92224700" style="zoom:50%;" />



<img src="GoNote2.assets/image-20240804下午81813022.png" alt="image-20240804下午81813022" style="zoom:50%;" />



读写互斥锁: sync.RWMutex

<img src="GoNote2.assets/image-20240804下午83742873.png" alt="image-20240804下午83742873" style="zoom:50%;" />



sync.RWMutex



#### 延迟初始化: sync.Once

<img src="GoNote2.assets/image-20240808下午30621871.png" alt="image-20240808下午30621871" style="zoom:50%;" />



#### 竞态检测器

<img src="GoNote2.assets/image-20240808下午30650365.png" alt="image-20240808下午30650365" style="zoom:50%;" />





<img src="GoNote2.assets/image-20240808下午41900155.png" alt="image-20240808下午41900155" style="zoom:50%;" />

##### 从通道读取数据 , 这个操作会一直阻塞到通道关闭 , 并得到零值



#### goroutine 与线程



<img src="GoNote2.assets/image-20240808下午43537387.png" alt="image-20240808下午43537387" style="zoom:50%;" />

##### 一个线程2MB , 一个协程 goroutine 2KB 的栈内存



<img src="GoNote2.assets/image-20240808下午44138545.png" alt="image-20240808下午44138545" style="zoom:50%;" />



#### GOMAXPROCS

<img src="GoNote2.assets/image-20240808下午51323944.png" alt="image-20240808下午51323944" style="zoom:50%;" />



#### goroutine 没有标识

<img src="../../../../go_program_note.assets/image-20240808下午51959235.png" alt="image-20240808下午51959235" style="zoom:50%;" />



---



### 泛型

Go 1.18版本增加了对泛型的支持，泛型也是自 Go 语言开源以来所做的最大改变。

#### 什么是泛型

泛型允许程序员在强类型程序设计语言中编写代码时使用一些以后才指定的类型，在实例化时作为参数指明这些类型。ーー换句话说，在编写某些代码或数据结构时先不提供值的类型，而是之后再提供。

泛型是一种独立于所使用的特定类型的编写代码的方法。使用泛型可以编写出适用于一组类型中的任何一种的函数和类型。

#### 为什么需要泛型

假设我们需要实现一个反转切片的函数——`reverse`。

```go
func reverse(s []int) []int {
	l := len(s)
	r := make([]int, l)

	for i, e := range s {
		r[l-i-1] = e
	}
	return r
}

fmt.Println(reverse([]int{1, 2, 3, 4}))  // [4 3 2 1]
```

可是这个函数只能接收`[]int`类型的参数，如果我们想支持`[]float64`类型的参数，我们就需要再定义一个`reverseFloat64Slice`函数。

```go
func reverseFloat64Slice(s []float64) []float64 {
	l := len(s)
	r := make([]float64, l)

	for i, e := range s {
		r[l-i-1] = e
	}
	return r
}
```

如果要想支持`[]string`类型切片就要定义`reverseStringSlice`函数，如果想支持`[]xxx`就需要定义一个`reverseXxxSlice`…

一遍一遍地编写相同的功能是低效的，实际上这个反转切片的函数并不需要知道切片中元素的类型，但为了适用不同的类型我们把一段代码重复了很多遍。

Go1.18之前我们可以尝试使用反射去解决上述问题，但是使用反射在运行期间获取变量类型会降低代码的执行效率并且失去编译期的类型检查，同时大量的反射代码也会让程序变得晦涩难懂。

类似这样的场景就非常适合使用泛型。从Go1.18开始，使用泛型就能够编写出适用所有元素类型的“普适版”`reverse`函数。

```go
func reverseWithGenerics[T any](s []T) []T {
	l := len(s)
	r := make([]T, l)

	for i, e := range s {
		r[l-i-1] = e
	}
	return r
}
```

#### 泛型语法

泛型为Go语言添加了三个新的重要特性:

1. 函数和类型的类型参数。
2. 将接口类型定义为类型集，包括没有方法的类型。
3. 类型推断，它允许在调用函数时在许多情况下省略类型参数。

#### 类型参数

#### 类型形参和类型实参

我们之前已经知道函数定义时可以指定形参，函数调用时需要传入实参。![形参与实参](GoNote2.assets/parameter-argument.png)

现在，Go语言中的函数和类型支持添加类型参数。类型参数列表看起来像普通的参数列表，只不过它使用方括号（`[]`）而不是圆括号（`()`）。![类型形参与类型实参](GoNote2.assets/type-parameter-argument.png)

借助泛型，我们可以声明一个适用于**一组类型**的`min`函数。

```go
func min[T int | float64](a, b T) T {
	if a <= b {
		return a
	}
	return b
}
```

#### 类型实例化

这次定义的`min`函数就同时支持`int`和`float64`两种类型，也就是说当调用`min`函数时，我们既可以传入`int`类型的参数。

```go
m1 := min[int](1, 2)  // 1
```

也可以传入`float64`类型的参数。

```go
m2 := min[float64](-0.1, -0.2)  // -0.2
```

向 `min` 函数提供类型参数(在本例中为`int`和`float64`)称为实例化（ *instantiation* ）。

类型实例化分两步进行：

1. 首先，编译器在整个泛型函数或类型中将所有类型形参（type parameters）替换为它们各自的类型实参（type arguments）。
2. 其次，编译器验证每个类型参数是否满足相应的约束。

在成功实例化之后，我们将得到一个非泛型函数，它可以像任何其他函数一样被调用。例如：

```go
fmin := min[float64] // 类型实例化，编译器生成T=float64的min函数
m2 = fmin(1.2, 2.3)  // 1.2
```

`min[float64]`得到的是类似我们之前定义的`minFloat64`函数——`fmin`，我们可以在函数调用中使用它。

#### 类型参数的使用

除了函数中支持使用类型参数列表外，类型也可以使用类型参数列表。

```go
type Slice[T int | string] []T

type Map[K int | string, V float32 | float64] map[K]V

type Tree[T interface{}] struct {
	left, right *Tree[T]
	value       T
}
```

在上述泛型类型中，`T`、`K`、`V`都属于类型形参，类型形参后面是类型约束，类型实参需要满足对应的类型约束。

泛型类型可以有方法，例如为上面的`Tree`实现一个查找元素的`Lookup`方法。

```go
func (t *Tree[T]) Lookup(x T) *Tree[T] { ... }
```

要使用泛型类型，必须进行实例化。`Tree[string]`是使用类型实参`string`实例化 `Tree` 的示例。

```go
var stringTree Tree[string]
```

#### 类型约束

普通函数中的每个参数都有一个类型; 该类型定义一系列值的集合。例如，我们上面定义的非泛型函数`minFloat64`那样，声明了参数的类型为`float64`，那么在函数调用时允许传入的实际参数就必须是可以用`float64`类型表示的浮点数值。

类似于参数列表中每个参数都有对应的参数类型，类型参数列表中每个类型参数都有一个**类型约束**。类型约束定义了一个类型集——只有在这个类型集中的类型才能用作类型实参。

Go语言中的类型约束是接口类型。

就以上面提到的`min`函数为例，我们来看一下类型约束常见的两种方式。

类型约束接口可以直接在类型参数列表中使用。

```go
// 类型约束字面量，通常外层interface{}可省略
func min[T interface{ int | float64 }](a, b T) T {
	if a <= b {
		return a
	}
	return b
}
```

作为类型约束使用的接口类型可以事先定义并支持复用。

```go
// 事先定义好的类型约束类型
type Value interface {
	int | float64
}
func min[T Value](a, b T) T {
	if a <= b {
		return a
	}
	return b
}
```

在使用类型约束时，如果省略了外层的`interface{}`会引起歧义，那么就不能省略。例如：

```go
type IntPtrSlice [T *int] []T  // T*int ?

type IntPtrSlice[T *int,] []T  // 只有一个类型约束时可以添加`,`
type IntPtrSlice[T interface{ *int }] []T // 使用interface{}包裹
```

#### 类型集

**Go1.18开始接口类型的定义也发生了改变，由过去的接口类型定义方法集（method set）变成了接口类型定义类型集（type set）。**也就是说，接口类型现在可以用作值的类型，也可以用作类型约束。

![type set](GoNote2.assets/type-set.png)

把接口类型当做类型集相较于方法集有一个优势: 我们可以显式地向集合添加类型，从而以新的方式控制类型集。

Go语言扩展了接口类型的语法，让我们能够向接口中添加类型。例如

```go
type V interface {
	int | string | bool
}
```

上面的代码就定义了一个包含 `int`、 `string` 和 `bool` 类型的类型集。![type set](GoNote2.assets/type-set-2.png)

从 Go 1.18 开始，一个接口不仅可以嵌入其他接口，还可以嵌入任何类型、类型的联合或共享相同底层类型的无限类型集合。

当用作类型约束时，由接口定义的类型集精确地指定允许作为相应类型参数的类型。

- `|`符号

  `T1 | T2`表示类型约束为T1和T2这两个类型的并集，例如下面的`Integer`类型表示由`Signed`和`Unsigned`组成。

  ```go
  type Integer interface {
  	Signed | Unsigned
  }
  ```

- `~`符号

  `~T`表示所以底层类型是T的类型，例如`~string`表示所有底层类型是`string`的类型集合。

  ```go
  type MyString string  // MyString的底层类型是string
  ```

  **注意：**`~`符号后面只能是基本类型。

接口作为类型集是一种强大的新机制，是使类型约束能够生效的关键。目前，使用新语法表的接口只能用作类型约束。

#### any接口

空接口在类型参数列表中很常见，在Go 1.18引入了一个新的预声明标识符，作为空接口类型的别名。

```go
// src/builtin/builtin.go

type any = interface{}
```

由此，我们可以使用如下代码：

```go
func foo[S ~[]E, E any]() {
	// ...
}
```

#### constrains

https://pkg.go.dev/golang.org/x/exp/constraints 包提供了一些常用类型。

#### 类型推断

最后一个新的主要语言特征是类型推断。从某些方面来说，这是语言中最复杂的变化，但它很重要，因为它能让人们在编写调用泛型函数的代码时更自然。

#### 函数参数类型推断

对于类型参数，需要传递类型参数，这可能导致代码冗长。回到我们通用的 `min`函数：

```go
func min[T int | float64](a, b T) T {
	if a <= b {
		return a
	}
	return b
}
```

类型形参`T`用于指定`a`和`b`的类型。我们可以使用显式类型实参调用它：

```go
var a, b, m float64
m = min[float64](a, b) // 显式指定类型实参
```

在许多情况下，编译器可以从普通参数推断 `T` 的类型实参。这使得代码更短，同时保持清晰。

```go
var a, b, m float64

m = min(a, b) // 无需指定类型实参
```

这种从实参的类型推断出函数的类型实参的推断称为函数实参类型推断。函数实参类型推断只适用于函数参数中使用的类型参数，而不适用于仅在函数结果中或仅在函数体中使用的类型参数。例如，它不适用于像 `MakeT [ T any ]() T` 这样的函数，因为它只使用 `T` 表示结果。

#### 约束类型推断

Go 语言支持另一种类型推断，即*约束类型推断*。接下来我们从下面这个缩放整数的例子开始：

```go
// Scale 返回切片中每个元素都乘c的副本切片
func Scale[E constraints.Integer](s []E, c E) []E {
    r := make([]E, len(s))
    for i, v := range s {
        r[i] = v * c
    }
    return r
}
```

这是一个泛型函数适用于任何整数类型的切片。

现在假设我们有一个多维坐标的 `Point` 类型，其中每个 `Point` 只是一个给出点坐标的整数列表。这种类型通常会实现一些业务方法，这里假设它有一个`String`方法。

```go
type Point []int32

func (p Point) String() string {
    b, _ := json.Marshal(p)
    return string(b)
}
```

由于一个`Point`其实就是一个整数切片，我们可以使用前面编写的`Scale`函数：

```go
func ScaleAndPrint(p Point) {
    r := Scale(p, 2)
    fmt.Println(r.String()) // 编译失败
}
```

不幸的是，这代码会编译失败，输出`r.String undefined (type []int32 has no field or method String`的错误。

问题是`Scale`函数返回类型为`[]E`的值，其中`E`是参数切片的元素类型。当我们使用`Point`类型的值调用`Scale`（其基础类型为[]int32）时，我们返回的是`[]int32`类型的值，而不是`Point`类型。这源于泛型代码的编写方式，但这不是我们想要的。

为了解决这个问题，我们必须更改 `Scale` 函数，以便为切片类型使用类型参数。

```go
func Scale[S ~[]E, E constraints.Integer](s S, c E) S {
    r := make(S, len(s))
    for i, v := range s {
        r[i] = v * c
    }
    return r
}
```

我们引入了一个新的类型参数`S`，它是切片参数的类型。我们对它进行了约束，使得基础类型是`S`而不是`[]E`，函数返回的结果类型现在是`S`。由于`E`被约束为整数，因此效果与之前相同：第一个参数必须是某个整数类型的切片。对函数体的唯一更改是，现在我们在调用`make`时传递`S`，而不是`[]E`。

现在这个`Scale`函数，不仅支持传入普通整数切片参数，也支持传入`Point`类型参数。

这里需要思考的是，为什么不传递显式类型参数就可以写入 `Scale` 调用？也就是说，为什么我们可以写 `Scale(p, 2)`，没有类型参数，而不是必须写 `Scale[Point, int32](p, 2)` ？

新 `Scale` 函数有两个类型参数——`S` 和 `E`。在不传递任何类型参数的 `Scale(p, 2)` 调用中，如上所述，函数参数类型推断让编译器推断 `S` 的类型参数是 `Point`。但是这个函数也有一个类型参数 `E`，它是乘法因子 `c` 的类型。相应的函数参数是`2`，因为`2`是一个非类型化的常量，函数参数类型推断不能推断出 `E` 的正确类型(最好的情况是它可以推断出`2`的默认类型是 `int`，而这是错误的，因为Point 的基础类型是`[]int32`)。相反，编译器推断 `E` 的类型参数是切片的元素类型的过程称为**约束类型推断**。

约束类型推断从类型参数约束推导类型参数。当一个类型参数具有根据另一个类型参数定义的约束时使用。当其中一个类型参数的类型参数已知时，约束用于推断另一个类型参数的类型参数。

通常的情况是，当一个约束对某种类型使用 *~type* 形式时，该类型是使用其他类型参数编写的。我们在 `Scale` 的例子中看到了这一点。`S` 是 `~[]E`，后面跟着一个用另一个类型参数写的类型`[]E`。如果我们知道了 `S` 的类型实参，我们就可以推断出`E`的类型实参。`S` 是一个切片类型，而 `E`是该切片的元素类型。

#### 总结

总之，如果你发现自己多次编写完全相同的代码，而这些代码之间的唯一区别就是使用的类型不同，这个时候你就应该考虑是否可以使用类型参数。

泛型和接口类型之间并不是替代关系，而是相辅相成的关系。泛型的引入是为了配合接口的使用，让我们能够编写更加类型安全的Go代码，并能有效地减少重复代码。

