# Shell 命令

其他学习笔记：[学习笔记汇总](https://mcnuuzg9cgrt.feishu.cn/wiki/IafgwclaUiMVkBkkOCXcjLKinuc?from=from_copylink)

# Linux 简单介绍

在介绍 Shell 之前，先对 Linux 做简单介绍，以便后续学习

因为自己计算机目前是 Windows，所以这节课我会利用 wsl2，在 Windows 上的 Ubuntu 子系统来完成

对 wsl2 不了解的可以去看：[https://www.bilibili.com/video/BV1ce46ziE1r/?spm_id_from=333.337.search-card.all.click](https://www.bilibili.com/video/BV1ce46ziE1r/?spm_id_from=333.337.search-card.all.click)

## Linux 系统目录结构：

![](static/LFQrbXOJzoxxowxAlglcLPX8nlg.png)

```shell
tulei@tulei:~$ cd /
tulei@tulei:/$ ls
bin   dev  home  lib    lib64   lost+found  mnt  proc  run   snap  sys  usr
boot  etc  init  lib32  libx32  media       opt  root  sbin  srv   tmp  var
```

---

## 每个文件夹存储的内容：（初学可以不做详细了解）

### bin:

全部为执行命令，执行程序，（Linux 一般绿色默认为可执行程序）bin 中为普通用户也可执行的命令

### boot:

引导分区，用来装载开机启动项的一些东西

### dev:

一般存放一些存储介质

Linux 将硬件分区设备等都表示为文件:

![](static/XClxbVRuUojVLbxe8Yrc0aEynpe.png)

![](static/SNbUbnBd7oAO2VxUZR2cS9Tkn1e.png)

查看系统目录结构和磁盘分区：

```typescript
tulei@tulei:~$ cd /
tulei@tulei:/$ ls
bin   dev  home  lib    lib64   lost+found  mnt  proc  run   snap  sys  usr
boot  etc  init  lib32  libx32  media       opt  root  sbin  srv   tmp  var
tulei@tulei:/$ ls /dev
autofs           hvc2          loop6       ram11   sdc       tty13  tty3   tty46  tty62        vcs2   vcsu6
block            hvc3          loop7       ram12   sdd       tty14  tty30  tty47  tty63        vcs3   vfio
bsg              hvc4          mapper      ram13   sg0       tty15  tty31  tty48  tty7         vcs4   vga_arbiter
btrfs-control    hvc5          mcelog      ram14   sg1       tty16  tty32  tty49  tty8         vcs5   vhost-net
char             hvc6          mem         ram15   sg2       tty17  tty33  tty5   tty9         vcs6   vhost-vsock
console          hvc7          mptctl      ram2    sg3       tty18  tty34  tty50  ttyS0        vcsa   virtio-ports
core             hwrng         mqueue      ram3    shm       tty19  tty35  tty51  ttyS1        vcsa1  vport0p0
cpu_dma_latency  initctl       net         ram4    snapshot  tty2   tty36  tty52  ttyS2        vcsa2  vport0p1
cuse             kmsg          null        ram5    snd       tty20  tty37  tty53  ttyS3        vcsa3  vport0p2
disk             kvm           nvram       ram6    stderr    tty21  tty38  tty54  ttyS4        vcsa4  vsock
dxg              log           ppp         ram7    stdin     tty22  tty39  tty55  ttyS5        vcsa5  zero
fd               loop-control  ptmx        ram8    stdout    tty23  tty4   tty56  ttyS6        vcsa6
full             loop0         ptp0        ram9    tty       tty24  tty40  tty57  ttyS7        vcsu
fuse             loop1         ptp_hyperv  random  tty0      tty25  tty41  tty58  uinput       vcsu1
hpet             loop2         pts         rtc     tty1      tty26  tty42  tty59  urandom      vcsu2
hugepages        loop3         ram0        rtc0    tty10     tty27  tty43  tty6   userfaultfd  vcsu3
hvc0             loop4         ram1        sda     tty11     tty28  tty44  tty60  vcs          vcsu4
hvc1             loop5         ram10       sdb     tty12     tty29  tty45  tty61  vcs1         vcsu5
```

使用 df -Th 可以显示磁盘分区挂载：

```typescript
tulei@tulei:/$ df -Th
Filesystem     Type     Size  Used Avail Use% Mounted on
none           overlay  4.8G     0  4.8G   0% /usr/lib/modules/6.6.87.2-microsoft-standard-WSL2
none           tmpfs    4.8G  4.0K  4.8G   1% /mnt/wsl
drivers        9p       937G  567G  371G  61% /usr/lib/wsl/drivers
/dev/sdd       ext4    1007G  1.8G  954G   1% /
none           tmpfs    4.8G   80K  4.8G   1% /mnt/wslg
none           overlay  4.8G     0  4.8G   0% /usr/lib/wsl/lib
rootfs         rootfs   4.8G  2.7M  4.8G   1% /init
none           tmpfs    4.8G  508K  4.8G   1% /run
none           tmpfs    4.8G     0  4.8G   0% /run/lock
none           tmpfs    4.8G     0  4.8G   0% /run/shm
none           overlay  4.8G   76K  4.8G   1% /mnt/wslg/versions.txt
none           overlay  4.8G   76K  4.8G   1% /mnt/wslg/doc
C:\            9p       937G  567G  371G  61% /mnt/c
tmpfs          tmpfs    4.8G  4.0K  4.8G   1% /run/user/1000
```

### etc:

系统的与服务的一些配置文件

该根目录是系统上最重要的根目录之一。etc 文件夹（etcetera 的缩写）是存储操作系统使用的系统文件的常见位置。

### home

普通用户家目录

### lib

存放一些库文件

### lost+found

表示被挂载的文件夹

### media    mnt

挂载外部存储介质

### opt

一些大的应用程序

### proc

系统开机前不存在，存放一些临时文件

### root

管理员的家目录

/**根**

**与/home** 目录不同，**/root** 文件夹实际上是“root”系统用户的主目录。除了理解这是“root”用户的主目录之外，该文件夹没有其他任何内容

### sbin

root 账号可执行的命令

### selinux

系统防护

### srv

系统存放的一些目录

### sys

不是真实存在的文件，而是一些内核参数

### tmp（重要）

拥有对源代码的编译权限，一般通过 shell 把 exp 传入根目录后移动到此编译

这是 Linux 安装中的唯一根目录。**/tmp 目录是“ temporary** ”的缩写，是易失性目录，用于存储只需要访问一次或两次的数据。与计算机上的内存类似，一旦计算机重新启动，该文件夹的内容就会被清除。

对我们进行渗透测试有用的是，默认情况下任何用户都可以写入此文件夹。这意味着一旦我们能够访问一台机器，它就可以作为存储枚举脚本等内容的好地方。

### usr（重要）

源代码安装程序

### var（重要）

日志文件夹，网站根目录，网站日志

“/var”目录（“var”是变量数据的缩写）是 Linux 安装中的主要根文件夹之一。此文件夹存储系统上运行的服务或应用程序经常访问或写入的数据。例如，正在运行的服务和应用程序的日志文件写入此处（**/var/log**），或者不一定与特定用户关联的其他数据（即数据库等）。

---

# Shell 命令简单演示

## Linux 命令

Shell 是用 C 语言编写好的程序，用户可以用它来使用 Linux

同时他也是一种程序设计语言，可以用它来编写脚本（Script）

接下来所演示的诸如 echo 等在 Linux 中直接输入的命令，实际上是他人已经提前编写好的内容，你可以在下边的位置中找到他们：/bin/bash（在后边会学习 bash 脚本的编写）

一般来说，我们的 Linux 命令分为两类：

- 内部命令：属于 Shell 解释器的一部分
- 外部命令：独立于 Shell 解释器之外的程序文件

Linux 命令的通用命令格式  ：命令字  [选项]  [参数]

选项及参数含义

选项：用于调节命令的具体功能

参数：命令操作的对象，如文件、目录名等

---

## Help & man

在开始之前，先介绍 --help 以及 man 。在你遇到任何困难时，查看帮助文档往往可以快速解决问题

--help 会打开简短的帮助文档：

```shell
tulei@tulei:~$ pwd --help
pwd: pwd [-LP]
    Print the name of the current working directory.
    
    Options:
      -L        print the value of $PWD if it names the current working
                directory
      -P        print the physical directory, without any symbolic links
    
    By default, `pwd' behaves as if `-L' were specified.
    
    Exit Status:
    Returns 0 unless an invalid option is given or the current directory
    cannot be read.
```

Man 会直接打开完整文档：（按 q 即可退出文档的查看）

```shell
tulei@tulei:~$ man pwd
```

---

## 基本信息的查看：

接下来介绍几种你可能会用到的信息，当然，实际上往往并不不常用

### whoami:

```shell
#输出用户身份
tulei@tulei:~$ whoami
tulei
```

### Clear:

或者使用 CTRL+L 也是可以达到相同效果的

```shell
#清空界面
```

### uname:

```shell
#查看系统信息
#uname 查看系统
#uname -r 查看版本号
#uname -a 查看系统详细信息
tulei@tulei:~$ uname
Linux
tulei@tulei:~$ uname -r
6.6.87.2-microsoft-standard-WSL2
tulei@tulei:~$ uname -a
Linux tulei 6.6.87.2-microsoft-standard-WSL2 
#1 SMP PREEMPT_DYNAMIC Thu Jun  5 18:30:46 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
```

### hostname:

```shell
#查看主机信息，并修改名称（只有root权限用户才可修改）
tulei@tulei:~$ hostname
tulei
tulei@tulei:~$ hostname testname
hostname: you must be root to change the host name
```

### date:

```shell
tulei@tulei:~$ date
Sat Nov  8 14:42:38 CST
```

---

## Echo

### echo：

echo 这个程序会将他接收到的所有参数直接输出

```shell
tulei@tulei:~$ echo hello
hello
#因为shell的逻辑是：把你输入的第一个词作为程序，第二个词作为这个程序的参数（argument）
#所以要是输入的参数具有 “ ” 或者可能造成歧义的符号时，请使用'' 或""将他们括起来
tulei@tulei:~$ echo "Hello world"
Hello world
#当然也可以使用转义字符，来限制转义字符后边符号的意义
tulei@tulei:~$ echo Hello\ world
Hello world
```

### Echo $PATH 和 which：

计算机怎么知道他是程序呢？

一般来说，shell 解释器会有一些固定的程序，被存储在/bin 文件夹中

而对于外部命令，shell 第一时间在/bin 中没有找到对应的程序，就会去系统环境变量 PATH 中寻找

```shell
# echo $PATH会打印出你所配置的所有环境变量
tulei@tulei:~$ echo $PATH

#which echo 可以使用which查看echo这个脚本在哪里
tulei@tulei:~$ which echo
/usr/bin/echo
```

### echo & cat：

echo 可以直接将文字写入文档

cat 可以查看文档内容

```sql
tulei@tulei:~$ cd ~
tulei@tulei:~$ ls
tulei@tulei:~$ echo hello > hello.txt
tulei@tulei:~$ ls
hello.txt
tulei@tulei:~$ cat hello.txt
hello
tulei@tulei:~$
```

---

## 导航操作：pwd、ls、cd、find

### pwd

```shell
#查看目前所在位置：
tulei@tulei:~$ pwd
/home/tulei
```

### ls

```bash
#查看当前目录下文件：当前目录没有，回到上一级，可以看到
tulei@tulei:~$ ls
tulei@tulei:~$ cd ..
tulei@tulei:/home$ ls
tulei

#实际上可以直接使用ls ..来查看上一级目录中的文件，也就是和自己同级的文件有哪些
tulei@tulei:/home$ ls ..
bin   dev  home  lib    lib64   lost+found  mnt  proc  run   snap  sys  usr
boot  etc  init  lib32  libx32  media       opt  root  sbin  srv   tmp  var
tulei@tulei:/home$ cd tulei 
tulei@tulei:~$ ls ..
tulei

#查看详细的目录信息：ls -l
tulei@tulei:/$ ls
bin   dev  home  lib    lib64   lost+found  mnt  proc  run   snap  sys  usr
boot  etc  init  lib32  libx32  media       opt  root  sbin  srv   tmp  var
#信息包括：_文件类型，创建者权限，_所在分组对其权限，_其他人权限，节点，创建时属组，_目前所在分组，_大小，最后一次修改日期，名称_
_#对权限的说明：r可读，w可写，x可执行_
#想要进入一个目录的子目录时，需要对父目录和对应的子目录都具有执行权限x
#想要对一个目录使用ls 列出他的子目录有哪些，则需要对这个目录有阅读权限r
#想阅读一个目录的子文件时，需要对文件有阅读权限r
tulei@tulei:/$ ls -l
total 2728
lrwxrwxrwx   1 root root       7 Jan  7  2025 bin -> usr/bin
drwxr-xr-x   2 root root    4096 Apr 18  2022 boot
drwxr-xr-x  15 root root    3860 Nov  8 14:36 dev
drwxr-xr-x  81 root root    4096 Nov  8 14:36 etc
drwxr-xr-x   3 root root    4096 Nov  7 23:49 home
-rwxrwxrwx   1 root root 2724480 Jun 10 02:32 init
lrwxrwxrwx   1 root root       7 Jan  7  2025 lib -> usr/lib
lrwxrwxrwx   1 root root       9 Jan  7  2025 lib32 -> usr/lib32
lrwxrwxrwx   1 root root       9 Jan  7  2025 lib64 -> usr/lib64
lrwxrwxrwx   1 root root      10 Jan  7  2025 libx32 -> usr/libx32
drwx------   2 root root   16384 Nov  7 23:45 lost+found
drwxr-xr-x   2 root root    4096 Jan  7  2025 media
drwxr-xr-x   5 root root    4096 Nov  7 23:45 mnt
drwxr-xr-x   2 root root    4096 Jan  7  2025 opt
dr-xr-xr-x 232 root root       0 Nov  8 14:36 proc
drwx------   3 root root    4096 Nov  7 23:45 root
drwxr-xr-x  19 root root     560 Nov  8 14:36 run
lrwxrwxrwx   1 root root       8 Jan  7  2025 sbin -> usr/sbin
drwxr-xr-x   2 root root    4096 Nov  7 23:45 snap
drwxr-xr-x   2 root root    4096 Jan  7  2025 srv
dr-xr-xr-x  13 root root       0 Nov  8 14:36 sys
drwxrwxrwt   7 root root    4096 Nov  8 14:37 tmp
drwxr-xr-x  14 root root    4096 Jan  7  2025 usr
drwxr-xr-x  13 root root    4096 Jan  7  2025 var
```

### cd

```bash
#移动到绝对路径：
tulei@tulei:~$ cd /home
tulei@tulei:/home$ pwd
/home

#移动到上一级：
tulei@tulei:/home$ cd ..
tulei@tulei:/$ pwd
/

#移动到根目录: cd /
tulei@tulei:~$ cd /
tulei@tulei:/$ ls
bin   dev  home  lib    lib64   lost+found  mnt  proc  run   snap  sys  usr
boot  etc  init  lib32  libx32  media       opt  root  sbin  srv   tmp  var

#移动到用户所在目录：cd ~
tulei@tulei:/$ cd ~
tulei@tulei:~$ pwd
/home/tulei

#移动到刚才所在目录； cd -
tulei@tulei:/dev$ cd ~
tulei@tulei:~$ pwd
/home/tulei
tulei@tulei:~$ cd -
/dev
tulei@tulei:/dev$ cd -
/home/tulei
```

### find

```shell
#find 使用时记得加 . -name,并且，后边名称部分可以使用通配符*来进行模糊查找 
tulei@tulei:~$ ls
hello.txt  test
tulei@tulei:~$ ls test
hello_1.txt
tulei@tulei:~$ find hello.txt
hello.txt
tulei@tulei:~$ find hello_1.txt
find: ‘hello_1.txt’: No such file or directory
tulei@tulei:~$ find -name hello_1.txt
./test/hello_1.txt
tulei@tulei:~$ find . -name hello_1.txt
./test/hello_1.txt
tulei@tulei:~$
```

---

## 对文件的操作：

### mkdir & mv

mkdir：创建新的子目录

mv：重命名，或移动文件/文件夹

```shell
#重命名文件
tulei@tulei:~$ ls
hello.txt
tulei@tulei:~$ mv hello.txt a.txt
tulei@tulei:~$ ls
a.txt
tulei@tulei:~$ cat a.txt
hello

#创建一个文件夹test,并将之前的文件移动到文件夹
tulei@tulei:~$ mkdir test
tulei@tulei:~$ ls
hello.txt  test
tulei@tulei:~$ mv hello.txt test
tulei@tulei:~$ ls
test
tulei@tulei:~$ ls test
hello.txt
tulei@tulei:~$
```

### cp

复制到目标位置：

```bash
tulei@tulei:~$ ls
test
tulei@tulei:~$ cp test/hello.txt ./
tulei@tulei:~$ ls
hello.txt  test
tulei@tulei:~$ ls test
hello.txt
tulei@tulei:~$
```

### rm

rm 只能删除文件，不能删除文件夹，而删除文件夹需要 rmdir

但是 rmdir 只允许删除空文件夹，

所以想要完整删除整个文件夹，需要使用递归操作，不过真正使用时比较简短

```bash
tulei@tulei:~$ ls
hello.txt  test

#rm 无法删除文件夹但可以直接删除文件
tulei@tulei:~$ rm test
rm: cannot remove 'test': Is a directory
tulei@tulei:~$ rm hello.txt
tulei@tulei:~$ ls
test

#rmdir 只能删除空文件夹
tulei@tulei:~$ rmdir test
rmdir: failed to remove 'test': Directory not empty

#rm -r 递归的删除整个文件夹
tulei@tulei:~$ rm -r test
tulei@tulei:~$ ls
tulei@tulei:~$
```

---

## Stream

程序与外界交互实际上依赖于 stream

他的输入都来自于 input stream，而输出都会去 output stream（一般来说，input stream 和 output stream 都被定位在我们的屏幕上）

但，shell 提供了重定向这些 stream 的方法：

### > & <

<：重定向这个程序的输入流，把程序的输入变成后边文件的内容

> ：重定向这个程序的输出流，把程序的输出变成后边文件的输入

```shell
#最开始，当我使用 echo hello，这个程序的输出流就有了 hello,并将输出流的内容打印在屏幕上
tulei@tulei:~$ echo hello
hello

#而我可以使用 > 将上述程序的输出流重定向到一个文件
tulei@tulei:~$ ls
tulei@tulei:~$ echo hello > hello.txt
tulei@tulei:~$ ls
hello.txt
tulei@tulei:~$ cat hello.txt
hello

#实际上,cat 的逻辑是,将输入原封不动的打印出来
#所以我可以使用 < 将一个文件的内容替换为程序的输入
tulei@tulei:~$ cat < hello.txt
hello
tulei@tulei:~$ 

#尽管上边几个操作看起来差不多,但请你理解stream实在做什么
```

在理解了 input stream 和 output stream 之后,我想你可以理解以下操作:

```shell
#流操作是从左向右依次执行的
tulei@tulei:~$ cat < hello.txt > hello_2.txt
tulei@tulei:~$ ls
hello.txt  hello_2.txt
tulei@tulei:~$ cat hello_2.txt
hello
tulei@tulei:~$
```

他将一个文件的内容读取入自己的 input stream, 并把自己的 out stream 重定向为另一个文件,实现了复制操作

### >>

‘>>' :他不是像 > 一样暴力地覆盖掉原内容,而是将输出内容加入原内容后边


```bash
tulei@tulei:~$ ls
hello.txt
tulei@tulei:~$ cat hello.txt
hello
tulei@tulei:~$ echo test > hello.txt
tulei@tulei:~$ cat hello.txt
test
tulei@tulei:~$ echo "hello again" >> hello.txt
tulei@tulei:~$ cat hello.txt
test
hello again
tulei@tulei:~$
```

### |

Pipe(|):他可以将左边程序的输入变成右边程序的输出

```shell
tulei@tulei:~$ ls
hello.txt  hello_2.txt
tulei@tulei:~$ ls -l
total 8
-rw-r--r-- 1 tulei tulei 6 Nov  8 20:19 hello.txt
-rw-r--r-- 1 tulei tulei 6 Nov  8 20:28 hello_2.txt
# tail 是将输入内容的最后几行输出的程序,后边的选项 -1表示只输出最后一行
#下边这个程序就将ls -l输出的内容,只选取最后一行输出
tulei@tulei:~$ ls -l | tail -n1
-rw-r--r-- 1 tulei tulei 6 Nov  8 20:28 hello_2.txt
tulei@tulei:~$
```

实际上你可以意识到,shell 通过对 stream 的操作,可以实现不同程序之间的交互

pipe 不只可以用来处理文本,他也可以被用来处理二进制图片,甚至视频

---

## sudo

在我们需要以 root 权限运行某些程序时，实际上就在使用 sudo

直接使用会报错：

```shell
#因为在使用linux子系统，所以实际上这个系统中并不存在一个真实的屏幕，
#所以也就无法完成这个演示的后半部分：修改屏幕亮度

#使用普通用户权限时会出现：Permission denied
#但直接使用sudo echo 500 > brightness，也会出现 Permission denied

#为什么呢？ 
#因为程序只知道自己的输入和输出，所以上边的代码做了如下两件事情：
#1，为程序 sudo 输入 “echo 500”
#2,发送sudo的输出到 brightness这个文件 
#（但shell 在打开 brightness这个文件时，并不是在使用root权限，依旧是自己在尝试打开brightness,所以没有权限）

#正确的打开方式
#使用sudo su 登录root用户的身份，之后再进行更改

tulei@tulei:/$ ls
bin   dev  home  lib    lib64   lost+found  mnt  proc  run   snap  sys  usr
boot  etc  init  lib32  libx32  media       opt  root  sbin  srv   tmp  var
tulei@tulei:/$ cd sys
tulei@tulei:/sys$ ls
block  bus  class  dev  devices  firmware  fs  hypervisor  kernel  module  power
tulei@tulei:/sys$ cd class
tulei@tulei:/sys/class$ ls
ata_device   devlink          hwmon             mdio_bus        power_supply    sas_expander   thermal
ata_link     dma              input             mem             powercap        sas_host       tpm
ata_port     drm              intel_scu_ipc     misc            ppp             sas_phy        tpmrm
backlight    fc_host          iommu             nd              pps             sas_port       tty
bdi          fc_remote_ports  iscsi_connection  net             ptp             scsi_device    vc
block        fc_transport     iscsi_endpoint    nvme            pwm             scsi_disk      virtio-ports
bsg          fc_vports        iscsi_host        nvme-generic    raid_devices    scsi_generic   vtconsole
dca          firmware         iscsi_iface       nvme-subsystem  rtc             scsi_host      wakeup
devcoredump  gpio             iscsi_session     pci_bus         sas_device      spi_host       watchdog
devfreq      graphics         iscsi_transport   phy             sas_end_device  spi_transport
tulei@tulei:/sys/class$ cd backlight
tulei@tulei:/sys/class/backlight$ ls
```

登录 root 用户身份

```shell
tulei@tulei:/sys/class/backlight$ sudo su
[sudo] password for tulei: 
root@tulei:/sys/class/backlight#
```

可以看到，现在我以 root 身份登录了终端，并且由原来的 $ 变成了 #，表明我可以使用管理员权限来进行操作

退出 root 身份：

```typescript
root@tulei:/sys/class/backlight# exit
exit
tulei@tulei:/sys/class/backlight$
```

当然上述打开亮度的操作还有比较方便的一种方式

```shell
#tee 是一个命令，他将自己的收到的输入 写入 一个文件中，并打印结果
echo 500 | sudo tee brightness
```

为什么他能成功？

因为在打开 brightness 时，使用了 root 权限（之前不行是因为你依旧是在使用用户权限去打开文件）

# Exercises

- For this course, you need to be using a Unix shell like Bash or ZSH. If you are on Linux or macOS, you don’t have to do anything special. If you are on Windows, you need to make sure you are not running cmd.exe or PowerShell; you can use <u>Windows Subsystem for Linux</u> or a Linux virtual machine to use Unix-style command-line tools. To make sure you’re running an appropriate shell, you can try the command `echo $SHELL`. If it says something like `/bin/bash` or `/usr/bin/zsh`, that means you’re running the right program.

![](static/G42xb00nToyuABxn9G4cgLmSnfb.png)

- Create a new directory called `missing` under `/tmp`.
  ![](static/Aepvb5SqgoHXedx6xRLcOegxnHc.png)
- Look up the `touch` program. The `man` program is your friend.
- Use `touch` to create a new file called `semester` in `missing`.

![](static/W58wbEh3yoiz5OxToeNcTwnhnjc.png)

- Write the following into that file, one line at a time:

```
#!/bin/sh
curl --head --silent https://missing.csail.mit.edu
```

- The first line might be tricky to get working. It’s helpful to know that `#` starts a comment in Bash, and `!` has a special meaning even within double-quoted (`"`) strings. Bash treats single-quoted strings (`'`) differently: they will do the trick in this case. See the Bash <u>quoting</u> manual page for more information.

![](static/EOfkbIxhHoeutIxDcUUc84Ecnrg.png)

- Try to execute the file, i.e. type the path to the script (`./semester`) into your shell and press enter. Understand why it doesn’t work by consulting the output of `ls` (hint: look at the permission bits of the file).

![](static/LfDlbzKtqoDV58xuPA9cOI4Jnyd.png)

![](static/GXJNbtBOAoUOupxbuohckxgEnFb.png)

- Run the command by explicitly starting the `sh` interpreter, and giving it the file `semester` as the first argument, i.e. `sh semester`. Why does this work, while `./semester` didn’t?

![](static/XUjpb8zfzo4NbFx0PxJcXrgAndr.png)

- Look up the `chmod` program (e.g. use `man chmod`).
- Use `chmod` to make it possible to run the command `./semester` rather than having to type `sh semester`. How does your shell know that the file is supposed to be interpreted using `sh`? See this page on the <u>shebang</u> line for more information.

![](static/P9I9b4ClnoHPVMxYIZacQCODndf.png)

![](static/IkImbsgw8oMbdsxX1vBc4dxvngf.png)

- Use `|` and `>` to write the “last modified” date output by `semester` into a file called `last-modified.txt` in your home directory.

![](static/MTP7bl1Jooxq2wxaDPycF3RBnxb.png)

- Write a command that reads out your laptop battery’s power level or your desktop machine’s CPU temperature from `/sys`. Note: if you’re a macOS user, your OS doesn’t have sysfs, so you can skip this exercise.

windows 中的 linux 子系统貌似也找不到对应的温度信息
