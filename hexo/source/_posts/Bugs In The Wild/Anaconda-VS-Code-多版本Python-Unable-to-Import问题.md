---
title: Anaconda+VS Code+多版本Python的"Unable to Import"问题解决方案
date: 2020-02-27 12:18:43
author:
img:
categories: Bugs In The Wild
tags:
    - Python
    - VS Code
    - Anaconda
summary: 如题。自行研究（Google）后得到的解决方案。
top:
hidden:
password:
mathjax:
---
#### 问题描述

Win10系统，借助Anaconda2安装了Python（2.7版本），同时又通过`conda create --name py3 python=3.6`命令安装了Python 3.6版本。

这样可以在两个版本之间任意切换。以VS Code为例，只需使用`Ctrl + Shift + P` -> `Python: Select Interpreter`即可。

但在使用过程中遇到了问题，尽管Anaconda已经安装了绝大部分常用库，如果在VS Code使用Python 3.6版本，pylint会对诸如`import numpy`的命令进行报错，提示没有这个库（具体提示信息记不清了）。如果这时进行`pip install numpy`，又会提示已经有这个库了。切换回Python 2.7则一切正常。

#### 解决办法

这个的原因在于pylint只搜索默认的路径。解决办法是打开`settings.json`（或在设置里搜索`python.autoComplete.extraPaths`），如果已经有`python.autoComplete.extraPaths`这一项，则在其中加入库所在的路径，如果没有则加入类似以下代码：
~~~
"python.autoComplete.extraPaths": [
        "D:\\Special\\Anaconda2\\Lib",
        "D:\\Special\\Anaconda2\\Library",
        "D:\\Special\\Anaconda2\\libs",
        "D:\\special\\Anaconda2\\Lib\\site-packages\\numpy"
    ],
~~~
把其中的具体路径替换成自己的本地路径即可。实际应该只加入Lib文件夹路径就够了。如果是其他外部库，途径类似（Ref中的例子是google-cloud-sdk的库）。

**注意**：更改完毕设置后，需要重启电脑才能生效，只重启VS Code似乎不行。

****

更新：

在某一次装了另一个版本的Python后不久，这个方法莫名失效了……:unamused::unamused::unamused:


****
*Ref*:

https://stackoverflow.com/questions/43574995/visual-studio-code-pylint-unable-to-import-protorpc
