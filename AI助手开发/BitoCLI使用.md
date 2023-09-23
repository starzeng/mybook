# Bito CLI 使用

## MAC安装

### 下载 BitoCLI到本地

https://github.com/gitbito/CLI

修改文件名为**bito**，并移动到 **/tmp/** 目录下



### 执行shell脚本

```shell
#!/bin/bash

echo "Bito CLI install ..."
set -e

BITO_FILE="bito"

# Move the bito binary to /usr/local/bin
sudo mv /tmp/$BITO_FILE /usr/local/bin

if [ -d "/var/log/bito" ] 
then
    sudo touch /var/log/bito/bitocli.log
else
    sudo mkdir /var/log/bito
    sudo touch /var/log/bito/bitocli.log
fi

sudo chmod -R 0777 /var/log/bito
sudo chmod +x /usr/local/bin/$BITO_FILE

# Ensure that /usr/local/bin is in the PATHs
if ! echo $PATH | grep -q /usr/local/bin ; then
  echo 'export PATH=$PATH:/usr/local/bin' >> ~/.bashrc
  export PATH=$PATH:/usr/local/bin
fi
echo "Bito CLI is now installed"
```





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

