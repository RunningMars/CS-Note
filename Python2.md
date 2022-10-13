# 函数

## 1. 函数的定义和调用

学习函数目的：提高代码的复用性，减少代码的冗余

```python
# 定义函数
def show():
    # 实现某个功能的代码要放到函数里面
    for _ in range(3):
        print("人生苦短")
        
# 调用函数
show()

############## 输出结果 ###############
人生苦短
人生苦短
人生苦短
```

### 	函数的文档说明

> 函数的文档说明: 解释说明函数的功能的。

```python
def show():
    """实现两个数字和计算"""
    result = 1 + 2
    print(result)
show()
############## 输出结果 ###############
3
```

#### 查看函数的文档说明，使用`help()`函数。

执行`help(show)`代码可以看到`show`函数的文档说明：

```python
help(show)

############## 输出结果 ###############
Help on function show in module __main__:

show()
    实现两个数字和计算
```

## 2.带有参数的函数

**函数带有参数的语法格式 ：**

```python
def 函数名(形参1, 形参2, .....):
    实现某个功能的具体代码

函数名(实参1, 实参2, ....)        #调用
```

**定义带有参数的函数：**

```python
# 定义带有参数的函数
def sum_num(num1, num2):
    result = num1 + num2
    print(result)

#调用函数，并给函数传递参数
sum_num(5, 3)

############## 输出结果 ###############
8
```

## 3. 函数的返回值

```python
# 定义一个函数，实现两个数字和的计算，并把计算后的结果返回出去

def sum_num(num1, num2):
    result = num1 + num2
# 把函数内部的数据通过return返回给函数的调用者
    return result

# 调用函数
value = sum_num(1, 2)

# 打印出函数的返回值
print(value)
```

### 函数返回值的扩展

> 扩展1：return可以结合None使用

```python
def search_str(params):
    my_str = "hello"
    for value in my_str:
        if value == params:
            return value
    else:
        return None
```

> 扩展3： 函数没有提供返回值，那么调用函数的时候如果使用变量接收返回结果，那么返回的结果是None。

```python
# 定义一个函数
def show():
    my_name = "huangjing"
    print("my name's", my_name)

# 调用函数
result = show()
print("result is", result)


############## 输出结果 ###############
my name's huangjing
result is None
```

## 5. 局部变量和全局变量

### 局部变量

> **局部变量：在函数内定义的变量称为局部变量。**

```python
def show():
    # 定义了一个局部变量
    num1 = 100
    # 局部变量的作用域：只能在函数内部使用，无法在函数外使用
    print("函数内的变量:", num1)

# 调用函数
show()
```

测试：在函数外部使用局部变量

```python
print(num1)

######## 报错信息为：########
NameError: name 'num1' is not defined
```

#### 总结：

- 局部变量的作用：临时存储函数内的数据，当函数执行结束时，局部变量保存的数据会销毁，在内存中进行释放。
- 局部变量的作用域：只能在函数内部使用，无法在函数外使用。

### 全局变量

> 全局变量：在函数外定义的变量称为全局变量。

```python
# 定义全局变量
g_num = 1000

def show():
    # 全局变量可以在不同函数内使用
    print("show函数使用的全局变量:", g_num)

def show_info():
    # 全局变量可以在不同函数内使用
    print("show_info函数使用的全局变量:", g_num)

show()
show_info()

# 测试：全局变量还可以在函数外部使用
print("函数外使用的全局变量为:", g_num)

############## 输出结果 ###############

show函数使用的全局变量: 1000
show_info函数使用的全局变量: 1000
函数外使用的全局变量为: 1000
```

#### 总结：

- 全局变量的作用域(使用范围): 可以在不同函数内或者函数外使用这个全局变量。
- 全局变量的作用：可以在不同函数内共享数据(全局变量数据)

## 6.修改全局变量

> 在函数内修改全局变量需要使用关键字 `global`。

我们先看一下，在函数内如果不使用关键字 `global`来修改全局变量会出现什么情况：

```python
# 定义全局变量
g_num = 100

def modify_value():
#其实这里没有对全局变量进行修改，而是定义了一个局部变量，只不过局部变量的名字和全局变量的名字相同而已
    g_num = 200
    print(g_num)

modify_value()
print(g_num)


############## 输出结果 ###############
200
100
```

我们发现全局变量并没有发生改变。还是之前的`100`。
 **使用关键字 `global`来修改全局变量：**

```python
g_num = 100

def modify_value():

    global g_num
    g_num = 200
    print(g_num)

modify_value()
print(g_num)


############## 输出结果 ###############
200
200
```

## 7. 多个函数共享数据

> 多个函数共享数据的操作：
>
> - 全局变量：全局变量可以在不同函数内共享数据(全局变量)

### 全局变量共享数据的操作：

```python
g_num = 1

def modify_value():
    # 修改全局变量的数据
    global g_num
    g_num = 2

def show():
    print("show:", g_num)

modify_value()
show()


############## 输出结果 ###############
show: 2
```

以上可以看出，全局变量可以在`modify_value()`函数和`show()`函数中同时使用。

## 8. return返回多个值

> **return返回多个数据的解决方案：把多个数据封装到一个容器类型里面，这样通过return可以一次性返回多个数据。**

返回列表：（返回元组同理）

```python
# 定义函数
def return_more_value():
    #一次性返回多个数据
    return [1, 3, 5]

# 调用函数
result = return_more_value()
print(result, type(result))

############## 输出结果 ###############
[1, 3, 5] <class 'list'>
```

返回字典：

```python
# 定义函数
def return_more_value():
    #一次性返回多个数据
    return {"name": "林黛玉", "age": 20}

# 调用函数
result = return_more_value()
print(result, type(result))

############## 输出结果 ###############
{'name': '林黛玉', 'age': 20} <class 'dict'>
```

## 9. 函数的缺省参数：

> **缺省参数：形参设置了默认值，那么这个形参就是缺省参数。**

#### 缺省参数的特点：

- 当调用函数时如果给缺省参数传值了，那么使用传入的数据；
- 当调用函数时如果没有给缺省参数传值，那么使用缺省的参数默认值。

#### 缺省参数的注意点：

- 缺省参数后面不能再定义普通参数；
- 缺省参数后面还可以再次定义缺省参数

