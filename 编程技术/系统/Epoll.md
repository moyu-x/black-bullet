# 用类比的方式描述

ET（边沿触发）：来了信件，指示板上的灯只闪一下。

LT（水平触发）：来了信件，指示板上的灯一直亮着，直到信箱中的信全部被取走。

# 组成

```c
int epoll_create(int size);
int epoll_ctl(int epfd, int op, int fd, struct epoll_event * event);
int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);
```
