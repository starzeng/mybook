### 根据jar包名称 kill Java 进程后启动

```shell
kill -9 $(ps -ef | grep xhotel-ebooking-api-0.0.1-SNAPSHOT.jar | grep -v grep | awk '{print $2}'); nohup java -Xms64m -Xmx128m -jar /workspace/wxay-product-server/source/xhotel-ebooking-api/target/xhotel-ebooking-api-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev> /workspace/wxay-product-server/xhotel-ebooking.out 2>& 1&
```

