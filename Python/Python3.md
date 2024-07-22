# 继承 (inheritance)

观向对象的三大特征

- 封装：根据需求将属性和方法封装到一个抽象的类中；
- 继承：实现代码的重用，也就是说相同部分的代码不需要重复编写；
- 多态：不同的对象调用相同的方法，产生不同的执行结果，增加代码的灵活度。

## 继承的概念、语法和特点

类继承的概念：子类拥有父类的所有方法和属性。

- 子类继承自父类，可以直接享受父类中已经封装好的方法，不需要再次编写相同的代码；
- 子类中应根据需求，封专门讲用子类特有的属性和方法。

### 继承的语法

```python
class 类名(父类):
    
    pass
```

现在我们就以前面的动物类(Animal)与狗类(Dog)说明一下，如下所示：

```python
class Animal:
    def eat(self):
        print("吃")

    def drink(self):
        print("喝")

    def run(self):
        print("跑")

    def sleep(self):
        print("睡")

class Dog(Animal):

    def bark(self):
        print("汪汪叫")

```

### 继承的传递性

**子类**拥有**父类**以及**父类的父类**中封闭的所有属性和方法。

```python
class Animal:
    def eat(self):
        print("吃---")
    def drink(self):
        print("喝---")
    def run(self):
        print("跑---")
    def sleep(self):
        print("睡---")

class Dog(Animal):
    def bark(self):
        print("汪汪叫")

class XiaoTianQuan(Dog):
    def fly(self):
        print("我会飞")
```

## 方法的重写

**重写**父类方法有两种情况：

- **覆盖**父类的方法；
- **扩展**父类的方法。

### 覆盖父类的方法

​        如果在开发中，父类的方兴未艾实现和子类的方法实现完全不同，就可以采用覆盖的方式，在子类中重新编写父类的方法。具体的实现方法就是在子类中定义一个与父类相同的方法，但代码不同。

重写之后，代码运行时只会调用子类中重写的方法，而不会调用父类中封装的方法，下面看一下案例。

```python
class Animal:
    def eat(self):
        print("吃---")

    def drink(self):
        print("喝---")

    def run(self):
        print("跑---")

    def sleep(self):
        print("睡---")

class Dog(Animal):
    def bark(self):
        print("汪汪叫")

class XiaoTianQuan(Dog):

    def fly(self):
        print("我会飞")

    def bark(self):
        print("叫得跟神一样...")

# 创建一个哮天犬对象
xtq = XiaoTianQuan()
xtq.bark()
# 使用XiaoTianQuan类中的叫(bark)这个方法
```

### 扩展方法案例分析

1. 在子类中重写**父类**的方法；
2. 在需要的位置使用`super().父类方法`来调用**父类中同样的方法**；
3. 代码其他的位置针对子类的需求，编写**子类中特有的代码实现方式**。

#### 关于super

- `super`是Python中的一个特殊的类；
- `super()`就是使用`super`类创建出来的对象；
- 最常用的场景就是在重写父类方法时，调用父类中封装好的方法。

```python
def bark(self):
    # 1. 针对子类特有的需求进行编写
    print("神一样的叫唤...")
    # 2. 使用super().调用原本在父类中封装的方法
    super().bark()
    # 3. 增加其他子类的代码
    print("#$@#@!@#@$!!@")
```

## 父类的私有属性和私有方法

1. **子类对象**不能在自己的方法内部**直接**访问父类的**私有属性或私有方法**；
2. **子类对象**可以通过父类的公有方法**间接**访问到**私有属性或私有方法**。

先看下面的一张示意图：

