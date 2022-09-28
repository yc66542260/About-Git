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