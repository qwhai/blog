## C语言中的导出函数

在C或C++中有两种函数导出方式，一种需要添加头文件，一种需求借助于`def`文件。

### 两种导出函数的方式

**方式一：添加头文件导出**

*cppdll.h*

```c
extern "C" _declspec(dllexport) int add(int a, int b);
```

*cppdll.cpp*

```c
#include "cppdll.h"
...
int add(int a, int b)
{
	return a + b;
}
```

这样，我们在`C#`端就可以这样来调用了
```cs
[DllImport("cppdll.dll", CallingConvention = CallingConvention.Cdecl, EntryPoint = "add")]
private extern static int add(int a, int b);

int x = add(1, 2);
```

**方式二：添加`def`文件导出**<br>

*cppdll.cpp*
```c
__declspec(dllexport) int sub(int a, int b)
{
	return a * b;
}
```
*cppdll.def*
```
LIBRARY
EXPORTS
  sub @ 1
```

在`C#`端，我们可以这样调用

```cs
[DllImport("cppdll.dll", CallingConvention = CallingConvention.Cdecl, EntryPoint = "sub")]
private extern static int sub(int a, int b);

int x = sub(3, 2);
```

### 其他数据类型导出细节

#### 1.char*

**1.1 仅参数为`char*`**

`char*`在C语言里表示指向一个字符串的指针。我们可以这样来编写我们的函数

*cppdll.h*

```c
extern "C" _declspec(dllexport) void say(char* label);
```

*cppdll.cpp*

```c
#include "stdio.h"

void say(char* label)
{
	printf("say: %s\n", label);
}
```
在C#端的调用是这样子的
```cs
[DllImport("cppdll.dll", CallingConvention = CallingConvention.Cdecl, EntryPoint = "say")]
private extern static void say(string s);

say("Hello world");
```

**1.2 修改`char*`参数的值，并以其本身返回**

在C代码中修改char*的值其实相对简单，直接赋值就可以解决问题了。但是，如果要通过这个参数把修改过的值回传回去，这个就要稍微难一些了。需要用到内存拷贝相关的知识。

*cppdll.h*

```c
extern "C" _declspec(dllexport) void redirect(char* url);
```

*cppdll.cpp*

```c
void redirect(char* url)
{
	memcpy_s(url, 1024, "http://www.google.com", 1024);
}
```
C#中的调用像这样
```cs
[DllImport("cppdll.dll", CallingConvention = CallingConvention.Cdecl, EntryPoint = "redirect")]
private extern static void redirect(StringBuilder url);

StringBuilder sb = new StringBuilder(1024);
redirect(sb);
Console.WriteLine(sb); // http://www.google.com
```
**Note:** 这里一定要指定StringBuilder的缓冲区大小，不然会报错。

#### 2.bool

使用C中的`bool`进行函数返回值，在C#端同样使用`bool`接收，其结果一直为`true`。

*cppdll.h*

```c
extern "C" _declspec(dllexport) bool flag(int x);
```

*cppdll.cpp*

```c
bool flag(int x)
{
	if (0 == x) return false;
	return true;
}
```

C#调用代码如下：

```cs
[DllImport("cppdll.dll", CallingConvention = CallingConvention.Cdecl, EntryPoint = "flag")]
private extern static bool flag(int x);

bool f1 = flag(1);
Console.WriteLine(f1); // True
bool f2 = flag(0);
Console.WriteLine(f2); // True
```
基返回结果一直为`True`。

**2.1 使用byte接收**

这里不能使用C#端的`bool`来接收C端导出的`bool`，而是要用`byte`来接收，所以C#端代码需要改写成如下这样：
```cs
[DllImport("cppdll.dll", CallingConvention = CallingConvention.Cdecl, EntryPoint = "flag")]
private extern static byte flag(int x);

byte f1 = flag(1);
Console.WriteLine(f1); // 1
byte f2 = flag(100);
Console.WriteLine(f2); // 1
byte f3 = flag(0);
Console.WriteLine(f3); // 0
```
其中`1`表示返回`true`，`0`表示返回`false`。

**2.2 使用BOOL**

除了在C#端使用`byte`接收外，也可以通过修改C代码以达到匹配的目的，那就是将`bool`修改为`BOOL`（对应的返回值：TRUE/FALSE）。

*cppdll.h*

```c
extern "C" _declspec(dllexport) BOOL flagX(int x);
```

*cppdll.cpp*

```c
BOOL flagX(int x)
{
	if (0 == x) return FALSE;
	return TRUE;
}
```

C#调用代码如下：

```cs
[DllImport("cppdll.dll", CallingConvention = CallingConvention.Cdecl, EntryPoint = "flagX")]
private extern static bool flagX(int x);

bool f4 = flagX(1);
Console.WriteLine(f4); // True
bool f5 = flagX(0);
Console.WriteLine(f5); // False
```
