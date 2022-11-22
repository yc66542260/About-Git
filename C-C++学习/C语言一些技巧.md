# 宏定义的常用方法

写好C语言，漂亮的宏定义很重要，使用宏定义可以防止出错，提高可移植性，可读性，方便性等等。下面列举一些成熟软件中常用的宏定义。

1. 防止一个头文件被重复包含

```text
1#ifndef COMDEF_H
2#define COMDEF_H
3//头文件内容
4#endif
```

2. 重新定义一些类型，防止由于各种平台和编译器的不同，而产生的类型字节数差异，方便移植。

```text
1typedef unsigned char boolean; /* Boolean value type. */
2typedef unsigned long int uint32; /* Unsigned 32 bit value */
3typedef unsigned short uint16; /* Unsigned 16 bit value */
4typedef unsigned char uint8; /* Unsigned 8 bit value */
5typedef signed long int int32; /* Signed 32 bit value */
6typedef signed short int16; /* Signed 16 bit value */
7typedef signed char int8; /* Signed 8 bit value */

下面的不建议使用
 1typedef unsigned char byte; /* Unsigned 8 bit value type. */
 2typedef unsigned short word; /* Unsinged 16 bit value type. */
 3typedef unsigned long dword; /* Unsigned 32 bit value type. */
 4typedef unsigned char uint1; /* Unsigned 8 bit value type. */
 5typedef unsigned short uint2; /* Unsigned 16 bit value type. */
 6typedef unsigned long uint4; /* Unsigned 32 bit value type. */
 7typedef signed char int1; /* Signed 8 bit value type. */
 8typedef signed short int2; /* Signed 16 bit value type. */
 9typedef long int int4; /* Signed 32 bit value type. */
10typedef signed long sint31; /* Signed 32 bit value */
11typedef signed short sint15; /* Signed 16 bit value */
12typedef signed char sint7; /* Signed 8 bit value */
```

3. 得到指定地址上的一个字节或字

```text
1#define MEM_B( x ) ( *( (byte *) (x) ) )
2#define MEM_W( x ) ( *( (word *) (x) ) )
```

4. 求最大值和最小值

```text
1#define MAX( x, y ) ( ((x) > (y)) ? (x) : (y) )
2#define MIN( x, y ) ( ((x) < (y)) ? (x) : (y) )
```

5. 得到一个field在结构体(struct)中的偏移量

```text
1#define FPOS( type, field ) \
2/*lint -e545 */ ( (dword) &(( type *) 0)-> field ) /*lint +e545 */
```

6. 得到一个结构体中field所占用的字节数

```text
1#define FSIZ( type, field ) sizeof( ((type *) 0)->field )
```

7. 按照LSB格式把两个字节转化为一个Word

```text
1#define FLIPW( ray ) ( (((word) (ray)[0]) * 256) + (ray)[1] )
```

8. 按照LSB格式把一个Word转化为两个字节

```text
1#define FLOPW( ray, val ) \
2(ray)[0] = ((val) / 256); \
3(ray)[1] = ((val) & 0xFF)
```

9. 得到一个变量的地址(word宽度)

```text
1#define B_PTR( var ) ( (byte *) (void *) &(var) )
2#define W_PTR( var ) ( (word *) (void *) &(var) )
```

10. 得到一个字的高位和低位字节

```text
1#define WORD_LO(xxx) ((byte) ((word)(xxx) & 255))
2#define WORD_HI(xxx) ((byte) ((word)(xxx) >> 8))
```

11. 返回一个比X大的最接近的8的倍数

```text
1#define RND8( x ) ((((x) + 7) / 8 ) * 8 )
```

12. 将一个字母转换为大写

```text
1#define UPCASE( c ) ( ((c) >= 'a' && (c) <= 'z') ? ((c) - 0x20) : (c) )
```

13. 判断字符是不是10进制的数字

```text
1#define DECCHK( c ) ((c) >= '0' && (c) <= '9')
```

