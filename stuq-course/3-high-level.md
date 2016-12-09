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
- 善用长连接
- 正确设置连接池大小
- 链路首次授权认证
- 合理利用提速特性
```



### 3.1 redis 授权登录压力测试
```
使用ab对nginx --> redis 测试，
- 短连接情况下，并发在3700
- 长连接情况下，并发在8000以上
```
