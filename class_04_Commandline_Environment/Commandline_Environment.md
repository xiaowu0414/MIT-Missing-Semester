---
date: 2025-11-14
tags:
  - Shell
---
## Job Control

### killing a process

shell 使用一种称为信号（signal） 的 UNIX 通信机制来向进程传递信息。大多数情况下，您可以按 Ctrl-C 来停止命令.按下 Ctrl-C 会提示 shell 向进程发送 SIGINT 信号。

这是一个最简单的 Python 程序示例，它可以捕获 SIGINT 并忽略它，不再停止运行。现在，我们可以通过输入 Ctrl-\ 来使用 SIGQUIT 信号终止此程序。

```python
#!/usr/bin/env python
import signal, time

def handler(signum, time):
    print("\nI got a SIGINT, but I am not stopping")

signal.signal(signal.SIGINT, handler)
i = 0
while True:
    time.sleep(.1)
    print("\r{}".format(i), end="")
    i += 1
```

可以看到，在使用python3运行这个python脚本时，并不会终止进程，需要使用 Ctrl-\ 来进行终止。

```bash
tulei@tulei:~/test_0$ python3 sigint.py 
670^C
I got a SIGINT, but I am not stopping
680^C
I got a SIGINT, but I am not stopping
691^C
I got a SIGINT, but I am not stopping
768^_
1034^\Quit (core dumped)
```

虽然 SIGINT 和 SIGQUIT 通常都与终端相关的请求有关，但更通用的用于请求进程优雅退出的信号是 SIGTERM 信号。要发送此信号，我们可以使用 kill 命令，语法为```kill -TERM <PID> ``` 。

### 暂停和后台进程

除了终止进程之外，信号还可以执行其他操作。例如， SIGSTOP 信号可以暂停进程。在终端中，按下 Ctrl-Z 会提示 shell 发送 SIGTSTP 信号，然后，我们可以分别使用 fg 或 bg 在前台或后台继续执行暂停的作业。

```bash
tulei@tulei:~/test_0$ python3 sigint.py 
20^Z
[1]+  Stopped                 python3 sigint.py
tulei@tulei:~/test_0$ fg
python3 sigint.py
34^Z
[1]+  Stopped                 python3 sigint.py
tulei@tulei:~/test_0$ bg
[1]+ python3 sigint.py &
72gei@tulei:~/test_0$ 
python3 sigint.py
95^Z
[1]+  Stopped                 python3 sigint.py
tulei@tulei:~/test_0$ 
```

按bg后，进入后台执行的作业可以再次按fg，让他来到前台。在后台运行的程序是不能被 CTRL + z , Ctrl + \ 等信号关闭的。

jobs 命令会列出与当前终端会话关联的未完成作业。您可以使用作业的进程 ID (PID) 来引用这些作业（可以使用 pgrep 查找 PID）。更直观的方式是，您还可以使用百分号 (%) 后跟作业编号（由 jobs 显示）来引用进程。要引用最后一个后台作业，您可以使用特殊参数 $!

```bash
tulei@tulei:~/test_0$ jobs
tulei@tulei:~/test_0$ python3 sigint.py 
14^Z
[1]+  Stopped                 python3 sigint.py
tulei@tulei:~/test_0$ jobs
[1]+  Stopped                 python3 sigint.py
tulei@tulei:~/test_0$ bg
[1]+ python3 sigint.py &
160python3 sigint.py 
208^Z
[2]+  Stopped                 python3 sigint.py
240gi@tulei:~/test_0$ 
[2]+ python3 sigint.py &
283si@tulei:~/test_0$ 
[1]-  Running                 python3 sigint.py &
[2]+  Running                 python3 sigint.py &
```

当你按下 %1 时，会把id为1的任务从后台移到前台。如果他之前被暂停了，也会被重新启动。

还有一点，当你使用jobs 来查看还没结束的程序时，```& ``` 符号表示这个程序存在于后台。在运行程序时你也可以加上此后缀，来表示将程序放在后台运行。

```bash
tulei@tulei:~/test_0$ jobs
tulei@tulei:~/test_0$ python3 sigint.py 
39^Z
[1]+  Stopped                 python3 sigint.py
tulei@tulei:~/test_0$ bg
[1]+ python3 sigint.py &
82sei@tulei:~/test_0$ 
[1]+  Running                 python3 sigint.py &
1681i@tulei:~/test_0$ 
python3 sigint.py
186^\Quit (core dumped)
tulei@tulei:~/test_0$ 
```

请注意，后台进程仍然是终端的子进程，如果您关闭终端，它们将会终止（这将发送另一个信号 SIGHUP ）。为了防止这种情况发生，您可以使用 nohup （一个忽略 SIGHUP 的包装器）运行程序，或者如果进程已经启动，则使用 disown 。此外，您还可以使用终端复用器terminal multiplexers，我们将在下一节中看到。

下边示例中，任务2使用了nohup,任务一没有使用，我们可以看到，任务一在收到 kill -HUP 时就停止了，而任务二就算收到了 kill -HUP 依然会忽略他，继续运行。

