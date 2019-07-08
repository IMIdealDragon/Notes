# memcheck工具检查内存泄露

C/C++代码很容易出现内存泄露问题，linux下有一个叫valgrind的工具箱，里面有一个叫memcheck的工具，可以用来检查内存泄露

主要可以检查以下几种情况：

```markdown
1. 使用未初始化的内存
2. 使用已经释放了的内存
3. 使用超过malloc分配的内存
4. 对堆栈的非法访问
5. 申请的内存是否有释放
6. malloc/free,new/delete申请和释放内存的匹配
7. 内存拷贝函数中原指针和目标指针重叠
```

## 内存泄露检查释放

`valgrind --tool=memcheck ./executable ` 使用这个命令行就可以检查可执行文件的executable的内存泄露情况，这个时候仅仅能看出是否有内存泄露的情况，要想详细的看到哪一行代码出现了内存泄漏，还要用

`valgrind --tool=memcheck --show-leak-kinds=all --leak-check=full`会打印出具体的泄露的代码。

