# Bito CLI 使用

## 安装



## Java代码调用

```java
 private static void aiChat(String prompt) throws IOException, InterruptedException {
        // 构建命令行命令
        // bito -p prompt.txt -f filename.txt -c context.txt
        // ProcessBuilder processBuilder = new ProcessBuilder("bito", "-p", "prompt.txt", "-f", "filename.txt", "-c", "context.txt");
        
        // 构建命令行命令
        // echo "hello" | bito
        List<String> command = new ArrayList<>();
        command.add("bash");
        command.add("-c");
        command.add("echo \"" + prompt + "\" | bito");
        
        // 创建ProcessBuilder对象
        ProcessBuilder processBuilder = new ProcessBuilder(command);
        
        // 执行命令
        Process process = processBuilder.start();
        
        // 读取命令输出
        System.out.println("Output: ");
        BufferedReader reader = new BufferedReader(new InputStreamReader(process.getInputStream()));
        String line;
        while ((line = reader.readLine()) != null) {
            System.out.println(line);
        }
        
        // 等待命令执行完成
        int exitCode = process.waitFor();
        System.out.println("Exit Code: " + exitCode);
        
        reader.close();
    }
```