不给缺省参数传值，缺省参数使用默认值：



```python
# 此时的age参数就是缺省参数
def show(name, age=18, sex="男"): 
    print("姓名:", name, "年龄:", age, "性别:", sex)

# 不给缺省参数传值，缺省参数使用默认值
show("贾宝玉")

############## 输出结果 ###############
姓名: 贾宝玉 年龄: 18 性别: 男
```

给缺省参数传值了，那么使用传入的数据：

```python
def show(name, age=18, sex="男"): # 此时的age参数就是缺省参数
    print("姓名:", name, "年龄:", age, "性别:", sex)

show("林黛玉", 20, "女")

############## 输出结果 ###############
姓名: 林黛玉 年龄: 20 性别: 女
```

## 10. 调用函数传参的三种方式

- 按照位置参数的方式进行传参， 注意点：强调的是调用函数时位置参数的顺序要和函数定义时参数的顺序保持一致；
- 按照关键字参数的方式进行传参, 注意点：强调的是调用函数是关键字的名字要和函数定义时参数的名字保持一致；
- 前面是按照位置参数的方式进行传参后面是关键字参数的方式进行传参。

首先我们先定义一个函数：

```python
def show_info(name, age):
    print("姓名:", name, "年龄:", age)
```

接下来没我，我们按照不同的方式进行传参

#### 按照位置参数的方式进行传参：

positional argument： 表示位置参数

```python
show_info("李四", 30)
```

#### 按照关键字参数的方式进行传参：

keyword argument: 表示关键字参数

```python
show_info(name="李四", age=30)
```

#### 前面是按照位置参数的方式进行传参后面是关键字参数的方式进行传参：

```python
show_info("李四", age=30)
```

> 注意：如果前面使用关键字参数后面必须使用关键字参数

## 11. 函数的不定长参数

函数的不定长参数：函数参数的个数不确定，可能是0个或者1个或者多个这样的参数称为不定长参数。

#### 不定长参数可以分为两种：

- 不定长位置参数: *args，提示：当调用函数时，所有的位置参数封装成一个元组给args参数；
- 不定长关键字参数: **kwargs，提示：当调用函数时，所有的关键字参数封装成字典给kwargs参数

#### 缺省参数结合不定长位置参数使用

注意点：把缺省参数放到不定长位置参数的后面。

如果不这样操作，那按照位置参数的方式进行传参时，这里的缺省参数的默认值永远使用不了。*听上去很复杂的样子，其实稍微操作一下非常容易理解*

```python
def show(name, age=18, *args):
    print("name:", name, "age:", age, "args:", args)

# 调用函数时使用位置参数的方式进行传参
show("李四", 20, 1, 2, 3)

############## 输出结果 ###############
name: 李四 age: 20 args: (1, 2, 3)
```

我们可以看到在按照位置参数的方式进行传参时，第二个参数默认给了`age`参数，剩下的所有数据打包成元组的形式传给了不定长位置参数`args`，这里的缺省参数的默认值永远使用不了。
 **解决办法：**把缺省参数放到不定长位置参数的后面。

```python
def show(name, *args, age=18):
    print("name:", name, "age:", age, "args:", args)

show("李四", 20, 1, 2, 3)

############## 输出结果 ###############
name: 李四 age: 18 args: (20, 1, 2, 3)
```

也可以在传参的时候，修改默认参数

```python
def show(name, *args, age=18):
    print("name:", name, "age:", age, "args:", args)

show("李四", 20, 1, 2, age=30)

############## 输出结果 ###############
name: 李四 age: 30 args: (20, 1, 2)
```

#### 缺省参数结合不定长位置参数和不定长关键字参数的使用

> 注意点：不定长关键字参数需要放到所有参数的最后面

```python
def show(name, *args, age=18, **kwargs):
    print("name:", name, "age:", age, "args:", args, "kwargs:", kwargs)

show("张三", 1, 3, 5)
show("王五", 1, 3, 5, 20, a=1, b=2, c=3, age=40)

############## 输出结果 ###############
name: 张三 age: 18 args: (1, 3, 5) kwargs: {}
name: 王五 age: 40 args: (1, 3, 5, 20) kwargs: {'a': 1, 'b': 2, 'c': 3}
```

“王五”的例子说明了：当使用关键字传参的时候，优先判断有没有对于的参数名，如果有数据给对于的参数名，否则kwargs参数。

#### **不定长参数的扩展**

在函数定义的时候，可以使用不定长位置参数`*args`和不定长关键字参数`**kwargs`来定义。在调用的时候也可以用同样的方法来拆包
 比如，我们把要传的位置参数都封装到了元组里面，把关键字参数都封装到了字典里面

```python
my_tuple = (1, 2)
my_dict = {"a": 1, "b": 2}
```

使用`show(*my_tuple)`调用所有不定长位置参数：

```python
# *my_tuple: 表示对元组进行拆包，把元组中的每项数据按照位置参数的方式进行传参
show(*my_tuple)  # 等价于 show(1, 2)
```

使用`show(**my_dict)`调用所有不定长关键字参数：

```python
show(**my_dict) # 等价于 show(a=1, b=2)

show(*my_tuple, **my_dict)  # 等价于show(1, 2, a=1, b=2)
```

> 注意点：`*my_tuple`和`**my_dict`不能单独使用, 只能结合不定长位置参数和不定长关键字参数使用。

**看实战效果**
 `show(*my_tuple)`  这里等价于`show(a=1, b=2)`

```python
def show(*args, **kwargs):
    print(args, kwargs)

my_tuple = (1, 2)
my_dict = {"a": 1, "b": 2}

show(*my_tuple)

############## 输出结果 ###############
(1, 2) {}
show(**my_dict)` 等价于 `show(a=1, b=2)
```

```python
def show(*args, **kwargs):
    print(args, kwargs)

my_tuple = (1, 2)
my_dict = {"a": 1, "b": 2}

show(**my_dict)

############## 输出结果 ###############
() {'a': 1, 'b': 2}
show(*my_tuple, **my_dict)` 等价于`show(1, 2, a=1, b=2)
```

## 12. 拆包

> 拆包：使用不同变量保存容器类型中的每一个数据，这样的操作就是拆包。
> 注意点：拆包是结合容器类型使用的, 变量的个数要和容器类型中数据个数要一致
> 容器类型: 字符串，列表，元组，字典，range，集合(set)

