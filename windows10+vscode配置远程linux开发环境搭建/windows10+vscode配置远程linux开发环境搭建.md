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

调试之前必须安装：C/C++ Extension Pack模块。

表示也可以在主机端进行，但是需要重启，同时主机和远程都需要安装很多插件，同时远端需要安装编译器gcc。

![](./%5C2.png)

这些参数都需要进行设置，所以远端需要有cmake和gcc等工具链才能调试。

## 关于ssh一些问题

在windows中，首先需要安装ssh，具体方式可以参考以下博文：

[win安装SSH](https://blog.csdn.net/orDream/article/details/122915148)

在此基础上才可以进一步测试主机和服务器之间的联系

## 关于登陆密码重复输入的问题

可以设置ssh密匙，从而解决相关问题，不必每次登陆都输入密码，具体参考以下博文：

> [[基于VSCode的C++远程开发环境搭建教程](https://www.cnblogs.com/create-serenditipy/p/16191732.html)](https://www.cnblogs.com/create-serenditipy/p/16191732.html)

### 免密登录远程主机

通过上面的步骤，我们就完成了远程开发环境的搭建了，完全可以满足日常使用。但是，有一个基本的问题是，每次登录远程服务器都要输入密码，这也太麻烦了吧，接下来通过`ssh-keygen`和`ssh-copy-id`两条命令实现**免密登录远程服务器主机。**
这儿先简单的介绍下免密登录（不关心原理的忽略下面这段话）

> 免密登录，需要先在本机生成公钥，然后将本机公钥拷贝到远程主机，拷贝的过程，既可以手动（在远程主机主目录下创建.ssh目录，然后将公钥存入该目录下authorized_keys文件中即可），也可以使用命令`scp`或`ssh-copy-id`操作。操作完成后即可免密登录远程主机。

#### 免密登录远程主机的操作步骤

（1）在本机按下快捷键`Win + X`，选择`Windows PoweShell（管理员）`进入终端，再输入`ssh-keygen`命令，接着输入4次回车，就在本机的个人用户目录下生成了公钥。

![](.\3.png)

2）输入`ssh-copy-id -i C:\Users\yimen\.ssh\id_rsa.pub(这里改成你自己的公钥路径) 用户名@远程服务器IP`命令,即可把本地的ssh公钥文件安装到远程主机对应的账户下,并设置了合适的权限。
以我本机为例：

![](.\4.png)

【**注意**】如果没有ssh-copy-id命令的话，win10会提示：

> ssh-copy-id : “ssh-copy-id” 不是内部或外部命令，也不是可运行的程序

解决方案是在`powershell`中，先执行以下内容：

```shekll
function ssh-copy-id([string]$userAtMachine, $args){   
    $publicKey = "$ENV:USERPROFILE" + "/.ssh/id_rsa.pub"
    if (!(Test-Path "$publicKey")){
        Write-Error "ERROR: failed to open ID file '$publicKey': No such file"            
    }
    else {
        & cat "$publicKey" | ssh $args $userAtMachine "umask 077; test -d .ssh || mkdir .ssh ; cat >> .ssh/authorized_keys || exit 1"      
    }
}
```

再次执行`ssh-copy-id -i C:\Users\yimen\.ssh\id_rsa.pub root（用户名）@192.168.10.66(服务器IP）`，根据提示输入密码即可。从此，连接远程服务器再也不用输入密码了。

