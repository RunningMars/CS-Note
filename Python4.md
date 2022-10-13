# 异常

程序在运行时，如果`Python解释器`遇到了一个错误，Python解释器就会停止程序的执行，并且提示一些错误信息，这就是**异常**。

程序停止执行并提示错误信息的这个动作我们称之为**抛出异常**。

<img src="https:////upload-images.jianshu.io/upload_images/3803770-bc44df60e98618c1.png?imageMogr2/auto-orient/strip|imageView2/2/w/574/format/webp" alt="img" style="zoom:67%;" />

## 捕获异常语法

在程序开发中，如果对某些代码执行不能确定是否正确，可以增加`try`来捕获异常，捕获异常最简单的语法格式如下所示：

```python
try:
    尝试执行的代码
except:
    出现错误的处理
```

### 捕获异常的案例

```python
try:
    num = int(input("请输入一个整数： "))
except:
    print("请输入正确的整数： ")

print("-" *50)
```

现在我们输入一个`a`，结果如下所示：

```python
请输入一个整数： a
请输入正确的整数： 
--------------------------------------------------
```

## 错误类型的捕获

在程序执行时，可能遇到**不同类型的异常**，并且需要**针对不同类型的异常，做出不同的响应**，这个时候就需要捕获错误类型了，这种情况下的语法格式如下所示：

```python
try:
    # 尝试执行的代码
    pass
except 错误类型1:
    # 针对错误类型1，对应的代码处理
    pass
except (错误类型2, 错误类型3):
    # 针对错误类型2和3，对应的代码处理
    pass
except Exception as result:
    print("未知错误 %s" % result)
```

### 异常类型捕获案例

```python
try:
    # 提示用户输入一个整数
    num = int(input("输入一个整数： "))


    # 使用 8 除以用户输入的整数并输入
    result = 8/num
    print(result)

except ZeroDivisionError:
    print("除0错误")

except ValueError:
    print("输入的不是整数")
```

现在我们运行代码，输入一个字母`a`，如下所示：

```python
输入一个整数： a
输入的不是整数
```

这个案例说明了针对不止一种错误类型的处理方式。

### 捕获未知错误

在开发中，我们要预判到所有可能出现的错误有一定难度。如果希望程序无论出现任何错误，都**不会因为Python解释器抛出异常而被终止**，我们可能再增加一个except，如下所示：

```python
except Exception as result:
    print("未知错误 %s" % result)
```

现在我们把前面的案例中的那个除数为0的错误删除，使用这种未知错误来写一下，如下所示：

```python
try:
    # 提示用户输入一个整数
    num = int(input("输入一个整数： "))

    # 使用 8 除以用户输入的整数并输入
    result = 8/num
    print(result)

except ValueError:
    print("输入的不是整数")

except Exception as result:
    print("未知错误 %s" % result)
```

运行结果如下所示：

```python
输入一个整数： 0
未知错误 division by zero
```

因此，在捕获异常时，最好在最后加上这么这么一句，即`except Exception as result:`，其中`Exception`是Python是有关错误的一个类。

## 异常捕获的完整语法

在实际开发中，为了能够处理复杂的异常情况，完整的异常语法如下所示：

```python
try:
    # 尝试执行的代码
    pass
except 错误类型1:
    # 针对错误类型1，对应的代码处理
    pass
except 错误类型2:
    # 针对错误类型2，对应的代码处理
    pass
except (错误类型3，错误类型4):
    # 针对错误类型3和4，对应的代码处理
    pass
except Exception as result:
    # 打印错误信息
    print(result)
else:
    # 没有异常才会执行的代码
    pass
finally:
    # 无论是否有异常，都会执行的代码
    print("无论是否有异常，都会执行的代码")
```

其中：

- `else`只有在没有异常时才会执行的代码；
- `finally`无论是否有异常，都会执行的代码。

现在我们求救一下前面案例的**完整捕获异常**，代码如下所示：

```python
try:
    # 提示用户输入一个整数
    num = int(input("输入一个整数： "))

    # 使用 8 除以用户输入的整数并输入
    result = 8/num
    print(result)

except ValueError:
    print("输入的不是整数")

except Exception as result:
    print("未知错误 %s" % result)
else:
    print("尝试成功")
finally:
    print("无论是否出现错误，都会执行的代码")

print("-" * 50)
```

现在我们输入数字`1`，结果如下所示：

```python
输入一个整数： 1
8.0
尝试成功
无论是否出现错误，都会执行的代码
--------------------------------------------------
```

## 异常的传递

异常的传递是指，当**函数/方法**执行出现异常时，会将异常传递给函数/方法的调用一方，此时程序还不会被终止，如果传递到主程序，仍然没有异常处理，这个时候，程序才会被终止。

现在我们来验证一下异常的传递，先来看一下需求：

1. 定义函数`demo1()`，提示用户输入一个整数并且返回；
2. 定义函数`demo2()`，调用`demo1()`；
3. 在主程序中调用`demo2()`。

完整代码如下所示：

```python
def demo1():
    return int(input("输入整数： "))

print(demo1())
```

```python
输入整数： 1
1
```

程序运行正常。现在我们输入一个字母`a`，如下所示：

```python
输入整数： a
Traceback (most recent call last):
  File "D:/netdisk/bioinfo.notes/Python/黑马教程笔记/异常/hm_05_异常的传递.py", line 4, in <module>
    print(demo1())
  File "D:/netdisk/bioinfo.notes/Python/黑马教程笔记/异常/hm_05_异常的传递.py", line 2, in demo1
    return int(input("输入整数： "))
ValueError: invalid literal for int() with base 10: 'a'
```

程序报错，最下面的错误信息是`ValueError: invalid literal for int() with base 10: 'a'`，这是说明，输入的字母无法被`int()`函数识别。

我们再往上看，有`line 2, in demo1 return int(input("输入整数： "))`字样，也就是说错误出现在了第2行，第2行在转换整数的时候出现了问题。但是，第2行代码出现问题的时候，会把异常交给第4行，也就是解释器出现的这个信息`line 4, in <module> print(demo1())`。第4行代码是主程序，也就是`print(demo1())`，它是在调用第2行的函数，第2行代码本身是不做异常处理的，主程序中没有做异常处理，因此程序就终止了。

现在我们再改造一下原代码，如下所示：

```python
def demo1():
    return int(input("输入整数： "))

def demo2():
    return demo1()

print(demo1())
```

现在我们再输入一个字母`a`，如下所示：

```python
输入整数： a
Traceback (most recent call last):
  File "D:/netdisk/bioinfo.notes/Python/黑马教程笔记/异常/hm_05_异常的传递.py", line 9, in <module>
    print(demo2())
  File "D:/netdisk/bioinfo.notes/Python/黑马教程笔记/异常/hm_05_异常的传递.py", line 6, in demo2
    return demo1()
  File "D:/netdisk/bioinfo.notes/Python/黑马教程笔记/异常/hm_05_异常的传递.py", line 2, in demo1
    return int(input("输入整数： "))
ValueError: invalid literal for int() with base 10: 'a'
```

从错误信息中我们可以知道这些信息：

1. 出错的代码还是第2行，提示demo1函数出现的问题；
2. 错误信息往上，我们发现了第2行代码出错时，会把错误传递给第6行，因为第6行中的demo2()函数调用了demo1()函数，继续提示demo2出现了问题，但此时我们并没有在demo2内部针对异常进行处理，此时继续向上传递，传递到了第9行；
3. 第9行传递到了主程序，也就是`print(deom2())`。

从上面的结果我们可以知道，一旦出现异常，异常会一级一级向上传递，一直到主程序，直到主程序终止运行。

如果我们在实际设计程序中，如果在每一个函数中都设计异常处理，这样代码量会非常大，不现实。因此我们可以利用异常的传递性，可以在主程序中捕获异常即可，如下所示：

```python
def demo1():
    return int(input("输入整数： "))

def demo2():
    return demo1()

# 利用异常的传递性，在主程序捕获异常
try:
    print(demo2())
except Exception as result:
    print("未知3错误 %s" % result)
```

现在运行程序，输入一个字母`a`，如下所示：

```python
输入整数： a
未知错误 invalid literal for int() with base 10: 'a'
```

从结果中我们可以发现，我们只在主程序中添加了异常捕获，就不用在每个函数中对异常进行捕获了。

## 抛出异常

在开发中，除了代码执行出错，Python解释会自己抛出异常之外。我们还可以根据应用程序特有的功能来主动抛出异常，现在我们来看一个案例。

例如我们要添加一个用户登录模块，里面有一个函数，这个函数的功能是：提示用户输入密码，如果长度少于8，那么就抛出异常，如下所示：