#### 对字符串进行拆包：

```python
my_str = "abc"

value1, value2, value3 = my_str

print(value1, value2, value3)

############## 输出结果 ###############
a b c
```

#### 对列表进行拆包：

```python
my_list = [1, 2, 3]

value1, value2, value3 = my_list

print(value1, value2, value3)

############## 输出结果 ###############
1 2 3
```

#### 对元组进行拆包：

```python
my_tuple = (2, 2, 3)

value1, value2, value3 = my_tuple

print(value1, value2, value3)

############## 输出结果 ###############
2 2 3
```

#### 对字典进行拆包：

```python
my_dict = {"name": "李四", "age": 20}

value1, value2 = my_dict

print(value1, value2)

############## 输出结果 ###############
name age
```

对字典直接进行拆包，获取的是每一个key，对`values`进行拆包：

```python
my_dict = {"name": "李四", "age": 20}
value1, value2 = my_dict.values()
print(value1, value2)

############## 输出结果 ###############
李四 20
```

#### 对`range`进行拆包：

```python
my_range = range(1, 3)

value1, value2 = my_range

print(value1, value2)

############## 输出结果 ###############
1 2
```

### 拆包应用场景

> 拆包应用场景一：用拆包来获取容器类型中的每一个数据

```python
def return_value():
    return 1, 3, 5

result = return_value()
print(result, type(result))

############## 输出结果 ###############
(1, 3, 5) <class 'tuple'>
```

上面定义的`return_value()`函数返回了一个元组，接下来我们可以**用拆包来获取元组中的每一个数据：**

```python
def return_value():
    return 1, 3, 5

# 拆包: 获取容器类型中的每一个数据
value1, value2, value3 = return_value()
print(value1, value2, value3)

############## 输出结果 ###############
1 3 5
```

> 拆包应用场景二：用拆包交换两个变量的值

```python
a = 1
b = 2

print(a, b)

a, b = b, a

print(a, b)

############## 输出结果 ###############
1 2
2 1
```

看似很简单的过程，其实中间经历了一个拆包的过程，在pyth里有等号`=`先看等号右面的内容，**这里其实是把`b`和`a`的数据封装到一个元组里面，然后使用变量`a`和变量`b`保存元组里面的第一个值和第二个值。**

## 13. 引用

> 引用：就是使用的意思， 就是使用变量保存了数据在内存的地址，变量没有直接保存数据，而保存的是数据在内存中的地址。
> `num1 = 1` ：定义一个变量，名称是num1, 使用该变量保存了一下数据1在内存中的地址
> `num2 = num1`：定义一个变量，名称是num2, 使用该变量保存了num1存储数据的内存地址
> `id(num1)`：使用`id()`函数查看变量的内存地址。

```python
num1 = 1
num2 = num1
print("num1的内存地址是:", id(num1), "num2的内存地址是:", id(num2))
num1 = 34
print(num1, num2)
print("num1的内存地址是:", id(num1), "num2的内存地址是:", id(num2))
num2 = 99
print("num1的内存地址是:", id(num1), "num2的内存地址是:", id(num2))
############## 输出结果 ###############
num1的内存地址是: 4491512112 num2的内存地址是: 4491512112
34   1
num1的内存地址是: 4491513168 num2的内存地址是: 4491512112
num1的内存地址是: 4491513168 num2的内存地址是: 4491703728
```

我们发现这两个变量的内存地址是一样的，说明了赋值其实就是传递了数据的内存地址。
 变量重新赋值内存地址就是发生变化：

## 14. 可变类型和不可变类型

### 不可变类型

> 不可变类型: 不允许在原有内存空间的基础上修改数据，修改数据后内存地址会发生变化。

不可变类型有: 数字，字符串，元组

数字：

```python
num1 = 100

# 查看变量的内存地址
num1_id = id(num1)
print(num1, num1_id)

# 不可变类型想要修改数据，都是通过重新赋值来完成
num1 = 200
# 查看变量的内存地址
num1_id = id(num1)
print(num1, num1_id)

############## 输出结果 ###############
100 140727306386784
200 140727306389984
```

字符串：

```python
my_str = "abc"
print(my_str, id(my_str))

my_str = "adc"
print(my_str, id(my_str))

############## 输出结果 ###############
abc 2191203856688
adc 2191203947568
```

元组：

```python
my_tuple = (1, 5)

print(my_tuple, id(my_tuple))

my_tuple = (2, 5)

print(my_tuple, id(my_tuple))

############## 输出结果 ###############
(1, 5) 1361982166344
(2, 5) 1361982405960
```

对于不可变类型来说，不能根据下标修改数据，**以下操作都是错误的操作：**

```python
my_str = "abc"
my_str[1] = "d"  #这种操作不对，不可变类型不能根据下标修改数据

my_tuple = (1, 5)
my_tuple[0] = 2  #这种操作不对，不可变类型不能根据下标修改数据
```

#### 总结：

- 对于不可变类型来说，不能在原有内存空间的基础上修改数据，想要修改数据都是通过重新赋值来完成。
- 重新赋值一个新值，那么变量保存的内存地址会发生变化。

### 可变类型

> 可变类型：允许在原有内存空间的基础上修改(添加，删除，修改)数据，修改后内存地址不变。
> 可变类型: 列表，字典，集合

列表

```python
my_list = [1, 4, 6]
print(my_list)
print(id(my_list))

print("===== 修改数据内存地址不变 ======")

my_list.append(8)
print(my_list)
print(id(my_list))

############## 输出结果 ###############

[1, 4, 6]
1524680118856
===== 修改数据内存地址不变 ======
[1, 4, 6, 8]
1524680118856
```

字典

```python
my_dict = {"name": "李四", "age": 20}
print(my_dict)
print(id(my_dict))

print("===== 修改数据内存地址不变 ======")

my_dict["age"] = 50
print(my_dict)
print(id(my_dict))

############## 输出结果 ###############
{'name': '李四', 'age': 20}
2017536906808
===== 修改数据内存地址不变 ======
{'name': '李四', 'age': 50}
2017536906808
```

不管是列表还是字典修改数据内存地址都不会变化，但如果重新赋值那内存地址就会发生变化：

