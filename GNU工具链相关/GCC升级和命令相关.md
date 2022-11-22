[TOC]

# OpenCV+VSCode+C++环境配置

## 注意点：

1.一定要在windows本地源码编译并安装一遍opencv，这样坑会比较少；

2.这一设置环境变量

3.Mingw必须要用posix版本的

## 1.安装Cmake

> 下载地址：https://cmake.org/

双击安装即可，选择环境变量即可。在windows环境下会自动安装CMAKE-GUI工具。

![](C:%5CUsers%5CRTT%5CDesktop%5Clearning%5CAbout-Tools%5COpenCV+Windows+VSCode+C++%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE%5CFig%5C1.png)

## 2.安装OpenCV

> 下载地址：https://opencv.org/

下载好OpenCV，将其放在D盘下，现在是源码；

1.在源码中建立进文件夹：build

2.使用cmake-gui工具在根目录下运行

3.选择源文件和生成目标的文件夹（build）

4.选择如下选项：

![](C:%5CUsers%5CRTT%5CDesktop%5Clearning%5CAbout-Tools%5COpenCV+Windows+VSCode+C++%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE%5CFig%5C2.png)

5.选择工具链：

![](C:%5CUsers%5CRTT%5CDesktop%5Clearning%5CAbout-Tools%5COpenCV+Windows+VSCode+C++%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE%5CFig%5C3.png)

6.在生成cmake文件后，使用cmd进行编译，这里需要注意：

执行如下命令:

`minGW32-make -j 4`，有可能 `make -j 4` 也可以编译，所以都试一下，第一种应该更标准一些。

最后`minGW32-make install` 或 `make install`，注意安装的位置。

7.完成编译和安装后，我们将一下两个路径加入到环境变量：

![](C:%5CUsers%5CRTT%5CDesktop%5Clearning%5CAbout-Tools%5COpenCV+Windows+VSCode+C++%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE%5CFig%5C4.png)

## 3.安装VSCode

参考另外一篇记录。



## 4.配置MinGW

> 下载地址：https://www.mingw-w64.org/downloads/

注意下载posix版本：

![](C:%5CUsers%5CRTT%5CDesktop%5Clearning%5CAbout-Tools%5COpenCV+Windows+VSCode+C++%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE%5CFig%5C5.png)

将其解压后放在D盘，然后加入到环境变量中。

![](C:%5CUsers%5CRTT%5CDesktop%5Clearning%5CAbout-Tools%5COpenCV+Windows+VSCode+C++%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE%5CFig%5C6.png)

## 5.使用中的一点小贴士，在vscode中需要设置一些编译的选项，比如库的位置、头文件索引的位置等等

launch.json文件

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug",
            "type": "cppdbg",
            "request": "launch",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "miDebuggerPath": "D:\\mingw64\\bin\\gdb.exe",
            "program": "${workspaceFolder}/output/main.exe",
            "preLaunchTask": "build"
        }
    ]
}
```

task.json文件

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "type": "cppbuild",
            "label": "C/C++: g++.exe 生成活动文件",
            "command": "D:\\mingw64\\bin\\g++.exe",
            "args": [
                "-fdiagnostics-color=always",
                "-g",
                "${file}",
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe",
                "-I",
                "D:\\opencv-4.x\\build\\install\\include",
                "-I",
                "D:\\opencv-4.x\\build\\install\\include\\opencv2"
                "-L","D:\\opencv-4.x\\build\\install\\x64\\mingw\\bin",
                "-l", "libopencv_calib3d460",
                "-l", "libopencv_core460",
                "-l", "libopencv_dnn460",
                "-l", "libopencv_features2d460",
                "-l", "libopencv_flann460",
                "-l", "libopencv_gapi460",
                "-l", "libopencv_highgui460",
                "-l", "libopencv_imgcodecs460",
                "-l", "libopencv_imgproc460",
                "-l", "libopencv_ml460",
                "-l", "libopencv_objdetect460",
                "-l", "libopencv_photo460",
                "-l", "libopencv_stitching460",
                "-l", "libopencv_video460",
                "-l", "libopencv_videoio460"
            ],
            "options": {
                "cwd": "D:\\mingw64\\bin"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": "build",
            "detail": "编译器: D:\\mingw64\\bin\\g++.exe"
        }
    ]
}
```

c_cpp_properties.json文件

```json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**",
                "D:\\opencv-4.x\\build\\install\\include",
                "D:\\opencv-4.x\\build\\install\\include\\opencv2"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "windowsSdkVersion": "10.0.19041.0",
            "compilerPath": "D:\\mingw64\\bin\\g++.exe",
            "cStandard": "c17",
            "cppStandard": "c++17",
            "intelliSenseMode": "windows-gcc-x64",
            "configurationProvider": "ms-vscode.cmake-tools"
        }
    ],
    "version": 4
}
```

6.一些重要的参考

> 1.[Vscode配置opencv(简洁)](https://blog.csdn.net/qq_45022687/article/details/120949170)
>
> 2.[VSCode配置 c++ 环境（小白教程）](https://blog.csdn.net/Zhouzi_heng/article/details/115014059)
>
> 3.[win10 系统 VSCODE 配置opencv](https://blog.csdn.net/scott198510/article/details/125843447)
>
> 4.[win10下VSCode配置opencv4.4.0（超详细教程，亲测有效）](http://www.javashuo.com/article/p-rjxqoufk-nx.html)
>
> 5.