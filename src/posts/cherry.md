---
icon: pen-to-square
date: 2022-01-09
category:
  - 樱桃
tag:
  - 红
  - 小
  - 圆
---

# Nginx

​		Nginx (engine x) 是一个高性能的HTTP和反向代理web服务器，同时也提供了IMAP/POP3/SMTP服务。Nginx是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，公开版本1.19.6发布于2020年12月15日。



## 1.正向代理

我们平时访问外网，通常响应比较慢，比如GitHub，我们自己的网络没办法访问，找一个可以访问GitHub的本地服务器，我们通过访问本地服务器去访问GitHub。这就是正向代理。

## 2.反向代理

像抖音，b站这些网站，他们的数据比较多，所有不止一台服务器，但是服务器中间session不共享，如果没有反向代理，我们就需要频繁的去登录，所有他们就专门搞了一台服务器，让其对用户的请求进行转发到他们的后台服务器，这样我们就不需要频繁的去登录了，这种找一个服务器给多台服务器去转发的就是反向代理。

## 3.负载均衡

1.轮询 

根据时间将请求顺序的分配到不同的服务器上
如果某个服务器down掉，nginx可以将其自动踢出集群

```
upstream backserver {
    server 192.168.0.14;
    server 192.168.0.15;
}
```

2.加权weight

指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的 情况。

```
upstream backserver {
    server 192.168.0.14 weight=3;
    server 192.168.0.15 weight=7;
}
```

3.ip_hash

每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决***session的问题\***。

```
upstream backserver {
    ip_hash;
    server 192.168.0.14:88;
    server 192.168.0.15:80;
}
```

4.fair

按后端服务器的响应时间来分配请求，响应时间短的优先分配。

```
upstream backserver {    
	server server1;    server server2;    fair; 
}
```

5.url_hash

按访问url的hash结果来分配请求，使每个url定向到同一个（对应的）后端服务器，后端服务器为缓存时比较有效。

```
upstream backserver {
    server squid1:3128;
    server squid2:3128;
    hash $request_uri;
    hash_method crc32;
}
```