```python
my_list = ["a", "b"]

print(my_list)
print(id(my_list))

print("===== 重新赋值内存地址改变 =====")

my_list = ["c", "d"]

print(my_list)
print(id(my_list))

############## 输出结果 ###############
['a', 'b']
2441131479624
===== 重新赋值内存地址改变 =====
['c', 'd']
2441132757640
```

#### 总结：

- 在原有内存空间的基础上进行修改数据，修改后内存地址不变
- 重新赋值一个新值，这样做变量保存的内存地址会变化

### 函数的可变类型和不可变类型

定义一个函数：

```python
def show(params):
    print(params, id(params))

    print("==== 修改前VS修改后 ====")
    
    params += params
    print(params, id(params))
```

调用函数，给函数传参的时候，如果`params`变量是一个不可变类型，那么 `+=` 后 `params` 保存的内存地址会发生变化，如果 `params` 变量是一个可变类型，那么 `+=`后 `params` 保存的内存地址不变，因为可变类型允许在原有内存空间的基础上修改数据。

给函数的参数传一个定义可变类型的变量——列表

```python
def show(params):
    print(params, id(params))

    print("==== 修改前VS修改后 ====")
    
    params += params
    print(params, id(params))

# 定义可变类型的变量
my_list = [1, 3]
print(my_list, id(my_list))

#调用函数
show(my_list)

print(my_list, id(my_list))

############## 输出结果 ###############
[1, 3] 2682280956488
[1, 3] 2682280956488
==== 修改前VS修改后 ====
[1, 3, 1, 3] 2682280956488
[1, 3, 1, 3] 2682280956488
```

*内存地址没有变化*

给函数的参数传一个定义不可变类型的变量——数字

```python
def show(params):
    print(params, id(params))

    print("==== 修改前VS修改后 ====")

    params += params
    print(params, id(params))

# 定义不可变类型的变量
my_num = 1

print(my_num, id(my_num))

# 调用函数
show(my_num)

print(my_num, id(my_num))
############## 输出结果 ###############
1 4495489328
1 4495489328
==== 修改前VS修改后 ====
2 4495489360
1 4495489328
```

*内存地址改变*

## 15.函数使用的注意点

#### 局部变量和全局变量的作用域

- 局部变量的作用域: 只能在函数内部使用；
- 全局变量的作用域: 可以在不同函数内或者函数外使用

#### 函数名重名

- 如果函数名相同，后面的函数会把前面的同名函数进行覆盖，前面的函数就不能再使用了；
- 在python里面没有函数或者方法的重载

```python
def show():
    print("明天大家休息，可以出去浪一下~")

def show():
    print("浪什么浪，继续学Python！")

show()

############## 输出结果 ###############
浪什么浪，继续学Python！
```

可以看到两个`show()`函数重名，后面的把前面的覆盖掉了

## 16. 递归函数

> 递归函数：在一个函数内部又调用函数本身那么这样的函数称为递归函数。
> 递归函数的注意点：递归函数必须要有结束递归调用条件。

```python
def show(num):
    print("人生苦短")
    if num == 3:
        print("我用Python")
    else:
        show(num+1)

# 调用函数
show(2)
```

**递归调用扩展：**递归函数并不是可以无限次的调用，**系统的默认递归调用次数是1000次。**

查看系统默认递归调用次数：

```python
import sys
count = sys.getrecursionlimit()
print(count)

############## 输出结果 ###############
1000
```

修改默认最大递归次数：

```python
# 修改默认递归次数
import sys
sys.setrecursionlimit(2000)

# 查看修改后的结果
print(sys.getrecursionlimit())

############## 输出结果 ###############
2000
```

## 17. 匿名函数

> 匿名函数：使用 `lambda` 关键字定义函数称为匿名函数。
> 匿名函数语法格式：`lambda 形参1, 形参2, .... : 返回值或者调用其他函数`

#### 匿名函数的注意点：

- 匿名函数也是属于函数的；
- 匿名函数只能写一行代码；
- 匿名函数的返回值不需要使用return关键字

匿名函数的使用：

```python
# 使用变量add_func保存了一个匿名函数
noname_func = lambda num1, num2 : num1 + num2

# 给匿名函数传递参数
result = noname_func(1, 2)
print(result)

############## 输出结果 ###############
3
```

解释一下以上匿名函数 `lambda num1, num2 : num1 + num2`
 冒号`:`前是匿名函数`noname_func`的两个形参，待会儿调用的时候需要给他们赋值，冒号后面是该函数的功能：求两个参数的和。
 在调用该匿名函数`noname_func(1, 2)`时，传递了两个参数1和2，求两个参数的和便是3。

没有参数的匿名函数：

```python
no_param = lambda : print("人生苦短，我用Python")
no_param()

############## 输出结果 ###############
人生苦短，我用Python
```

**学习匿名函数目的：使用匿名函数可以简化普通函数的代码。**

## 18. 列表推导式

> 列表推导式: 使用for循环快速创建一个列表，也就是说列表推导式最终返回的类型是列表。
> 语法结构：`[xx for xx in 容器类型]`

#### 使用`for`循环快速创建列表即列表推导式

示例：

```python
result = [value for value in range(1, 6)]
print(result, type(result))

############## 输出结果 ###############
[1, 2, 3, 4, 5] <class 'list'>
```

#### 列表推导式结合`if`语句

比如我们只要1-10之间所有的奇数：

```python
result = [value for value in range(1, 11) if value % 2 == 1]

print(result, type(result))

############## 输出结果 ###############
[1, 3, 5, 7, 9] <class 'list'>
```

#### 列表推导式结合多个for循环使用

```python
result = [(value1, value2) for value1 in range(2) for value2 in range(1, 4)]
print(result, type(result))

############## 输出结果 ###############
[(0, 1), (0, 2), (0, 3), (1, 1), (1, 2), (1, 3)] <class 'list'>
```

> 练一练1：使用列表推导式取出`my_list`列表里年龄大于20岁的
> `my_list = [{"name": "李四", "age": 20}, {"name": "王五", "age": 22}]`

```python
result = [my_dict for my_dict in my_list if my_dict["age"] > 20]
print(result)

############## 输出结果 ###############
[{'name': '王五', 'age': 22}]
```

练一练2：使用已知列表`my_list = ["A", "B", "C"]`和列表推导式生成一个新列表`new_list = ["AAA", "BBB", "CCC"]`