14. 判断字符是不是16进制的数字

```text
1#define HEXCHK( c ) ( ((c) >= '0' && (c) <= '9') ||\
2((c) >= 'A' && (c) <= 'F') ||\
3((c) >= 'a' && (c) <= 'f') )
```

15. 防止溢出的一个方法

```text
1#define INC_SAT( val ) (val = ((val)+1 > (val)) ? (val)+1 : (val))
```

16. 返回数组元素的个数

```text
1#define ARR_SIZE( a ) ( sizeof( (a) ) / sizeof( (a[0]) ) )
```

17. 返回一个无符号数n尾的值MOD_BY_POWER_OF_TWO(X,n)=X%(2^n)

```text
1#define MOD_BY_POWER_OF_TWO( val, mod_by ) \
2( (dword)(val) & (dword)((mod_by)-1) )
```

18. 对于IO空间映射在存储空间的结构，输入输出处理

```text
1#define inp(port) (*((volatile byte *) (port)))
2#define inpw(port) (*((volatile word *) (port)))
3#define inpdw(port) (*((volatile dword *)(port)))
4#define outp(port, val) (*((volatile byte *) (port)) = ((byte) (val)))
5#define outpw(port, val) (*((volatile word *) (port)) = ((word) (val)))
6#define outpdw(port, val) (*((volatile dword *) (port)) = ((dword) (val)))
```

19. 使用一些宏跟踪调试

```text
A N S I标准说明了五个预定义的宏名。它们是：
1_ L I N E _
2_ F I L E _
3_ D A T E _
4_ T I M E _
5_ S T D C _
```

如果编译不是标准的，则可能仅支持以上宏名中的几个，或根本不支持。记住编译程序也许还提供其它预定义的宏名。_ L I N E _及_ F I L E _宏指令在有关# l i n e的部分中已讨论，这里讨论其余的宏名。_ D AT E _宏指令含有形式为月/日/年的串，表示源文件被翻译到代码时的日期。源代码翻译到目标代码的时间作为串包含在_ T I M E _中。串形式为时：分：秒。如果实现是标准的，则宏_ S T D C _含有十进制常量1。如果它含有任何其它数，则实现是非标准的。可以定义宏，例如: 当定义了_DEBUG，输出数据信息和所在文件所在行1#ifdef

```text
_DEBUG
2#define DEBUGMSG(msg,date) printf(msg);printf(“%d%d%d”,date,_LINE_,_FILE_)
3#else
4#define DEBUGMSG(msg,date)
5#endif
```

20. 宏定义防止使用时错误用小括号包含。

例如：

```text
1#define ADD(a,b) (a+b)
```

用do{}while(0)语句包含多语句防止错误
例如：

```text
1#difne DO(a,b) a+b;\
2a++;
```

应用时：

```text
1if(….)
2DO(a,b); //产生错误
3else
```

解决方法:

```text
1#define DO(a,b) do{a+b;\
2a++;}while(0)
```



# 简单[宏定义](https://so.csdn.net/so/search?q=宏定义&spm=1001.2101.3001.7020)实现

## 简单宏定义 - 方式一

这种方式将主要实现部分放在一个宏定义中，利用字符替换的方式实现不同 type 的运算，详细思路见代码：

```c
#include <stdint.h>
 
#define INT8 8
#define INT16 16
#define INT32 32
 
#define DO_MAIN(type) do {                      \
        int i;                                  \
        type *p = buf;                          \
                                                \
        for (i = 0; i < len; i++) {             \
                p[i] *= k;                      \
        }                                       \
} while(0)
 
void func(void *buf, int len, float k, int request)
{
        if (request == INT8) {
                DO_MAIN(int8_t);
        } else if (request == INT16) {
                DO_MAIN(int16_t);
        } else if (request == INT32) {
                DO_MAIN(int32_t);
        }
}
```

## 简单宏定义 - 方式二

这种方式直接利用宏定义实现几个同类函数的定义，详见代码：

