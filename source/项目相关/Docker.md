# Docker

## 区别

- 虚拟机：依赖硬件设备来提供资源隔离，需要更大的资源开销
- 容器：操作系统级别的进程隔离，Docker 本身只是操作系统的一个进程，进程之间网络、空间是隔离的
- 多个容器之间共享了宿主机的操作系统内核 kernel