```python
my_list = ["A", "B", "C"]
result = [value * 3 for value in my_list]

print(result)

############## 输出结果 ###############
['AAA', 'BBB', 'CCC']
```

> 练一练3：使用已知列表`my_list = ["A", "B", "C"]`和列表推导式生成一个新列表`new_list = ["A!", "B!", "C!"]`

```python
my_list = ["A", "B", "C"]
result = [value + "!" for value in my_list]

print(result)

############## 输出结果 ###############
['A!', 'B!', 'C!']
```

## 19. 集合

> **集合(set): 也是一个容器类型，可以存储多个数据，但是集合里面的数据不能重复。**
> **学习集合目的：以后可以通过集合对容器类型数据进行去重**

#### 集合特点：

- 数据不能重复
- 集合也是一个可变类型
- 集合里面的数据是无序的—“无序”的意思就是，我们定义一个集合每次的输出顺序都不一样。

#### 定义集合

```python
my_set = {1, 3, "A", "b"}
print(my_set, type(my_set), id(my_set))

############## 输出结果 ###############
{1, 'A', 3, 'b'} <class 'set'> 2132515827272
```

因为**集合里面的数据是无序的**所以我们可以看出，输出的集合里的数据顺序和我们定义的顺序并不一致。

#### 添加数据

使用`my_set.add(2)`添加数据

```python
my_set = {1, 3, "A", "b"}
print(my_set, type(my_set), id(my_set))

my_set.add(2)
print(my_set, type(my_set), id(my_set))

############## 输出结果 ###############
{1, 'A', 3, 'b'} <class 'set'> 2266825698888
{1, 2, 3, 'b', 'A'} <class 'set'> 2266825698888
```

我们发现，向集合添加数据后，集合的`id`地址并没有发生变化，所以说**集合也是一个可变类型。**

知道如何向集合里面添加数据后，我们向集合里添加一个集合里本来就存在的数据，来验证一下**集合里的数据不能重复：**

```python
my_set = {1, 3, "A", "b"}
print(my_set)

my_set.add("A")
print(my_set)

############## 输出结果 ###############
{1, 'b', 3, 'A'}
{1, 'b', 3, 'A'}
```

我们可以看到向集合里添加一个集合里本来就有的数据`"A"`后，输出并没有变化。**我们常常使用集合里面的数据不能重复这一特性来对其他容器类型进行去重操作。**

#### 空集合的写法 — `set()`

> 空的集合不能直接使用大括号，大括号表示字典。

错误演示：

```python
my_set = {}
print(my_set, type(my_set))

############## 输出结果 ###############
{} <class 'dict'>
```

我们可以看到，空集合如果直接用大括号定义，输出的类型是一个字典`dict`。

**定义空集合正确演示：**

```python
my_set = set()
print(my_set, type(my_set))

############## 输出结果 ###############
set() <class 'set'>
```

> 注意点：因为集合是无序的所以**不能根据下标获取和修改数据。**

不能根据下标获取数据的报错演示：

```csharp
my_set = {1, 3, "A", "b"}

print(my_set[1])

############## 输出结果 ###############
TypeError: 'set' object is not subscriptable
```

不能根据下标修改数据的报错演示：

```csharp
my_set = {1, 3, "A", "b"}

my_set[2] = "C"

############## 输出结果 ###############
TypeError: 'set' object does not support item assignment
```

> **集合里面只能存放不可变类型数据(字符串，数字，元组)，不能存放可变类型数据。**

集合里如果存放列表等可变类型的数据就会报错：

```bash
my_set = {1, "A", (1, 2), [1, 2]}
print(my_set)

############## 输出结果 ###############
TypeError: unhashable type: 'list'
```

#### 集合的拆包：

```bash
value1, value2, value3 = {1, 3, "A"}
print(value1, value2, value3)

############## 输出结果 ###############
1 3 A
```

#### 遍历集合：

```csharp
for value in {1, 3, "A"}:
    print(value)

############## 输出结果 ###############
1
3
A
```

#### 使用set对容器中的数据进行去重：

> 需求：对已知列表`my_list = [1, 1, 2, 4]`去重。

思路：把已知列表转成集合(set) ---> 把集合再转成列表
列表转换成集合后会自动对列表里面的数据去重，再把该集合转换回列表。

```bash
my_list = [1, 1, 2, 4]

# 把列表转成集合(set)
new_set = set(my_list)
print(new_set)

# 把集合再转成列表
new_list = list(new_set)
print(new_list)

############## 输出结果 ###############
{1, 2, 4}
[1, 2, 4]
```

> 需求：对已知元组`my_tuple = (1, 22, 1, 33)`去重。

与上面列表的去重操作完全一样：

```bash
my_tuple = (1, 22, 1, 33)

# 把元组转成集合
new_set = set(my_tuple)
print(new_set)

# 把集合转成元组
new_tuple = tuple(new_set)
print(new_tuple)

############## 输出结果 ###############
{1, 22, 33}
(1, 22, 33)
```

## 20.高阶函数

> 高阶函数：函数的参数或者返回值是一个函数类型，那么这样的函数称为高阶函数

#### 高阶函数 — 函数的参数是函数类型

```python
def show(new_func): # 此时new_func参数就是一个函数类型

    num1 = 1
    num2 = 2

    # 调用输入过来的函数
    result = new_func(num1, num2)

    print("显示结果:", result)

# 此时调用show函数时，传入一个匿名函数。
show(lambda x, y: x - y)

############## 输出结果 ###############
显示结果: -1
```

#### 高阶函数 — 函数返回值是一个函数类型

```python
def show_info():

    # 定义子函数
    def inner_func():

        print("我是一个子函数或者内部函数")

    # 调用内部函数
    # inner_func()
    return inner_func  # 返回一个函数，函数名是inner_func
    # return inner_func()# 返回是一个函数调用后的结果 inner_func() = 》 None

new_func = show_info()

new_func()

############## 输出结果 ###############

我是一个子函数或者内部函数
```

## python提供的高阶函数

#### python提供的高阶函数-reduce

> reduce: 根据提供的功能函数对容器类型中每一个数据进行相关计算

