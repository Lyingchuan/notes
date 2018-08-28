### 1. expires
```
expires: Thu, 16 May 2019 03:05:59 GMT
```
在 http 头中设置一个过期时间，在这个过期时间之前，浏览器的请求都不会发出，而是自动从缓存中读取文件，除非缓存被清空，或者强制刷新。缺陷在于，服务器时间和用户端时间可能存在不一致，所以 HTTP/1.1 加入了 cache-control 头来改进这个问题。

### 2. cache-control
```
cache-control: max-age=31536000
```
设置过期的时间长度（秒），在这个时间范围内，浏览器请求都会直接读缓存。当 expires 和 cache-control 都存在时，cache-control 的优先级更高。