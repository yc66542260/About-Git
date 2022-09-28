# CMake简单教程
> 原文链接：[超详细的cmake入门教程](https://www.jb51.net/article/180467.htm)

什么是cmake**

你或许听过好几种 Make 工具，例如 GNU Make ，QT 的 qmake ，微软的 MSnmake，BSD Make（pmake），Makepp，等等。这些 Make 工具遵循着不同的规范和标准，所执行的 Makefile 格式也千差万别。这样就带来了一个严峻的问题：如果软件想跨平台，必须要保证能够在不同平台编译。而如果使用上面的 Make 工具，就得为每一种标准写一次 Makefile ，这将是一件让人抓狂的工作。
CMake CMake附图 1 CMake就是针对上面问题所设计的工具：它首先允许开发者编写一种平台无关的 CMakeList.txt 文件来定制整个编译流程，然后再根据目标用户的平台进一步生成所需的本地化 Makefile 和工程文件，如 Unix 的 Makefile 或 Windows 的 Visual Studio 工程。从而做到“Write once, run everywhere”。显然，CMake 是一个比上述几种 make 更高级的编译配置工具。一些使用 CMake 作为项目架构系统的知名开源项目有 VTK、ITK、KDE、OpenCV、OSG 等。

在 linux 平台下使用 CMake 生成 Makefile 并编译的流程如下：

1. 编写 CMake 配置文件 CMakeLists.txt 。
2. 执行命令 cmake PATH 或者 ccmake PATH 生成 Makefile。其中， PATH 是 CMakeLists.txt 所在的目录。（ccmake 和 cmake 的区别在于前者提供了一个交互式的界面）
3. 使用 make 命令进行编译。

**入门案例：单个源文件**

本节对应的源代码所在目录：Demo1。
对于简单的项目，只需要写几行代码就可以了。例如，假设现在我们的项目中只有一个源文件 main.cc ，该程序的用途是计算一个数的指数幂。

```c++
#include <stdio.h>
#include <stdlib.h>
/**
 * power - Calculate the power of number.
 * @param base: Base value.
 * @param exponent: Exponent value.
 *
 * @return base raised to the power exponent.
 */
double power(double base, int exponent)
{
  int result = base;
  int i;
  
  if (exponent == 0) {
    return 1;
  }
  
  for(i = 1; i < exponent; ++i){
    result = result * base;
  }
  return result;
}
int main(int argc, char *argv[])
{
  if (argc < 3){
    printf("Usage: %s base exponent \n", argv[0]);
    return 1;
  }
  double base = atof(argv[1]);
  int exponent = atoi(argv[2]);
  double result = power(base, exponent);
  printf("%g ^ %d is %g\n", base, exponent, result);
  return 0;
}

```

**编写 CMakeLists.txt**

首先编写 CMakeLists.txt 文件，并保存在与 main.cc 源文件同个目录下：

> \# CMake 最低版本号要求
> cmake_minimum_required (VERSION 2.8)
> \# 项目信息
> project (Demo1)
> \# 指定生成目标
> add_executable(Demo main.cc)

CMakeLists.txt 的语法比较简单，由命令、注释和空格组成，其中命令是不区分大小写的。符号 # 后面的内容被认为是注释。命令由命令名称、小括号和参数组成，参数之间使用空格进行间隔。

对于上面的 CMakeLists.txt 文件，依次出现了几个命令：

1. cmake_minimum_required：指定运行此配置文件所需的 CMake 的最低版本；
2. project：参数值是 Demo1，该命令表示项目的名称是 Demo1 。
3. add_executable： 将名为 main.cc 的源文件编译成一个名称为 Demo 的可执行文件。

**编译项目**

之后，在当前目录执行 cmake . ，得到 Makefile 后再使用 make 命令编译得到 Demo1 可执行文件。

```c++
[ehome@xman Demo1]$ cmake .
-- The C compiler identification is GNU 4.8.2
-- The CXX compiler identification is GNU 4.8.2
-- Check for working C compiler: /usr/sbin/cc
-- Check for working C compiler: /usr/sbin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working CXX compiler: /usr/sbin/c++
-- Check for working CXX compiler: /usr/sbin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/ehome/Documents/programming/C/power/Demo1
[ehome@xman Demo1]$ make
Scanning dependencies of target Demo
[100%] Building C object CMakeFiles/Demo.dir/main.cc.o
Linking C executable Demo
[100%] Built target Demo
[ehome@xman Demo1]$ ./Demo 5 4
5 ^ 4 is 625
[ehome@xman Demo1]$ ./Demo 7 3
7 ^ 3 is 343
[ehome@xman Demo1]$ ./Demo 2 10
2 ^ 10 is 1024

```

**多个源文件**

同一目录，多个源文件

本小节对应的源代码所在目录：Demo2。
上面的例子只有单个源文件。现在假如把 power 函数单独写进一个名为MathFunctions.c 的源文件里，使得这个工程变成如下的形式：

> ./Demo2
>   |
>   +--- main.cc
>   |
>   +--- MathFunctions.cc
>   |
>   +--- MathFunctions.h

这个时候，CMakeLists.txt 可以改成如下的形式：

> \# CMake 最低版本号要求
> cmake_minimum_required (VERSION 2.8)
> \# 项目信息
> project (Demo2)
> \# 指定生成目标
> add_executable(Demo main.cc MathFunctions.cc)

唯一的改动只是在 add_executable 命令中增加了一个 MathFunctions.cc 源文件。这样写当然没什么问题，但是如果源文件很多，把所有源文件的名字都加进去将是一件烦人的工作。更省事的方法是使用 aux_source_directory 命令，该命令会查找指定目录下的所有源文件，然后将结果存进指定变量名。其语法如下：

> aux_source_directory(<dir> <variable>)

因此，可以修改 CMakeLists.txt 如下：

> \# CMake 最低版本号要求
> cmake_minimum_required (VERSION 2.8)
> \# 项目信息
> project (Demo2)
> \# 查找当前目录下的所有源文件
> \# 并将名称保存到 DIR_SRCS 变量
> aux_source_directory(. DIR_SRCS)
> \# 指定生成目标
> add_executable(Demo ${DIR_SRCS})

这样，CMake 会将当前目录所有源文件的文件名赋值给变量 DIR_SRCS ，再指示变量 DIR_SRCS 中的源文件需要编译成一个名称为 Demo 的可执行文件。

**多个目录，多个源文件**

本小节对应的源代码所在目录：Demo3。
现在进一步将 MathFunctions.h 和 MathFunctions.cc 文件移动到 math 目录下。

> ./Demo3
>   |
>   +--- main.cc
>   |
>   +--- math/
>      |
>      +--- MathFunctions.cc
>      |
>      +--- MathFunctions.h



对于这种情况，需要分别在项目根目录 Demo3 和 math 目录里各编写一个 CMakeLists.txt 文件。为了方便，我们可以先将 math 目录里的文件编译成静态库再由 main 函数调用。

根目录中的 CMakeLists.txt ：

> \# CMake 最低版本号要求
> cmake_minimum_required (VERSION 2.8)
> \# 项目信息
> project (Demo3)
> \# 查找当前目录下的所有源文件
> \# 并将名称保存到 DIR_SRCS 变量
> aux_source_directory(. DIR_SRCS)
> \# 添加 math 子目录
> add_subdirectory(math)
> \# 指定生成目标
> add_executable(Demo main.cc)
> \# 添加链接库
> target_link_libraries(Demo MathFunctions)

该文件添加了下面的内容: 第3行，使用命令 add_subdirectory 指明本项目包含一个子目录 math，这样 math 目录下的 CMakeLists.txt 文件和源代码也会被处理 。第6行，使用命令 target_link_libraries 指明可执行文件 main 需要连接一个名为 MathFunctions 的链接库 。

子目录中的 CMakeLists.txt：

> \# 查找当前目录下的所有源文件
> \# 并将名称保存到 DIR_LIB_SRCS 变量
> aux_source_directory(. DIR_LIB_SRCS)
> \# 生成链接库
> add_library (MathFunctions ${DIR_LIB_SRCS})

 在该文件中使用命令 add_library 将 src 目录中的源文件编译为静态链接库。

**自定义编译选项**

本节对应的源代码所在目录：Demo4。
CMake 允许为项目增加编译选项，从而可以根据用户的环境和需求选择最合适的编译方案。

例如，可以将 MathFunctions 库设为一个可选的库，如果该选项为 ON ，就使用该库定义的数学函数来进行运算。否则就调用标准库中的数学函数库。
修改 CMakeLists 文件

我们要做的第一步是在顶层的 CMakeLists.txt 文件中添加该选项： 

> \# CMake 最低版本号要求
> cmake_minimum_required (VERSION 2.8)
> \# 项目信息
> project (Demo4)
> \# 加入一个配置头文件，用于处理 CMake 对源码的设置
> configure_file (
> "${PROJECT_SOURCE_DIR}/config.h.in"
> "${PROJECT_BINARY_DIR}/config.h"
> )
> \# 是否使用自己的 MathFunctions 库
> option (USE_MYMATH
> "Use provided math implementation" ON)
> \# 是否加入 MathFunctions 库
> if (USE_MYMATH)
> include_directories ("${PROJECT_SOURCE_DIR}/math")
> add_subdirectory (math)
> set (EXTRA_LIBS ${EXTRA_LIBS} MathFunctions)
> endif (USE_MYMATH)
> \# 查找当前目录下的所有源文件
> \# 并将名称保存到 DIR_SRCS 变量
> aux_source_directory(. DIR_SRCS)
> \# 指定生成目标
> add_executable(Demo ${DIR_SRCS})
> target_link_libraries (Demo ${EXTRA_LIBS})

其中：

第7行的 configure_file 命令用于加入一个配置头文件 config.h ，这个文件由 CMake 从 config.h.in 生成，通过这样的机制，将可以通过预定义一些参数和变量来控制代码的生成。
第13行的 option 命令添加了一个 USE_MYMATH 选项，并且默认值为 ON 。
第17行根据 USE_MYMATH 变量的值来决定是否使用我们自己编写的 MathFunctions 库。

**修改 main.cc 文件**

之后修改 main.cc 文件，让其根据 USE_MYMATH 的预定义值来决定是否调用标准库还是 MathFunctions 库：

```c++
#include 
#include 
#include "config.h"
#ifdef USE_MYMATH
 #include "math/MathFunctions.h"
#else
 #include 
#endif
int main(int argc, char *argv[])
{
  if (argc < 3){
    printf("Usage: %s base exponent \n", argv[0]);
    return 1;
  }
  double base = atof(argv[1]);
  int exponent = atoi(argv[2]);
  
#ifdef USE_MYMATH
  printf("Now we use our own Math library. \n");
  double result = power(base, exponent);
#else
  printf("Now we use the standard library. \n");
  double result = pow(base, exponent);
#endif
  printf("%g ^ %d is %g\n", base, exponent, result);
  return 0;
}
```

**编写 config.h.in 文件**

上面的程序值得注意的是第2行，这里引用了一个 config.h 文件，这个文件预定义了 USE_MYMATH 的值。但我们并不直接编写这个文件，为了方便从 CMakeLists.txt 中导入配置，我们编写一个 config.h.in 文件，内容如下：

> \#cmakedefine USE_MYMATH

这样 CMake 会自动根据 CMakeLists 配置文件中的设置自动生成 config.h 文件。
编译项目
现在编译一下这个项目，为了便于交互式的选择该变量的值，可以使用 ccmake 命令(也可以使用 cmake -i 命令，该命令会提供一个会话式的交互式配置界面。 )

![img](https://img.jbzj.com/file_images/article/202002/2020021418463921.png)

从中可以找到刚刚定义的 USE_MYMATH 选项，按键盘的方向键可以在不同的选项窗口间跳转，按下 enter 键可以修改该选项。修改完成后可以按下 c 选项完成配置，之后再按 g 键确认生成 Makefile 。ccmake 的其他操作可以参考窗口下方给出的指令提示。

我们可以试试分别将 USE_MYMATH 设为 ON 和 OFF 得到的结果：

> USE_MYMATH 为 ON

运行结果：

> [ehome@xman Demo4]$ ./Demo
> Now we use our own MathFunctions library.
> 7 ^ 3 = 343.000000
> 10 ^ 5 = 100000.000000
> 2 ^ 10 = 1024.000000

此时 config.h 的内容为：

\#define USE_MYMATH

> USE_MYMATH 为 OFF

运行结果：

> [ehome@xman Demo4]$ ./Demo
> Now we use the standard library.
> 7 ^ 3 = 343.000000
> 10 ^ 5 = 100000.000000
> 2 ^ 10 = 1024.000000

此时 config.h 的内容为：

> /* #undef USE_MYMATH */

 **下面是其他网友的补充**

使用cmake编译,组织C++项目

前言
这篇博客是我对cmake用法的一些经验总结, 还很浅显, 如果有错误或者更好的方案, 欢迎指正~

使用方法统一为在build目录中执行:

> $: cmake ..
> $: make

我觉得养成外部编译是一个好习惯

**例一**

目录结构为:

> lzj@lzj:~/C-Plus-Plus/makefile_cmake/cmake_1$ tree
> .
> ├── build
> ├── CMakeLists.txt
> └── src
>   ├── hello
>   │  ├── hello.cc
>   │  └── hello.h
>   ├── main.cpp
>   └── world
>     ├── world.cc
>     └── world.h

src 目录中不同属性类维护在不同目录中

**main.cpp中使用hello.h和world.h**

CMakeLists.txt为 :

> cmake_minimum_required (VERSION 3.0)
> project (test_1)
>
> aux_source_directory(${CMAKE_CURRENT_LIST_DIR}/src/hello SOURCE_HELLO)
> aux_source_directory(${CMAKE_CURRENT_LIST_DIR}/src/world SOURCE_WORLD)
>
> add_definitions("-g -Wall -std=c++11")
>
> add_executable(main
>         ${CMAKE_CURRENT_LIST_DIR}/src/main.cpp
>         ${SOURCE_HELLO}
>         ${SOURCE_WORLD})

**例二**

目录结构为:

> lzj@lzj:~/C-Plus-Plus/makefile_cmake/cmake_2$ tree
> .
> ├── build
> ├── CMakeLists.txt
> ├── include
> │  └── person.h
> └── src
>   ├── main.cpp
>   └── person.cc

include目录下统一包含头文件和宏定义之类, 源文件放在 src 目录下维护

person 类是一个简单的空类, 拥有一个私有成员变量val, 一个公有成员函数来打印该变量, 在main.cpp中调用

CMakeLists.txt为 :

> cmake_minimum_required(VERSION 3.0)
> project(test_2)
>
> include_directories(${PROJECT_SOURCE_DIR}/include)
>
> add_definitions("-g -Wall -std=c++11")
>
> add_executable(main
>         ${PROJECT_SOURCE_DIR}/src/main.cpp #这个路径看这个main.cpp位于哪里了       
>         ${PROJECT_SOURCE_DIR}/src/person.cc)

**例三**

目录结构为:

> lzj@lzj:~/C-Plus-Plus/makefile_cmake/cmake_3$ tree
> .
> ├── build
> ├── CMakeLists.txt
> ├── main.cpp
> └── src
>   ├── CMakeLists.txt
>   ├── hello.cc
>   ├── hello.h
>   ├── world.cc
>   └── world.h

将编写的代码编译为库, 在main.cpp中使用, 编译main.cpp时链接该库

顶层目录中CMakeLists.txt为:

> cmake_minimum_required (VERSION 3.0)
> project (test_3)
>
> add_subdirectory(src)
>
> add_definitions("-g -Wall -std=c++11")
>
> add_executable(main main.cpp)
> target_link_libraries(main TEST3) #自己的库名为TEST3

子目录 src 中的CMakeLists.txt为:

> aux_source_directory(. DIR_LIB_SRCS)
>
> add_library (TEST3 ${DIR_LIB_SRCS})

当然如果src目录下为多文件时, 每个目录下都要添加该语句的CMakeLists.txt

[源代码](https://github.com/lzj112/C-Plus-Plus/tree/master/makefile_cmake)

 这篇文章就介绍到这了，希望大家以后多多支持脚本之家。