```python
import functools  # 函数工具模块

# reduce: 根据提供的功能函数对容器类型中每一个数据进行相关计算

my_list = ["A", "B", "C"]

# 定义功能函数
def calc_str(my_str1, my_str2):
    return my_str1 + my_str2

# 1. 提供的功能函数
# 2. 要计算的容器类型
result = functools.reduce(calc_str, my_list)
print(result, type(result))

############## 输出结果 ###############

ABC <class 'str'>
```

> **应用：计算1-100之间的累加和**

```python
import functools  # 函数工具模块

new_list = [value for value in range(1, 101)]
print(new_list)
# 提示：匿名函数以后都是结合高阶函数去使用
# functools.reduce(匿名函数, new_list)
result = functools.reduce(lambda x, y: x + y, new_list)
print(result)

############## 输出结果 ###############

[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99, 100]
5050
```

合并简写版：

```python
# 提示：函数如果结合reduce高阶函数使用，那么提供函数的参数必须有两个
import functools  # 函数工具模块
result = functools.reduce(lambda x, y: x + y, range(1, 101))
print(result)

############## 输出结果 ###############

5050
```

#### python提供的高阶函数-filter高阶函数

> `filter` 高阶函数所需要的功能函数，功能函数的参数只有一个

```python
my_list = [1, 3, 5, 10, 11, 20]

def my_filter(value):
    return value % 2 == 1

new_filter = filter(my_filter, my_list)

print(new_filter)

# 把过滤结果转成列表
result = list(new_filter)
print(result)

############## 输出结果 ###############

<filter object at 0x00000220F4FAC388>
[1, 3, 5, 11]
```

简化后的写法：

```python
my_list = [1, 3, 5, 10, 11, 20]

new_filter = filter(lambda x: x % 2 == 1, my_list)

result = list(new_filter)
print(result)

############## 输出结果 ###############

[1, 3, 5, 11]
```

```python
my_list = [{"name": "李四", "age": 20}, {"name": "王四", "age": 22}]

new_filter = filter(lambda my_dict: my_dict["age"] > 20, my_list)

result = list(new_filter)
print(result)

############## 输出结果 ###############

[{'name': '王四', 'age': 22}]
```

---

# 类

类是一种对象，每个对象都属于特定的类，并被称为该类的实例。

### 创建类

在程度开发中，要设计一个类，通常需要满足以下三个要求：

1. 类名。类名的命名要以驼峰式命名法进行命名，也就是每个单词的首字母要大写，例如`CapWords`；
2. 属性。属性赋予事物具有什么样的特性；
3. 方法。方法赋予事物具有什么样的行为。

### 属性和方法的确定

描述对象特征的内容可以定义为`属性`。对象具有的某些行为（动词），就可以定义为`方法`。

## 面向对象基础语法

在Python中，对象无所不在，变量、数据、函数都是对象。

先看第一种方法，在`ipython`中定义一个列表变量，即`gl_list=[]`，然后输入`gl_list.`（后面有一个点），按下`TAB`键，后面就会出现这个对象能使用的方法，如下所示：

```python
In [3]: gl_list = []

In [4]: gl_list.
    append() count() insert() reverse()
    clear() extend() pop() sort()
    copy() index() remove()
```

现在定义一个函数，如下所示：

```python
In [6]: def demo():
   ...:     """这是一个测试函数"""
   ...:     print("Hello python")
   ...:

In [7]: demo()
Hello python

In [8]: demo.
```

此时输入demo后面再加一个点`.`，没有任何信息，此时就需要用另外一种方法来验证它是一个对象，也就是使用`dir`，如下所示：

```python
In [10]: dir(demo)
Out[10]:
['__annotations__',
 '__call__',
 '__class__',
 '__closure__',
 '__code__',
 '__defaults__',
 '__delattr__',
 '__dict__',
 '__dir__',
 '__doc__',
 '__eq__',
 '__format__',
 '__ge__',
 '__get__',
 '__getattribute__',
 '__globals__',
 '__gt__',
 '__hash__',
 '__init__',
 '__init_subclass__',
 '__kwdefaults__',
 '__le__',
 '__lt__',
 '__module__',
 '__name__',
 '__ne__',
 '__new__',
 '__qualname__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__setattr__',
 '__sizeof__',
 '__str__',
 '__subclasshook__']
```

从上面的我们可以看出来，这里面是一个列表，它列出了很多方法，这些方法都是双下划线开头，双下划线结尾，这些东西都是Python内置的方法（`__方法名__`)，有些可以直接使用，例如`__doc__`，如下所示：

```python
In [11]: demo.__doc__
Out[11]: '这是一个测试函数'
```

此时我们就可以看到，使用`__doc__`这个内置方法就能查看函数的文档说明，常用的一些内置方法/属性有以下这些：

| 序号 | 方法名     | 类型 | 作用                                    |
| ---- | ---------- | ---- | --------------------------------------- |
| 1    | `__new__`  | 方法 | 创建对象时，会被自动调用                |
| 2    | `__init__` | 方法 | 对象被初始化时，会被自动调用            |
| 3    | `__del__`  | 方法 | 对象被从内存中销毁之前，会被自动调用    |
| 4    | `__str__`  | 方法 | 返回对象的工描述信息，print函数输出使用 |

因此，`dir()`这个函数很有用，例如当我想调用某个函数的方法时，一时想不起来，就可以使用`dir()`这个函数。

## 定义简单的类

### 格式

在Python中要定义一个只包含方法的类，格式如下：

```python
class 类名
    def 方法1(self, 参数列表):
        pass
    
    def 方法2(self, 参数列表):
        pass
```

### pass

```python
def sample(n_samples):
    pass
```

该处的 pass 便是占据一个位置，因为如果定义一个空函数程序会报错，当你没有想好函数的内容是可以用 pass 填充，使程序可以正常运行

```python
class Test:
		pass
```

```python
while True:
  	pass
```

简单而言，pass 是一种空操作（null operation），解释器执行到它的时候，除了检查语法是否合法，什么也不做就直接跳过。
它跟 return、break、continue 和 yield 之类的非空操作相比，最大的区别是它不会改变程序的执行顺序。它就像我们写的注释，除了占用一行代码行，不会对所处的作用域产生任何影响。

## 创建对象

当一个类定义完全成，要使用这个类来创建对象，语法格式如下：

```python
对象变量=类名()
```

