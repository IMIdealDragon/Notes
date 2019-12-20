# const 完全解析

[TOC]

## const修饰常量

const修饰常量表示这个量在后面不会被改变，有效范围默认是本文件内。

### 常量引用

把引用绑定到常量对象上面称为**常量引用**

```cpp
const int ci = 1024;
const int &r1 =ci;
r1 = 42;//错误   
int &r2 = ci;//错误,非常量对象不能绑定到常量对象
```

### const与指针

分为两种：指向常量的指针 ===>  底层const

​                         常量指针       ===>  顶层const

指向常量的指针：

```cpp
const double pi = 3.14;
const double *ptr = &pi;//正确，必须加const
double *ptr = &pi;//错误
*ptr = 3;//错误，不能改变对象指向的值
```

> const修饰了后面所有，所以是修饰了*ptr

常量指针

```cpp
int err = 0;
int *const cur = &err;//cur将一直指向err
int *p = NULL;
cur = p;//错误，不能改变cur
```

> const修饰了后面的所有，所以只是修饰了cur 而不是 *cur

## const修饰函数的三种位置

const可以用来修饰函数，并且可以出现在函数的三个位置，分别代表不同的含义。但是const主要就是帮助我们保证代码的正确性，但用const修饰的函数出现了不应该有的行为时，编译器就会报错，这样可以让程序员及时发现错误

看一下常见的形式：

```cpp
const read(const int size) const { ...}
```

这里const一共有三个位置，从左往右表示的意思依次是：

```markdown
1. 表示read函数返回的值（或者指针）是不可以被改变的
2. 表示传入的参数size在函数体内不可被改变
3. 表示这是一个常量成员函数，调用时不会改变对象
```

### 1. 返回值不可变

返回值分为两类，一类是指针，一类是值。由于值类型的返回，在赋值时会被复制一份，所以const加不加没有什么意义。但是如果是指针类型返回值，那就必须要用const指针来接收这个返回值。

```cpp
const char* getString();
char getstringvalue();
char* s1 = getstring();//错误的，必须用const
const char* s1 = getstring();//正确
char s2 = getstringvalue();//正确
```



### 2. const形参传入

const形参传入的时候不会在函数中被修改

强烈建议的写法是传常值引用

```cpp
char find_char(const string& s1)
```

### 3. 常量成员函数

```cpp
class complex
{
  public:
    	complex (double r = 0, double i = 0)
            : re_(r), im_(i) { }
        complex& operator += (const complex&);
        void  change_real(int real) {re_ = real;}
    	double real () const {return re_;}
    	double imag() const {return im_;}
   private:
    	double re_,im_;
    
} 
```

在一个类中，成员函数包括**改变对象**（如change_real）和**不改变对象**（如real() 和 imag()）的，而如果程序中，对象是被定义为const的，那么非const的成员函数，将不能被调用，此时如果调用，编译器就会报错。

原因：const对象的this指针是const T*，而非const成员对象的this指针是T *，const T *不能赋值给T *。

```cpp
const complex n1(1,2);
n1.real();//正确
n1.change_real(3);//错误，编译器报错
complex n2(3,4);
n1.real();n1.change_real(5);//均正确，不会报错
```

>mutable：修饰变量，表示该变量在const函数中也能被改变

```cpp
mutable int size_;
change(int size) const {size_ = size;}//这是被允许的。
```



## effective建议

```markdown
1. 用const代替define
2. 尽可能多的使用const
3. 用non-const调用const减少代码重复
```



