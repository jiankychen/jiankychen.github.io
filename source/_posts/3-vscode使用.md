---
title: vscode 使用
categories:
  - Tutorial
cover: false
abbrlink: b84c37ec
date: 2021-10-26 21:42:36
tags:
---

# vscode 快捷键

## 主命令框
Ctrl + Shift + P: 打开命令面板

## 编辑器与窗口
Ctrl + K, V: 打开预览面板

Ctrl + `: 打开/关闭终端

Ctrl + Tab: 文件之间切换

Ctrl + W: 关闭当前窗口


## 代码编辑
Ctrl + F: 查找

Ctrl + H: 查找替换

Ctrl + Shift + T: 重新打开刚关闭的编辑页面

Ctrl + Backspace: 删除上一个单词

Ctrl + N: 新建文件

Ctrl + S: 保存

## 光标
Home: 光标移动至行首

End: 光标移动至行末

Ctrl + Home: 光标移至文件开头

Ctrl + End: 光标移至文件末尾

Shift + Home: 选择从光标到行首的内容

Shift + End: 选择从光标到行尾的内容

Alt + Up: 向上移动一行

Alt + Down: 向下移动一行

## 调试、运行
Ctrl + Shift + B: 重启调试

Ctrl + F5: 运行但不调试

F5: 启动调试


# 终端 Terminal

## 配置 cmd

以管理员身份运行`powershell`

 - 输入`set-ExecutionPolicy RemoteSigned`然后回车

 - 根据提示，输入`Y`回车即可


## hexo 指令
`hexo n " "`: 新建md文件

`hexo c`: 清理

`hexo g`: 生成

`hexo d`: 部署到github

## g++ 指令
`g++ hello.cpp -o hello.exe`: 输出编译后的结果到指定的文件file中 (输出指定的可执行文件 hello.exe)，-o不仅可以指定输出的可执行文件，还可以指定中间文件的输出

`g++ -o hello.exe hello.cpp`: 同上

`g++ hello.cpp`: 输出的是 a.exe

`g++ -E -o hello.i hello.cpp`: 对源文件进行预处理，预处理后生成.i(或者是 .ii)文件，对hello.cpp进行预处理生成hello.i文件

`g++ -S -o hello.s hello.cpp`: 只进行预处理和编译，生成汇编文件hello.s

`g++ -c -o hello.o hello.cpp`: 只进行预处理，编译，汇编操作，生成.o (.obj)文件，不进行链接