### 创建对象

```python
class Cat:

    def eat(self):
        print("小猫爱吃鱼")

    def drink(self):
        print("小猫要喝水")

# 创建猫对象
tom = Cat()

tom.eat()
tom.drink()
```

如果要用`print`函数直接输出对象，那么输出结果就是此对象创建的内存地址，如下所示：

```python
print(tom)
<__main__.Cat object at 0x0000017F72807A90>
```

```python
lazy_cat = Cat()
lazy_cat2 = lazy_cat
print(lazy_cat2)
print(lazy_cat)
```

结果如下：

```python
<__main__.Cat object at 0x000001C0424B7A90>
<__main__.Cat object at 0x000001C0424B7A90>
```

## 方法中的self参数

### 给对象添加属性

```python
tom.name = "Tom"
```

### 输出对象属性

前面是找到，在类中定义的方法中的一个**参数`self`，这个self是指：哪一个对象调用的方法，self就是哪一个对象的引用**。

例如代码中的`tom = Cat()`，tom指向的对象就是由Cat这个类创建的，此时这个对象调用了Cat中的eat这个方法时，self指的也是这个对象，如果要想访问这个对象的属性，那么就是self加一个点(`.`)即可，如下所示：

```python
class Cat:

    def eat(self):
        print("%s 爱吃鱼"% self.name)#这一步我们可以使用self.name来访问对象的属性

    def drink(self):
        print(""%s 要喝水"% self.name)

# 创建猫对象
tom = Cat()

# 可能使用 .属性名 利用赋值语句就可以为对象添加属性
tom.name = "Tom"
tom.eat()

# 再创建一个猫对象
lazy_cat = Cat()
lazy_cat.name = "大懒猫"
lazy_cat.eat()
```

## 初始化方法

当使用`类名()`创建对象时，会自动执行以下操作：

1. 为对象在内存中分配空间——创建对象
2. 为对象的属性设置初始值——初始化方法(init)

这个初始化方法就是`__init__`方法，`__init__`是对象的内置的方法，此方法是**专门用来定义一个类具有哪些属性的方法**，这个方法是固定的。我们在`Cat`中增加`__init__`方法，验证此方法在创建对象时会被自动调用，如下所示：

```python
class Cat:

    def __init__(self):

        print("这是一个初始化方法")

tom = Cat()
```

### 在初始化方法内部定义属性

在`__init__`方法内部使用`self.属性名 = 属性的初始值`就可以定义属性

定义属性之后，再使用Cat类创建的对象都会拥有该属性，如下所示：

```python
class Cat:

    def __init__(self):

        print("这是一个初始化方法")

        # self.属性名 = 属性的初始值
        self.name = "Tom"

tom = Cat()

print(tom.name)
```

### 初始化方法的改造

```python
class Cat:

    def __init__(self, new_name):

        print("这是一个初始化方法")

        # self.属性名 = 属性的初始值
        self.name = new_name

    def eat(self):
        print("%s爱吃鱼"%self.name)

tom = Cat("Tom")

print(tom.name)

lazy_cat = Cat("大懒猫")
lazy_cat.eat()
```

## 内置方法和属性

这一部分介绍两个内置方法，如下所示：

| 序号 | 方法名    | 类型 | 作用                                  |
| ---- | --------- | ---- | ------------------------------------- |
| 01   | `__del__` | 方法 | 对象被从内存中销毁之前，会被自动调用  |
| 02   | `__str__` | 方法 | 返回对象的描述信息，print函数输出使用 |

### `__del__`方法

- 在Python中，当使用`类名()`创建对象时，为对象分配完空间后，自动调用`__init__`方法。
- 当一个对象被从内存中销毁之前，会自动调用`__del__`方法。

### `__str__`方法

- 在Python中，使用`print`输出对象变量，默认情况下，会输出这个变量引用的对象是由哪一个类创建的对象，以及在内存中的地址（十六进制表示）。
- 如果在开发中，希望使用`print`输出对象变量时，能够打印自定义的内容，就可以利用`__str__`这个内置方法了。需要注意的是，`__str__`方法必须返回一个字符。

看下面的案例，在这个案例，我们直接输出一个对象（有的教程在此处称之为“实例”），如下所示：

```python
class Cat:
    def __init__(self, new_name):
        self.name = new_name
        print("%s 来了" % self.name)

    def __str__(self):
        return "我是小猫[%s]"% self.name

# tom是一个全局变量
tom = Cat("Tom")
print(tom)
```

## 面向对象封装案例

### 封装

1. 封装是面向对象编程的一大特点；
2. 面向对象编程的第一步就是将属性和方法封装到一个抽象的类中；
3. 外界使用类创建对象（有的教程叫实例），然后让对象调用方法；
4. 对象方法中的细节都被封装在类的内部。

#### 身份运算符

再来看一下前面代码中的某一句，即`if self.gun == None`，选中后，会出现如下提示信息：