```bash
tulei@tulei:~/test_0$ jobs
tulei@tulei:~/test_0$ python3 sigint.py 
196^Z
[1]+  Stopped                 python3 sigint.py
tulei@tulei:~/test_0$ nohup python3 sigint.py 
nohup: ignoring input and appending output to 'nohup.out'
^Z
[2]+  Stopped                 nohup python3 sigint.py
tulei@tulei:~/test_0$ jobs
[1]-  Stopped                 python3 sigint.py
[2]+  Stopped                 nohup python3 sigint.py
tulei@tulei:~/test_0$ kill -HUP %1
[1]-  Hangup                  python3 sigint.py
tulei@tulei:~/test_0$ jobs
[2]+  Stopped                 nohup python3 sigint.py
tulei@tulei:~/test_0$ kill -HUP %2
tulei@tulei:~/test_0$ jobs
[2]+  Running                 nohup python3 sigint.py &
tulei@tulei:~/test_0$ 
```

nohup 运行的命令可以被 Ctrl + \ 来终止，因为 nohup 只是让程序忽略了 kill -HUP 信号。

```bash
tulei@tulei:~/test_0$ jobs
[2]+  Running                 nohup python3 sigint.py &
tulei@tulei:~/test_0$ %2
nohup python3 sigint.py
^\Quit (core dumped)
tulei@tulei:~/test_0$ jobs
tulei@tulei:~/test_0$ 
```

kill -STOP 也可以作为暂停符号来替代 Ctrl + Z

SIGKILL 信号比较特殊，因为它无法被进程捕获，并且会立即终止进程。但是，它也可能带来一些不良的副作用，例如留下孤儿进程。（也就是，这个进程还有很多子进程在运行，如果你只关掉了父进程，但是子进程依旧在运行）

您可以点击此处或输入 man signal 或 kill -l 来了解更多关于这些信号和其他信号的信息。

---
## terminal multiplexers

像 tmux 这样的终端复用器允许你使用 panes and tabs 来复用多个终端窗口，从而可以同时与多个 shell sessions进行交互。此外，终端复用器还允许你分离当前的终端会话，并在稍后重新连接。这可以大大提升你与远程机器交互时的工作流程，因为它避免了使用 nohup 等类似技巧。


在tmux中运行的程序不会因为关闭外部终端（比如你通过SSH连接的客户端，或者本地终端窗口）而停止。tmux是一个终端多路复用器，它会在后台运行，保持会话中的程序继续执行。

当你断开与tmux的连接（比如关闭终端窗口）时，tmux会话会在后台继续运行，包括其中正在运行的程序。你可以重新连接到这个tmux会话来查看程序的输出和交互。

他的一些常用命令在下边：有人会将tmux 的 Ctrl +b 映射为 Ctrl + a.

tmux expects you to know its keybindings, and they all have the form `<C-b> x` where that means (1) press Ctrl+b, (2) release Ctrl+b, and then (3) press x. tmux has the following hierarchy of objects:

- **Sessions** - a session is an independent workspace with one or more windows
	- `tmux` starts a new session.
	- `tmux new -s NAME` starts it with that name.
	- `tmux ls` lists the current sessions
	- Within `tmux` typing `<C-b> d` detaches the current session
	- `tmux a` attaches the last session. You can use `-t` flag to specify which
- **Windows** - Equivalent to tabs in editors or browsers, they are visually separate parts of the same session
	- `<C-b> c` Creates a new window. To close it you can just terminate the shells doing `<C-d>`
	- `<C-b> N` Go to the N th window. Note they are numbered
	- `<C-b> p` Goes to the previous window
	- `<C-b> n` Goes to the next window
	- `<C-b> ,` Rename the current window
	- `<C-b> w` List current windows
- **Panes** - Like vim splits, panes let you have multiple shells in the same visual display.
	- `<C-b> "` Split the current pane horizontally
	- `<C-b> %` Split the current pane vertically
	- `<C-b> <direction>` Move to the pane in the specified direction. Direction here means arrow keys.
	- `<C-b> z` Toggle zoom for the current pane
	- `<C-b> [` Start scrollback. You can then press `<space>` to start a selection and `<enter>` to copy that selection.
	- `<C-b> <space>` Cycle through pane arrangements.

For further reading, [here](https://hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) is a quick tutorial on tmux and [this](http://linuxcommand.org/lc3_adv_termmux.php) has a more detailed explanation that covers the original screen command. You might also want to familiarize yourself with [screen](https://www.man7.org/linux/man-pages/man1/screen.1.html), since it comes installed in most UNIX systems.

## Aliases

输入包含大量标志或冗长选项的长命令可能会很麻烦。因此，大多数 shell 都支持别名 。shell 别名是另一个命令的简写形式，shell 会自动将其替换为你实际使用的命令。例如，bash 中的别名结构如下：

```bash
alias alias_name="command_to_alias arg1 arg2"
```

这里等号左右没有空格，和一般shell的变量命名相同。

```bash
# Make shorthands for common flags
alias ll="ls -lh"

# Save a lot of typing for common commands
alias gs="git status"
alias gc="git commit"
alias v="vim"

# Save you from mistyping
alias sl=ls

# Overwrite existing commands for better defaults
alias mv="mv -i"           # -i prompts before overwrite
alias mkdir="mkdir -p"     # -p make parent dirs as needed
# 在shell那一节，使用df -Th 可以输出人类可读的磁盘挂载
alias df="df -h"           # -h prints human readable format

# Alias can be composed
alias la="ls -A"
alias lla="la -l"

# To ignore an alias run it prepended with \
\ls
# Or disable an alias altogether with unalias
unalias la

# To get an alias definition just call it with alias
alias ll
# Will print ll='ls -lh'
```

## Dotfiles









