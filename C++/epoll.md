epoll是个非常重要的主题，现在暂时先mark一下

要从原理，源码实现，应用使用注意事项三个方面去深究。

记录几个小问题：

1.ET模式下，如果原来有数据没读完，从无数据变为有数据的状态变化会不会引发epoll，答案是会的。具体为什么？需要探究源码吧。