![img](https:////upload-images.jianshu.io/upload_images/3803770-1b4a95838506b6f4.png?imageMogr2/auto-orient/strip|imageView2/2/w/894/format/webp)

这里需要注意的是，当前这个函数**只负责**提示用户输入密码，如果密码长度不正确，需要其他的函数进行额外处理。

### 抛出异常

Python中提供了一个`Exception`异常类。在开发时，如果满足特定功能需求时，希望抛出异常，可以做两件事情：

1. 创建一个`Exception`的**对象**；
2. 使用`raise`关键字抛出**异常对象**。

```python
def input_password():

    # 1. 提示用户输入密码
    pwd = input("请输入密码： ")

    # 2. 判断密码长度 >= 8， 返回用户输入的密码
    if len(pwd) >= 8:
        return pwd

    # 3. 如果密码 < 8，主动抛出异常
    print("主动抛出异常")
    # 第一步：创建异常对象-可以使用错误信息字符串作为参数
    ex = Exception("密码长度不够")

    # 第二步：主动抛出异常
    raise ex

# 提示用户输入密码
try:
    print(input_password())
except Exception as result:
    print(result)
```

继续运行，还是输入123，如下所示：

```python
请输入密码： 123
主动抛出异常
密码长度不够
```



---

# 模块

- Python 模块(Module)，是一个 Python 文件，以 .py 结尾，包含了 Python 对象定义和Python语句；
- 模块名同样也是一个标识符，需要符合标识符的命名规则；
- 在模块中定义的全局变量、函数、类都是提供给外界直接使用的工具；
- 模块就好比工具包，要想使用这个工具包中的工具，就需要先导入这个模块。

## 模块的导入

模块的导入要用到`import`命令，如下所示：

```python
import 模块名1
import 模块名2
```

当模块导入后，通过`模块名.`就可以使用模块提供的工具，例如全局变量、函数和类。

### 模块导入案例

```python
# 全局变量
title = "模块1"

# 函数
def say_hello():
    print("我是 %s" % title)

# 类
class Dog():
    pass
```

`hm_02_测试模块2.py`代码如下所示：

```python
# 全局变量
title = "模块2"

# 函数
def say_hello():
    print("我是 %s" % title)

# 类
class Cat():
    pass
```

现在创建导入模块的文件，即`hm_03_import导入模块.py`文件，代码如下所示：

```python
import hm_01_测试模块1
import hm_02_测试模块2

hm_01_测试模块1.say_hello()
hm_02_测试模块2.say_hello()

dog = hm_01_测试模块1.Dog()
print(dog)

cat = hm_02_测试模块2.Cat()
print(cat)
```

运行结果如下所示：

```python
我是 模块1
我是 模块2
<hm_01_测试模块1.Dog object at 0x0000021F03B475F8>
<hm_02_测试模块2.Cat object at 0x0000021F03B47668>
```

### 使用as指定模块的别名

如果一个模块的名字太长，可以使用`as`指定模块的名称，以方便在代码中的使用，格式如下：

```python
import 模块名1 as 模块名
```

需要注意的是，给模块取别名的时候，要使用大驼峰命名法，还是以前面的案例为例说明一下：

```python
import hm_01_测试模块1 as DogModule # 将hm_01_测试模块1重命名为DogModule
import hm_02_测试模块2 as CatModule

DogModule.say_hello()
CatModule.say_hello()

dog = DogModule.Dog()
print(dog)

cat = CatModule.Cat()
print(cat)
```

运行结果如下所示：

```python
我是 模块1
我是 模块2
<hm_01_测试模块1.Dog object at 0x0000026D1FFF85F8>
<hm_02_测试模块2.Cat object at 0x0000026D1FFF8668>
```

需要注意的是，输出的结果中，模块名称还是原来的名称，例如`hm_01_测试模块1`。

### 导入模块中的部分工具

当我们遇到一种情况，也就是说，只想导入某一模块中的一部分东西，不想全部导入的情况。此时就需要使用`from...import`这种方式。

前面提到的`import 模块名`这种方式是**一次性**把模块中的所有工具全部导入，并且通过`模块名/别名`来访问。现在提到的这个`from...import`的使用语法如下所示：

```python
# 从模块中导入某一工具
from 模块名1 import 工具名
```

在这种情况下，当我们导入某个模块的部分工具后，不需要通过`模块名.部分工具`来访问，可以直接使用。

现在还是看前面的案例，如下所示：

```python
from hm_01_测试模块1 import Dog
from hm_02_测试模块2 import say_hello

say_hello()
wangcai = Dog()
print(wangcai)
```

运行结果如下所示：

```python
我是 模块2
<hm_01_测试模块1.Dog object at 0x0000026122974F98>
```

但是，有一种情况需要注意，如果两个模块存在**同名的函数**，**最后导入的函数**会覆盖**先前导入的函数**，因此一旦出现这种情况，最好使用`as`经其中的一个取一个别名，现在看案例：

```python
# from hm_01_测试模块1 import say_hello
from hm_02_测试模块2 import say_hello as module2_say_hello
from hm_01_测试模块1 import say_hello

say_hello()
module2_say_hello()
```

运行结果如下所示：

```python
我是 模块1
我是 模块2
```

这样就避免了同名函数的覆盖问题。

### 导入模块的所有内容

如果我们使用`from...import *`时，就可以导入某个模块的所有内容。这与前面的`import 模块名`的区别就在，`import 模块名`导入后，需要通过`模块名.函数/类`才能访问某个函数，而使用`from...import *`则是直接访问即可，不用加`模块名.`，如下所示：

```python
from hm_01_测试模块1 import *

print(title)
say_hello()

wangcai = Dog()
print(wangcai)
```

运行结果如下所示：

```python
模块1
我是 模块1
<hm_01_测试模块1.Dog object at 0x000001ACD3C14F98>
```

但是这种方法不建议使用，因为一旦出现函数重名，就会出问题。

## 模块的搜索顺序

当Python解释器导入一个模块时，它会遵循一定的规则来搜索相应的导入顺序：

1. 搜索**当前目录**指模块名的文件，如果有就直接导入；
2. 如果没有，再搜索**系统目录**。

在Python中，每一个模块都有一个内置属性`__file__`可以查看模块的**完整路径**。

当我们在实际开发时，给文件取名时，**不要和系统的模块文件重名**。

来看一个案例，这个代码文件我们命名为`hm_08_模块的搜索顺序.py`：

```python
import random

# 生成一个 0 ~ 10的数字
rand = random.randint(0, 10)

print(rand)
```

运行结果如下所示：

```python
3
```

当我们运行上面的代码时，`Python解释器`会加载系统目录中下的`random.py`模块。如果我们在当前目录下也有一个`random.py`文件，那么上面的代码就不会运行，此时我们在这个文件的同一个目录下新建一个`random.py`文件，输入以下内容：

```python
print("Hello, python")
```

再次运行前面的那个代码，也就是`hm_08_模块的搜索顺序.py`文件，就出现了错误，如下所示：

```python
Hello, python
Traceback (most recent call last):
  File "D:\netdisk\bioinfo.notes\Python\黑马教程笔记\模块\hm_08_模块的搜索顺序.py", line 4, in <module>
    rand = random.randint(0, 10)
AttributeError: module 'random' has no attribute 'randint'
```

错误信息显示，`random.py`这个模块不含有`randint`函数。这是因为当前目录下已经有了`random.py`这个文件，Python解释器就不再导入系统目录中的`random.py`这个模块。

这里还需要注意的是，当我们导入了`random.py`这个模块时，它的内容`Hello, python`也被显示了出来，这个后面我们会再次提到，这里先放下。

现在我们来查看一下这个`random.py`到底是哪个，就用到了`__file__`这个属性，如下所示：

```python
import random

print(random.__file__)
```

结果如下所示：

```python
Hello, python
D:\netdisk\bioinfo.notes\Python\黑马教程笔记\模块\random.py
```

从结果我们可以看出来，这个`random.py`文件来源于`D:\netdisk\bioinfo.notes\Python\黑马教程笔记\模块\`这个目录。

现在我们把当前目录的`random.py`这个文件删除，再运行代码，如下所示：

```python
C:\Anaconda3\lib\random.py
```

从结果中我们可以发现，现在`random.py`这个文件来源于`C:\Anaconda3\lib\random.py`这个目录。

## 模块的开发原则

一个独立的Python文件就是一个模块，因此每一个我们写的Python文件都应该是可以被导入的。在实际工作中，当我们每开发一个Python时，它都可以被导入，这样就会极大地提高工作效率。

当一个Python被当作模块被导入时，这个Python文件中**所有没有任何缩进的代码都会被执行一遍**。现在我们来说明一下这句话是什么意思。

现在我们在一个目录中新建一个文件，命名为`hm_09_name模块.py`，输入以下内容：

```python
print("小明开发的模块")
```

在同一个目录中再新建一个文件，命名为`hm_10_name测试导入.py`，输入以下内容：

```python
import hm_09_name模块

print("-" * 50)
```

现在我们运行`hm_10_name测试导入.py`文件，结果如下所示：

```python
小明开发的模块
--------------------------------------------------
```

从结果中我们可以知道以下信息：

1. `hm_10_name测试导入.py`文件导入了`hm_09_name模块.py`这个模块后，`hm_09_name模块.py`中的代码`print("小明开发的模块")`就自动执行了；
2. 根据我们前面提到的内容，也就是“当一个Python被当作模块被导入时，这个Python文件中**所有没有任何缩进的代码都会被执行一遍**”这句话，因为在`hm_09_name模块.py`中，`print("小明开发的模块")`这句话前面是没有空格的，它就被自动执行了，现在我们改变一下代码，如下所示：

```python
def say_hello():
    print("你好，我是Say hello函数")

print("小明开发的模块")
say_hello()
```

再次运行`hm_10_name测试导入.py`文件，如下所示：

```python
小明开发的模块
你好，我是Say hello函数
--------------------------------------------------
```

从结果中我们还可以发现，`say_hello()`这个函数前面也是没有空格的，也被自动执行了。但是，我们想要的最终结果中时，不想让它执行，这就涉及到Python的`__name__`属性。

### `__name__`属性

`__name__`是Python的一个内置属性，记录着一个字符串，如果**一个Python是被其他文件导入的**，`__name__`就是模块名，如果是**当前执行的程序**，`__name__`就是`__main__`。

现在看一下前面的案例，解释一下上面的这段话是什么意思。

在`hm_09_name模块.py`中输入以下代码：

```python
def say_hello():
    print("你好，我是Say hello函数")

# 如果直接执行这个模块，那么__name__就是_main__
print(__name__)
print("小明开发的模块")
say_hello()
```

结果如下所示：

```python
__main__
小明开发的模块
你好，我是Say hello函数
```

从结果中我们可以发现，`hm_09_name模块.py`这个模块中的`print(__name__)`输出的结果是`__main__`。

现在我们执行`hm_10_name测试导入.py`这个文件，结果如下所示：

```python
hm_09_name模块
小明开发的模块
你好，我是Say hello函数
--------------------------------------------------
```

从结果中我们可以发现，执行`hm_10_name测试导入.py`这个文件，`print(__name__)`输出的结果是`hm_09_name模块`。

从上面的结果我们就可以知道了前面那段话的意思：

> `__name__`是Python的一个内置属性，记录着一个字符串，如果一个Python是被其他文件导入的，`__name__`就是模块名，如果是**当前执行的程序**，`__name__`就是`__main__`。

那么如果我们想要这样的一个目的：怎么样才能做到，模块测试的代码（也就是`print("小明开发的模块")`和`say_hello()`这两句代码）只有在执行模块时才会被运行？

那么我们就需要先判断`__name__`这个属性内容，如果`__name__`的内容是`__main__`，就说明我们当前在执行这个模块。因此我们可以在`hm_09_name模块.py`中添加一个段if语句来判断`__name__`内容，如下所示：

```python
def say_hello():
    print("你好，我是Say hello函数")

if __name__ == "__main__":

    print(__name__)
    print("小明开发的模块")
    say_hello()
```

结果运行如下所示：

```python
__main__
小明开发的模块
你好，我是Say hello函数
```

现在我们再切换到`hm_10_name测试导入.py`中，代码如下所示：

```python
import hm_09_name模块

print("-" * 50)
```

代码运行如下所示：

```python
--------------------------------------------------
```

现在`hm_10_name测试导入.py`就不再运行原来的些内容了。原来的运行结果如下所示：

```python
hm_09_name模块
小明开发的模块
你好，我是Say hello函数
--------------------------------------------------
```

对比一下两者的运行结果，就明白了。这里我们再说一下，如果在网络上查找一些Python原代码文件，有的时候会看到以下这样的格式：

```python
# 导入模块
# 定义全局变量
# 定义类
# 定义函数

# 在代码的最下文
def main():
    # ...
    pass

# 根据 __name__判断是否执行下方代码
if __name__ == "__main__":
    main()
```

如果出现了这样格式的代码，就说明了这样的代码不仅能够作为一个独立文件进行测试，还可以被当作模块导入其它的文件中。

## 包

### 包的概念

包是一个包含多个模块的特殊目录，在这个目录下有一个特殊的文件，命名为`__init__.py`，包的命名方式和变量一致，通常是`小写字母_xxxx`。

使用包的好处就是，可以通过`import包名`这样的方式一次性导入包中**所有的模块**。

### 案例使用

现在来演示一下包，需求如下：

1. 新建一个`hm_message`的包；
2. 在目录下，新建两个文件`send_message`和`receive_message`；
3. 在`send_message`文件中定义一个`send`函数；
4. 在`receive_message`文件中定义一个`receive`函数；
5. 在外部直接导入`hm_message`包。

#### 第一步：新建一个包目录

在PyCharm中单右键，新建一个`Directory`，命名为`hm_message`，如下所示：

<img src="https:////upload-images.jianshu.io/upload_images/3803770-763d6e0e6ade7da4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1055/format/webp" alt="img" style="zoom:33%;" />

新建后的包如下所示：

<img src="https:////upload-images.jianshu.io/upload_images/3803770-62ee9bf71e1be8cd.png?imageMogr2/auto-orient/strip|imageView2/2/w/766/format/webp" alt="img" style="zoom:50%;" />

#### 第二步：在包中新建一个`__init__.py`文件

包中必须要包含一个`__init__.py`文件，因此要在包这个目录上单击右键，新建一个python文件，取名为`__init__.py`，如下所示：

<img src="https:////upload-images.jianshu.io/upload_images/3803770-25ef425028b786db.png?imageMogr2/auto-orient/strip|imageView2/2/w/755/format/webp" alt="img" style="zoom:50%;" />

现在我们已经创建了一个包的基本结构。现在删除这个包，看另外一种创建包的方式。那就是右键，新建`Python Package`，如下所示：

<img src="https:////upload-images.jianshu.io/upload_images/3803770-b6ee0beb623e4716.png?imageMogr2/auto-orient/strip|imageView2/2/w/994/format/webp" alt="img" style="zoom:50%;" />

这样的话，PyCharm就能自动创建一个`__init__.py`文件。

#### 第三步：创建相应的Python文件

由于我们在需求中提到以下内容：

1. 新建一个`hm_message`的包；
2. 在目录下，新建两个文件`send_message`和`receive_message`；
3. 在`send_message`文件中定义一个`send`函数；
4. 在`receive_message`文件中定义一个`receive`函数；
5. 在外部直接导入`hm_message`包。

现在新建`send_message.py`文件与`receive_message.py`文件，现在在`send_message.py`文件中输入以下内容：

```python
def send(text):
    print("正在发达 %s..." % text)
```

这样我们就在`send_message.py`文件中就定义了`send()`这个函数。

现在在`receive_message.py`文件中输入以下代码：

```python
def receive():
    return "这是来自于 10xxx的 短信"
```

这样我们就在`receive_message.py`中创建了`receive()`这个函数。

#### 第四步：写入`__init__.py`文件

如果想让上述的包`hm_message`能够顺利地被其他文件所调用，那么我们还需要更改一下`__init__.py`文件，输入以下代码：

```python
from . import send_message
from . import receive_message
```

这个代码表示在`__init__.py`中导入`send_message.py`和`receive_message.py`文件。

现在我们新建一个Python文件，命名为`hm_11_导入包.py`，导入这个包试一下，代码如下所示：

```python
import hm_message

hm_message.send_message.send("Hello")
txt = hm_message.receive_message.receive()
print(txt)
```

运行结果如下所示：

```python
正在发达 Hello...
这是来自于 10xxx的 短信
```

上述过程就是创建一个包，并且调用的过程，需要注意的就是包(package)中`__init__.py`文件的书写。

## 发布模块

如果希望自己开发的模块分享给其他的人，那么就需要按以下步骤操作即可。

### 制作发布压缩包的步骤

现在我们把前面的那个包`hm_message`制作成一个可以分享的压缩包。

我们先新建一个`New Project`，命名为`12_发布模块`，现在把原来的那个包文件复制到这个目录中去，如下所示：

![img](https:////upload-images.jianshu.io/upload_images/3803770-191c6c5aa6daff04.png?imageMogr2/auto-orient/strip|imageView2/2/w/477/format/webp)

#### 第一步：制作setup.py文件

新建一个`setup.py`文件，这个文件的内容非常固定，如下所示：

```python
from distutils.core import setup

setup(name="hm_message",  # 包名
      version="1.0",  # 版本
      description="itheima's 发送和接收消息模块",  # 描述信息
      long_description="完整的发送和接收消息模块",  # 完整描述信息
      author="itheima",  # 作者
      author_email="itheima@itheima.com",  # 作者邮箱
      url="www.itheima.com",  # 主页
      py_modules=["hm_message.send_message",
                  "hm_message.receive_message"])
setup()`这个函数接收的参数是多值字典参数，即`**attrs`。
```

`setup（）`中有包的名称，版本，作者等等信息。最后一行是`py_modules`信息，这是一个列表，列表里面是这个包的不同模块名称。

#### 第二步：构建模块

现在通过命令行进入模块所在的目录，输入`python setup.py build`，运行后的结果如下所示：

注意：这是在Windows下的cmd命令行运行的，不是在Linux环境下。

```shell
D:\netdisk\bioinfo.notes\Python\黑马教程笔记\12_发布模块>python setup.py build
running build
running build_py
creating build
creating build\lib
creating build\lib\hm_message
copying hm_message\__init__.py -> build\lib\hm_message
copying hm_message\send_message.py -> build\lib\hm_message
copying hm_message\receive_message.py -> build\lib\hm_message
```

输入`tree`命名，查看文件目录，如下所示：

```python
D:\netdisk\bioinfo.notes\Python\黑马教程笔记\12_发布模块>tree
Folder PATH listing for volume 新加卷
Volume serial number is C0C6-2E4F
D:.
├───.idea
│   └───dictionaries
├───build
│   └───lib
│       └───hm_message
└───hm_message
    └───__pycache__
```

#### 第三步：生成发布压缩包

执行`python setup.py sdist`命名，回车，所示信息如下所示：

```python
D:\netdisk\bioinfo.notes\Python\黑马教程笔记\12_发布模块>python setup.py sdist
running sdist
running check
warning: sdist: manifest template 'MANIFEST.in' does not exist (using default file list)

warning: sdist: standard file not found: should have one of README, README.txt

writing manifest file 'MANIFEST'
creating hm_message-1.0
creating hm_message-1.0\hm_message
making hard links in hm_message-1.0...
hard linking setup.py -> hm_message-1.0
hard linking hm_message\__init__.py -> hm_message-1.0\hm_message
hard linking hm_message\receive_message.py -> hm_message-1.0\hm_message
hard linking hm_message\send_message.py -> hm_message-1.0\hm_message
creating dist
Creating tar archive
removing 'hm_message-1.0' (and everything under it)
```

输入`tree`命名，再次查看目录信息，如下所示：

```python
D:\netdisk\bioinfo.notes\Python\黑马教程笔记\12_发布模块>tree
Folder PATH listing for volume 新加卷
Volume serial number is C0C6-2E4F
D:.
├───.idea
│   └───dictionaries
├───build
│   └───lib
│       └───hm_message
├───dist
└───hm_message
    └───__pycache__
```

可以发现，又出现了一个`dist`目录，输入`tree /f`命令查看整个目录下的内容，如下所示：

```python
D:\netdisk\bioinfo.notes\Python\黑马教程笔记\12_发布模块>tree /f
Folder PATH listing for volume 新加卷
Volume serial number is 000001E9 C0C6:2E4F
D:.
│   MANIFEST
│   setup.py
│
├───.idea
│   │   12_发布模块.iml
│   │   misc.xml
│   │   modules.xml
│   │   workspace.xml
│   │
│   └───dictionaries
│           20161111.xml
│
├───build
│   └───lib
│       └───hm_message
│               receive_message.py
│               send_message.py
│               __init__.py
│
├───dist
│       hm_message-1.0.tar.gz
│
└───hm_message
    │   receive_message.py
    │   send_message.py
    │   __init__.py
    │
    └───__pycache__
            receive_message.cpython-36.pyc
            send_message.cpython-36.pyc
            __init__.cpython-36.pyc
```

我们可以发现，dist目录下面有一个压缩文件，也就是`hm_message-1.0.tar.gz`，至此，发布包的任务完成。

### 安装模块

视频中的安装过程是在Linux环境下的，下面只输入安装包的命令，如下所示：

```python
cp 12_发布模块/dist/*. # 复制dist目录下所有的压缩文件到当前目录
tar zxvf hm_message-1.0.tar.gz # 解压
# 解压后的文件中有一个PKG-INFO文件，这个文件里面记录了Python包的一些信息，其实就是package-information的缩写
sudo python setup.py install # 安装模块
# 这一步的包会被安装的python的系统目录
```

### 卸载模块

进入模块所在的目录，使用`rm`删除即可。

这个时候，如果要查看这个模块所有的上当，就需要查看一下`__file__`文件即可。

### pip安装第三方模块

第三方椟通常是指由知名的第三方团队开发的并且被程序员广泛使用的Python包/模块。例如pygame就是一套非常成熟的游戏开发模块。

pip是一个通过的Python包管理工作，它提供到了对Python包的查找、下载、安装、卸载等功能。

#### pip安装和卸载命令如下：

```python
# Ubuntu环境
# 安装python3环境的pip
sudo pip3 install pygame

#卸载pip
sudo pip3 uninstall pygame

# Linux环境安装ipython
sudo apt install ipython3
```

---

# 文件的基本操作

### 操作文件的步骤

在计算机中要操作文件的套路非常固定，一共包含三个步骤：

1. 打开文件；
2. 读、写文件：**读**是指将文件内容读入内存；**写**将内存内容写入文件；
3. 关闭文件。

### 操作文件的函数/方法

在Python中要担任文件需要记住1个函数和3个方法，如下所示：

| 序号 | 函数/方法 | 说明                         |
| ---- | --------- | ---------------------------- |
| 01   | open      | 打开文件，并返回文件操作对象 |
| 02   | read      | 将文件内容读取到内存         |
| 03   | write     | 将指定内容写入文件           |
| 04   | close     | 关闭文件                     |

其中`open`函数负责打开文件，并返回文件对象，而`read/write/close`这三个方法都需要通过`文件对象`来调用。

## read方法——读取文件

1. `open`函数的第一个参数是要打开的文件（文件名是区分大小写的），如果文件**存在**，返回**文件操作对象**，如果文件**不存在**，会**抛出异常**。
2. `read`文件可以一次性读入并返回文件的所有内容。
3. `close`方法负责关闭文件。如果**忘记关闭文件，会造成系统资源消耗，而且会影响到后续对文件的访问**。因此，当我们`open()`文件后，就在最后面输入`close()`，对应起来，再写中间部分，这样会避免忘记关闭文件。

需要注意的是，方法执行后，会把**文件指针**移动到**文件的末尾**。

现在来看一个案例，我们新建一个`README.txt`文件，如下所示：



```python
hello python!
hello world!
```

在同一个目录下新建一个python文件，命名为`hm_01_读取文件.py`，如下所示：



```python
# 1. 打开文件
file = open("README")

# 2. 读取文件内容
text = file.read()
print(text)

# 3. 关闭文件
file.close()
```

运行结果如下所示：



```python
hello python!
hello world!
```

### 文件指针

文件指针会标记从哪个位置开始读取数据，第一次打开文件时，通常文件指针会指向文件的开始位置，如下所示：

![img](https:////upload-images.jianshu.io/upload_images/3803770-ca0dbcbfa3ee6010.png?imageMogr2/auto-orient/strip|imageView2/2/w/296/format/webp)

image

当执行了`read`方法后，文件指标会移动到读取内容的末尾，默认情况下会移动到文件末尾，如下所示：

![img](https:////upload-images.jianshu.io/upload_images/3803770-8b4127efc0b82f03.png?imageMogr2/auto-orient/strip|imageView2/2/w/339/format/webp)

image

因此，当我们执行了一次`read`方法后，读取了所有内容，那么再次调用`read`方法，就无法获取内容，因此此时文件指标已经移到了文件的末尾。现在我们来验证一下，还是改造原来的代码：



```python
# 1. 打开文件
file = open("README.txt")

# 2. 读取文件内容
text = file.read()
print(text)

print("-" * 50)

text = file.read()
print(text)

# 3. 关闭文件
file.close()
```

运行结果如下所示：



```python
Hello, python!
Hello, world!
--------------------------------------------------
```

从结果中我们可以发现，第二次调用`read`方法后，没有内容读取。我们还可以看一下读取文件内容的长度，如下所示：



```python
# 1. 打开文件
file = open("README.txt")

# 2. 读取文件内容
text = file.read()
print(text)
print(len(text)) #输出读取文件长度

print("-" * 50)

text = file.read()
print(text)
print(len(text))

# 3. 关闭文件
file.close()
```

运行结果如下所示：



```python
Hello, python!
Hello, world!
29
--------------------------------------------------

0
```

从结果也可以发现，第二次读取的长度是0。

## 打开文件的方式

`open`在默认的情况下是以只读的方式打开文件，并且返回文件对象，语法如下所示：



```python
f = open("文件名", "访问方式")
```

| 模式 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| r    | 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。 |
| rb   | 以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。 |
| r+   | 打开一个文件用于读写。文件指针将会放在文件的开头。           |
| rb+  | 以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。 |
| w    | 打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| wb   | 以二进制格式打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| w+   | 打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| wb+  | 以二进制格式打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。 |
| a    | 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| ab   | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| a+   | 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。 |
| ab+  | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。 |

## 写入文件

现在我们向`README.txt`文件中写入`HELLO`，如下所示：



```python
# 1. 打开文件
file = open("README.txt", "w")

# 2. 写入文件
file.write("HELLO")

# 3. 关闭文件
file.close()
```

查看一下`README.txt`文件，我们使用了`w`参数来写入文件，如果原文件中有内容，那么就会覆盖，如果没有这个文件，就新建。

现在我们来看一下参数`a`，这个参数的功能在于以追加的方式写入文件，如下所示：



```python
# 1. 打开文件
file = open("README.txt", "a")

# 2. 写入文件
file.write("123HELLO")

# 3. 关闭文件
file.close()
```

现在我们打开文件`README.txt`，结果为`HELLO123HELLO`。如果分别在参数`r,w,a`的后面添加上`+`号，即`r+, w+, a+`，那么就会以**读写**的方式打开文件，这会造成频繁地移动文件指标，影响文件的读写效率，在开发中，更多的是采用**只读**或**只写**的方式来操作文件。

## 按行读取文件内容

`read`方法会默认把文件的**所有内容一次性读取到内存**，如果文件太大，对内存的占用会非常严重。此时我们可以使用`readline`方法。

### readline方法

`readline`方法可以一次读取一行内容，方法执行后，会把**文件指针**移动到下一行，准备再次读取。因此我们在读取大文件时的代码通常是如下所示：



```python
file = open("README.txt")

while True:
    text = file.readline()

    # 判断是否读取到内容
    if not text:
        break

    print(text)

file.close()
```

结果运行如下所示：



```python
Hello1

Hello2

Hello3
```

## 文件读写案例——复制文件

在这个案例中，使用代码的方式来实现文件的复制过程。如果我们要复制的源文件是一个小文件，那么我们就可以使用`read`方法直接把文件的内容全部读取下来，再写入到另外一个文件中，如下所示：



```python
# 1. 打开文件
file_read = open("README.txt")
file_write = open("README[复制]", "w")

# 2. 读、写
text = file_read.read()
file_write.write(text)

# 3. 关闭文件
file_read.close()
file_write.close()
```

现在在同一个目录下出现了`README[复制].txt`这个文件，里面的内容与`README.txt`完全相同。

再看一个案例，这个是案例是复制大文件。

如果要复制大文件，就不能使用`read`方法，因为这会对内存造成很大的压力，因此可以使用`readline`方法。

现在我们在源文件`README.txt`中输入以下内容：



```python
Hello1
Hello2
Hello345
```

运行复制大文件的代码，如下所示：



```python
# 1. 打开文件
file_read = open("README.txt")
file_write = open("README[复制]", "w")

# 2. 读、写
while True:
    # 读取一行内容
    text = file_read.readline()

    # 判断是否读取到内容
    if not text:
        break

    file_write.write(text)

# 3. 关闭文件
file_read.close()
file_write.close()
```

运行后，打开复制好的文件，`README[复制]`，结果与源文件一样。

## 文件/目录的常用管理操作

在**终端/浏览器**中可以执行常规的**文件/目录**操作，例如创建、重命名、删除、改变路径、查看目录内容等，在python中，如果要实现上述功能，通常使用`os`模块。

Python中的目录操作有这些：

| 序号 | 方法及描述                                                   |
| ---- | ------------------------------------------------------------ |
| 1    | os.access(path, mode)：检验权限模式                          |
| 2    | os.chdir(path)：改变当前工作目录                             |
| 3    | os.chflags(path,   flags)：设置路径的标记为数字标记。        |
| 4    | os.chmod(path, mode)：更改权限                               |
| 5    | os.chown(path, uid, gid)：更改文件所有者                     |
| 6    | os.chroot(path)：改变当前进程的根目录                        |
| 7    | os.close(fd)：关闭文件描述符 fd                              |
| 8    | os.closerange(fd_low,   fd_high)：关闭所有文件描述符，从 fd_low (包含) 到 fd_high (不包含), 错误会忽略 |
| 9    | os.dup(fd)：复制文件描述符 fd                                |
| 10   | os.dup2(fd, fd2)：将一个文件描述符 fd   复制到另一个 fd2     |
| 11   | os.fchdir(fd)：通过文件描述符改变当前工作目录                |
| 12   | os.fchmod(fd,   mode)：改变一个文件的访问权限，该文件由参数fd指定，参数mode是Unix下的文件访问权限。 |
| 13   | os.fchown(fd, uid,   gid)：修改一个文件的所有权，这个函数修改一个文件的用户ID和用户组ID，该文件由文件描述符fd指定。 |
| 14   | os.fdatasync(fd)：强制将文件写入磁盘，该文件由文件描述符fd指定，但是不强制更新文件的状态信息。 |
| 15   | os.fdopen(fd[, mode[,   bufsize]])：通过文件描述符 fd 创建一个文件对象，并返回这个文件对象 |
| 16   | os.fpathconf(fd,   name)：返回一个打开的文件的系统配置信息。name为检索的系统配置的值，它也许是一个定义系统值的字符串，这些名字在很多标准中指定（POSIX.1,   Unix 95, Unix 98, 和其它）。 |
| 17   | os.fstat(fd)：返回文件描述符fd的状态，像stat()。             |
| 18   | os.fstatvfs(fd)：返回包含文件描述符fd的文件的文件系统的信息，像   statvfs() |
| 19   | os.fsync(fd)：强制将文件描述符为fd的文件写入硬盘。           |
| 20   | os.ftruncate(fd,   length)：裁剪文件描述符fd对应的文件, 所以它最大不能超过文件大小。 |
| 21   | os.getcwd()：返回当前工作目录                                |
| 22   | os.getcwdu()：返回一个当前工作目录的Unicode对象              |
| 23   | os.isatty(fd)：如果文件描述符fd是打开的，同时与tty(-like)设备相连，则返回true,   否则False。 |
| 24   | os.lchflags(path,   flags)：设置路径的标记为数字标记，类似 chflags()，但是没有软链接 |
| 25   | os.lchmod(path, mode)：修改连接文件权限                      |
| 26   | os.lchown(path, uid,   gid)：更改文件所有者，类似 chown，但是不追踪链接。 |
| 27   | os.link(src, dst)：创建硬链接，名为参数   dst，指向参数 src  |
| 28   | os.listdir(path)：返回path指定的文件夹包含的文件或文件夹的名字的列表。 |
| 29   | os.lseek(fd, pos, how)：设置文件描述符   fd当前位置为pos, how方式修改: SEEK_SET 或者 0 设置从文件开始的计算的pos; SEEK_CUR或者 1 则从当前位置计算;   os.SEEK_END或者2则从文件尾部开始. 在unix，Windows中有效 |
| 30   | os.lstat(path)：像stat(),但是没有软链接                      |
| 31   | os.major(device)：从原始的设备号中提取设备major号码   (使用stat中的st_dev或者st_rdev field)。 |
| 32   | os.makedev(major,   minor)：以major和minor设备号组成一个原始设备号 |
| 33   | os.makedirs(path[,   mode])：递归文件夹创建函数。像mkdir(), 但创建的所有intermediate-level文件夹需要包含子文件夹。 |
| 34   | os.minor(device)：从原始的设备号中提取设备minor号码   (使用stat中的st_dev或者st_rdev field )。 |
| 35   | os.mkdir(path[,   mode])：以数字mode的mode创建一个名为path的文件夹.默认的 mode 是 0777 (八进制)。 |
| 36   | os.mkfifo(path[,   mode])：创建命名管道，mode 为数字，默认为 0666 (八进制) |
| 37   | os.mknod(filename[, mode=0600,   device])：创建一个名为filename文件系统节点（文件，设备特别文件或者命名pipe）。 |
| 38   | os.open(file, flags[,   mode])：打开一个文件，并且设置需要的打开选项，mode参数是可选的 |
| 39   | os.openpty()：打开一个新的伪终端对。返回 pty 和   tty的文件描述符。 |
| 40   | os.pathconf(path,   name)：返回相关文件的系统配置信息。      |
| 41   | os.pipe()：创建一个管道. 返回一对文件描述符(r, w)   分别为读和写 |
| 42   | os.popen(command[, mode[,   bufsize]])：从一个 command 打开一个管道 |
| 43   | os.read(fd, n)：从文件描述符 fd 中读取最多 n   个字节，返回包含读取字节的字符串，文件描述符 fd对应文件已达到结尾, 返回一个空字符串。 |
| 44   | os.readlink(path)：返回软链接所指向的文件                    |
| 45   | os.remove(path)：删除路径为path的文件。如果path   是一个文件夹，将抛出OSError; 查看下面的rmdir()删除一个 directory。 |
| 46   | os.removedirs(path)：递归删除目录。                          |
| 47   | os.rename(src, dst)：重命名文件或目录，从 src   到 dst       |
| 48   | os.renames(old,   new)：递归地对目录进行更名，也可以对文件进行更名。 |
| 49   | os.rmdir(path)：删除path指定的空目录，如果目录非空，则抛出一个OSError异常。 |
| 50   | os.stat(path)：获取path指定的路径的信息，功能等同于C   API中的stat()系统调用。 |
| 51   | os.stat_float_times([newvalue])：决定stat_result是否以float对象显示时间戳 |
| 52   | os.statvfs(path)：获取指定路径的文件系统统计信息             |
| 53   | os.symlink(src, dst)：创建一个软链接                         |
| 54   | os.tcgetpgrp(fd)：返回与终端fd（一个由os.open()返回的打开的文件描述符）关联的进程组 |
| 55   | os.tcsetpgrp(fd,   pg)：设置与终端fd（一个由os.open()返回的打开的文件描述符）关联的进程组为pg。 |
| 56   | os.tempnam([dir[,   prefix]])：Python3 中已删除。返回唯一的路径名用于创建临时文件。 |
| 57   | os.tmpfile()：Python3   中已删除。返回一个打开的模式为(w+b)的文件对象 .这文件对象没有文件夹入口，没有文件描述符，将会自动删除。 |
| 58   | os.tmpnam()：Python3   中已删除。为创建一个临时文件返回一个唯一的路径 |
| 59   | os.ttyname(fd)：返回一个字符串，它表示与文件描述符fd   关联的终端设备。如果fd 没有与终端设备关联，则引发一个异常。 |
| 60   | os.unlink(path)：删除文件路径                                |
| 61   | os.utime(path,   times)：返回指定的path文件的访问和修改的时间。 |
| 62   | os.walk(top[, topdown=True[,   onerror=None[, followlinks=False]]])：输出在文件夹中的文件名通过在树中游走，向上或者向下。 |
| 63   | os.write(fd, str)：写入字符串到文件描述符 fd中.   返回实际写入的字符串长度 |

目录操作的函数太多，用到的时候再学习，下面只列出几个简单的案例：

### 显示某文件夹下的所有文件名

代码如下：



```python
C:\Users\20161111>type practice.py
import os
for filename in os.listdir('d:/Software'):
    print(filename)
    
C:\Users\20161111>python practice.py
office_tools
Professional_tools
ProgramTool
SnapGene 3.2.1 Win
system_enhance
windows_iso
谷歌批量翻译
```

### 创建某个目录



```python
>>> import os
>>> os.mkdir("d:/Software/test")
>>> exit()
C:\Users\20161111>d:
D:\>cd software
D:\software>dir
 Volume in drive D is 新加卷
 Volume Serial Number is C0C6-2E4F

 Directory of D:\software

2018/05/24  09:33    <DIR>          .
2018/05/24  09:33    <DIR>          ..
2018/05/08  13:46    <DIR>          office_tools
2018/05/21  10:30    <DIR>          Professional_tools
2018/05/08  13:46    <DIR>          ProgramTool
2017/11/22  20:21    <DIR>          SnapGene 3.2.1 Win
2018/05/13  21:38    <DIR>          system_enhance
2018/05/24  09:33    <DIR>          test # 刚刚创建的文件夹
2018/05/11  00:48    <DIR>          windows_iso
2017/11/02  10:52    <DIR>          谷歌批量翻译
               1 File(s)              0 bytes
              10 Dir(s)  49,307,074,560 bytes free
```

### 删除某个文件夹

命令`rmdir`，如下所示：



```python
>>> import os
>>> os.rmdir('d:/Software/test')
>>> exit()

D:\software>dir
 Volume in drive D is 新加卷
 Volume Serial Number is C0C6-2E4F

 Directory of D:\software

2018/05/25  12:40    <DIR>          .
2018/05/25  12:40    <DIR>          ..
2018/05/08  13:46    <DIR>          office_tools
2018/05/21  10:30    <DIR>          Professional_tools
2018/05/08  13:46    <DIR>          ProgramTool
2017/11/22  20:21    <DIR>          SnapGene 3.2.1 Win
2018/05/13  21:38    <DIR>          system_enhance
2018/05/11  00:48    <DIR>          windows_iso
2017/11/02  10:52    <DIR>          谷歌批量翻译
               1 File(s)              0 bytes
               9 Dir(s)  48,131,309,568 bytes free
```

把`D:\Software\test`这个文件夹删除了 。

### 重命名某个文件

如下所示：



```python
D:\netdisk\bioinfo.notes\Python\黑马教程笔记\13_文件>ipython
Python 3.6.3 |Anaconda, Inc.| (default, Oct 15 2017, 03:27:45) [MSC v.1900 64 bit (AMD64)]
Type 'copyright', 'credits' or 'license' for more information
IPython 6.2.1 -- An enhanced Interactive Python. Type '?' for help.

In [1]: import os

In [2]: os.rename("README.txt","TEST.txt")

In [3]: ls
 Volume in drive D is 新加卷
 Volume Serial Number is C0C6-2E4F

 Directory of D:\netdisk\bioinfo.notes\Python\黑马教程笔记\13_文件

2019/06/09  16:51    <DIR>          .
2019/06/09  16:51    <DIR>          ..
2019/06/09  16:46    <DIR>          .idea
2019/06/09  15:39               164 hm_01_读取文件.py
2019/06/09  16:19               229 hm_02_读取文件后文件指针会改变.py
2019/06/09  16:25               129 hm_03_写入文件
2019/06/09  16:37               177 hm_04_分行读取文件.py
2019/06/09  16:41               224 hm_05_复制文件.py
2019/06/09  16:44               347 hm_06_复制大文件.py
2019/06/09  16:46                24 README[复制]
2019/06/09  16:45                24 TEST.txt
               8 File(s)          1,318 bytes
               3 Dir(s)  52,836,925,440 bytes free
```

### 显示当前目录内的所有文件

使用`os`模块下的`listdir`命令，如下所示：



```python
In [4]: os.listdir(".")
Out[4]:
['.idea',
 'hm_01_读取文件.py',
 'hm_02_读取文件后文件指针会改变.py',
 'hm_03_写入文件',
 'hm_04_分行读取文件.py',
 'hm_05_复制文件.py',
 'hm_06_复制大文件.py',
 'README[复制]',
 'TEST.txt']
```

### 判断文件是目录还是文件

使用`os`模块中的`path.isdir`命令，如下所示：



```python
In [5]: os.path.isdir("TEST.txt")
Out[5]: False

In [6]: os.path.isdir(".idea")
Out[6]: True
```

## eval函数

`eval()`函数可以接受一个字符串，将其当成有效的表达式来求值，并返回计算结果，来看一下面代码的运行结果：



```python
In [1]: # 基本的数学计算

In [2]: eval("1 + 1")
Out[2]: 2

In [3]: # 字符串重复

In [4]: eval("'*' * 10")
Out[4]: '**********'

In [5]: # 将字符串转换成列表

In [6]: type(eval("[1, 2, 3, 4, 5]"))
Out[6]: list

In [7]: # 将字符串转换成字典

In [8]: type(eval("{'name':'xiaoming', 'age':18}"))
Out[8]: dict
```

### eval案例——计算器

需求如下：

1. 提示用户输入一个加减乘除混合运行；
2. 返回过计算结果。

代码如下所示：



```python
input_str = input("请输入算术题:  ")

print(eval(input_str))
```

运行结果如下所示：



```python
请输入算术题:  (5+4)*5
45
```

### 不要滥用eval

在开发过程中，不要使用`eval`来直接转换`input`结果，现在我们来看一下为什么不要这么做。

我们先来看一下这行代码（由于本人是在windows环境下运行的，因此使用的是`dir`命令，它等于同Linux环境下的`ls`命令，这一点与视频中的不一样）：



```python
__import__('os').system('dir')
```

上面的这行代码等于：



```python
import os
os.system("终端命令")
```

如果执行成功，返回0，执行失败，返回错误信息。现在还看上面的计算器案例，运行后，如果我们输入的内容是`__import__('os').system('dir')`，我们看一下计算的结果：



```python
C:\Anaconda3\python.exe D:/netdisk/bioinfo.notes/Python/黑马教程笔记/13_文件/hm_08_eval计算器.py
请输入算术题:  __import__('os').system('dir')
 驱动器 D 中的卷是 新加卷
 卷的序列号是 C0C6-2E4F

 D:\netdisk\bioinfo.notes\Python\黑马教程笔记\13_文件 的目录

2019/06/09  19:54    <DIR>          .
2019/06/09  19:54    <DIR>          ..
2019/06/09  19:57    <DIR>          .idea
2019/06/09  15:39               164 hm_01_读取文件.py
2019/06/09  16:19               229 hm_02_读取文件后文件指针会改变.py
2019/06/09  16:25               129 hm_03_写入文件
2019/06/09  16:37               177 hm_04_分行读取文件.py
2019/06/09  16:41               224 hm_05_复制文件.py
2019/06/09  16:44               347 hm_06_复制大文件.py
2019/06/09  19:54                68 hm_08_eval计算器.py
2019/06/09  16:46                24 README[复制]
2019/06/09  16:45                24 TEST.txt
               9 个文件          1,386 字节
               3 个目录 52,828,184,576 可用字节
```

现在我们再换一个命令输入，输入`__import__('os').system('cd.>a.txt')`，结果就会在同一个目录下创建了一个名为`a.txt`的文本文件。

现在我们再换一个命令，输入`__import__('os').system('del a.txt')`，此时就会把刚刚创建的那个`a.txt`文件删除。

此时我们应该就明白了，使用`eval()`函数直接转换`input`的结果了，如果用户使用这个函数直接调用`os`模块中的命令，就会执行任何终端命令，一旦操作出现失误，会造成严重的后果。



作者：backup备份
链接：https://www.jianshu.com/p/fb2b04ffb729
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



# 正则表达式

### 1. 正则表达式的介绍

在实际开发过程中经常会有查找符合某些复杂规则的字符串的需要，比如:邮箱、图片地址、手机号码等，这时候想匹配或者查找符合某些规则的字符串就可以使用正则表达式了。

### 2. 正则表达式概念

正则表达式就是记录文本规则的代码

### 3. 正则表达式实例

`0\d{2}-\d{8}` 这个就是一个正则表达式，表达的意思是匹配的是座机号码

### 4. 正则表达式的特点

- 正则表达式的语法很令人头疼，可读性差
- 正则表达式通用行很强，能够适用于很多编程语言

## re模块的介绍

首先要导入模块：`import re`
 `match`方法进行匹配操作用法：`result = re.match(正则表达式,要匹配的字符串)`
 如果上一步匹配到数据的话，可以使用`group`方法来提取数据：`result.group()`



```csharp
### 导入re模块
import re

### 使用match方法进行匹配操作
result = re.match(正则表达式,要匹配的字符串)

### 如果上一步匹配到数据的话，可以使用group方法来提取数据
result.group()
```

### rem模块的使用

从字符串`huangjing6344@novogene.com`中匹配出`huangjing`：



```python
import re

# 使用match方法进行匹配操作
result = re.match("huangjing","huangjing6344@novogene.com")
# 获取匹配结果
info = result.group()
print(info)
```

运行结果：



```undefined
huangjing
```

再来，如果我想从字符串`huangjing6344@novogene.com`中匹配出`novogene`就会报错：



```python
import re

# 使用match方法进行匹配操作
result = re.match("novogene","huangjing6344@novogene.com")
# 获取匹配结果
info = result.group()
print(info)
```

报错结果：



```csharp
Traceback (most recent call last):
  File "C:/Users/huangjing00liang/Desktop/练习.py", line 6, in <module>
    info = result.group()
AttributeError: 'NoneType' object has no attribute 'group'
```

**为什么呀？匹配出`huangjing`和`novogene`按说是一个道理呀？**
 **因为 `re.match()` 根据正则表达式从头开始匹配字符串数据**

**错误分析：**
 属性错误：'NoneType' 对象没有属性 'group'
 说明由`re.match()`函数返回给变量`match`的是一个空的类型，所以在调用`group()`方法时会报错。
 为什么会返回一个空变量呢？
 是因为这个函数是从一个字符串的开始位置匹配正则表达式，然而这个字符串的起始位置并不符合正则表达式，所以匹配失败，返回一个空变量，空变量并没有`group()`方法，所以调用不成功。

## 匹配单个字符

![img](https:////upload-images.jianshu.io/upload_images/15992481-ebe8983ca97580df.png?imageMogr2/auto-orient/strip|imageView2/2/w/718/format/webp)

正则表达式的单字符匹配

#### 示例1：\d — 匹配一个数字，即0-9

我想找到这句话`Total number of C's analysed: 27474733`里面的数字
 **"\d+"匹配多个数字：**



```python
import re

ret = re.search(r"\d+","Total number of C's analysed:   27474733")
print(ret.group())
```

运行结果：



```undefined
27474733
```

**"\d"匹配一个数字：**



```swift
import re

ret = re.search("\d","Total number of C's analysed: 27474733")
print(ret.group())
```

运行结果：



```undefined
2
```

体会下`"\d"` 和 `"\d+"` 的区别

#### 示例2：. — 匹配任意1个字符（除了\n）

注意：是一个哦



```swift
import re

ret = re.match(".","raw_data")
print(ret.group())

ret = re.match("raw.","raw_data")
print(ret.group())

ret = re.match("raw_.","raw_data")
print(ret.group())
```

运行结果：



```undefined
r
raw_
raw_d
```

#### 示例3：[] — 匹配[ ]中列举的1个字符

**如果hello的首字符小写，那么正则表达式需要小写的h：**



```swift
import re

ret = re.match("h","hello Python")
print(ret.group())
```

运行结果：



```undefined
h
```

**如果hello的首字符大写，那么正则表达式需要大写的H：**



```swift
import re

ret = re.match("H","Hello Python")
print(ret.group())
```

运行结果：



```undefined
H
```

**使用[]匹配大小写h都可以：**



```swift
import re

ret = re.match("[hH]","hello Python")
print(ret.group())
ret = re.match("[hH]","Hello Python")
print(ret.group())
ret = re.match("[hH]ello Python","Hello Python")
print(ret.group())
```

运行结果：



```undefined
h
H
Hello Python
```

**匹配0到9另一种写法：**



```swift
import re

ret = re.search("[0123456789]+","huangjing6344@novogene.com")
print(ret.group())
```

运行结果：



```undefined
6344
```

**匹配0到9另一种写法：**



```swift
import re

ret = re.search("[0-9]","huangjing6344@novogene.com")
print(ret.group())

ret = re.search("[0-9]+","huangjing6344@novogene.com")
print(ret.group())
```

运行结果：



```undefined
6
6344
```

**下面这个正则不能够匹配到数字4：**



```swift
import re

ret = re.search("[0-35-9]+","huangjing6344@novogene.com")
print(ret.group())
```

运行结果：



```undefined
63
```

#### 示例4：\D — 表示匹配一个非数字，即不是数字

**注意"\D"和"\D+"的区别：**



```swift
import re

match_obj = re.match("\D", "huangjing6344@novogene.com")
print(match_obj.group())

match_obj = re.match("\D+", "huangjing6344@novogene.com")
print(match_obj.group())
```

运行结果：



```undefined
h
huangjing
```

#### 示例5：\s — 匹配一个空白字符，即空格和tab键

**空格属于空白字符：**



```swift
import re

match_obj = re.match("hello\sworld", "hello world")
if match_obj:
    result = match_obj.group()
    print(result)
else:
    print("匹配失败")
```

运行结果：



```undefined
hello world
```

**`\t`也属于空白字符：**



```swift
import re

match_obj = re.match("hello\sworld", "hello world")
if match_obj:
    result = match_obj.group()
    print(result)
else:
    print("匹配失败")
```

运行结果：



```undefined
hello world
```

#### 示例6：\S — 匹配一个非空白



```swift
import re

match_obj = re.search("\S", "hello&world")
print(match_obj.group())

match_obj = re.search("hello\Sworld", "hello&world")
print(match_obj.group())
```

运行结果：



```undefined
h
hello&world
```

#### 示例7：\w — 匹配一个非特殊字符，即a-z、A-Z、0-9、_、汉字



```python
import re

# 匹配非特殊字符中的一位
match_obj = re.match("\w", "huangjing6344@novogene.com")
print(match_obj.group())

# 匹配非特殊字符中的多位
match_obj = re.match("\w+", "huangjing6344@novogene.com")
print(match_obj.group())
```

运行结果：



```undefined
h
huangjing6344
```

#### 示例8：\W — 匹配一个特殊字符，即非字母、非数字、非汉字



```swift
import re

match_obj = re.search("\W", "huangjing6344@novogene.com")
print(match_obj.group())
```

运行结果：



```python
@
```

注意这里使用`re.match`会报错，因为`huangjing6344@novogene.com`中的特殊字符没在开头（如果是在开头那使用`re.match`和`re.search`是一样的效果）：



```swift
import re

match_obj = re.match("\W", "huangjing6344@novogene.com")
print(match_obj.group())
```

报错：



```csharp
Traceback (most recent call last):
  File "C:/Users/huangjing00liang/Desktop/练习.py", line 5, in <module>
    print(match_obj.group())
AttributeError: 'NoneType' object has no attribute 'group'
```

总结：

- `.` 表示匹配任意1个字符（除了\n）
- `[ ]` 表示匹配[ ]中列举的1个字符
- `\d` 表示匹配一个数字，即0-9
- `\D` 表示匹配一个非数字，即不是数字
- `\s` 表示匹配一个空白字符，即 空格，tab键
- `\S` 匹配一个非空白字符
- `\S` 匹配一个非空白字符
- `\W` 匹配一个特殊字符，即非字母、非数字、非汉字

## 匹配多个字符

![img]()

匹配多个字符

#### 示例1：* — 匹配前一个字符出现0次或者无限次，即可有可无

匹配出一个字符串第一个字母为大写字符，后面都是小写字母并且这些小写字母可有可无：



```swift
import re

ret = re.match("[A-Z][a-z]*","Huangjing6344")
print(ret.group())

ret = re.match("[A-Z][a-z]*","HuangJing6344")
print(ret.group())
```

运行结果：



```undefined
Huangjing
Huang
```

匹配出一个字符串开头的大写字母：



```swift
import re

ret = re.match("[A-Z]*","Huangjing6344")
print(ret.group())

ret = re.match("[A-Z]*","HJing6344")
print(ret.group())
```

运行结果：



```undefined
H
HJ
```

#### 示例2：+ — 匹配前一个字符出现1次或者无限次，即至少有1次

匹配一个字符串，第一个字符是h，最后一个字符串是g，中间至少有一个字符：



```swift
import re

ret = re.match("h.+g","huangjing6344")
print(ret.group())
```

运行结果：



```undefined
huangjing
```

#### 示例3：? — 匹配前一个字符出现1次或者0次，即要么有1次，要么没有

匹配出这样的数据，但是https 这个s可能有，也可能是http 这个s没有



```swift
import re

ret = re.match("https?", "http")
print(ret.group())

ret = re.match("https?", "https")
print(ret.group())
```

运行结果：



```undefined
http
https
```

#### 示例4：{m}、{m,n}

> - {m}表示匹配前一个字符出现m次
> - {m,n}表示匹配前一个字符出现从m到n次



```swift
import re

ret = re.match("[a-zA-Z0-9_]{8}","huang00jing")
print(ret.group())

ret = re.match("[a-zA-Z0-9_]{5,8}","huang00jing")
print(ret.group())
```

运行结果：



```undefined
huang
huang00j
```

总结：

- `*` 表示匹配前一个字符出现0次或者无限次，即可有可无
- `+` 表示匹配前一个字符出现1次或者无限次，即至少有1次
- `?` 表示匹配前一个字符出现1次或者0次，即要么有1次，要么没有
- `{m}` 表示匹配前一个字符出现m次
- `{m,n}` 表示匹配前一个字符出现从m到n次

## 匹配开头和结尾

![img]()

匹配开头和结尾

#### 示例1：^ — 匹配字符串开头

需求：匹配以数字开头的数据



```python
import re

# 匹配以数字开头的数据
match_obj = re.match("^\d.*", "1.QC_Methy")
if match_obj:
    # 获取匹配结果
    print(match_obj.group())
else:
    print("匹配失败")
```

运行结果：



```css
1.QC_Methy
```

#### 示例2：$ — 匹配字符串结尾

需求: 匹配以数字结尾的数据



```python
import re
# 匹配以数字结尾的数据
match_obj = re.match(".*\d$", "huangjing6344")
if match_obj:
    # 获取匹配结果
    print(match_obj.group())
else:
    print("匹配失败")
```

运行结果：



```undefined
huangjing6344
```

#### 示例3：^ 和 $

需求: 匹配以数字开头中间内容不管以数字结尾



```python
import re
match_obj = re.match("^\d.*\d$", "0.log_1")
if match_obj:
    # 获取匹配结果
    print(match_obj.group())
else:
    print("匹配失败")
```

运行结果：



```css
0.log_1
```

## 除了指定字符以外都匹配

- [^指定字符]: 表示除了指定字符都匹配

- 例如：**[^h]: 表示除了指定字符h以外都匹配**



```python
import re

match_obj = re.match("[^h]", "jing")
if match_obj:
    # 获取匹配结果
    print(match_obj.group())
else:
    print("匹配失败")
```

运行结果：



```undefined
j
```

总结：

- `^` 表示匹配字符串开头
- `$` 表示匹配字符串结尾

## 匹配分组

![img]()

匹配分组相关正则表达式

#### 示例1：| — 匹配左右任意一个表达式

> 需求：在列表中["apple", "banana", "orange", "pear"]，匹配apple和pear



```csharp
import re

# 水果列表
fruit_list = ["apple", "banana", "orange", "pear"]

# 遍历数据
for value in fruit_list:
    # |    匹配左右任意一个表达式
    match_obj = re.match("apple|pear", value)
    if match_obj:
        print("%s是我想要的" % match_obj.group())
    else:
        print("%s不是我要的" % value)
```

运行结果：



```undefined
apple是我想要的
banana不是我要的
orange不是我要的
pear是我想要的
```

#### 示例2：( ) — 将括号中字符作为一个分组

> 需求：匹配出163、126、qq等邮箱



```python
import re

match_obj = re.match("[a-zA-Z0-9_]{4,20}@(163|126|qq|sina|yahoo)\.com", "hello@163.com")
if match_obj:
    print(match_obj.group())
    # 获取分组数据
    print(match_obj.group(1))
else:
    print("匹配失败")
```

运行结果：



```css
hello@163.com
163
```

> 需求: 匹配qq:10567这样的数据，提取出来qq文字和qq号码



```python
import re

match_obj = re.match("(qq):([1-9]\d{4,10})", "qq:10567")

if match_obj:
    print(match_obj.group())
    # 分组:默认是1一个分组，多个分组从左到右依次加1
    print(match_obj.group(1))
    # 提取第二个分组数据
    print(match_obj.group(2))
else:
    print("匹配失败")
```

运行结果：



```css
qq:10567
qq
10567
```

#### 示例3：`\num` — 引用分组num匹配到的字符串

需求：匹配出`<html>hh</html>`



```python
import re

match_obj = re.match("<[a-zA-Z1-6]+>.*</[a-zA-Z1-6]+>", "<html>hh</div>")

if match_obj:
    print(match_obj.group())
else:
    print("匹配失败")

###引用分组1匹配到的字符串
### 正则表达式中必须是两个斜杠：\\1
match_obj = re.match("<([a-zA-Z1-6]+)>.*</\\1>", "<html>hh</html>")

if match_obj:
    print(match_obj.group())
else:
    print("匹配失败")
```

#### 示例4：`(?P<name>) (?P=name)` — 引用别名为name分组匹配到的字符串

匹配出<html><h1>[www.processon.com](https://links.jianshu.com/go?to=http%3A%2F%2Fwww.processon.com)</h1></html>：



```swift
import re
match_obj = re.match("<(?P<name1>[a-zA-Z1-6]+)><(?P<name2>[a-zA-Z1-6]+)>.*</(?P=name2)></(?P=name1)>", "<html><h1>www.processon.com</h1></html>")

if match_obj:
    print(match_obj.group())
else:
    print("匹配失败")
```

运行结果：

```xml
<html><h1>www.processon.com</h1></html>
```

## 注意：

- 分组的数据是从左向右的方式进行分配的
- `re.match` 只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回`None`；而`re.search`匹配整个字符串，直到找到一个匹配。

## 总结：

- `|` 表示匹配左右任意一个表达式
- `(ab)` 表示将括号中字符作为一个分组
- `\num` 表示引用分组num匹配到的字符串
- `(?P<name>)` 表示分组起别名
- `(?P=name)` 表示引用别名为name分组匹配到的字符串
- `(分组数据)` 分组数是从左到右的方式进行分配的，最左边的是第一个分组，依次类推



作者：黄晶_id
链接：https://www.jianshu.com/p/3cdf70d2f353
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



# 项目需求

程序启动，显示名片管理系统欢迎界面，并显示功能菜单

------

欢迎使用【名片管理系统】V1.0

1. 新建名片
2. 显示全部
3. 查询名片
4. 退出系统

------

1.用户用数字选择不同的功能
 2.根据功能选择，执行不同的功能
 3.用户名片需要记录用户的 姓名、电话、QQ、邮件
 4.如果查询到指定的名片，用户可以选择 修改 或者 删除 名片

## 源代码

cards_main() 主程序入口
 cards_tool()  操作卡片方法
 cards_main()



```python
#! /Library/Frameworks/Python.framework/Versions/3.7/bin/python3
# 主入库文件
import  cards_tool

# 无限循环，由用户主动决定什么时候退出循环
while True :
    #  显示欢迎菜单
    cards_tool.show_menu()

    handler = input("请选择希望执行的操作：")
    print("您选择的操作是【% s】" % handler)

    user_input = ["0","1","2","3"]

    if handler in user_input:
        if handler=="0":
            # print("用户输入的是%s,需要调用退出系统功能" % handler)
            # 调用退出系统函数
            # cards_tool.exit_card()
            print("欢迎下次使用【名片管理系统】")
            break
        elif handler=="1":
            # print("用户输入的是%s,需要调用新建名片功能" % handler)
            # 调用新建名片函数
            cards_tool.new_card()
            # break
        elif handler=="2":
            # print("用户输入的是%s,需要调用显示全部功能" % handler)
            # 调用显示全部函数
            cards_tool.show_all()
            # break
        elif handler=="3":
            # print("用户输入的是%s,需要调用查询名片功能" % handler)
            # 调用查询名片函数
            cards_tool.query_card()
            # break
    else:
        print("用户输入的是%s,请重新输入" % handler)
```

cards_tool()



```python
# 具体方法文件

# 记录所有名片的列表，里面的保存方式是字典
card_list = []



def show_menu():
    """显示菜单"""
    print("**************************************************")
    print("欢迎使用【名片管理系统】V1.0")
    print("\n")
    print("1. 新建名片")
    print("2. 显示全部")
    print("3. 查询名片")
    print("\n")
    print("0. 退出系统")
    print("**************************************************")



def new_card():
    """
    新增名片
    :return: 
    """
    print("-"*50)
    print("1---新增名片")
    # 1. 提示用户输入名片的详细信息  姓名、电话、QQ、邮件
    name = input("请输入姓名： ")
    phone = input("请输入电话： ")
    qq = input("请输入QQ： ")
    mail =  input("请输入邮件： ")
    # 2.使用用户的输入的信息建立一个名片字典
    dic = {
        "name":name,
        "phone":phone,
        "qq":qq,
        "mail":mail
    }
    print(dic)
    # 3.将名片字典添加到列表中
    # card_list.extend(dic) 不能用extend
    card_list.append(dic)
    # 4.告知用户输入成功
    print("添加%s的名片成功" % name)


def show_all():
    print("-" * 50)
    # print("显示所有名片")
    # 如果列表没有内容，不执行打印表头操作
    if len(card_list)==0:
        print("当前没有任何的名片，请选择新建名片")
        return

    # 打印表头
    for header in ["姓名","电话","QQ","邮箱"]:
        print(header,end="\t\t")
    # 打印分隔符
    print()
    print("-" * 50)
    # 遍历名片字典，输出所有值
    for dic in card_list:
        print("%s\t\t%s\t\t%s\t\t%s" % (dic["name"],
                                        dic["phone"],
                                        dic["qq"],
                                        dic["mail"]))
    # 打印分隔符
    print("-" * 50)

def query_card():
    """
    查找名片
    :return: 
    """
    print("-" * 50)
    print("3---查询名片")
    # 1.提示用户输入要查找的姓名
    find_name = input("请输入要搜索的用户姓名：")
    # 2遍历名片列表，查询要搜索的姓名，如果没有，提示用户
    for dic in card_list:
        if dic["name"]==find_name:
            print("找到了%s，信息如下" % find_name)
            # 打印表头
            print("姓名\t\t电话\t\tQQ\t\t邮箱")
            # 打印分隔符
            print()
            print("-" * 50)
            # 打印用户信息
            print("%s\t\t%s\t\t%s\t\t%s" % (dic["name"],
                                            dic["phone"],
                                            dic["qq"],
                                            dic["mail"]))
            # 针对找到的名片需要进行修改和删除操作
            deal_card(dic)
            break
    else:
        print("抱歉，没有找到%s" % find_name)




def deal_card(find_dic):

    """
    修改或者删除名片   
    :param find_dic: 要查找的用户信息
    """
    print(find_dic)
    action_str = input("请输入对名片的操作：【1】修改 【2】删除 【0】返回上一级菜单")
    if action_str=="1":
        find_dic["name"] = input_card_info(find_dic["name"],"姓名[回车不修改]：")
        find_dic["phone"] = input_card_info(find_dic["phone"],"电话[回车不修改]：")
        find_dic["qq"] = input_card_info(find_dic["qq"],"qq[回车不修改]：")
        find_dic["mail"] = input_card_info(find_dic["mail"],"邮箱[回车不修改]：")

        print("修改名片成功")
    elif action_str=="2":
        card_list.remove(find_dic)
        print("删除名片成功！")



def input_card_info(dic_value,tip_message):
    """
    判断用户输入的内容
    :param dic_value: 字典中原有的值
    :param tip_message: 控制台的提示信息
    :return: 如果用户输入了内容，返回输入的内容，如果没有输入，返回原有的值
    """
    result_str = input(tip_message)

    if len(result_str)>0:
        return result_str
    else:
        return dic_value
```

## 执行程序

- 方法1：在pycharm中直接执行
- 方法2：在终端中执行 python3 cards_main.py
- 方法3：在终端中执行./cards_main.py

> 方法3需要提前配置：
> 1.通过which python3找到python3的位置
> 2.将python3路径加在主程序cards_main.py上方 #! /Library/Frameworks/Python.framework/Versions/3.7/bin/python3
> 3.给文件赋值可执行权限 sudo chmod 777 cards_main.py
> 4.执行./cards_main.py