```c
#define DECLARE_FUNC(n)                                 \
static void func_##n(int##n##_t *p, int len, float k)   \
{                                                       \
    int i;                                              \
                                                        \
    for (i = 0; i < len; i++)                           \
        p[i] *= k;                                      \
}
 
DECLARE_FUNC(8)
DECLARE_FUNC(16)
DECLARE_FUNC(32)
```

接下来就可以使用 `func_8()`、`func_16()` 和 `func_32()` 三个函数了。

## 包装函数实现

这种方式理解起来非常简单，就是简单地利用一个函数将多个同类函数进行统一整合，详见代码：

```c
/*************************************
* foo(), bar(), baz() 三个函数为已有函数
**************************************/
 
static inline int process_image(void *img, int width, int height, const int n)
{
    int x, y;
 
    for (y = 0; y < height; y++) {
        for (x = 0; x < width; x++) {
            if      (n == 0) foo(img, x, y);
            else if (n == 1) bar(img, x, y);
            else             baz(img, x, y);
        }
    }
}
 
int process_image_foo(void *img, int width, int height)
{
    return process_image(img, width, height, 0);
}
 
int process_image_bar(void *img, int width, int height)
{
    return process_image(img, width, height, 1);
}
 
int process_image_baz(void *img, int width, int height)
{
    return process_image(img, width, height, 2);
}
```

## 宏定义和包装函数混合使用实现

针对上述方式，`process_image_foo`、`process_image_bar` 和 `process_image_baz` 三个函数可以利用宏定义简化书写，如下：

```c
static inline int process_image(void *img, int width, int height, const int n)
{
    int x, y;
 
    for (y = 0; y < height; y++) {
        for (x = 0; x < width; x++) {
            if      (n == 0) foo(img, x, y);
            else if (n == 1) bar(img, x, y);
            else             baz(img, x, y);
        }
    }
}
 
#define DECLARE_PROCESS_IMAGE_FUNC(name, n)                 \
int process_image_##name(void *img, int width, int height)  \
{                                                           \
    return process_image(img, width, height, n);            \
}
 
DECLARE_PROCESS_IMAGE_FUNC(foo, 0)
DECLARE_PROCESS_IMAGE_FUNC(bar, 1)
DECLARE_PROCESS_IMAGE_FUNC(baz, 2)
```

这样就可以实现上一种方式同样的效果

## 外部文件实现

我们还可以用单独的源文件和头文件来实现模板函数，像这样：

```c
#if defined(TEMPLATE_U16)
 
#    define RENAME(N)   N ## _u16
#    define TYPE        uint16_t
#    define SUM_TYPE    uint32_t
 
#elif defined(TEMPLATE_U32)
 
#    define RENAME(N)   N ## _u32
#    define TYPE        uint32_t
#    define SUM_TYPE    uint64_t
 
#elif defined(TEMPLATE_FLT)
 
#    define RENAME(N)   N ## _flt
#    define TYPE        float
#    define SUM_TYPE    float
 
#elif defined(TEMPLATE_DBL)
 
#    define RENAME(N)   N ## _dbl
#    define TYPE        double
#    define SUM_TYPE    double
 
#endif
 
TYPE RENAME(func)(const TYPE *p, int n)
{
    int i;
    SUM_TYPE sum = 0;
 
    for (i = 0; i < 1<<n; i++)
        sum += p[i];
    return sum;
}
 
#undef RENAME
#undef TYPE
#undef SUM_TYPE
```

可以像下面这样使用模板函数：main.c

```c
#include <stdint.h>
 
#define TEMPLATE_U16
#include "evil_template.c"
#undef TEMPLATE_U16
 
#define TEMPLATE_U32
#include "evil_template.c"
#undef TEMPLATE_U32
 
#define TEMPLATE_FLT
#include "evil_template.c"
#undef TEMPLATE_FLT
 
#define TEMPLATE_DBL
#include "evil_template.c"
#undef TEMPLATE_DBL
```

