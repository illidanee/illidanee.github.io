---
layout: post
title: Win7下安装配置CodeBlocks
categories: 
 - C
tags:
 - C
---

* content
{:toc}

> CodeBlocks简单配置使用, 这里使用MinGW编译器。




## 工具

* [CodeBlocks](www.codeblocks.org)

## 安装配置

> 安装CodeBlocks

直接安装。

> 配置CodeBlocks

1. 配置ToolChain

点击[Settings]->[Compiler]->[ToolsChain executables]，配置如下：

```
[Compiler's installation directory] = [MinGW安装目录]
[C compiler] = [gcc.exe]
[C++ compiler] = [g++.exe]
[Linker for dynamic libs] = [g++.exe]
[Linker for static libs] = [ar.exe]
[Resource] = [windres.exe]
[Make program] = [make.exe]
```

2. 配置Debugger

点击[Settings]->[Debugger]->[GDB/CDB debugger]->[Default], 配置如下：

```
[Executable path] = [MinGW安装目录\bin\gdb.exe]
```