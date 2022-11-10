# 惊群问题

维基百科，自由的百科全书

[跳到导航](https://zh.wikipedia.org/wiki/%E6%83%8A%E7%BE%A4%E9%97%AE%E9%A2%98#mw-head)

[跳到搜索](https://zh.wikipedia.org/wiki/%E6%83%8A%E7%BE%A4%E9%97%AE%E9%A2%98#searchInput)

**惊群问题**是[计算机科学](https://zh.wikipedia.org/wiki/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)中，当许多进程等待一个事件，事件发生后这些进程被唤醒，但只有一个进程能获得CPU执行权，其他进程又得被阻塞，这造成了严重的系统上下文切换代价。[[1]](https://zh.wikipedia.org/wiki/%E6%83%8A%E7%BE%A4%E9%97%AE%E9%A2%98#cite_note-1)[[2]](https://zh.wikipedia.org/wiki/%E6%83%8A%E7%BE%A4%E9%97%AE%E9%A2%98#cite_note-2)

解决办法可能有：

1. 不希望把所有进程都唤醒，就采用定点唤醒某一个进程的做法。
2. 尽量避免进程上下文切换。

C10K问题（Concurrent 10,000 Connections），指服务器同时支持上万个客户端连接的问题。可用Linux的epoll，FreeBSD的kqueue，Solaris的/dev/poll来解决。

C10M问题，是千万级并发实现。Linux上通常用epoll实现。

# C10k problem

The **C10k problem** is the problem of optimizing [network sockets](https://en.wikipedia.org/wiki/Network_socket) to handle a large number of clients at the same time.[[1]](https://en.wikipedia.org/wiki/C10k_problem#cite_note-C10K-1) The name C10k is a [numeronym](https://en.wikipedia.org/wiki/Numeronym) for [concurrently](https://en.wikipedia.org/wiki/Concurrent_computing) handling ten thousand connections.[[2]](https://en.wikipedia.org/wiki/C10k_problem#cite_note-Liu-Deters-2) Handling many concurrent connections is a different problem to handling many [requests per second](https://en.wikipedia.org/wiki/Requests_per_second):
 the latter requires high throughput (processing them quickly), while 
the former does not have to be fast, but requires efficient scheduling 
of connections.

The problem of socket server optimisation has been studied 
because a number of factors must be considered to allow a web server to 
support many clients. This can involve a combination of operating system
 constraints and web server software limitations. According to the scope
 of services to be made available and the capabilities of the operating 
system as well as hardware considerations such as multi-processing 
capabilities, a multi-threading model or a [single threading](https://en.wikipedia.org/wiki/Single_threading)
 model can be preferred. Concurrently with this aspect, which involves 
considerations regarding memory management (usually operating system 
related), strategies implied relate to the very diverse aspects of the 
I/O management.[[2]](https://en.wikipedia.org/wiki/C10k_problem#cite_note-Liu-Deters-2)

## History

The term *C10k* was coined in 1999 by software engineer Dan Kegel,[[3]](https://en.wikipedia.org/wiki/C10k_problem#cite_note-aosa2:nginx-3)[[4]](https://en.wikipedia.org/wiki/C10k_problem#cite_note-Dan_Kegel,_kegel.com,_1999-4) citing the [Simtel](https://en.wikipedia.org/wiki/Simtel) FTP host, [cdrom.com](https://en.wikipedia.org/wiki/Cdrom.com), serving 10,000 clients at once over 1 [gigabit per second](https://en.wikipedia.org/wiki/Gigabit_per_second) [Ethernet](https://en.wikipedia.org/wiki/Ethernet) in that year.[[1]](https://en.wikipedia.org/wiki/C10k_problem#cite_note-C10K-1)
 The term has since been used for the general issue of large number of 
clients, with similar numeronyms for larger number of connections, most 
recently "C10M" in the 2010s to refer to 10 million concurrent 
connection.[[5]](https://en.wikipedia.org/wiki/C10k_problem#cite_note-C10M-5)

By the early 2010s millions of connections on a single commodity 
1U rackmount server became possible: over 2 million connections ([WhatsApp](https://en.wikipedia.org/wiki/WhatsApp), 24 cores, using [Erlang](https://en.wikipedia.org/wiki/Erlang_(programming_language)) on [FreeBSD](https://en.wikipedia.org/wiki/FreeBSD)),[[6]](https://en.wikipedia.org/wiki/C10k_problem#cite_note-WhatsApp_blog,_2012-6)[[7]](https://en.wikipedia.org/wiki/C10k_problem#cite_note-Reed,_Erlang_Factory,_2012-7) 10–12 million connections (MigratoryData, 12 cores, using [Java](https://en.wikipedia.org/wiki/Java_(Programming_language)) on [Linux](https://en.wikipedia.org/wiki/Linux)).[[5]](https://en.wikipedia.org/wiki/C10k_problem#cite_note-C10M-5)[[8]](https://en.wikipedia.org/wiki/C10k_problem#cite_note-C10M-howto-8)

Common applications of very high number of connections include 
general public servers, or private servers of very big companies whose 
sparse offices are connected to those servers via their [virtual private networks](https://en.wikipedia.org/wiki/Virtual_private_network), that have to serve thousands or even millions of users at a time, such as [file servers](https://en.wikipedia.org/wiki/File_server), [FTP servers](https://en.wikipedia.org/wiki/FTP_server), [proxy servers](https://en.wikipedia.org/wiki/Proxy_servers), web servers, [load balancers](https://en.wikipedia.org/wiki/Load_balancing_(computing)), and so on.[[9]](https://en.wikipedia.org/wiki/C10k_problem#cite_note-conn-very-high-file-9)[[5]](https://en.wikipedia.org/wiki/C10k_problem#cite_note-C10M-5)

## See also

- [Event-driven architecture](https://en.wikipedia.org/wiki/Event-driven_architecture)
- [Event-driven programming](https://en.wikipedia.org/wiki/Event-driven_programming)
- [Reactor pattern](https://en.wikipedia.org/wiki/Reactor_pattern)