只需要在使用前对相应类型进行宏定义即可，对应的函数分别是 `func_u16()`、`func_u32()`、`func_flt()` 和 `func_dbl()`。



# [Templating in C](http://blog.pkh.me/p/20-templating-in-c.html#content)

[prog](http://blog.pkh.me/x/index-prog.html)

The question of how to do templating in C is often raised, and I couldn't find a good overview of the different approaches so I'll try to make a relatively exhaustive list here.

Note: the methods presented here rely on the C preprocessor, which you can use directly yourself by calling the `cpp` command, paste your code and see the output by sending an `EOS` (pressing `control + d` typically).

## Simple C macro

So the most common method is to dump some code enclosed (or not) into the [`do { ... } while (0)`](https://duckduckgo.com/?q=do+while+0) form:

```c
#define DO_RANDOM_STUFF(type) do {      \
    int i;                              \
    type *p = buf;                      \
                                        \
    for (i = 0; i < len; i++)           \
        p[i] = p[i] * k;                \
} while (0)
```

... and using it directly:

```c
int func(void *buf, int len, float k, int request)
{
    if      (request == INT8)   DO_RANDOM_STUFF(int8_t);
    else if (request == INT16)  DO_RANDOM_STUFF(int16_t);
    else if (request == INT32)  DO_RANDOM_STUFF(int32_t);
}
```

## Simple C macro, full function

It is also common to create the whole function that way:

```c
#define DECLARE_FUNC(n)                                 \
static void func_##n(int##n##_t *p, int len, float k)   \
{                                                       \
    int i;                                              \
                                                        \
    for (i = 0; i < len; i++)                           \
        p[i] = p[i] * k;                                \
}

DECLARE_FUNC(8)
DECLARE_FUNC(16)
DECLARE_FUNC(32)
```

... which you will then use by simply calling `func_8()`, `func_16()` and `func_32()`.

## Alternative function creator

So far we used the templating just to avoid typing redundancy, but it is sometimes motivated by performance. Let's observe the following pattern:

```c
int process_image(void *img, int width, int height, const int n)
{
    int x, y;

    for (y = 0; y < height; y++) {
        for (x = 0; x < width; x++) {
            if      (n == 0) foo(img, x, y);
            else if (n == 1) bar(img, x, y);
            else             baz(img, x, y);
        }
    }
}
```

We will assume here that `foo()`, `bar()` and `baz()` functions are meant to be inlined for performance reasons (be it justified or not, we assume you do not want a per-pixel function call), so you do not want to use an array of function pointers and do `func_lut[n](img, x, y)` in the inner loop. We could also consider the scenario where the functions take a completely different set of parameters.

Your compiler will sometimes be smart enough to see that the `n` check in the inner loop can instead enclose the whole logic just as if you had written this:

```c
int process_image(void *img, int width, int height, const int n)
{
    int x, y;

    if (n == 0)
        for (y = 0; y < height; y++)
            for (x = 0; x < width; x++)
                foo(img, x, y);
    else if (n == 1)
        for (y = 0; y < height; y++)
            for (x = 0; x < width; x++)
                bar(img, x, y);
    else
        for (y = 0; y < height; y++)
            for (x = 0; x < width; x++)
                baz(img, x, y);
}
```

Unfortunately, it is very likely that your code is much more complex (otherwise you could even just put the 2 loops into your inner functions) which will cause your compiler not to do it.

One solution for this is to create wrapper for each scenario. It would look like this:

```c
static inline int process_image(void *img, int width, int height, const int n)
{
    int x, y;

    for (y = 0; y < height; y++) {
        for (x = 0; x < width; x++) {
            if      (n == 0) foo(img, x, y);
            else if (n == 1) bar(img, x, y);
            else             baz(img, x, y);
        }
    }
}

int process_image_foo(void *img, int width, int height)
{
    return process_image(img, width, height, 0);
}

int process_image_bar(void *img, int width, int height)
{
    return process_image(img, width, height, 1);
}

int process_image_baz(void *img, int width, int height)
{
    return process_image(img, width, height, 2);
}
```

Notice how the `process_image()` function has been marked as `static inline` in order to make sure it is fully inlined in each wrapper.

By doing so, the compiler can very easily see the dead paths (because `n` is now a constant in the wrappers) and generate 3 standalone functions with zero call nor inner condition. These functions can now be put into a function lookup table mapped to `n` (`process_image_lut[n](img, width, height)`).

One great thing about this method is that you can also do type-specific code in each of the function (by interpreting `img` as `int16_t`, `float`, `uint64_t`, etc for example).

As a side effect, you will notice that the main logic is not under a huge hardly maintainable macro, and the redundancy overhead is also very small.

Note: you might want to rely on `__attribute__((always_inline))` if your compiler supports it and you want the inline to be effective at every optimization level.

## Mixing full functions mechanism and macros

If the redundancy overhead in the previous example is still too much for you, you can mix it with a small macro mechanism:

```c
static inline int process_image(void *img, int width, int height, const int n)
{
    int x, y;

    for (y = 0; y < height; y++) {
        for (x = 0; x < width; x++) {
            if      (n == 0) foo(img, x, y);
            else if (n == 1) bar(img, x, y);
            else             baz(img, x, y);
        }
    }
}

#define DECLARE_PROCESS_IMAGE_FUNC(name, n)                 \
int process_image_##name(void *img, int width, int height)  \
{                                                           \
    return process_image(img, width, height, n);            \
}

DECLARE_PROCESS_IMAGE_FUNC(foo, 0)
DECLARE_PROCESS_IMAGE_FUNC(bar, 1)
DECLARE_PROCESS_IMAGE_FUNC(baz, 2)
```

This avoids the painful redundancy and still benefits from the same advantages as previously.

FFmpeg's [XBR filter](http://git.videolan.org/?p=ffmpeg.git;a=blob;f=libavfilter/vf_xbr.c;h=5c14565b3a03f66f1e0296623dc91373aeac1ed0;hb=HEAD#l316) and [paletteuse filter](http://git.videolan.org/?p=ffmpeg.git;a=blob;f=libavfilter/vf_paletteuse.c;h=cba0eb34c23a7c7c5238b4f7c4cf8cd483615fa6;hb=HEAD#l957) are a few real cases examples.

## External file

One alternative to all of this is to use external files.

The logic is pretty simple; let's start with the content of `caller.c`:

```c
#include <stdint.h>

#define TEMPLATE_U16
#include "evil_template.c"
#undef TEMPLATE_U16

#define TEMPLATE_U32
#include "evil_template.c"
#undef TEMPLATE_U32

#define TEMPLATE_FLT
#include "evil_template.c"
#undef TEMPLATE_FLT

#define TEMPLATE_DBL
#include "evil_template.c"
#undef TEMPLATE_DBL
```

The content of `evil_template.c` could look like something like this:

```c
#if defined(TEMPLATE_U16)

#    define RENAME(N)   N ## _u16
#    define TYPE        uint16_t
#    define SUM_TYPE    uint32_t
#    define XDIV(x, n)  (((x) + ((1<<(n))-1)) >> (n))

#elif defined(TEMPLATE_U32)

#    define RENAME(N)   N ## _u32
#    define TYPE        uint32_t
#    define SUM_TYPE    uint64_t
#    define XDIV(x, n)  (((x) + ((1<<(n))-1)) >> (n))

#elif defined(TEMPLATE_FLT)

#    define RENAME(N)   N ## _flt
#    define TYPE        float
#    define SUM_TYPE    float
#    define XDIV(x, n)  ((x) / (float)(1<<(n)))

#elif defined(TEMPLATE_DBL)

#    define RENAME(N)   N ## _dbl
#    define TYPE        double
#    define SUM_TYPE    double
#    define XDIV(x, n)  ((x) / (double)(1<<(n)))

#endif

TYPE RENAME(func)(const TYPE *p, int n)
{
    int i;
    SUM_TYPE sum = 0;

    for (i = 0; i < 1<<n; i++)
        sum += p[i];
    return XDIV(sum, n);
}

#undef RENAME
#undef TYPE
#undef SUM_TYPE
#undef XDIV
```

This will produce the following functions:

```shell
% gcc -Wall -c caller.c -o caller.o && readelf -s caller.o | grep func
 9: 0000000000000000   110 FUNC    GLOBAL DEFAULT    1 func_u16
10: 000000000000006e   119 FUNC    GLOBAL DEFAULT    1 func_u32
11: 00000000000000e5   128 FUNC    GLOBAL DEFAULT    1 func_flt
12: 0000000000000165   131 FUNC    GLOBAL DEFAULT    1 func_dbl
```

This way of templating could be relatively handy if you have a set of functions in the template but can be considered by many as the root of all evils. Nevertheless, this is still a few order of magnitude better than C++ templating.

Note: make sure your build system properly handles the dependency of `evil_template.c` from `caller.c`.

[Resampling code in libswresample](http://git.videolan.org/?p=ffmpeg.git;a=blob;f=libswresample/resample_dsp.c;h=a811b8b6fa8eab8c1568a29f633136aed3ece2bb;hb=HEAD) (which inspired this example) is one of the many real case example.

## Conclusion

Mixing the full functions mechanism and macros covers most of the cases and can lead to very decent code with high level of readability and maintenance.

For more complex cases, you also can simply rely on another language, generally a scripting one (but you can use C to generate C as well of course). This can often be the most appropriate approach if the complexity is increasing and your project already has a dependency on such a language.

Edit: As [pointed by someone on HN](https://news.ycombinator.com/item?id=9124200), it is also worth mentioning `__Generic` in C11 that can be used for similar purpose if your project can afford the dependency on C11.

------

For updates and more frequent content you can follow me on [Twitter](https://twitter.com/insouris) or [Mastodon](https://fosstodon.org/@bug). Feel also free to subscribe to the [RSS](http://blog.pkh.me/rss.xml) in order to be notified of new write-ups. It is also usually possible to reach me through other means (check the footer below). Finally, discussions on some of the articles can sometimes be found on HackerNews, Lobste.rs and Reddit.

# C语言结构体中如何包含函数

C语言结构体里面也可以包含函数，如同类中有方法一样，但是不能通过直接放过一个函数进去，需要通过函数指针的方式，同时，关于类的[构造函数](https://so.csdn.net/so/search?q=构造函数&spm=1001.2101.3001.7020)与析构函数C语言表示是没有的，需要你自己手动解决这些问题。
下面讲讲如何在C语言中的结构体包含函数。如下的一段代码：

```c++
#include<iostream>
#include<string>
using namespace std;
class Hello{
public:
	void sayHello(string name){
		cout<<"你好，"<<name<<endl;
	}
};
int main(){
	Hello* hello=new Hello();
	hello->sayHello("a");
	return 0;
}
```

如果需要用纯C实现上面的东西，也就是在一个Hello的结构体里面包含一个函数，你可以写成这样：

```c++
#include<stdio.h>
#include<malloc.h>
struct Hello{
	void (*sayHello)(char* name); 
};
void sayHello(char* name){
	printf("你好，%s\n",name);
}
int main(){
	struct Hello* hello=(struct Hello *)malloc(sizeof(struct Hello));
	hello->sayHello=sayHello;//这个结构体有多少个函数，就要在这个有多少个结构体内，函数指针指向函数的声明。
	hello->sayHello("a");
	return 0;
}
```

运行结果和上面一模一样的，但是你要注意，本来已经比较坑爹的C++变成C语言来表达，就变得更加坑，每用这个包含函数指针的结构体，一定要声明这结构体内的指针一次。