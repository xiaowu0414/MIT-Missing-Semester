---
date: 2025-11-10
tags:
  - ShellScript
---
[Shellscript教程](https://www.shellscript.sh/)

[菜鸟教程](https://www.runoob.com/linux/linux-shell.html)
## 什么是bash？

我们通常所说的bash和shell script之间的关系可以这样理解：

1. shell是一个统称，它是一种命令行解释器，提供了用户与操作系统内核交互的界面。
2. bash（Bourne-Again SHell）是shell的一种，是许多Linux发行版和macOS（在较新版本中已改为zsh，但bash仍常见）默认的shell。
3. shell script（shell脚本）是一种为shell编写的脚本程序，它可以用各种shell的解释器来执行，包括bash、sh、zsh等。

区别：

- bash是一个具体的shell程序，而shell script是一种脚本语言，它可以用bash来执行，也可以用其他shell（如sh、csh、ksh等）来执行。
- 换句话说，bash是shell的一种实现，而shell script是使用shell语法编写的脚本。

  

脚本的第一行通常会是 `#!/bin/bash`（称为 Shebang），这指明了这个脚本必须用Bash来解释执行。如果写的是 `#!/bin/sh`，则可能被链接到更精简的Shell（如`dash`）来执行。

---

## Hello World

Shell脚本一般以 **.sh** 作为文件的后缀名

```bash
#!/bin/bash
echo "hello World"
```

```#!``` 是一个约定标记，告诉系统这个解释器用什么脚本来运行

注意，在尝试运行你所写的shell脚本是，command一定要写成 **./hello.sh**，而不是 **hello.sh**，运行其它二进制的程序也一样，直接写 test.sh，linux 系统会去 PATH 里寻找有没有叫 test.sh 的，而只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 test.sh 是会找不到命令的，要用 ./test.sh 告诉系统说，就在当前目录找。

```Shell
tulei@tulei:~/test$ ls
hello.sh  hello_1.txt
tulei@tulei:~/test$ ls -l
total 8
-rw-r--r-- 1 tulei tulei 31 Nov  9 13:43 hello.sh
-rw-r--r-- 1 tulei tulei  6 Nov  8 23:30 hello_1.txt

# 使sh可以执行，是因为，sh这个程序在/bin文件内，而他只是把后边程序的内容作为参数输入
tulei@tulei:~/test$ sh hello.sh
Hello,World

# 直接运行 hello.sh，会导致找不到程序，即使为他赋予执行权限依旧无法执行
tulei@tulei:~/test$ hello.sh
hello.sh: command not found
tulei@tulei:~/test$ chmod +x hello.sh
tulei@tulei:~/test$ ls -l
total 8
-rwxr-xr-x 1 tulei tulei 31 Nov  9 13:43 hello.sh
-rw-r--r-- 1 tulei tulei  6 Nov  8 23:30 hello_1.txt
tulei@tulei:~/test$ hello.sh
hello.sh: command not found

# 需要使用相对路径来告诉计算机这个程序在哪
tulei@tulei:~/test$ ./hello.sh
Hello,World

# 删掉执行权限，哪怕可以定位，但也是无法执行的
tulei@tulei:~/test$ chmod -x hello.sh
tulei@tulei:~/test$ ls -l
total 8
-rw-r--r-- 1 tulei tulei 31 Nov  9 13:43 hello.sh
-rw-r--r-- 1 tulei tulei  6 Nov  8 23:30 hello_1.txt
tulei@tulei:~/test$ ./hello.sh
bash: ./hello.sh: Permission denied
tulei@tulei:~/test$ 
```

---

## Shell 变量

### 命名变量

一定记得变量名和等号之间**不能**有空格

```bash
# 变量命名不能有空格
tulei@tulei:~/test$ foo=bar
tulei@tulei:~/test$ echo $foo
bar

# 有命名时会报错
tulei@tulei:~/test$  foo = bar
Command 'foo' not found, did you mean:
  command 'roo' from snap roo (2.0.3)
  command 'goo' from deb goo (0.155+ds-4)
  command 'fox' from deb objcryst-fox (1.9.6.0-2.2)
  command 'fio' from deb fio (3.28-1)
  command 'foot' from deb foot (1.11.0-2)
  command 'fop' from deb fop (1:2.6-2)
See 'snap info <snapname>' for additional versions.
```

同时，变量名的命名须遵循如下规则：

- **只包含字母、数字和下划线：** 变量名可以包含字母（大小写敏感）、数字和下划线 _，不能包含其他特殊字符。
- **不能以数字开头：** 变量名不能以数字开头，但可以包含数字。
- **避免使用 Shell 关键字：** 不要使用Shell的关键字（例如 if、then、else、fi、for、while 等）作为变量名，以免引起混淆。
- **使用大写字母表示常量：** 习惯上，常量的变量名通常使用大写字母，例如 PI=3.14。
- **避免使用特殊符号：** 尽量避免在变量名中使用特殊符号，因为它们可能与 Shell 的语法产生冲突。
- **避免使用空格：** 变量名中不应该包含空格，因为空格通常用于分隔命令和参数。

使用一个定义过的变量，只要在变量名前面加**美元符号**即可，如：

```bash
your_name="qinjx"  
echo $your_name  
echo ${your_name}  
```

**单引号和双引号**有时会不一样：

```bash
tulei@tulei:~/test$ echo "Value is $foo"
Value is bar
tulei@tulei:~/test$ echo 'Vlaue is $foo'
Vlaue is $foo
```

**变量使用时最好加上{}**

变量名外面的**花括号**是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界，比如下面这种情况：

```bash
for skill in Ada Coffe Action Java; do  
    echo "I am good at ${skill}Script"  
done  
```

如果不给skill变量加花括号，写成```
``` 
echo "I am good at $skillScript" 
```
 ，解释器就会把$skillScript当成一个变量（其值为空），代码执行结果就不是我们期望的样子了。

推荐给所有变量加上花括号，这是个好的编程习惯。

---
### $ 符号的使用(参数的获取)

在开始之前，先把本节中可能用到的知识放在这里
 
-  $1代表输入的第一个参数
-  $? 返回上次命令是否成功
-  $_ 获取上次命令最后一个参数
- `!!` 会替代掉你上次想要执行，但未能成功执行的命令$
-  $0 是运行的脚本的文件名
-  $# 代表给定参数的个数
- `$$` 代表这个命令进程的ID
-  $@ 代表所有参数

**$1代表输入的第一个参数：**

```shell
cmd () {
    mkdir -p "$1"
    cd "$1"
}
```

之后，使用 `source` 即可让解释器知道我们所定义的函数

```bash
tulei@tulei:~/test$ source hello.sh 
tulei@tulei:~/test$ cmd test_1
tulei@tulei:~/test/test_1$ 
```

这里的 `$1` 代表函数接收到的第一个参数，实际上从 `$1` 到 `$9` 都是允许的

**`$?` 返回上条错误的代码**

返回0代表一切正常，没有错误

```bash
tulei@tulei:~/test$ echo "hello"
hello
tulei@tulei:~/test$ $?
0: command not found$
```

返回了上次的报错

```
tulei@tulei:~/test$ ls
# grep 表示在收到的第二个参数的文件里，寻找第一个参数
tulei@tulei:~/test$ grep hello hello.txt
grep: hello.txt: No such file or directory
tulei@tulei:~/test$ $?
2: command not found
```

实际上可以进行**逻辑运算**

```bash
tulei@tulei:~/test$ false
tulei@tulei:~/test$ $?
1: command not found

tulei@tulei:~/test$ false || echo "hello"
hello
tulei@tulei:~/test$ $?
0: command not found

# 或运算：第一个运行失败了第二个才会执行
tulei@tulei:~/test$ true || echo "hello"
tulei@tulei:~/test$ $?
0: command not found
tulei@tulei:~/test$

# 与运算：第一个只要失败，第二个就不会执行
 tulei@tulei:~/test$ false && echo "hello"
tulei@tulei:~/test$ $?
1: command not found
tulei@tulei:~/test$ true && echo "hello"
hello
tulei@tulei:~/test$ $?
0: command not found
tulei@tulei:~/test$ 
```

在同一命令中使用分号连接，那么不论怎样，都会依次输出

```bash
tulei@tulei:~/test$ false ; echo "hello"
hello
tulei@tulei:~/test$ $?
0: command not found
```

 **`$_` 获取上条命令的最后一个参数**

```bash
tulei@tulei:~$ ls
tulei@tulei:~$ mkdir test
tulei@tulei:~$ cd $_
tulei@tulei:~/test$ 
```

把参数赋值给变量后再使用

```bash
tulei@tulei:~/test$ foo=$(pwd)
tulei@tulei:~/test$ echo foo
foo
tulei@tulei:~/test$ echo $foo
/home/tulei/test
tulei@tulei:~/test$ echo ${foo}
/home/tulei/test
```

可以不把参数赋值给变量而直接使用

```bash
tulei@tulei:~/test$ echo "we are in $(pwd)"
we are in /home/tulei/test
tulei@tulei:~/test$ 
```

**案例**：记得去回顾以下$后加不同符号，代表什么

```shell
#!/bin/bash

echo "Starting program at $(date)"

echo "Running program $0 with $# arguments with pid $$"
  
for file in "$@"; do
    #将得到的输出全部丢弃（被放在dev这个文件夹下的信息都会被丢弃）
    grep foobar "$file" > /dev/null 2> /dev/null  
    #如果找到了，$?就是0，没找到就不是0
    if [[ "$?" -ne 0 ]]; then
        echo "File $file doed not have any foobar"
        echo "# foobar" >> "$file"
    fi
done
```

运行结果如下：

```bash
tulei@tulei:~/test$ ./example.sh example.sh aaa.md 
Starting program at Mon Nov 10 20:41:15 CST 2025
Running program ./example.sh with 2 arguments with pid 36453
File aaa.md doed not have any foobar
```

aaa.md没有“foobar”这个词组，就为他后边添加了"# foobar"这个词组

---
### 通配符

从上边的例子我们发现，实际上如果每次输入时都需要完整输入所有参数，是有点丑陋的，所以，我们介绍通配符，他允许批量确定参数

- `？`用于替代 **一个** 你不是很清楚的字符：
- `*` 用于替代你不知道的 **任意多个** 字符

你可以把`*`理解为一个万能的替换符号，不论什么不知道时，都可以用它换掉

```bash
tulei@tulei:~/test$ ls
aaa.md  example.sh  example_1.sh
tulei@tulei:~/test$ ls *.sh
example.sh  example_1.sh
```

`*` 与`?` 的差别：

```bash
# 都可以进行快速查找
tulei@tulei:~$ find tes*
test
test/example.sh
test/example_1.sh
test/aaa.md
tulei@tulei:~$ find tes?
test
test/example.sh
test/example_1.sh
test/aaa.md

# 但 ？表示不知道的一个字符，而*可以是任意多个
tulei@tulei:~$ find t?
find: ‘t?’: No such file or directory
tulei@tulei:~$ find t*
test
test/example.sh
test/example_1.sh
test/aaa.md
tulei@tulei:~$ 
```





