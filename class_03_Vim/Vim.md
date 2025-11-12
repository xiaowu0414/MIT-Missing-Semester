---
date: 2025-11-11
tags:
  - Vim
---
## Vim 的模式转换

Vim 是基于 model 的编辑器，他有以下几种模式，使用 esc 即可返回 normal. 

![](attachments/Pasted%20image%2020251112163738.png)

因为我们需要经常返回Normal模式，而```esc``` 键又举例手指太远，所以推荐使用imap将esc映射为键盘上比较方便的按键，具体教程如下：[alternative mapping](https://vim.fandom.com/wiki/Avoid_the_escape_key#Mappings)

- **打开**：在shell界面，按下 vim + 文件名，即可打开vim编辑器
- **退出**：不断按 esc 直到退回到normal模式，之后按 : 进入command-line，按下wq，即可退出

好的，你已经学会打开与关闭了，请自己去下边提供的资源练一练吧。

下边介绍一个totorial 和一个游戏，你可能会用到

- `vimtutor` is a tutorial that comes installed with Vim - if Vim is installed, you should be able to run `vimtutor` from your shell
- [Vim Adventures](https://vim-adventures.com/) is a game to learn Vim







