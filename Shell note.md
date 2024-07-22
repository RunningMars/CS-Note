# 

Shell 是解释型语言，不需要编译即可执行，Shell中可以直接调用Linux系统命令；

Shell的主要语法类型有Bourne和C

Bourne家族 ：sh，Bash，ksh，psh，zsh;
C家族：csh，tcsh

sh 与Bash 兼容

```shell
Ctrl + a 光标移动到行首(Ahead of line),相当于通常的Home键
Ctrl + e 光标移动到行尾(End of line) Ctrl + f 光标向前(Forward)移动一个字符位置 Ctrl + b
```



vim
0（Num）：移动光标到行首

$ ：移动光标到行尾

```shell
history | grep cd
```



空格代表命令的分隔

输出重定向符号，将正常输出（输出屏幕）重定向到输出到文件中
>  覆盖写入
>> 追加写入

输出重定向符号，将错误输出（输出屏幕）重定向到输出到文件中
2>  覆盖写入
2>> 追加写入

正常输出和错误输出都追加写入到test.log

```sh
Ls  &>> test.log
```

将正常输出和错误输出都重定向写入到垃圾箱中，避免被屏幕输出刷屏

```shell
ls   &>> /dev/null
```

```shell
find / -name access.log > test.log 2>> /dev/null
```



多命令顺序

；不管前面的命令是否执行成功，后面的命令继续执行
&&   前面的命令执行成功后，后面的命令才能继续执行，否则不执行
||    前面的命令执行失败后，后面的命令才能继续执行，否则不执行

管道符 |
格式 ：   1命令 | 命令2
命令1的正确输出，作为命令2的输入
例如：

```shell
Ps -ef | grep php

grep ‘die;’ controller.php
```




通配符
？匹配一个任意字符
*匹配0个或任意多个任意字符，也就是可以匹配任意内容，包括没有
[ ]匹配中括号中任意一个字符，例如：[abc]代表一定匹配一个字符，或者是a，或者是b，或者是c。
[-]匹配中括号中任意一个字符，-代表一个范围。例如[a - z]代表匹配一个小写字母。
[^]逻辑非，标识匹配不是中括号内的一个字符。例如：[^0 - 9]代表匹配一个不是数字的字符。


Bash中其他特殊符号

‘’ 在单引号中的特殊符号，如$ ` 都没有特殊含义
“”
``
$()
#
$
\


变量
变量不能以数字开头
变量的默认累心个都是字符串型，如果需要数字型需要指定



位置参数变量:
取用户输入的内容

```shell
Test.sh rdg sdy rdgsdy
$0  Test.sh
$1  rdg
$2  sdy
$3  rdgsdy
```



$*

$@

$#

```shell
[ -e read.sh ]; exist=$? && echo $exist

test -e read.sh ; exist=$? && echo $exist

test -e read.sh && exist=1 || exist=0 ;echo $exist

[ -e read.sh ] && exist=1 || exist=0 ;echo $exist
```



grep  内容  (绝对/相对路径)目标文件
> grep  sbin  /etc/passwd       //在passwd文件中查找sbin字样，会把sbin所在行的内容都输出

```shell
find . -name *liuyazhuang* 查找当前目录下名称中含有"liuyazhuang"的文件
find / -name *.conf  查找根目录下（整个硬盘）下后缀为.conf的文件
find  目录 -name  "an*"[部分名称]    模糊查找文件名字以an开始的

find /Users/rdg -name \*youdao\*
```



```shell
#内容替换 "cont1"被替换为"cont2"
:s/cont1/cont2/         //把光标所在行的"第一个"cont1替换为cont2
:s/cont1/cont2/g        //把光标"所在行"的全部cont1替换为cont2
:%s/cont1/cont2/g       //把"整个文档"中的全部cont1替换为cont2
```



3) 追加内容(文件不存在会“自动”创建)
> echo  内容 > filename    //给文件“覆盖写”方式追加内容
> echo  内容 >> filename   //给文件纯追加内容



-z参数将归档后的归档文件进行gzip压缩以减少大小。
-c：创建一个新tar文件
-v：显示运行过程的信息
-f：指定文件名
-z：调用gzip压缩命令进行压缩
-t：查看压缩文件的内容
-x：解开tar文件

```shell

tar  -cvf test.tar  *：将所有文件打包成test.tar,扩展名.tar需自行加上
tar  -zcvf test.tar.gz  *：将所有文件打包成test.tar,再用gzip命令压缩
tar -tf   test.tar ：查看test.tar文件中包括了哪些文件
tar -xvf test.tar       将test.tar解开
tar -zxvf foo.tar.gz   解压缩

echo：输出系统变量或者常量的值得到命令行终端
       echo $JAVA_HOME
```



```shell
#!/bin/bash
ssh -i /Users/rdg/.ssh/iapps_ihg_bastion.pem bastionuser@iapps_ihg_bastion > /dev/null 2>&1 << eeooff
touch abcdefg.txt
touch abc.txt
eeooff
echo done!

#!/bin/bash
ssh -i /Users/rdg/.ssh/iapps_ihg_bastion.pem bastionuser@iapps_ihg_bastion &> /dev/null << eeooff
touch abcdefg.txt
touch abc.txt
eeooff
echo done!
```



```shell
#!/bin/bash  
#变量定义  
ip_array=("192.168.1.1" "192.168.1.2" "192.168.1.3")  
user="test1"  
remote_cmd="/home/test/1.sh"  

#本地通过ssh执行远程服务器的脚本  
for ip in ${ip_array[*]}  
do  
    if [ $ip = "192.168.1.1" ]; then  
        port="7777"  
    else  
        port="22"  
    fi  
    ssh -t -p $port $user@$ip "remote_cmd"  
done  
```





ARG timezone

ENV TIMEZONE=${timezone:-"Asia/Shanghai"} \
    COMPOSER_VERSION=1.9.0 \
    APP_ENV=prod



```shell
update

RUN set -ex \
    && php -v \
    && php -m \

    #  ---------- some config ----------

​    && cd /etc/php7 \

    # - config PHP

​    && { \
​        echo "upload_max_filesize=100M"; \
​        echo "post_max_size=108M"; \
​        echo "memory_limit=1024M"; \
​        echo "date.timezone=${TIMEZONE}"; \
​    } | tee conf.d/99-overrides.ini \

    # - config timezone

​    && ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
​    && echo "${TIMEZONE}" > /etc/timezone \

    # ---------- clear works ----------

​    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man \
​    && echo -e "\033[42;37m Build Completed :).\033[0m\n"

WORKDIR /opt/www
```





-e 脚本中的命令一旦运行失败就终止脚本的执行
-x 用于显示出命令与其执行结果（默认shell脚本中只显示执行结果
+ex表示不终止错误，不显示结果