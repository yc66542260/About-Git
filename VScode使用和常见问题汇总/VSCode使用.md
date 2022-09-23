---

---



# VSCode使用技巧和经验

## 使用技巧

### 1.配置VSCode的C++编译、调试、运行环境（Window环境）

> 主要参考：1. [在vscode创建c/C++项目（一劳永逸版）](https://www.bilibili.com/read/cv12725027)
>
> ​                   2. [在VSCode中搭建C++编译环境](https://blog.csdn.net/weixin_45695382/article/details/125835139)

主要步骤：

1.安装mingw，把其中的bin文件放入“系统的PATH”

安装MinGW
附上下载链接，下载安装即可。

> 链接：[MinGW](https://nuwen.net/mingw.html)或者[MinGW - Minimalist GNU for Windows](https://sourceforge.net/projects/mingw/)

配置系统环境变量
打开安装后的MinGW文件，找到并打开目录下的bin目录，复制目录绝对路径。（红框前面部分换成自己MinGW存放的目录）

打开此电脑，右键空白部分点击属性->高级系统设置->环境变量，双击打开用户变量中的Path目录（也可以是系统变量中的Path目录），选择新建，将复制好的MinGW\bin绝对路径复制进去，确认即可。如下图所示。

![](./%5CFig%5C1.png)

配置完后，win+R，输入cmd打开命令行，输入g++ --version，可返回版本信息说明路径配置成功。

此时会出现GCC的版本信息。

![](./%5CFig%5C2.png)

2.安装vscode

下载并安装

3.安装C/C++ 相关三个插件：

1. C/C++

   ![](./%5CFig%5C3.png)

2. C/C++ Project Generator

   ![](./%5CFig%5C4.png)

3. C/C++ run coder

   安装code runner后，就可以使用ctrl+alt+n快捷键运行程序，使用ctrl+alt+m快捷键暂停运行中的程序

![](./%5CFig%5C5.png)

4.新建项目（C++ project）

a. ctrl+shift+p，选择Create C/C++ project，创建了一个project。

![](./%5CFig%5C6.png)

b. 运行程序可以通过上述的快捷键，也可以通过下图方式运行。

![](./%5CFig%5C7.png)

## 相关问题

### 1.关于VSCode中文乱码

如果中文出现乱码情况
点击左上角文件-首选项-设置，在设置中搜索encoding，将默认设置的UTF-8修改为GBK，即可显示中文。

![](./%5CFig%5C8.png)

PS：对已经创建的文件无效，如果要修改已经创建好的文件，则点击右下角按键，按照GBK编码再保存一次即可。

![](./%5CFig%5C9.png)