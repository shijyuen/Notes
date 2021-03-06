# epoll

相对于select和poll来说，epoll更加灵活，没有描述符限制。epoll使用一个文件描述符管理多个描述符，将用户关系的文件描述符的事件存放到内核的一个事件表中，这样在用户空间和内核空间的copy只需一次。

在 select/poll中，进程只有在调用一定的方法后，内核才对所有监视的文件描述符进行扫描，而**epoll事先通过epoll_ctl()来注册一 个文件描述符，为每个fd指定一个回调函数，一旦基于某个文件描述符就绪时，回调函数会把就绪的fd加入一个就绪链表。epoll_wait( ) 就是查看链表有没有就绪的fd**。(**不是遍历文件描述符，而是通过监听回调的的机制**)





## 工作模式

**LT水平模式**：当epoll_wait检测到描述符事件发生并将此事件通知应用程序，**应用程序可以不立即处理该事件**。下次调用epoll_wait时，会**再次响应应用程序并通知此事件**。

**ET边缘模式**：当epoll_wait检测到描述符事件发生并将此事件通知应用程序，**应用程序必须立即处理该事件**。如果不处理，下次调用epoll_wait时，**不会再次响应应用程序并通知此事件**。

- ET模式下只有socket的状态发生变化时才会通知，也就是读取缓冲区由无数据到有数据时通知read事件，发送缓冲区由满变成未满通知write事件。

-  ET模式在很大程度上减少了epoll事件被重复触发的次数，因此效率要比LT模式高。epoll工作在ET模式的时候，必须使用非阻塞套接口



## 优点

1. 监视的描述符数量不受限制，它所支持的FD上限是最大可以打开文件的数目，这个数字一般远大于2048。一般来说这个数目和系统内存关系很大。select的最大缺点就是进程打开的fd是有数量限制的。这对于连接数量比较大的服务器来说根本不能满足。
2. 采用事件驱动，避免轮询查看可读写事件，**索引就绪文件描述符的时间复杂度：O(1)**
3. 适用于连接数量多，但活动连接少的情况



## select

参数fd_set，没有将文件描述符和事件绑定；其仅仅是一个文件描述符集合

1. **每次调用select、poll，都需要把fd集合从用户态拷贝到内核态**，不管文件描述符是否就绪
2. **需要在内核遍历所有fd**，索引就绪文件描述符的时间复杂度：O(n)
3. **select支持的文件描述符数量太小**

## poll

pollfd结构：将文件描述符和事件都定义其中，任何事件都被统一处理，使得编程接口简洁



## 区别

1. select和poll的动作基本一致，只是poll采用链表来进行文件描述符的存储，而select采用fd标注位来存放，所以select会受到最大连接数的限制，而poll不会。
2. select、poll、epoll虽然都会返回就绪的文件描述符数量。但是select和poll并不会明确指出是哪些文件描述符就绪，而epoll会。造成的区别就是，系统调用返回后，调用select和poll的程序需要遍历监听的整个文件描述符找到是谁处于就绪，而epoll则直接处理即可。
3. select、poll都需要将有关文件描述符的数据结构拷贝进内核，最后再拷贝出来。而epoll创建的有关文件描述符的数据结构本身就存于内核态中，系统调用返回时利用mmap()文件映射内存加速与内核空间的消息传递：即epoll使用mmap减少复制开销。
4. select、poll采用轮询的方式来检查文件描述符是否处于就绪态，而epoll采用回调机制。造成的结果就是，随着fd的增加，select和poll的效率会线性降低，而epoll不会受到太大影响，除非活跃的socket很多。
5. epoll的边缘触发模式效率高，系统不会充斥大量不关心的就绪文件描述符