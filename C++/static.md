# static 

[TOC]

用static声明外部变量或者函数，可以将该对象的作用域**限定在所属源文件中**，通过static限定外部对象，可以达到隐藏外部对象的目的。而如果在函数内使用static变量，该变量就只能在函数内使用。

具体来说，作用有三条：

## 1. 隐藏

当我们同时编译多个文件时，所有未加static前缀的全局变量和函数都具有全局可见性。举例来说：

在一个a.c的文件中：

```c
char a = 'A';//全局可见，可以在main.c中被访问
static b = 'B';//外部不可见，在main.c中无法访问

void msg()
{
    printf("Hello\n");
}
```

main.c

```c
extern char a;//正确
extern char b;//报错error LNK2001: 无法解析的外部符号 "char d"
```

## 2. 保持内容的持久

存储在静态数据区的变量会在程序刚开始运行时就完成初始化，也是唯一一次初始化，存储在静态存储区的变量类型有两种，一种是全局变量，一种是static变量，都是只初始化一次，并且数据会一直保存，区别在于作用域不同。



## 3. 默认初始化为0

全局变量和静态变量一样，都存储在静态数据区，在静态数据区，内存中所有的字节默认值都是0x00。



## 4. 常见面试题

### **static全局变量与普通的全局变量有什么区别 ?**

全局变量(外部变量)的说明之前再冠以static 就构成了静态的全局变量。

　　全局变量本身就是静态存储方式， 静态全局变量当然也是静态存储方式。 这两者在存储方式上并无不同。

　　这两者的区别在于**非静态全局变量的作用域是整个源程序， 当一个源程序由多个源文件组成时，非静态的全局变量在各个源文件中都是有效的**。 而静态全局变量则限制了其作用域， 即只在定义该变量的源文件内有效， 在同一源程序的其它源文件中不能使用它。由于静态全局变量的作用域局限于一个源文件内，只能为该源文件内的函数公用，因此可以避免在其它源文件中引起错误。 

　　static全局变量只初使化一次，防止在其他文件单元中被引用; 　 

### **static局部变量和普通局部变量有什么区别 ？**

把局部变量改变为静态变量后是改变了它的存储方式即改变了它的**生存期**。把全局变量改变为静态变量后是改变了它的**作用域**，限制了它的使用范围。  

　　static局部变量只被初始化一次，下一次依据上一次结果值； 　 

### **static函数与普通函数有什么区别？**

static函数与普通函数**作用域不同,仅在本文件**。只在当前源文件中使用的函数应该说明为内部函数(static修饰的函数)，内部函数应该在当前源文件中说明和定义。对于可在当前源文件以外使用的函数，应该在一个头文件中说明，要使用这些函数的源文件要包含这个头文件.

　　**static函数在内存中只有一份，普通函数在每个被调用中维持一份拷贝**
