# 用 OpenResty 快乐的搭建高性能服务端

### 1.1 OpenResty简介 
```

高性能服务端（重点关注）： 异步非阻塞 + 缓存
OpenResty = Nginx + LuaJIT

缓存：多层，命中率越高越好
异步非阻塞: 不必等待IO,直接返回，有数据准备好，由操作系统通知用户进程

内存 > SSD > 机械磁盘
本机 > 网络
进程内 > 进程间

node.js 异步非阻塞，使用回调实现异步非阻塞，回调次数太多。不符合开发习惯
python 3.4加入异步IO，aiohttp，3.5引入协程
golang go关键字


OpenResty 使用同步方式编写代码，OpenResty帮你实现异步
出问题了，不重启服务，看到程序内部情况
SystemTap
BaaS（后端即服务）


LuaJIT just in time

原理： LuaJIT虚拟机嵌入nginx的worker中
高性能，开发效率高。

nginx --> redis
nginx --> postgresql
nginx --> mysql


```



### 1.2 hello world 
```
lua 代码可以内嵌在nginx.conf中
也可以放在外部

不用重启nginx而让lua代码修改后生效，lua_code_cache off;

```


### 1.3 OpenResty入门 
```
开源书： openresty最佳实践


```
### 1.4 ngx lua API介绍 
```
ngx lua api
40多个指令，120多个api

ngx.lua的api是非阻塞的
而lua的api在nginx里写会阻塞worker，导致一个worker只能响应一个请求
千万不要在代码里直接调用lua自己的api

```
### 1.5 连接数据库 
```
MySQL
redis

写同步的代码，在openresty里的操作是异步非阻塞
```


### 1.6 缓存
```
2种
shared_dict
lua-resty-lrucache
缓存失效风暴


对比redis使用缓存QPS 25000，不使用缓存QPS 13000


```

### 1.7 FFI和第三方模块
FFI 类似 python里的ctypes
luajit FFI 可以让我们调用外部C函数和在lua代码里使用C的数据结构
增加第三方模块



### 1.8 子查询
```
location /lua {
	ngx.location.capture("/some_other_location")
	# 性能高，C函数调用
}

可以将请求串联起来


商品详情页 很多api
无法降级服务

给前端最好一个接口
# 并发请求
ngx.location.capture_multi{
	{"/foo", {}}
	{"bar",}
	{"",}
}

```
### 1.9 执行阶段 
```
nginx本身有十几个执行阶段
openresty自己有七个执行阶段


1. set_by_lua
流程分支判断

2. rewrite_by_lua
转发 重定向 缓存

3. access_by_lua
IP 准入/接口权限/配合iptables

4. content_by_lua
内容生成

5. header_filter_by_lua
头部处理

6. body_filter_by_lua
body 处理

7. log_by_lua
记录日志

# 对于需要加解密过程的需求
location /mixed {
	access_by_lua '...';  # 加密请求的解码
	content_by_lua '';   # 请求处理，不关心是否加解密
	body_filter_by_lua '';  # 应答加密
}


# 


```



