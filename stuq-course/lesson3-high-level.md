# 《OpenResty 高阶实战》 
http://www.stuq.org/my/courses/study/1015

### 3.1 redis 授权登录
```
1. connetion create
2. auth
3. get
4. set 
5. connetion close
可以把nginx -> redis的连接改成长连接，减少认证步骤，提高性能。
10s内都使用长连接，get/set指令也可以集中一次发送。

# 总结
- 善用长连接
- 正确设置连接池大小
- 链路首次授权认证
- 合理利用提速特性
```



### 3.2 redis 授权登录压力测试
```
使用ab对nginx --> redis 测试，
- 短连接情况下，并发在3700
- 长连接情况下，并发在8000以上
```

### 3.3 NGINX MODULE不足
```
传统的nginx传统模块redis2_query指令不太灵活，无法进行业务逻辑判断，
如果想实现复杂的业务逻辑判断，可能需要多个location配合来实现，如果配置错误nginx服务会无法启动。
如果使用lua代码，则业务逻辑非常灵活，代码错误不会影响nginx整体。
redis_lua pipeline
```
### 3.4 唯一实例

```
 生产者/消费者
 
 p1  \                                 / c1
      \                              /
 p2 ---  Queue(Redis/Kafka/RabbitMQ) ---- c2
      /                              \
 p3 /                                  \  c3
 
 有时需要精准的控制生产者或消费者的数量
 ngx.worker.id()
 ngx.timer.at
 pcall
 
 
 # 总结/实现
 单一实例
 定期执行
 容错
```
 
### 3.5 redis 接口封装（其他数据库理念相通）
```
- 需要隐藏的2个信息，对业务来说
connect
set_keepalive

- 合并相同行为的api
合并get和set

```
 
### 3.6 普通模块与实例化模块
```
lua module is global
```
 
### 3.7 模块实例化的"坑"
```
# 对全局变量说NO
因为全局变量对于多个请求来说是共享的，多个请求共用一个变量，所以造成的结果很大概率会是，
多个不同的请求，却得到了同样的响应结果，
Lua代码就是一个对每一个执行流的单次处理逻辑，共用变量显然不是我们想要的，
因为每一个请求都有自己的独特参数，需要获得不同的结果。
我们在变量前面加一个local关键字，就可以把变量声明为局部变量，作用域为函数内部。
数据就不会冲突了。
检查工具（lua-releng）

# local变量、函数可以让模块运行更快？
善用local

使用LuaJIT比标准lua快很多，不在一个数量级上。

```

### 3.8 如何用好LuaJIT技术
```
为什么 LuaJIT 比 Lua快
just in time
JIT本来在自己的虚拟机里执行
运行阶段转化为机器代码
技术实现上注意点

如何确认代码使用了JIT技术
wiki.luajit.org  查看函数是否可以被编译  2.1版本
http://luajit.org/
```

### 3.9 测试用例
```
提升质量，持续迭代
单元测试/模块测试
API/压力测试

```

##### by 王院生 yuansheng@openresty.org
##### note by netqyq
 
