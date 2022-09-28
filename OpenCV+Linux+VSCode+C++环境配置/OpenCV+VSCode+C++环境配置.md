[TOC]

# OpenCV++VSCode+C++环境配置

## 参考：

> 参考：[Ubuntu安装OpenCV (C++）](https://blog.csdn.net/Sir666888/article/details/125456214)
>
> ​           [Ubuntu20.04安装opencv-C++接口](https://blog.csdn.net/qq_45488453/article/details/110289423)

## 安装&&编译过程：

**1.安装CMake（3.5.1以上），以实际自己安装的为准，Opencv会自己匹配cmake版本。**

```bash
sudo apt-get update   
sudo apt-get upgrade
sudo apt-get install build-essential cmake 
```

**2.安装依赖项**

```shell
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libxvidcore-dev libx264-dev libatlas-base-dev gfortran libgtk2.0-dev libjpeg-dev libpng-dev
```

**3.下载OpenCV安装包（Sources）**

下载网址：OpenCV

注意将此压缩包解压在一个简单的路径中，要可以记住，安装完成之后不要删除，以后卸载opencv还需要这个文件夹。我这里的解压路径是：`/home/Download/opencv-460`
创建路径（尽量简单），打开解压的OpenCV文件夹

```shell
cd /home/Download/opencv-460 
mkdir build   
cd build
```

**4.编译OpenCV**

```shell
sudo mkdir /usr/local/opencv460    
cmake -D CMAKE_BUILD_TYPE=RELEASE -D OPENCV_GENERATE_PKGCONFIG=YES -D CMAKE_INSTALL_PREFIX=/usr/local/opencv460 ..     
sudo make -j4     
sudo make install
```

**5.配置环境变量**

```shell
sudo gedit /etc/ld.so.conf.d/open.conf     
# 执行这一步会弹出文档，在文档中输入     
/usr/local/opencv4.4.0/lib     # 保存退出

sudo ldconfig     
sudo gedit /etc/bash.bashrc     # 这一步也会弹出文档，输入     
PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/opencv4.4.0/lib/pkgconfig     
export PKG_CONFIG_PATH     # 保存退出
 
source /etc/bash.bashrc     
sudo updatedb     
# 这一步如果报错，执行sudo apt-get install mlocate

```

**6. 测试安装是否成功**

新建工程test_install，CMakeLists内容：

```shell
cmake_minimum_required(VERSION 3.20) 
project(test_install)
find_package(OpenCV REQUIRED) #加这一句 
include_directories(${OpenCV_INCLUDE_DIRS}) #加这一句 
set(CMAKE_CXX_STANDARD 11)
add_executable(test_install test.cpp) 
target_link_libraries(test_install ${OpenCV_LIBS}) #加这一句 ```
```

test.cpp 内容（图片网上下载一张，放到工程目录中，注意路径问题）

```C++
include <iostream>
include "opencv2/core/core.hpp"
include "opencv2/opencv.hpp"
include "opencv2/highgui/highgui.hpp"
using namespace std; 
using namespace cv; 
int main() {     
        Mat img = imread("../test.jpg");     
        //namedWindow("DisplayImage");     
        imshow("test", img);     
        waitKey();     
        return 0; 
}
```

查看OpenCV安装的版本

`pkg-config --modversion opencv4`