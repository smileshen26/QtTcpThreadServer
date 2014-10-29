QtTcp---Server
==============
这个版本是原来用来测试和自己测试的旧的代码，现重新组织，所以就建个分支单独留着吧。

### QtTcp多线程Server
> 继承自QTcpServer，多线程处理Qtcpsocket,有两种方式分配线程，固定线程和固定线程处理的连接数。
> 
> 代码基于Qt5和C++11，建议win下用mingw版Qt。
> 
> 代码中的信号曹大多用Qt5的新语法，以便使用lambda表达式，
> 
> 所以有很多lambda表达式的应用。
> 
> 
> 
> [我博客关于此实现的简单说明](http://www.dushibaiyu.com/2013/12/qtcpserver多线程实现.html)<br />

    注：此为我学习所写，实际应用很多地方没考虑到、、分配的地方应该还有可以优化地方，如果您发现问题请告诉我，有改进最好也告诉我下、、
    QLibeventTcpServer为用libevent的时间选择器、、速度好像的确有所增加，但是多线程的话就没有优势了、、

    
==============
    Qt的线程里的事件循环和主线程的事件循环所用的事件选择器不是一个时间选择器的，Qt默认的事件循环和I/O复用是seclect，Linux默认用的Glib，是poll，所以做大量长连接不是十分适合，
    但是，Qt把选择器单独可以自定义的。QLibeventTcpServer：是以libevent的事件循环来替代Qt的，libevent在linux下是epoll，所以替换为libevent的会有点优势的，
    只是，需要在每个线程都要替换下。
    这个多线程tcp服务器的实现更多是是对线程的用法，只是提供一个思路，在实际生产环境中，没有任何测试，如使用可以一起优化下，在替换掉默认的事件选择器后，利用每个平台最优的事件循环后，
    理论上做高性能服务器是足够的，其实在一般情况下，因为是事件型的，服务端没有多大必要多线程的、、处理用线程池即可。
    对于资源的占用，在1000个链接下，也就是10M左右内存，现在里面的简单的停止99ms的处理，CPU占用会在80%左右。