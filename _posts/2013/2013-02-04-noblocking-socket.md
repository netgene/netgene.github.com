---
layout: post
title: 非阻塞socket的recv与send
date: 2013-02-04 18:28:58
categories:
- 网络与安全
tags:
- socket
---

socket recv send 与 应用 buffer处理。


```c++
int tcp_recv(int fd, char *buf, int len)
{
    int ret = 0, rlen;
    if (len < 0)
        return 0;

    while (1)
    {
        rlen = recv(fd, buf + ret, len - ret, 0);
        if (rlen == 0)//表示对端已经关闭了这个连接
        {
            LOG_WARN("recv data lenth = 0, connection closed, fd:%d.\n", fd);
            sock_err = SOCK_ERR_BROKEN;
            break;
        }
        else if (rlen < 0)
        {
            int err = errno;
            if (err == EINTR)//表示被中断了，可以继续读，或者等待epoll或select后续的通知。
                continue;
            else if (err == EAGAIN || err == EWOULDBLOCK)//表示暂时无数据可读，可以继续读，或者等待epoll或select的后续通知。
            {
                LOG_DEBUG("errno == EAGAIN\n");
                break;
            }
            else
            {
                LOG_ERROR("recv data error: ret = %d, err = %s, fd:%d\n", rlen,
                          strerror(err), fd);
                sock_err = SOCK_ERR_OTHER;
                break;
            }
        }

        ret += rlen;
        if (ret == len)
            break;
    }

    if(CHECK_IS_DEBUG())
    {
        LOG_DEBUG("recv, fd = %d data datalen = %d\n", fd, ret);
    }

    return ret;
}

int tcp_send(int fd, char *buf, int len)
{
    int ret = 0;
    if (len <= 0)
        return 0;

    while (1)
    {

        int slen = send(fd, buf + ret, len - ret, 0);
        if (slen < 0)
        {
            if (errno == EINTR) //如果errno为EINTR  ，表示被中断了，可以继续写，或者等待epoll或select的后续通知。
                continue;
            else if (errno == EAGAIN || errno == EWOULDBLOCK) //表示当前缓冲区写满，可以继续写，或者等待epoll或select的后续通知，一旦有缓冲区，就会触发写操作。
            {
                LOG_WARN("tcp_send:EAGAIN, fd = %d len = %d\n", fd, len);
                break;
            }
            else 
            {
                int err = errno;
                LOG_ERROR("send data error: ret = %d, err = %s, fd = %d\n",
                          slen, strerror(err), fd);
                sock_err = SOCK_ERR_OTHER;
                break;
            }
        }

        ret += slen;
        if (ret == len) //等于要求发送的长度，发送完毕
            break;
    }

    if(CHECK_IS_DEBUG())
    {
        LOG_DEBUG("send, fd = %d len = %d\n", fd, ret);
    }

    return ret;
}


int set_nonblock(int fd)
{
    int opts;
    opts = fcntl(fd, F_GETFL);
    if (opts < 0)
    {
        LOG_WARN("fcntl F_GETFL failed, fd = %d\n", fd);
        return -1;
    }

    opts = opts | O_NONBLOCK;
    if (fcntl(fd, F_SETFL, opts) < 0)
    {
        LOG_WARN("fcntl F_SETFL failed, fd = %d\n", fd);
        return -1;
    }

    return 0;
}
```