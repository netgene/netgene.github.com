---
layout: post
title: traffic server 缓存
date: 2018-02-07 20:20:20
categories:
- opensource
tags:
---

[https://trafficserver.apache.org/](https://trafficserver.apache.org/)  
[https://github.com/apache/trafficserver](https://github.com/apache/trafficserver)  
[https://cwiki.apache.org/confluence/display/TS/Apache+Traffic+Server](https://cwiki.apache.org/confluence/display/TS/Apache+Traffic+Server)  

Traffic Server is a fast, scalable and extensible HTTP/1.1 and HTTP/2 compliant caching proxy server. It was formerly a commercial product created by Inktomi and later aquired by Yahoo! in 2002. Yahoo! maintained the source until its open source release in August 2009.

Today Traffic Server is now a top-level project at the Apache Software Foundation.

Ostatic.com featured an overview of Traffic Server written by Yahoo!'s Cloud Computing team.

futures:  

- Caching Improve your response time, while reducing server load and bandwidth needs by caching and reusing frequently-requested web pages, images, and web service calls.

- Proxying Easily add keep-alive, filter or anonymize content requests, or add load balancing by adding a proxy layer.

- Fast Scales well on modern SMP hardware, handling 10s of thousands of requests per second.

- Extensible APIs to write your own plug-ins to do anything from modifying HTTP headers to handling ESI requests to writing your own cache algorithm.

- Proven Handling over 400TB a day at Yahoo! both as forward and reverse proxies, Apache Traffic Server is battle hardened. Also visit our Customers page for some of our corporate users and supporters.


HTTP Proxy Caching

![HTTP Proxy Caching](https://docs.trafficserver.apache.org/en/latest/_images/cache_miss.jpg)  

object store. The Traffic Server cache consists of a high-speed object database called the object store that indexes cache objects according to URLs and their associated headers.
- indexes urls headers
- remove stale data

futures:  
- Ensuring Cached Object Freshness.  author-specified expiration dates. Expires header or max-age header. freshness_limit = ( date - last_modified ) * 0.10.
- Modifying Aging Factor for Freshness Computations
- Revalidating HTTP Objects
- Pushing Content into the Cache
- treats requests for objects that cannot or should not be cached


### HTTP／1.0／1.1／2.0

[HTTP／1.0／1.1／2.0 https://www.jianshu.com/p/52d86558ca57](https://www.jianshu.com/p/52d86558ca57)  

#### http1.1 header

chrome -> 开发者工具 -> network->选name->Headers

```
GET / HTTP/1.1
Host: trafficserver.apache.org
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.140 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
```

```
HTTP/1.1 200 OK
Date: Wed, 07 Feb 2018 06:38:57 GMT
Server: Apache/2.4.7 (Ubuntu)
Content-Location: index.html
Vary: negotiate,Accept-Encoding
TCN: choice
Accept-Ranges: bytes
Content-Encoding: gzip
Content-Length: 4427
Keep-Alive: timeout=30, max=100
Connection: Keep-Alive
Content-Type: text/html
```
