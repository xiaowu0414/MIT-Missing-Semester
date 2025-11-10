---
date: 2025-11-10
tags:
  - ShellScript
---
[Shellscript教程](https://www.shellscript.sh/)

  
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

注意，一定要写成 **./test.sh**，而不是 **test.sh**，运行其它二进制的程序也一样，直接写 test.sh，linux 系统会去 PATH 里寻找有没有叫 test.sh 的，而只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 test.sh 是会找不到命令的，要用 ./test.sh 告诉系统说，就在当前目录找。

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

## Shell 参数