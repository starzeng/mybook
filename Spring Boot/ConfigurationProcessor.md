# Configuration Processor



## @ConfigurationProperties 与 @Value 比较

| 属性               | @CongigurationProperties | @Value     |
| :----------------- | :----------------------- | :--------- |
| 功能               | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定(松散语法) | 支持                     | 不支持     |
| spEL               | 不支持                   | 支持       |
| JSP303数据校验     | 支持                     | 不支持     |



## 引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```



## 实例

```java
@Component
@EnableConfigurationProperties
@ConfigurationProperties(prefix = SwitchAndOptions.PREFIX)
public class SwitchAndOptions {

    public final static String PREFIX = "nacos.cmdb";

    private final int dumpTaskInterval = 3600;

    private final int eventTaskInterval = 10;

    private final int labelTaskInterval = 300;

    private final boolean loadDataAtStart = Boolean.FALSE;

    public int getDumpTaskInterval() {
        return dumpTaskInterval;
    }

    public int getEventTaskInterval() {
        return eventTaskInterval;
    }

    public int getLabelTaskInterval() {
        return labelTaskInterval;
    }

    public boolean isLoadDataAtStart() {
        return loadDataAtStart;
    }
}
```