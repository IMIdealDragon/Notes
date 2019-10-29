# extern "C"

为了能够正确的在C++代码中调用C语言的代码；

C++因为有函数重载，所以编译器会在函数签名上加一堆东西，但是C风格就不用加这些东西，不支持重载。

### 哪些情况下使用extern "C"：

（1）C++代码中调用C语言代码；

（2）在C++中的头文件中使用；

（3）在多个人协同开发时，可能有人擅长C语言，而有人擅长C++；

###  C++语言允许函数重载；但C语言是一门单一名字空间的语言，不允许函数重载；

为了能在C++程序里调用C语言程序，C++引入了链接规范，格式：extern "language string";

如下，在C++程序中调用C程序

![img](D:\A_目标！！！\笔记\C++\pic\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Nzc18zNjk=,size_16,color_FFFFFF,t_70)