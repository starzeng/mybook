```java
// 创建并启动计时器
Stopwatch stopwatch = Stopwatch.createStarted();
// 执行时间（1s）
Thread.sleep(1000);
// 停止计时器
stopwatch.stop();
// 执行统计
System.out.printf("执行时长：%d 毫秒. %n",
        stopwatch.elapsed(TimeUnit.MILLISECONDS));
// 清空计时器
stopwatch.reset();
// 再次启动统计
stopwatch.start();
// 执行时间（2s）
Thread.sleep(2000);
// 停止计时器
stopwatch.stop();
// 执行统计
System.out.printf("执行时长：%d 秒. %n",
        stopwatch.elapsed(TimeUnit.MILLISECONDS));
```

