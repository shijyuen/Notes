# Nginx



## 请求处理

### 多进程

服务器每当收到一个客户端时，就有 **服务器主进程**（`master process`）生成一个 **子进程**（`worker process`和客户端建立连接进行交互，直到连接断开，该子进程就结束了。

- 使用 **进程** 的好处是 **各个进程之间相互独立**，**不需要加锁**，减少了使用锁对性能造成影响。

- 独立的进程，可以让 **进程互相之间不会影响**

### 异步非阻塞机制

同时处理 **多个客户端请求**。



## 事件驱动模型

- 事件收集器：负责收集 `worker` 进程的各种 `IO` 请求；
- 事件发送器：负责将 `IO` 事件发送到 **事件处理器**；
- 事件处理器：负责各种事件的 **响应工作**。

**事件发送器** 将每个请求放入一个 **待处理事件列表**，使用非阻塞 `I/O` 方式调用 **事件处理器** 来处理该请求。（epoll）



## 进程处理

`Nginx` 服务器使用 `master/worker` **多进程模式**。

1. **主程序** `Master process` 启动后，通过一个 `for` 循环来 **接收** 和 **处理外部信号**；
2. **主进程** 通过 `fork()` 函数产生 `worker` **子进程**，每个 **子进程** 执行一个 `for` 循环来实现 `Nginx` 服务器 **对事件的接收** 和 **处理**。