![img](https:////upload-images.jianshu.io/upload_images/3803770-0c9b9cc204a2f269.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

PyCharm的提示信息显示，如果与`None`进行比较时，最好使用`is`或`is not`，而不是使用`==`。这里的`is`就是身份运算符。

身份运算符用于比较两个对象的内存地址是否一致，也就是说是否是对同一个对象的引用。在Python中，针对`None`进行比较时，建议使用`is`判断。Python中的身份运算符有2个，分别是`is`与`is not`，它们的功能如下所示：

| 运算符 | 描述                                       | 实例                         |
| ------ | ------------------------------------------ | ---------------------------- |
| is     | is是判断两个标识符是不是引用同一个对象     | x is y，类似id(x)==id(y)     |
| is not | is not是判断两个标识符是不是引用不同的对象 | x is not y，类似id(x)!=id(y) |

**这里需要区分一下`is`与`==`的区别**

**`is`用于判断两个变量引用对象是否为同一个；**

**`==`用于判断引用变量的值是否相等，如下所示：**

```python
In [1]: a = [1, 2, 3]

In [2]: id(a)
Out[2]: 2681339468296

In [3]: b = [1, 2, 3]

In [4]: id(b)
Out[4]: 2681339465864

In [5]: a == b
Out[5]: True

In [6]: a is b
Out[6]: False
```

从上面的案例我们可以知道，a与b这两个列表的值相等，但引用不相等（也就是说这两个变量引用的内存地址不相同）。

**而`None`在Python中算是一个空对象，空对象不能把它理解为零，它是Python中的一个特殊的常量，指向内存中的一个地址，一个变量如果是None，它一定和None指向同一个内存地址。**在上面的代码中，即`if self.gun == None`这句中，`==`后面接的是一个对象，因此最好使用`is`，因为`is`主要用于判断对象的引用，而不是值，因此Python建议使用`is`来判断None。

网上又检索到了一些资料，如下所示：

在区分`is`和`==`这两种运算符区别之前，首先要知道Python中对象包含的三个基本要素，分别是：`id`(身份标识)、`type`(数据类型)和`value`(值)。`is`和`==`都是对对象进行比较判断作用的，但对对象比较判断的内容并不相同。`==`比较操作符 和`is`同一性运算符区别

`==`是python标准操作符中的比较操作符，用来比较判断两个对象的value(值)是否相等，`is`也被叫做同一性运算符，这个运算符比较判断的是对象间的唯一身份标识，也就是id是否相同。

## 私有属性和私有方法

### 应用场景及定义方式

#### 定义方式

- 在定义属性和方法时，在属性名或者方法名前增加两个下划线，定义的就是私有属性或方法；
- 我们先看一个最常规的案例，如下所示：

```python
class Women:

    def __init__(self, name):

        self.name = name
        self.age = 18

    def secret(self):
        print("%s 的年龄是%d"%(self.name, self.age))

xiaofang = Women("小芳")

print(xiaofang.age)

xiaofang.secret()
```

运行结果如下所示：

```python
18
小芳 的年龄是18
```

在这个案例中，我们定义了一个女人类(Women)，通过这个类创建了一个xiaofang对象，然后输出了这个对象的属性(age)与方法(secret)。

现在我们将age这个属性与方法改为私有属性，也就是在它们前面加两个下划线，变成`__age`，如下所示：

```python
class Women:

    def __init__(self, name):

        self.name = name
        self.__age = 18

    def secret(self):
        print("%s 的年龄是%d"%(self.name, self.__age))

xiaofang = Women("小芳")

# 私有属性在外界不能被直接访问
print(xiaofang.__age)

xiaofang.secret()
```

运行结果如下所示：

```python
Traceback (most recent call last):
  File "D:/netdisk/bioinfo.notes/Python/黑马教程笔记/面向对象/hm_17_私有属性和方法.py", line 13, in <module>
    print(xiaofang.__age)
AttributeError: 'Women' object has no attribute '__age'
```

运行结果出错，系统提示缺少属性`__age`。现在我们将`print(xiaofang.__age)`这句注释掉，如下所示：

```python
class Women:

    def __init__(self, name):

        self.name = name
        self.__age = 18

    def secret(self):
        # 在对象的方法内部，是可以访问对象的私有属性的
        print("%s 的年龄是%d"%(self.name, self.__age))

xiaofang = Women("小芳")

# 私有属性在外界不能被直接访问
# print(xiaofang.__age)

xiaofang.secret()
```

运行结果如下所示：

```python
小芳 的年龄是18
```

能够正常运行，这说明在对象的`secret`这个方法内部，可以访问这个对象的私有属性，也就是`self.__age`。

现在我们把`secret`这个方法也改为私有方法，即改为`__secret`，如下所示：

```python
class Women:

    def __init__(self, name):

        self.name = name
        self.__age = 18

    def __secret(self):
        print("%s 的年龄是%d"%(self.name, self.__age))

xiaofang = Women("小芳")

# print(xiaofang.__age)

xiaofang.__secret()
```

结果运算如下所示：

```python
Traceback (most recent call last):
  File "D:/netdisk/bioinfo.notes/Python/黑马教程笔记/面向对象/hm_17_私有属性和方法.py", line 15, in <module>
    xiaofang.__secret()
AttributeError: 'Women' object has no attribute '__secret'
```

### 伪私有属性和伪私有方法

在Python中并没有真正意义上的私有：

- 在给属性、方法命名时，实际是对名称做了一些特殊处理，使得外界无法访问到；
- 如果我们要强行访问这些智能属性与方法，处理方式就是在名称前面加上`__类名`=>`_类名__名称`

因此在Python中是有办法访问这些私有属性和私有方法，但在日常开发中，我们最好不要用这种方式来访问对象的私有属性或私有方法。

还来看一下前面的案例，如下所示：

```python
class Women:

    def __init__(self, name):

        self.name = name
        self.__age = 18

    def __secret(self):
        print("%s 的年龄是%d"%(self.name, self.__age))

xiaofang = Women("小芳")

print(xiaofang.__age)

# xiaofang.__secret()
```

运行结果如下所示：

```python
Traceback (most recent call last):
  File "D:/netdisk/bioinfo.notes/Python/黑马教程笔记/面向对象/hm_18_伪私有属性和方法.py", line 13, in <module>
    print(xiaofang.__age)
AttributeError: 'Women' object has no attribute '__age'
```

解释器提示，没有`__age`这个属性，现在我们改变一下代码，也就是将`print(xiaofang.__age)`这句改为`print(xiaofang._Women__age)`，改动的地方就是将私有属性前面添加上类名，并在类名前面再加一个下划线，完整代码如下所示：

```python
class Women:

    def __init__(self, name):

        self.name = name
        self.__age = 18

    def __secret(self):
        print("%s 的年龄是%d"%(self.name, self.__age))

xiaofang = Women("小芳")

print(xiaofang._Women__age)

# xiaofang.__secret()
```

运行结果如下所示：

```python
18
```

通过这种方法，我们就能访问对象的私有属性。再来看一下`__secret`这个私有方法的访问，也是同样的方法，即`xiaofang._Women__secret`，如下所示：

```python
class Women:

    def __init__(self, name):

        self.name = name
        self.__age = 18

    def __secret(self):
        print("%s 的年龄是%d"%(self.name, self.__age))

xiaofang = Women("小芳")

print(xiaofang._Women__age)

xiaofang._Women__secret()
```

运行结果如下所示：

```python
18
小芳 的年龄是18
```

我们现在也能访问这个私有方法，因此在Python中，没有绝对意义上的私有属性与私有方法，因此可以称为`伪私有属性和伪私有方法`。
