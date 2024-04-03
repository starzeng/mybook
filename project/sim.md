# SIM（Starry Instant Messaging ）



## 架构

**核心架构**

```Java
1. 自建网关处理登录授权
2. 启动多个netty服务器注册到nacos集群上，将 nettyServerId 和 nettyChannel 缓存在本地内存的MAP中
3. 用户登录，将 userId 和 nettyServerId 对应存储到Redis中
4. 用户发送消息，通过 userId 查询Redis中的 nettyServerId
5. 通过 nettyServerId 查询 nacos中的 nettyServer
6. 找到netty服务器，在通过netty服务器的本地缓存查询 nettyChannel
7. 最后通过channel发送消息；
```



| 模块       |      |
| ---------- | ---- |
| sim-api    |      |
| sim-auth   |      |
| sim-common |      |

