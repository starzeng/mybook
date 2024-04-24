# Spring Data Redis Repository



https://spring.io/projects/spring-data-redis



## POM

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```



## Repository Example

### ServerConfigRedis.java

```java
/**
 * IM 服务 配置
 *
 * @author StarryZeng
 * @version 1.0.0
 * @date 2024/4/23 10:43
 */
@Data
@RedisHash("server_config")
public class ServerConfigRedis {
    
    @Id
    private Integer id;
    
    private NettyServerConfig nettyServerConfig;
    
    /**
     * 状态: true 在线; false 下线
     * 注意：这里使用了 @Indexed 索引，findFirstByStatus 才会生效
     * 参考：
     */
    @Indexed
    private Boolean status;
    
    //@TimeToLive
    //private long timeout;
    
}
```

### ServerConfigRepository.java

```JAVA
/**
 * @author StarryZeng
 * @version 1.0.0
 * @date 2024/4/23 10:47
 */
public interface ServerConfigRepository extends CrudRepository<ServerConfigRedis, Integer> {
    
    /**
     * 通过状态查询
     *
     * @param status 状态
     * @return 服务配置
     */
    Optional<ServerConfigRedis> findFirstByStatus(Boolean status);
    
}
```

### RedisService.java

```java

/**
 * @author StarryZeng
 * @version 1.0.0
 * @date 2024/4/23 14:38
 */
@Slf4j
@AllArgsConstructor
@Service
public class RedisService {
    
    private final ServerConfigRepository serverConfigRepository;
    
    
    public ServerConfigRedis save(ServerConfigRedis serverConfigRedis) {
        return serverConfigRepository.save(serverConfigRedis);
    }
    
    public ServerConfigRedis update(ServerConfigRedis serverConfigRedis) {
        Integer id = serverConfigRedis.getId();
        if (ObjectUtils.isEmpty(id)) {
            return null;
        }
        ServerConfigRedis update = serverConfigRepository.findById(id).orElse(null);
        if (ObjectUtils.isEmpty(update)) {
            return null;
        }
        update.setStatus(serverConfigRedis.getStatus());
        
        return serverConfigRepository.save(update);
    }
    
    public void delete(Integer id) {
        serverConfigRepository.deleteById(id);
    }
    
    
    public ServerConfigRedis findById(Integer id) {
        return serverConfigRepository.findById(id).orElse(null);
    }
    
    public List<ServerConfigRedis> findAll() {
        Iterable<ServerConfigRedis> iterable = serverConfigRepository.findAll();
        return StreamSupport.stream(iterable.spliterator(), false).collect(Collectors.toList());
    }
    
    public ServerConfigRedis findFirstByStatus(Boolean status) {
        return serverConfigRepository.findFirstByStatus(status).orElse(null);
    }
    
}
```

