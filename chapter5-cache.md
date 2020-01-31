# 缓存
![cache](./asset/chapter5-cache.png)

## Spring-Boot 集成
| 依赖                           | 描述                                | 备注                                 |
| ------------------------------ | ----------------------------------- | ------------------------------------ |
| spring-boot-starter-data-redis | 提供 redis 支持                     | 2.x 后底层不再使用jedis，而是Lettuce |
| commons-pool2                  | 用做 redis 连接池                   | 不加入，启动会报错                   |
| spring-session-data-redis      | 引入spring-session，用作共享session |                                      |

自定义模板：
```java
@Configuration
@AutoConfigureAfter(RedisAutoConfiguration)
public class RedisConfig {
  @Bean
  public RedisTemplate<String, Serializable> template(LettuceConnectionFactory connFactory) {
    RedisTemplate<String, Serializable> template = new RedisTemplate<>();
    template.setKeySerializable(new StringRedisSerializer());
    template.setValueSerializable(new GenericJackson2JsonRedisSerializer());
    template.setConnectionFactory(connFactory);

    return template;
  }
}
```
