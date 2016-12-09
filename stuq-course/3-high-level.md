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
 
 
 
 
 
