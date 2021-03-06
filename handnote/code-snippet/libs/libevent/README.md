# libevent概述
libevent api提供了一种机制，当文件描述符上发生特殊事件或一个超时事件发生，去执行一个回调函数。
甚至，libevent支持信号或常见超时的回调。
libevent 是为了却带事件驱动的网络服务器中的事件回环。一个应用仅需调用event_dispath()，然后不用改变事件回环即可动态添加或移除事件
目前，libevent支持/dev/poll、kqueue(2)、event ports、POSIX select(2)、Windows select(2)、poll(2)、epoll(4)，内部机制是完全独立的开放的事件api，这样libevent更新带来新功能却不用重新设计应用。结果，libevent方便于应用开发，提供了操作系统上最具扩展性的事件通知机制。libevent可用于多线程应用，可以每个线程一个event_base，也可以线程用锁独占event_base。
libevent可在linux windows mac os x bsd solaris上编译。

libevent额外提供缓冲网络io的专业级架构，支持sockets、filter、rate-limiting、SSL、zero-copy文件传输、和iocp。
libevent还支持DNS、HTTP、小型的rpc框架。

libevent 库实际上没有更换 select()、poll()或其他机制的基础。而是使用对于每个平台最高效的高性能解决方案在实现外加上一个包装器。

为了实际处理每个请求，libevent库提供一种事件机制，它作为底层网络后端的包装器。事件系统让为连接添加处理函数变得非常简便，同时降低了底层I/O 复杂性。这是 libevent 系统的核心。

libevent库的其他组件提供其他功能，包括缓冲的事件系统（用于缓冲发送到客户端/从客户端接收的数据）以及HTTP、DNS 和 RPC 系统的核心实现。

创建 libevent服务器的基本方法是，注册当发生某一操作（比如接受来自客户端的连接）时应该执行的函数，然后调用主事件循环event_dispatch()。执行过程的控制现在由 libevent系统处理。注册事件和将调用的函数之后，事件系统开始自治；在应用程序运行时，可以在事件队列中添加（注册）或删除（取消注册）事件。事件注册非常方便，可以通过它添加新事件以处理新打开的连接，从而构建灵活的网络处理系统。

## 示例
int main(int argc, char **argv)
{
	...
	ev_init();
	
	/* Setup listening socket */
	
	event_set(&ev_accept, listen_fd, EV_READ|EV_PERSIST,
	on_accept, NULL);
	event_add(&ev_accept, NULL);
	
	/* Start the event loop. */
	event_dispatch();
}

event_set() 函数创建新的事件结构，
event_add() 在事件队列机制中添加事件。
event_dispatch() 启动事件队列系统，开始监听（并接受）请求


## 1.timer
使用libevent的timer，每秒输出一段字符串

event_init()=>evtimer_set()=>event_add()=>event_dispatch()

## 2.tcp_service
监听本地8888端口，并输出客户端发送过来的信息

event_base_new()=>event_set()=>event_base_set()=>event_add()=>event_base_dispatch()
