##Caused by: redis.clients.jedis.exceptions.JedisDataException: ERR Client sent AUTH, but no password is set
```
意思就是redis服务器没有设置密码，但客户端向其发送了AUTH请求，于是把程序中所有jedis发送授权的地方都去掉
@Bean
public JedisPool jedisPool() {
return new JedisPool(new JedisPoolConfig(), "127.0.0.1", 6379);
}
关于redis的启动方式：
1、指定配置文件 $: ./redis-server /usr/local/redis.conf
2、不指定配置：$: ./redis-server &
不指定配置文件启动时采用默认配置，无密码
redis通过属性requirepass 设置访问密码，但没有设置该属性时，客户端向服务端发送AUTH请求，服务端就好返回异常：ERR Client sent AUTH, but no password is set


```