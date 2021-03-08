# Visio Studio 2019 学习

[TOC]

## 常用快捷键

如果选中行的目的是为了删除，复制，移动，就别选中任何东西，光标放在行中：
alt + ↑, alt + ↓;//上下移动该行
ctrl + c，ctrl + v ,ctrl + x, ctrl + l; //复制、粘贴、剪贴、删除整行
ctrl + enter, ctrl + shift + enter; //在当前行上下方插入新行
ctrl + u, ctrl+ shift +u; //光标右侧字符改为 小/大写，并 ++光标位置

ctrl+shift+/ 是注释代码块

tab 是应用自动补齐的内容

Ctrl+BackSpace，Ctrl+Delete整词删除

Ctrl+Left，Ctrl+Right按整词移动光标

Ctrl+k+f format代码

## 如何设置快捷键

工具 -> 选项 -> 环境 -> 键盘

<img src="C:\E\typora\Pictures\image-20200417105751226.png" alt="image-20200417105751226" style="zoom: 50%;" />

可以在位置1搜索命令，在位置2处设置新的快捷键，并且可以在位置3处看到冲突的快捷键，可以把它们一一搜索出来移除其快捷键，或者改到别的快捷键去。

> 参考：https://blog.csdn.net/wrzfeijianshen/article/details/77782939

## 如何建立多个各自带main函数的文件

### 方法一

文件 -> 添加 -> 新建项目

<img src="C:\E\typora\Pictures\image-20200417115540654.png" alt="image-20200417115540654" style="zoom:67%;" />

这样就可以在解决方案里面新添加一个项目，将想要运行的项目设置为启动项目后按ctrl + F5 就运行的是选中的项目了。

<img src="C:\E\typora\Pictures\image-20200417115739959.png" alt="image-20200417115739959"  />

> 参考文章：https://blog.csdn.net/u014485485/article/details/78127437

### 方法二

将不运行的带main函数的cpp文件从生成内容中排除就可以了（右键点击文件- > 属性 -> 从生成内容中排除设置为是），这样就一直只有一个main函数在运行了。

<img src="C:\E\typora\Pictures\image-20200417120544940.png" alt="image-20200417120544940" style="zoom:50%;" />

<img src="C:\E\typora\Pictures\image-20200417120611757.png" alt="image-20200417120611757" style="zoom: 50%;" />

# jetbrain学习

## 快捷键

ctrl+shift+y 翻译选中文字