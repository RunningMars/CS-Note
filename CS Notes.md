C语言和Go语言的相似处:
都是编译型语言

都有结构体

都有指针,c语言允许复杂且灵活的操作指针,而Go语言将复杂的指针操作封装到了切片内操作;

都有标准输出流,标准输入流

都没有try catch 异常机制,Go语言有panic机制;

数组作为基础数据容器,且要求数据类型,不同的是Go语言有空接口interface{}可存放任意类型数据,还有map型

都支持自定义类型 

不同点:

Go语言有接口

Go语言有goroutine,channel

Go语言的可见性, 包中所有首字母大写开头的成员（对象、函数）

Go语言函数提供defer钩子处理方法

Go语言多一个string类型

C常用NULL表示空指针,在go中将空指针赋值为nil









## PHP

### laravel

hasmany的关联查询

$collection = collect([]);

if( $collection )  为 true , $collection->isNotEmpty() 为 false



测试  php sleep() 方法对CPU的占用率；

调用sleep()，实际上php将自身进程标记为就绪状态，释放了CPU使用权，下一次调度CPU执行php时，php计算是否处于sleep中如果还在sleep范围内则继续标记为就绪状态，释放CPU使用权，知道下一次调度回来已经sleep完了，则继续向下执行。

```php
//php进程占用CPU使用率 = 96%
for($i=0;$i<1000000000;$i++){
}
echo '====finished empty=====';
```

```php
//php进程占用CPU使用率 = 0%
for($i=0;$i<1000000000;$i++){
    sleep(1);
}
echo '====finished sleep=====';
```



## composer

### 一、composer入门

1、每次安装新的包文件，会更新/vendor/autoload.php文件
 2、composer.lock与composer.json的关系
 文件composer.lock会根据composer.json的内容自动生成，和composer.json在同一位置，即在安装完所有需要的包之后，Composer会在composer.lock文件中生成一张标准的包版本的文件，这将锁定所有包的版本。可以使用composer.lock (当然是和composer.json一起)来控制项目的版本。
 composer.lock与composer.json的关系为，composer.json文件为包的元信息，composer.lock文件同样为包的元信息，但在composer.json文件中可以指定使用不明确的依赖包版本，如“>=1.0”，在composer.lock文件中的会是当前安装的版本。那么当使用Composer安装包时，它会优先从composer.lock文件读取依赖版本，再根据composer.json文件去获取依赖。这确保了该库的每个使用者都能得到相同的依赖版本。这对于团队开发来讲非常重要。

### 二、composer常用命令



![img](https:////upload-images.jianshu.io/upload_images/17307535-d1eb008960e62625.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image.png


 1、composer install
 当项目重新部署或者合并分支时，都需要执行`composer install`命令。如果当前项目根目录存在composer.lock文件，则会首先根据composer.lock文件指定的包版本从composer中下载相应的包，如果没有，则根据composer.json文件到composer中下载合适版本的包，并生成composer.lock文件。
 2、composer update
 如果直接执行`composer update`命令，后面不指定包名，则会更新项目依赖的所有包文件，因此**当项目已经成型，`composer update`命令要慎用，不能随便执行，特别是生产环境！**
 当然，我们可以在`composer update`命令后面跟上包名称，这样子就只会更新指定的包，具体步骤是：先修改composer.json文件中，对应包（比如monolog/monolog）的版本号为1.25.0，然后执行`composer update monolog/monolog`，则会更新monolog/monolog包到1.25.0版本，其他包不会受到影响。
 3、composer require
 使用频率最高的命令。当我们要往项目中引入某个包的时候，要使用该命令。
 该命令执行会更新composer.json文件，并下载相应包版本，同时也会更新composer.lock文件。这样一来，其他项目成员只要拿到composer.lock文件后，执`composer install`命令即可获取到相同的包。



### 三、composer命令执行流程

![img](https:////upload-images.jianshu.io/upload_images/17307535-7793d80f4f2e0e51.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image.png

### 四、搭建php框架时，composer的基础用法

1、composer.json中autoload的classmap自动加载
 在composer.json中的autoload中添加classmap后，需要执行`composer dump-autoload`命令才能真正生效，如下：



```json
"autoload" : {
    "classmap" : [
        "app/controllers"
    ]
}
```

在项目根目录中执行`composer dump-autoload`命令后，app/controllers下的类，才会被自动加载。
 2、composer.json中autoload的psr-4自动加载
 当然，如果按照上面第1点的方式，每次创建一个控制器类，都需要执行`composer dump-autoload`命令才能生效，太麻烦，因此psr4就派上用场了：在composer.json中添加



```json
"autoload" : {
    "psr-4" : [
       "controllers\\": "app/controllers/"
    ]
}
```

这样一来，只要执行一次`composer dump-autoload`后，在app/controllers/目录下任意添加新的控制器类，都会被自动加载
 3、利用composer单个文件自动加载
 在开发过程中，往往有些功能函数没必要形成一个类，这个时候我们往往需要一个工具函数，例如：



```php
if (!function_exists('dd')){
    function dd(...$args){
        foreach ($args as $arg){
            var_dump($arg);
        }
        die();
    }
}
```

这个时候，我们在composer.json文件中添加自动加载，如下：



```json
"autoload": {
    "files": [
      "app/Helpers/Helper.php"
    ]
  }
```

最后执行一次`composer dump-autoload`，app/Helpers/Helper.php文件即可在全局被自动加载了。



---



$ composer install 这样⼦他会⾃动下载monolog/monolog⽂件到你的vendor⽬录下⾯。

接下来需要说明⼀件事情就是composer.lock - 锁定⽂件在安装完所有需要的包之后，composer会⽣成⼀张标准的包版本的⽂件在composer.lock⽂件中。这将锁定所有包的版本。

使⽤composer.lock（当然是和composer.json⼀起）来控制你的项⽬的版本这⼀点⾮常的重要，我们使⽤install命令来处理的时候，它⾸先会判断composer.lock⽂件是否存在，如果存在，将会下载相对应的版本(不会在于composer.json⾥⾯的配置)，这意味着任何下载项⽬的⼈都将会得到⼀样的版本。如果不存在composer.lock，composer将会通过composer.json来读取需要的包和相对的版本，然后创建composer.lock⽂件这样⼦就可以在你的包有新的版本之后，你不会⾃动更新了，升级到新的版本，使⽤update命令即可，这样⼦就能获取最新版本的包并且也更新了你的composer.lock⽂件



---

1、`composer.json`
composer.json文件中保存的是我们安装的组件及组件的版本要求。
2、`comopser.lock`
composer.lock文件中保存的是组件及其依赖的具体版本，在多人协同开发的情况下，这个文件能很好的解决组件不同而产生的问题。在使用`composer install`的时候是不会修改`composer.lock`这个文件,所以会把这个文件也放入版本管理中，其它人在使用时只需要`composer install`就可以了。而使用`composer update`后修改这个文件。

```
composer dump-autoload
```

```
composer dumpautoload -o
```