![img](https:////upload-images.jianshu.io/upload_images/3803770-d626457031dcf0d3.png?imageMogr2/auto-orient/strip|imageView2/2/w/205/format/webp)

image

从上面的示意图我们知道，`A`是父类，`B`是`A`的子类，其中：

- `B`的对象不能直接访问`__num2`属性；
- `B`的对象不能在`demo`方法内访问`__num2`的属性；
- `B`的对象可以在`demo`方法内调用父类的`test`方法；
- 父类的`test`方法内部，能够访问`__num2`属性和`__test`方法。

### 案例分析1——子类无法直接访问父类的属性与方法

```python
class A:
    def __init__(self):
        self.num1 = 100
        self.__num2 = 200

    def __test(self):
        print("私有方法　%d %d" %(self.num1, self.__num2))

class B(A):
    pass

# 创建一个子类对象
b = B()
print(b)

# 在外界不能直接访问对象的私有属性/调用私有方法
print(b.__num2)
b.__test()
```

代码运行如下所示：

```python
Traceback (most recent call last):
<__main__.B object at 0x00000170DA378BA8>
  File "D:/netdisk/bioinfo.notes/Python/黑马教程笔记/封装/hm_07_父类的私有属性和私有方法.py", line 20, in <module>
    print(b.__num2)
AttributeError: 'B' object has no attribute '__num2'
```

### 案例分析2——间接访问父类私有属性，调用私有方法

虽然子类不能在自己的方法内部直接访问父类的私有属性和私有方法，但是可以访问其公有属性与公有方法，代码如下所示：

```python
class A:
    def __init__(self):
        self.num1 = 100
        self.__num2 = 200
    def __test(self):
        print("私有方法　%d %d" %(self.num1, self.__num2))

    def test(self):
        print("父类的公有方法")

class B(A):
    def demo(self):
        pass

b = B() # 创建一个子类对象
print(b) # 输出b的内存地址
print(b.num1) # 输出B的父类的公有属性num1
b.test() # 调用父类的公有方法
```

结果运行如下所示：

```python
<__main__.B object at 0x000002291BB00550>
100
父类的公有方法
```

```python
class A:
    def __init__(self):
        self.num1 = 100
        self.__num2 = 200
    def __test(self):
        print("私有方法　%d %d" %(self.num1, self.__num2))
    def test(self):
        print("父类的公有方法")
        
class B(A):
    def demo(self):
        # 1. 在子类的对象方法中，不能访问父类的私有属性
        # print("访问父类的私有属性 %d" % self.__num2)

        # 2. 在子类的对象方法中，不能调用父类的私有方法
        # self.__test()

        # 3. 访问父类的公有属性
        print("子类方法 %d" % self.num1)

        # 4. 调用父类的公有方法
        self.test()
        pass

b = B() # 创建一个子类对象
b.demo()
# print(b) # 输出b的内存地址
# print(b.num1) # 输出B的父类的公有属性num1
# b.test() # 调用父类的公有方法
```

```python
class A:
    def __init__(self):
        self.num1 = 100
        self.__num2 = 200

    def __test(self):
        print("私有方法　%d %d" %(self.num1, self.__num2))

    def test(self):
        print("父类的公有方法 %d" % self.__num2)
        # 在父类中，test()是一个公有方法，而__num2是一个私有属性
        self.__test()

class B(A):
    def demo(self):
        # 1. 在子类的对象方法中，不能访问父类的私有属性
        # print("访问父类的私有属性 %d" % self.__num2)

        # 2. 在子类的对象方法中，不能调用父类的私有方法
        # self.__test()

        # 3. 访问父类的公有属性
        print("子类方法 %d" % self.num1)

        # 4. 调用父类的公有方法
        self.test()
        pass

b = B() # 创建一个子类对象
b.demo()
# print(b) # 输出b的内存地址
# print(b.num1) # 输出B的父类的公有属性num1
# b.test() # 调用父类的公有方法
```

结果运行如下所示：

```python
子类方法 100
父类的公有方法 200
私有方法　100 200
```

## 多继承

### 单继承与多继承

- 子类可以拥有**多个父类**，并且具有**所有父类**的**属性**和**方法**；

![img](https:////upload-images.jianshu.io/upload_images/3803770-373605219c7bf00f.png?imageMogr2/auto-orient/strip|imageView2/2/w/389/format/webp)

#### 多继承的语法

多继承的语法如下所示：

```python
class 子类名(父类名1, 父类名2...)
    pass
```

```python
class A:
    def test(self):
        print("test 方法")

class B:
    def demo(self):
        print("demo 方法")

class C(A, B):
    pass

c = C()
c.test()
c.demo()
```

从结果中我们可以发现，`C`既可以调用`A`中的`test()`方法，也能调用`B`中的`demo()`方法。多继承可以在很大程度上节省代码量。

#### 多继承的注意事项

在使用多继承的时候，我们可以会遇到这样的问题：如果不同的父类中存在同名的方法，子类对象在调用方法时，会调用哪一个父类的方法呢？就像下面的这个样子：

![img](https:////upload-images.jianshu.io/upload_images/3803770-f6a3fa7ddac1ef29.png?imageMogr2/auto-orient/strip|imageView2/2/w/365/format/webp)

`A`中有`test()`方法，`B`中也有`test()`方法，如果`C`同时继承A与B，那么在调用`test()`方法时，就会有问题。

```python
class A:
    def test(self):
        print("A --- test 方法")
    def demo(self):
        print("A --- demo 方法")

class B:
    def demo(self):
        print("B --- demo 方法")
    def test(self):
        print("B --- test 方法")

class C(A, B):
    pass

c = C()
c.test()
c.demo()
```

结果运行如下所示：

```python
A --- test 方法
A --- demo 方法
```

从结果中可以发现，`C`中的`test()`与`demo()`方法都继承自`A`。从`class C(A,B)`这段代码中我们可以知道，`C`是先继承自`A`，再继承自`B`，因此，如果遇到相同名称的方法，那么就是先`A`后`B`，如果我们把代码改为`class C(B,A)`，那么运行结果就是下面的这个样子：

```python
B --- test 方法
B --- demo 方法
```

此时就会发现，`C`中的方法都继承自`B`。但Python中子类关于父类的继承顺序并不是如此简单，举这个例子主要是为了说明，当两个父类中含有相同名称的方法时，慎重选择多继承。

#### MRO

MRO全称是Method Resolution Order，即方法解析顺序。它用于在多继承时判断方法、属性的调用路径。Python中针对类提供了一个内置属性`__mro__`用于查看方法搜索顺序。

在前面的代码中补充一句`print(C.__mro__)`，结果如下所示：

```python
(<class '__main__.C'>, <class '__main__.B'>, <class '__main__.A'>, <class 'object'>)
```

从结果中可以看出以下信息：

1. 输出结果是一个元组；
2. 元组中的顺序就是一些类，分别是C，B，A，原来结果中的运行原理就是，当我们使用`c=C()`来创建一个`C`类的`c`对象时，如果有`C`类，那么就调用`C`类中的方法，如果没有，就按照从左到右的顺序进行搜索，也就是按C，B，A这三个类中方法进行调用，的顺序来进行调用。
3. 最后一个是`<class 'object'>`，它是**Python3中的所有类的基类**，也就是说，只要我们定义了一个类，所有类的基类都是`<class 'object'>`，此处先不详述。
4. 如果查找到最后一个基类，还没有找到相应的类，python就会报错。

### 新式类与旧式（经典）类

上面提到，`object`是Python为所有对象提供的基类，提供有一些内置的属性和方法，可以使用`dir`函数查看。

Python中有新式类与旧式类区分，其中：

- 新式类：以`object`为基类的类，推荐使用；
- 经典类：不以`object`为基类的类，不推荐使用。
- 在`Python 3.x`中定义的类，如果没有指定父类，会默认使用`object`作为该类的基类，`Python 3.x`中定义的类都是新式类；
- 在`Python 2.x`中定义类时，如果没有指定父类，则不会以`object`作为基类。
- 新式类和经典类在多继承时会影响到方法的搜索顺序。
- 为了保证编写的代码能够同时在`Python 2.x`和`Python 3.x`中同时运行，在定义类时，如果没有父类，建议统一继承自`object`类。

现在我们看一下代码：

```python
In [1]: class A(object):
   ...:     pass
   ...:

In [2]: a = A()

In [3]: dir(a)
Out[3]:
['__class__',
 '__delattr__',
 '__dict__',
 '__dir__',
 '__doc__',
 '__eq__',
 '__format__',
 '__ge__',
 '__getattribute__',
 '__gt__',
 '__hash__',
 '__init__',
 '__init_subclass__',
 '__le__',
 '__lt__',
 '__module__',
 '__ne__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__setattr__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 '__weakref__']
```

在上面的代码中我们可以知道，我们开始定义了一个A类，代码为`class A(object):`，从这里可以看出来，这是一个新式类，然后我们使用了`dir(a)`来查看这个类的方法，可以看到许多方法，但是，如果在Python3中，即使你输入的是`class A():`，没有指定继承自`object`，那么A类也是新式类，如下所示：

```python
In [5]: class B:
   ...:     pass
   ...:

In [6]: b = B()

In [7]: dir(b)
Out[7]:
['__class__',
 '__delattr__',
 '__dict__',
 '__dir__',
 '__doc__',
 '__eq__',
 '__format__',
 '__ge__',
 '__getattribute__',
 '__gt__',
 '__hash__',
 '__init__',
 '__init_subclass__',
 '__le__',
 '__lt__',
 '__module__',
 '__ne__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__setattr__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 '__weakref__']
```

# 多态

### 前言

面向对象的三大特征分别是：封装，继承，多态，如下所示：

1. 封装是根据需求将属性和方法封装到一个抽象的类中

2. 继承则能够实现代码的重用，相同的代码不需要重复编写；

3. 多态：不同的子类对象调用相同的父类方法，产生不同的执行结果，多态的思路：

   - 多态可以增加代码的灵活度
   - 以继承和重写父类方法为前提
   - 是调用方法的技巧，不会影响到类的内部设计

   示意图如下所示：

   ![img](https:////upload-images.jianshu.io/upload_images/3803770-9ee1496c159076cd.png?imageMogr2/auto-orient/strip|imageView2/2/w/367/format/webp)


### 多态的案例

现在我们来看一个案例需求：

1. 我们需要定义3个类，分别是人类(Person)，狗类(Dog)，哮天犬类(XiaoTianQuan)类；
2. 狗类(Dog)中封装一个玩耍的方法(game)；
3. 定义一个哮天犬类(XiaoTianQuan)类，这个类继承自(Dog)，但是这个类中的game方法需要进行修改，例如普通玩耍变成飞到天上玩耍；
4. 定义一个人类(Person)，在这个类中，让人(Person)与狗类(Dog)玩耍(game)，示意图如下所示：

![img](https:////upload-images.jianshu.io/upload_images/3803770-8670af674f0e4a97.png?imageMogr2/auto-orient/strip|imageView2/2/w/671/format/webp)

```python
class Dog():
    def __init__(self, name):
        self.name = name
    def game(self):
        print("%s 蹦蹦跳跳的玩耍..." % self.name)

class XiaoTianQuan(Dog):
    def game(self):
        print("%s 飞到天上去玩耍..."% self.name)

class Person():
    def __init__(self,name):
        self.name = name
    def game_with_dog(self,dog):
        print("%s 和 %s 快乐地玩耍..."% (self.name, dog.name))
        # 让狗玩耍
        dog.game()

# 1. 创建狗对象
wangcai = Dog("旺财")
# 创建普通的狗对象

feitianwangcai = XiaoTianQuan("飞天旺财")
# 创建哮天犬类的对象

# 2. 创建一个小明对象
xiaoming = Person("小明")

# 3. 让小明调用和狗玩的方法
xiaoming.game_with_dog(wangcai)
xiaoming.game_with_dog(feitianwangcai)
```

结果运行如下所示：

```python
小明 和 旺财 快乐地玩耍...
旺财 蹦蹦跳跳的玩耍...
小明 和 飞天旺财 快乐地玩耍...
飞天旺财 飞到天上去玩耍...
```

### 实例

面向对象开发步骤如下：

1. 使用面向对象开发，第1步是设计**类**；
2. 使用`类名()`创建对象，**创建对象**的动作有两步：①在内存中为对象分配空间；②调用初始化方法`__init__`为对象初始化；
3. 对象创建后，**内存**中就有一个对象的实实在在的存在，这个存在就是**实例**。

因此，通常也会有以下这种叫法：

1. 创建出来的对象叫做**类**的**实例**；
2. 创建对象的动作叫做**实例化**；
3. 对象的属性叫做**实例属性**；
4. 对象调用的方法叫做**实例方法**。

![img](https:////upload-images.jianshu.io/upload_images/3803770-cc4059b1748de70a.png?imageMogr2/auto-orient/strip|imageView2/2/w/565/format/webp)

在程序执行时：

1. 对象各自拥有自己的**实例属性**；
2. 调用对象方法，可以通过`self`来访问自己的属性，以及调用自己的方法。

**结论**

- 每一个对象都有自己**独立的内存空间**，保存**各自不同的属性**；
- **多个对象的方法，在内存中只有一份**，在调用方法时，需要把对象的引用传递到方法内部。

### 类是一个特殊的对象

在学习Python时，我们常常听到这样的话：

> `Python`中一切皆无明：
>
> - `class AAA:`定义的类属于**类对象**
> - `obj1 = AAA()`属于**实例对象**

- 在程序运行时，**类**同样会被加载到内存中；
- 在`Python`中，**类**是一个特殊的对象——**类对象**；
- 在程序运行时，**类对象**在内存中**只有一份**，使用**一个类**可以创建出**很多个对象实例**；
- 除了封装**实例**的**属性**和**方法**外，**类对象**还可以拥有自己的**属性**和**方法**：即①**类属性**；②**类方法**。
- 通过**类名.**的方式哦可以**访问类的属性**或者**调用类的方法**

## 补充知识：什么是对象

在面向对象的编程中，经常听到的一句话就是“一切皆对象”这句话到底是什么意思，可以看知乎的一个帖子，从Python的源码角度解释的：[《关于python中“赋值就是建立一个对象的引用”，大家怎么看？》](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.zhihu.com%2Fquestion%2F27026782%23answer-9368341)

## 类属性和实例属性

### 概念和使用

- 类属性给类对象中定义的属性
- 通常用来记录与这个类相关的特征
- 类属性不会以用于记录具体对象的特征。

```python
class Tool():
    #  使用赋值语句定义类属性， 记录所有工具对象的数量
    count = 0

    def __init__(self, name):
        self.name = name

        # 让类属性的值 + 1
        Tool.count += 1

# 1. 创建工具对象
tool1 = Tool("斧头")
tool2 = Tool("榔头")
tool3 = Tool("水桶")

# 2. 输出工具对象的总数
print(Tool.count)
```

运行结果如下所示：

```python
3
```

从代码中我们可以看出来，我们先定义了一个`Tool`类，并且定义了相应的类属性，用来记录与这个类相关的特征。

### 属性的获取机制

属性的获取机制指的是，我们要编写代码时，在一个变量的后面接一点，再接这个变量的属性，Python的解释器是如何找到这个值的。

Python中属性的获取存在一个**向上查找机制**。这个机制的具体表现就是，先在对象的属性中查找类属性，如果没有找到类属性，就向上，向类中寻找这个类属性，如下所示：

![img](https:////upload-images.jianshu.io/upload_images/3803770-5ef8af17f91392e3.png?imageMogr2/auto-orient/strip|imageView2/2/w/940/format/webp)

因此，要访问类属性有两种方式：①**类名.类属性**；②**对象.类属性**（不推荐）。

需要注意的是，如何使用`对象.类属性 = 值`这样的赋值语句，只会给对象添加一个属性，而不会影响到类属性的值（从前面的笔记中可以了解到这些内容）。

前面我们看到，当我们输出`print("工具/对象总数 %d" % tool3.count)`这个结果时，结果为`3`，因为通过对象可以访问类的创建数目，但是，如果我们使用了`对象.类属性 = 值`这样的赋值语句时，就会出问题，如下所示：

```python
class Tool():

    #  使用赋值语句定义类属性， 记录所有工具对象的数量
    count = 0

    def __init__(self, name):
        self.name = name

        # 让类属性的值 + 1
        Tool.count += 1


# 1. 创建工具对象
tool1 = Tool("斧头")
tool2 = Tool("榔头")
tool3 = Tool("水桶")

# 2. 输出工具对象的总数
print(Tool.count)
tool3.count = 99
print("工具/对象总数 %d" % tool3.count)
print("===> %d" % Tool.count)
```

结果如下所示：

```python
3
工具/对象总数 99
===> 3
```

从结果可以看出来，`tools.count`它此时就代表的是不是类属性的值（类的值是`Tool.count`），而是又给它赋的值（因为解释器会先查找对象内部的count，如果没有，它再向上，在类中寻找count），因此在访问类的属性时，并不采用这种方式（也就是对过`对象.属性`这样的方式）。

## 类方法和静态方法

### 类方法

- 类属性就是针对类对象定义的属性
  - 使用赋值语句在`class`关键字下方可以定义类属性；
  - 类属性用于记录与这个类相关的特征
- 类方法就是针对类对象定义的方法
  - 在类方法内部可以直接访问类属性或者调用其他的类方法

#### 类方法语法

类方法的语法与实例方法的语法非常类似，如下所示：

```python
@classmethod
def 类方法名(cls)
    pass
```

关于类方法需要注意以下几点：

- 类方法需要用修饰器(`@classmethod`)来标记，**告诉解释器这是一个类方法**；

- 类方法的第一个参数应该是

  ```
  cls
  ```

  - 由哪一个类调用的方法，方法内的`cls`就是哪一个类的引用；
  - 这个参数和实例方法的第一个参数是`self`类似；
  - 提示使用其他名称也可以，不过习惯使用`cls`

- 通过`类名.`调用`类方法`，调用方法时，不需要传递`cls`参数；

- 在方法内部：

  - 可以通过`cls.`访问类的属性；
  - 也可以通过`cls.`调用其他的类方法。

#### 示意需求

现在我们看一下需求：

- 定义一个工具类；
- 每件工具都有自己的`name`；
- 需求——在`类`中封装一个`show_tool_count`的类方法，输出使用当前这个类创建的对象个数，那么需求的大致要求就如下所示：

```python
class Tool():
    #  使用赋值语句定义类属性， 记录所有工具对象的数量
    count = 0

    # 定义一个类方法
    @classmethod
    def show_tool_count(cls):

        print("工具对象的数据 %d" % cls.count)

    def __init__(self, name):
        self.name = name

        # 让类属性的值 + 1
        Tool.count += 1

# 1. 创建工具对象
tool1 = Tool("斧头")
tool2 = Tool("榔头")
tool3 = Tool("水桶")

# 2. 输出工具对象的总数
print(Tool.count)
tool3.count = 99
print("工具/对象总数 %d" % tool3.count)
print("===> %d" % Tool.count)
```

运行结果如下所示：

```python
3
工具/对象总数 99
===> 3
```

从上面的案例我们就知道，在`Tool`这个类内部，我们定义了一个类方法，也就是`@classmethod`这一部分的内容。

### 静态方法

如果我们在开发过程中，在类中封装一个方法，这个方法有以下特性：

- **不需要**访问**实例属性**或者**调用实例方法**；
- **不需要**访问**类属性**或者**调用类方法**。

这个时候，我们就可以把这个方法封装成一个**静态方法**，静态方法的语法格式如下所示：

```python
@staticmethod
def 静态方法名():
    # 需要注意的是，括号里面没有self，也没有cls
     pass
```

- 静态方法需要用修饰器`@staticmethod`来标记，告诉解释器这是一个静态方法；
- 通过`类名.`来调用静态方法。

```python
class Dog():

    @staticmethod
    def run():
        print("小狗要跑...")

# 通过类名.调用静态方法
# 调用静态方法不需要创建对象
Dog.run()
```

结果运行如下所示：

```python
小狗要跑...
```

```python
class Game():
    # 历史最高分
    top_score = 0
    
    def __init__(self, player_name):
        self.player_name = player_name
        
    @staticmethod
    def show_help():
        print("帮助信息： 让僵尸进入大门")

    @classmethod
    def show_top_score(cls):
        print("历史记录 %d" % cls.top_score)

    def start_game(self):
        print("%s 开始游戏了..." % self.player_name)

# 1. 查看游戏的帮助信息
Game.show_help()

# 2. 查看历史最高分
Game.show_top_score()
# 3. 创建游戏对象
game = Game("小明")
game.start_game()
```

结果运行如下所示：

```python
帮助信息： 让僵尸进入大门
历史记录 0
小明 开始游戏了...
```

#### 总结

1. 实例方法——方法内部需要访问实例属性，实例方法内部可以使用`类名.`来访问属性。
2. 类方法——方法内部只需要访问类属性；
3. 静态方法——谅地内部，不需要访问实例属性和类属性。

## 单例

### 单例设计模式

##### 什么是设计模式？

直接引用菜鸟教程里面的话：

> 设计模式（Design pattern）代表了最佳的实践，通常被有经验的面向对象的软件开发人员所采用。设计模式是软件开发人员在软件开发过程中面临的一般问题的解决方案。这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的。

### 单例设计模式的应用场景

- 音乐播放对象（一次只播放一首歌曲）
- 回收站对象（一个电脑通常只有一个回收站）
- 打印机对象（打印一些文件时，通常是在同一台打印机进行打印）
- ……

### `__new__`方法

在Python中，当我们使用`类名()`创建对象时，Python解释器**首先**会调用`__new__`方法为对象**分配空间**。这个`__new__`方法是一个由object基类提供的**内置的静态方法**，它的作用主要有两个：

- 1. 在内存溃为对象分配空间；
- 2)**返回**对象的引用。

当Python的解释器获得对象的引用后，将引用作为**第一个参数**，传递给`__init__`方法。
重写`__new__`方法的代码非常固定，只需要注意以下几点：

- 重写`__new__`方法一定要`return super().__new__(cls)`
- 否则Python解释器**得不到**分配了空间的**对象引用**，就不会调用对象的初始化方法；
- 注意：`__new__`是一个静态方法，在调用时需要**主动传递(cls)**参数。

```python
  class MusicPlayer():
  
      def __init__(self):
          print("播放器初始化")
  
  
  # 创建播放器对象
  player = MusicPlayer()
  
  print(player)
```

运行结果如下所示：

```python
播放器初始化
<__main__.MusicPlayer object at 0x000001C0FDC7B0F0>
```

现在输出了对象中的内容（`播放器初始化`），并且还输出了对象的内存地址。现在我们重写`__new__`方法，如下所示：

```python
class MusicPlayer():

    def __new__(cls, *args, **kwargs):

        # 1. 创建对象时， new方法会被自动调用
        print("创建对象， 分配空间")
        
    def __init__(self):
        print("播放器初始化")

# 创建播放器对象
player = MusicPlayer()

print(player)
```

结果运行如下所示：

```python
创建对象， 分配空间
None
```

此时的结果中出现了`None`，这说明初始化方法并没有被调用（也就是没有出现`播放器初始化`字样）。

现在回顾一下前面的内容，也就是下面这一段文字：

> 重写`__new__`方法一定要`return super().__new__(cls)`

再对照代码，也就是下面的这些内容：

> ```python
> def __new__(cls, *args, **kwargs):
>  # 1. 创建对象时， new方法会被自动调用
>  print("创建对象， 分配空间")
> ```

因此我们在重写`__new__`方法时，并没有写`return super().__new__(cls)`，因此Python解释器就得不到**分配了空间的**对象引用，因此就不会调用对象的初始化方法（也就是输出`播放器初始化`字样），只能输出`None`，现在我们输入下面的代码：

```python
class MusicPlayer():

    def __new__(cls, *args, **kwargs):

        # 1. 创建对象时， new方法会被自动调用
        print("创建对象， 分配空间")

        # 2. 为对象分配空间
        instance = super().__new__(cls)

        # 3. 返回对象的引用
        return instance
```

完整的代码如下所示：

```python
class MusicPlayer():

    def __new__(cls, *args, **kwargs):

        # 1. 创建对象时， new方法会被自动调用
        print("创建对象， 分配空间")

        # 2. 为对象分配空间
        instance = super().__new__(cls)

        # 3. 返回对象的引用
        return instance

    def __init__(self):
        print("播放器初始化")

# 创建播放器对象
player = MusicPlayer()
print(player)
```

上述代码运行结果如下所示：

```python
创建对象， 分配空间
播放器初始化
<__main__.MusicPlayer object at 0x000002F2EDE6B2B0>
```

## Python中的单例

**单例**——让**类**创建的对象，在系统中**只有唯一的一个实例**，它的设计流程如下所示：

1. 定义一个**类属性**，初始值是`None`，用于记录**单例对象的引用**
2. 重写`__new__`方法；
3. 如果**类属性**`is None`，调用父类方法分配空间，并在类属性中记录结果；
4. 返回**类属性**中记录的**对象引用**

### 单例案例分析

在这个案例中，我们会看一下，无论我们创建了多少个实例，它们的内存地址都是一样的，也就是说由这个类创建出来的对象只有一个，如下所示：

```python
class MusicPlayer():

    # 记录第一个被创建对象的引用
    instance = None

    def __new__(cls, *args, **kwargs):

        # 1. 判断类属性是否是空对象
        if cls.instance is None:
            # 2. 调用父类的方法，为第一个对象分配空间
            cls.instance = super().__new__(cls)
        # 3. 返回类属性保存的对象引用
        return cls.instance

    pass

# 创建多个对象
player1= MusicPlayer()
print(player1)

player2 = MusicPlayer()
print(player2)
```

运行结果如下所示：

```python
<__main__.MusicPlayer object at 0x000001CA0EF2B2B0>
<__main__.MusicPlayer object at 0x000001CA0EF2B2B0>
```

### 只执行一次初始化工作

前面的暗到，当我们每次使用`类名()`创建对象时，Python的解释器都会自动调用两个方法：①`__new__`用来分配空间；②`__init__`对象初始化。

在前面一部分我们对`__new__`方法改造之后，每次都会得到**第一次被创建对象的引用**，但是，**初始化方法还会被再次调用**。

如果我们只想让**初始化动作**只被**执行一次**，那么就需要以下解决方法：

1. 定义一个类属性`init_flag`标记是否**执行过初始化动作**，初始值为`False`；
2. 在`__init__`方法中，判断`init_flag`，如果为`False`，就执行初始化动作；
3. 然后将`init_flag`设置为`True`；
4. 这样，再次**自动**调用`__init__`方法时，**初始化动作就不会再次执行了**。

```python
class MusicPlayer():

    # 记录第一个被创建对象的引用
    instance = None

    # 记录是否执行过初始化动作
    init_flag = False

    def __new__(cls, *args, **kwargs):

        # 1. 判断类属性是否是空对象
        if cls.instance is None:
            # 2. 调用父类的方法，为第一个对象分配空间
            cls.instance = super().__new__(cls)
        # 3. 返回类属性保存的对象引用
        return cls.instance

    def __init__(self):
        # 1. 判断是否执行过初始化动作
        if MusicPlayer.init_flag:
            return

        # 2. 如果没有执行过，再执行初始化动作
        print("初始化播放器")

        # 3. 修改类属性的标记
        MusicPlayer.init_flag = True

# 创建多个对象
player1= MusicPlayer()
print(player1)

player2 = MusicPlayer()
print(player2)
```

运行结果如下所示：

```python
初始化播放器
<__main__.MusicPlayer object at 0x000001922204B2B0>
<__main__.MusicPlayer object at 0x000001922204B2B0>
```

将代码更改后，从运行结果中我们就可以发现，初始化动作就只被执行了1次。

---

# 文件

> 文件：就是以硬盘为载体，存储计算机所产生的数据

学习文件的目的：把程序中所产生的数据保存到文件，能够让数据永久存储

永久存储的方式：

1. 文件
2. 数据库，读写性能非常快，效率更高

## 2、文件的读取

> 文件的读取方式为：
>
> - `r` 模式：以字符串的方式读取文件中的数据
> - `rb` 模式：以二进制(字节)的方式读取文件中的数据

### r 模式



```bash
# 第一步：打开文件
file = open("test.txt", "r", encoding="utf-8")
# 查看打开文件使用的编码格式
# cp936 => gbk
print(file.encoding)
# 第二步：读取文件中的数据
result = file.read() # 读取文件中的所有数据
print(result, type(result))
# 第三步: 关闭文件
file.close()
```

运行结果：



```kotlin
utf-8
人生苦短，我用python!
人生苦短，我用python!
人生苦短，我用python!
 <class 'str'>
```

#### r模式的注意点

- 1. 要打开的文件必须存在，否则就崩溃
- 1. 打开文件默认是在当前工程目录下面，如果打开的文件没有在当前工程里面，需要加上文件的路径
- 1. 在`window`的`python`解释器下面，打开文件默认使用的是`gbk`编码，`mac`或者`Linux`的`python`解释器，打开文件默认是`utf-8`

### rb 模式 — 以二进制(字节)的方式读取文件中的数据

> 比如：视频，图片，音频等数据需要使用`rb`模式读取文件中的数据

打开一张图片：



```bash
# # 第一步：打开文件
file = open("11.png", "rb")
# # 第二步：读取文件中的数据
result = file.read()
print(result, type(result))
# # 第三步：关闭文件
file.close()
```

运行结果是一行特别长的二进制代码，我简化一下展示：



```kotlin
b'\xe4\xba\xba\xa8python!\r\n' <class 'bytes'>
```

注意：数据前带`b` 表示字节数据或者二进制数据

**`rb` 模式读取文本文件中的数据：**



```bash
# 第一步：打开文件
file = open("test.txt", "rb")
# 第二步：读取文件中的数据
result = file.read()
print(result, type(result))

# 需要对二进制数据进行解码，转成字符串数据
content = result.decode("utf-8")
print(content, type(content))
# 第三步：关闭文件
file.close()
```

运行结果：



```kotlin
b'\xe4\xba\xba\xe7\x94\x9f\xa8python!\r\n' <class 'bytes'>
人生苦短，我用python!
人生苦短，我用python!
人生苦短，我用python!
 <class 'str'>
```

![img](https:////upload-images.jianshu.io/upload_images/15992481-c83b09bd82b43880.png?imageMogr2/auto-orient/strip|imageView2/2/w/675/format/webp)

## 3、文件的写入

> 向文件写入数据的操作为:

- 1. `w`模式： 以字符串的方式写入数据
- 1. `wb`模式： 以二进制(字节)的方式写入数据

### w模式



```bash
# 第一步：打开文件
file = open("2.txt", "w", encoding="utf-8")
# 查看打开文件，操作文件的编码格式
print(file.encoding)

# 第二步: 向文件中写入数据(字符串)
file.write("hello\n")
file.write("world\n")
file.write("哈哈")

# 第三步: 关闭文件
file.close()
```

#### w模式的注意点：

- 1. 当打开的文件不存在时，默认会创建一个指定的文件
- 1. 当打开的文件存在时，先把原文件中的所有数据清空，然后再写入指定的数据

### wb模式 — 以二进制(字节)的方式写入数据



```bash
# 第一步：打开文件
file = open("2.txt", "wb")

# 第二步: 向文件中写入数据(字符串)
content = "哈哈"
# 把字符串编码成二进制(字节)也就是把字符串转成二进制
content_data = content.encode("utf-8")
file.write(content_data)

# 第三步: 关闭文件
file.close()
```

## 4、文件的追加写入

- a模式： 以字符串的方式追加写入数据
- ab模式： 以二进制(字节)的方式追加写入数据

### a模式 — 以字符串的方式追加写入数据



```bash
# 第一步：打开文件
file = open("3.txt", "a", encoding="utf-8")

# 第二步: 追加写入数据(字符串数据)
file.write("嘻嘻")

# 第三步: 关闭文件
file.close()
```

#### a模式的注意点：

- 当打开的文件不存在时，那么默认会创建一个指定的文件
- 当打开的文件存在时，原文件中的数据会进行保留，在原文件数据的末尾追加写入指定的数据

### ab模式 - 以二进制(字节)的方式追加写入数据



```bash
# 第一步：打开文件
file = open("3.txt", "ab")

# 第二步: 追加写入数据(二进制数据)
content = "哈哈"
# 对字符串进行编码，把字符串转成二进制数据(字节数据)
data = content.encode("utf-8")
print(type(data))
file.write(data)

# 第三步: 关闭文件
file.close()
```

## 5、文件读取数据的其他常用方法

- read方法，需要指定参数
- readline方法：读取一行数据
- readlines方法：读取所有行数据

### read方法带有参数的使用

> read(数据长度)：如果文件的操作模式是r模式，这里的数据长度是字符串的数据长度



```bash
file = open("test.txt", "r", encoding="utf-8")

#6：表示读取6个字符串数据的长度
result = file.read(6)
print(result)

file.close()
```

运行结果：



```undefined
人生苦短，我
```

> read(数据长度)：如果文件的操作模式是rb模式，这里的数据长度是字节的数据长度。

提示：在utf-8编码格式下，一个汉字占用三个字节，一个字母，数字占用1个字节



```bash
file = open("test.txt", "rb")

### 3：表示读取3个字节数据的长度
# 提示：在utf-8编码格式下，一个汉字占用三个字节，一个字母，数字占用1个字节
result = file.read(3)
print(result)
# 对二进制数据进行解码转成字符串
content = result.decode("utf-8")
print(content)

result = file.read(3)
print(result)
# 对二进制数据进行解码转成字符串
content = result.decode("utf-8")
print(content)

file.close()
```

运行结果：



```bash
b'\xe4\xba\xba'
人
b'\xe7\x94\x9f'
生
```

### readlines方法：读取所有行数据



```bash
file = open("test.txt", "r", encoding="utf-8")

# 读取文件中的所有行数据
result = file.readlines()
print(result, type(result))

for row in result:
    print(row)

file.close()
```

运行结果：



```kotlin
['人生苦短，我用python!\n', '人生苦短，我用python!\n', '人生苦短，我用python!\n'] <class 'list'>
人生苦短，我用python!

人生苦短，我用python!

人生苦短，我用python!
```

### readline方法：读取一行数据



```bash
file = open("test.txt", "r", encoding="utf-8")

# 读取文件中的一行数据
result = file.readline()
print(result, type(result))

for row in result:
    print(row)

file.close()
```

运行结果：



```kotlin
人生苦短，我用python!
 <class 'str'>
人
生
苦
短
，
我
用
p
y
t
h
o
n
!
```

## with语句的使用

文件使用完后必须关闭，因为文件对象会占用操作系统的资源，并且操作系统同一时间能打开的文件数量也是有限的，文件在读写时都有可能产生`IOError`，一旦出错，后面的`f.close()`就不会调用。**Python提供了 with 语句的这种写法，既简单又安全，并且 with 语句执行完成以后自动调用关闭文件操作，即使出现异常也会自动调用关闭文件操作。**

#### with 语句的示例代码：



```python
# 1.以写的方式打开文件
with open("1.txt", "w") as f:
    # 2.读取文件内容
    f.write("hello world")
```

## 6、文件的拷贝

我们想把原文件`test.txt`拷贝一份为`test[复件].txt`：



```bash
# 原文件的名字
src_file_name = "test.txt"

# 根据点把原文件的名字分割成三部分
file_name, point, end_str = src_file_name.partition(".")

# 生成目标文件的名字  test[复件].txt
dst_file_name = file_name + "[复件]" + point + end_str
print(dst_file_name)

# 1. 打开目标文件(拷贝后的文件 test[复件].txt)
dst_file = open(dst_file_name, "wb")

# 2. 打开原文件，读取原文件中的数据
src_file = open(src_file_name, "rb")

# 读取原文件中的所有二进制数据
src_file_data = src_file.read()

# 3. 把读取到的原文件数据写入到目标文件(拷贝后的文件 test[复件].txt)
dst_file.write(src_file_data)

# 4. 把文件关闭
src_file.close()
dst_file.close()
```

### 大文件拷贝



```php
# 原文件的名字
src_file_name = "test.txt"

# 根据点把原文件的名字分割成三部分
file_name, point, end_str = src_file_name.partition(".")

# 生成目标文件的名字  test[复件].txt
dst_file_name = file_name + "[复件]" + point + end_str
print(dst_file_name)

# 1. 打开目标文件(拷贝后的文件 test[复件].txt)
dst_file = open(dst_file_name, "wb")

# 2. 打开原文件，读取原文件中的数据
src_file = open(src_file_name, "rb")

while True:
    # 读取原文件中的所有二进制数据
    src_file_data = src_file.read(1024) # 每次读取最大字节数是1024
    # if len(src_file_data) > 0:
    # if 判断的简写
    if src_file_data:
        # 3. 把读取到的原文件数据写入到目标文件(拷贝后的文件 test[复件].txt)
        dst_file.write(src_file_data)
    else:
        print("文件拷贝完毕")
        # 提示：如果读取到数据长度是0表示文件读取完成
        break
# 4. 把文件关闭
src_file.close()
dst_file.close()
```

## if 判断的扩展

- if语句结合 bool 类型
- if语句结合容器类型
- if语句结合非零即真
- if语句结合 None

#### if语句结合 bool 类型



```php
if False:
    print("ok")
else:
    print("error")
```

运行结果：



```undefined
error
```

#### if语句结合容器类型

> 说明：如果 `if` 语句结合容器类型使用，容器类型里面有数据表示条件成立，否则容器类型里面没有数据表示条件失败

容器类型：字符串，列表，元组，字典，集合，range, 二进制
 **if语句结合字符串：**



```bash
my_value = "b"
if my_value:
    print("ok")
else:
    print("error")

################ 运行结果 ################

ok
```

**if语句结合range：**



```bash
my_range = range(0)

if my_range:
    print("ok")
else:
    print("error")

################ 运行结果 ################

error
```

**if语句结合集合：**



```bash
my_set = set()
if my_set:
    print("ok")
else:
    print("error")

################ 运行结果 ################

error
```

**if语句结合列表：**



```bash
my_list = [1,3,5]
if my_list:
    print("ok")
else:
    print("error")

################ 运行结果 ################

ok
```

## 7、文件和文件夹的相关操作

**操作文件和文件夹的模块：**



```swift
import os
```

**文件和文件夹操作的高级模块：**



```swift
import shutil
```

### 创建空的文件



```go
file = open("abcd.txt", "wb")
file.close()
```

### 重命名



```css
import os

###将abcd.txt文件重命名为aaa.txt
os.rename("abcd.txt", "aaa.txt")
```

### 删除文件



```swift
import os
os.remove("1.txt")
```

#### 创建文件夹



```swift
import os
os.mkdir("AAA")
```

#### 查看当前操作目录的路径



```swift
import os
path = os.getcwd()

print(path)
```

#### 切换目录



```swift
import os
os.chdir("AAA")

path = os.getcwd()
print(path)
```

#### 切换到上一级路径



```swift
import os
os.chdir("..")
path = os.getcwd()

print(path)
```

#### 删除文件



```swift
import os
os.remove("AAA/abcd.txt")
```

#### 删除非空文件夹



```swift
import os
os.rmdir("CCC")
```

#### 删除空目录



```swift
import os
os.rmdir("BBB")
```

#### 删除非空目录



```swift
import shutil
shutil.rmtree("CCC")
```

#### 查看目录下的所有文件名



```python
import os

# 默认查看当前目录下的所有文件名
result = os.listdir()  #等价于 result = os.listdir(".")
print(result)
```

#### 查看指定目录下的所有文件名



```swift
import os

result = os.listdir("CCC")
print(result)
```

#### 判断指定文件是否存在



```swift
import os
result = os.path.exists("4.txt")
print(result)
```

#### 判断指定文件夹是否存在



```swift
import os
result = os.path.exists("AA")
print(result)
```

#### 获取指定文件的文件名和文件的后缀



```swift
import os
file_name, end_str = os.path.splitext("3.txt")
print(file_name, end_str)
```

#### 判断是否是文件



```swift
import os
result = os.path.isfile("DD")
print(result)
```

#### 判断是否是文件夹



```swift
import os
result = os.path.isdir("3.txt")
print(result)
```

## 批量修改文件名

> 我们把`test`目录下的所有文件名前面都加上一个`[黑马出品]-`：



```python
import os

# 1. 获取指定目录下的所有文件名
file_name_list = os.listdir("test")
print(file_name_list)

# 切换到test目录里面
path = os.getcwd()
print(path)

os.chdir("test")

path = os.getcwd()
print(path)

# 2. 遍历文件名列表获取每一个文件名，然后依次对文件进行重命名
for file_name in file_name_list:
    # 生成重命名后的文件名
    rename_file_name = "[黑马出品]-" + file_name
    print(file_name, rename_file_name)
    # 使用rename函数进行重命名
    os.rename(file_name, rename_file_name)
```

#### 学生管理系统文件版



```python
# 学生管理系统的分析：
# 1. 每个学生的信息使用字典进行存储
# 2. 对于学生管理系统，是需要管理不同学生的，所以学生管理系统里面需要在定义列表，使用列表管理不同的学生字典信息
import os

# 定义全局变量的学生列表
student_list = []


# 显示学生管理系统的功能菜单
def show_menu():
    print("--------------学生管理系统V1.0------------")
    print("1. 添加学生")
    print("2. 修改学生")
    print("3. 删除学生")
    print("4. 查询学生")
    print("5. 显示所有学生")
    print("6. 退出")


# 添加学生
def add_student():
    name = input("请输入学生的姓名:")
    age = input("请输入学生的年龄:")
    sex = input("请输入学生的性别:")

    # 使用字典存储每个学生的信息
    student_dict = {}
    # 添加键值对
    student_dict["name"] = name
    student_dict["age"] = age
    student_dict["sex"] = sex

    # 把学生字典添加到学生列表里面
    student_list.append(student_dict)
    print("添加成功")


# 显示所有学生的信息
def show_all_students():
    # 遍历学生列表，显示每一个学生信息
    for index, student_dict in enumerate(student_list):
        # 学号 = 下标 + 1
        student_no = index + 1
        print("学号: %d 姓名: %s 年龄: %s 性别: %s" % (student_no,
                                               student_dict["name"],
                                               student_dict["age"],
                                               student_dict["sex"]))


# 修改学生信息
def modify_student():
    # 接收用户输入学生的学号
    student_no = int(input("请输入您要修改学生的学号:"))

    # 计算学生列表的下标 = 学号 - 1
    index = student_no - 1

    # 判断下标的范围是否是一个合法的范围
    if index >= 0 and index < len(student_list):

        # 根据下标获取对应的学生字典信息
        student_dict = student_list[index]

        # 重新接收用户输入的学生信息
        new_name = input("请输入修改后学生的姓名:")
        new_age = input("请输入修改后学生的年龄:")
        new_sex = input("请输入修改后学生的性别:")

        # 修改字典信息
        student_dict["name"] = new_name
        student_dict["age"] = new_age
        student_dict["sex"] = new_sex

        print("修改成功")
    else:
        print("请输入合法的学号！")


# 删除学生信息
def remove_student():
    # 接收用户输入学号
    student_no = int(input("请输入要删除学生的学号:"))
    # 根据学号计算学生的下标
    index = student_no - 1

    # 判断下标的范围是否是一个合法的范围
    if index >= 0 and index < len(student_list):
        # 根据下标删除对应的学生信息
        del student_list[index]
        print("删除成功")
    else:
        print("请输入合法的学号！")


# 查询学生
def query_student():
    # 接收用户输入的学生姓名
    name = input("请输入要查询学生的姓名:")
    # 遍历列表获取每一个学生字典信息
    for index, student_dict in enumerate(student_list):
        if student_dict["name"] == name:
            # 生成学生的学号
            student_no = index + 1
            # 代码执行到此，说明找到了学生信息
            print("学号: %d 姓名: %s 年龄: %s 性别: %s" % (student_no,
                                                   student_dict["name"],
                                                   student_dict["age"],
                                                   student_dict["sex"]))
            break
    else:
        print("对不起，您查找的学生信息不存在!")


# 保存列表数据到文件
def save_data():
    # 1. 打开文件
    file = open("student_list.data", "w", encoding="utf-8")
    # 2. 把列表数据转成字符串保存到文件
    student_list_str = str(student_list)
    print(student_list_str, type(student_list_str))
    file.write(student_list_str)
    # 3. 关闭文件
    file.close()


# 加载缓存文件中的数据
def load_data():
    # 1. 判断缓存文件是否存在
    if os.path.exists("student_list.data"):
        # 2. 如果缓存文件存在，读取文件中的数据
        file = open("student_list.data", "r", encoding="utf-8")
        file_data = file.read()
        # 3. 把读取到数据给全局变量(student_list)
        # "[{'name': '李四', 'age': '20', 'sex': '男'}, {'name': '王五', 'age': '20', 'sex': '男'}]"
        # 如果字符串列表转成列表
        new_student_list = eval(file_data)

        # 把读取到列表中的每一个数据添加到全局变量列表里面
        student_list.extend(new_student_list)


# 程序的入口函数，程序第一个要执行的函数
def start():
    # 程序启动后，加载文件中的数据只加载一次即可
    load_data()
    while True:
        # 显示学生管理系统的功能菜单
        show_menu()

        # 接收用户输入的功能选项
        menu_option = input("请输入您需要的功能选项:")

        # 判断功能选项
        if menu_option == "1":
            print("添加学生")
            add_student()
        elif menu_option == "2":
            print("修改学生")
            modify_student()
        elif menu_option == "3":
            print("删除学生")
            remove_student()
        elif menu_option == "4":
            print("查询学生")
            query_student()
        elif menu_option == "5":
            print("显示所有学生")
            show_all_students()
        elif menu_option == "6":
            print("退出")
            # 把列表数据保存到文件，可以做到永久存储
            save_data()
            break

# 启动学生管理系统
start()

# 快捷键：ctr + - 把函数的代码折叠起来， ctr + + 把函数的代码展开
# 快捷键：ctr + shift + - 把所有函数的代码折叠起来， ctr + shift + + 把所有函数的代码展开
```



作者：黄晶_id
链接：https://www.jianshu.com/p/b784035aa6af
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。