[TOC]

# windows10+vscode配置远程linux开发环境搭建环境

重要参考：

1.[windows10+vscode配置远程linux开发环境](https://blog.csdn.net/wm20200220/article/details/121229472)

2.[Windows下如何使用VScode连接远程linux服务器进行远程开发](https://blog.csdn.net/weixin_42072280/article/details/124590338)

3.[ssh连接远程服务器](https://blog.csdn.net/jinxianwei1999/article/details/123244624)

4.[vscode连接远程Linux服务器及免密登陆](https://blog.csdn.net/qq_16763983/article/details/126254636)

5.[rsa在windows和linux,在winsshd 中添加id_rsa.pub 实现Windows 服务器主机自动信任Linux 客户端](https://blog.csdn.net/weixin_32233571/article/details/116800547)

注意：我们需要两个ssh连接：

1.vscode的ssh server

2.vscode终端的ssh连接

这样我们就可以开着ubuntu后台，然后在windows环境下进行编译开发了。

![](./%5C1.png)



## 关于调试重要参考：

> 1.[**使用VSCode进行远程C++开发**](https://blog.csdn.net/qq_42688495/article/details/118411274)
>
> 2.[基于VSCode的C++远程开发环境搭建教程](https://www.cnblogs.com/create-serenditipy/p/16191732.html)
>
> 3.[Using C++ and WSL in VS Code](https://code.visualstudio.com/docs/cpp/config-wsl)
>
> 4.[windows平台中使用vscode远程连接linux进行c++开发配置](**https://cloud.tencent.com/developer/article/1945311**)

## 注意：

需要安装的插件包库扩：

- [C/C++](https://link.zhihu.com/?target=https%3A//marketplace.visualstudio.com/items%3FitemName%3Dms-vscode.cpptools)
- [C/C++ Themes](https://link.zhihu.com/?target=https%3A//marketplace.visualstudio.com/items%3FitemName%3Dms-vscode.cpptools-themes)
- [CMake](https://link.zhihu.com/?target=https%3A//marketplace.visualstudio.com/items%3FitemName%3Dtwxs.cmake)
- [CMake Tools](https://link.zhihu.com/?target=https%3A//marketplace.visualstudio.com/items%3FitemName%3Dms-vscode.cmake-tools)
- [Doxygen Documentation Generator](https://link.zhihu.com/?target=https%3A//marketplace.visualstudio.com/items%3FitemName%3Dcschlosser.doxdocgen)
- [Better C++ Syntax](https://link.zhihu.com/?target=https%3A//marketplace.visualstudio.com/items%3FitemName%3Djeff-hykin.better-cpp-syntax)
- [Remote Development Extension Pack](https://link.zhihu.com/?target=https%3A//marketplace.visualstudio.com/items%3FitemName%3Dms-vscode-remote.vscode-remote-extensionpack)

## 关于调试：

表示也可以在主机端进行，但是需要重启，同时主机和远程都需要安装很多插件，同时远端需要安装编译器gcc。

![](./%5C2.png)

这些参数都需要进行设置，所以远端需要有cmake和gcc等工具链才能调试。