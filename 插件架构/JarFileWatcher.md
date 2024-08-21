

# Java Jar File Watcher

```java
package com.starry.framework.generator.filewatch;

import com.sun.nio.file.SensitivityWatchEventModifier;

import java.io.IOException;
import java.nio.file.FileSystems;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.StandardWatchEventKinds;
import java.nio.file.WatchEvent;
import java.nio.file.WatchKey;
import java.nio.file.WatchService;

/**
 * @author StarryZeng
 * @version 1.0.0
 * @date 2024/8/20 12:21
 */
public class JarFileWatcher {
    
    public static void main(String[] args) throws IOException, InterruptedException {
        // 监控的目录路径
        Path dir = Paths.get("/filewatch/jar");
        
        // 创建 WatchService 实例
        WatchService watchService = FileSystems.getDefault().newWatchService();
        
        // 注册目录并指定感兴趣的事件
        
        WatchEvent.Kind<Path>[] events = new WatchEvent.Kind[] {StandardWatchEventKinds.ENTRY_CREATE,
                StandardWatchEventKinds.ENTRY_DELETE, StandardWatchEventKinds.ENTRY_MODIFY};
        
        dir.register(watchService, events, SensitivityWatchEventModifier.HIGH);
        
        System.out.println("监控目录：" + dir.toString());
        
        while (true) {
            // 等待并获取事件
            WatchKey key = watchService.take();
            
            System.out.println("WatchKey: " + key);
            
            for (WatchEvent<?> event : key.pollEvents()) {
                WatchEvent.Kind<?> kind = event.kind();
    
                System.out.println("kind: " + kind);
                // 忽略其他事件
                if (kind == StandardWatchEventKinds.OVERFLOW) {
                    continue;
                }
                
                // 获取文件名
                WatchEvent<Path> ev = (WatchEvent<Path>) event;
                Path fileName = ev.context();
                
                // 处理创建事件
                if (kind == StandardWatchEventKinds.ENTRY_CREATE) {
                    if (fileName.toString().endsWith(".jar")) {
                        System.out.println("JAR 包添加: " + fileName);
                    }
                }
                
                // 处理删除事件
                if (kind == StandardWatchEventKinds.ENTRY_DELETE) {
                    if (fileName.toString().endsWith(".jar")) {
                        System.out.println("JAR 包删除: " + fileName);
                    }
                }
                
                // 处理修改事件
                if (kind == StandardWatchEventKinds.ENTRY_MODIFY) {
                    if (fileName.toString().endsWith(".jar")) {
                        System.out.println("JAR 修改: " + fileName);
                    }
                }
            }
            
            // 重置 key
            boolean valid = key.reset();
            
            System.out.println("valid: " + valid);
            
            if (!valid) {
                break;
            }
        }
        
        // 关闭 WatchService
        watchService.close();
    }
}

```

