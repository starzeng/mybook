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



## Bito CLI 使用

```shell
# bito --help

用法：
bito [选项] [标志]

选项：
config [用于更新/编辑Bito配置]

配置标志：
--global [指定使用用户的全局配置文件]，并支持以下标志

标志：
-l，--list [列出所有配置变量和值]
-e，--edit [使用默认编辑器打开配置文件]
-h，--help [bito的帮助]
-p，--prompt [包含提示或执行操作的说明的文件。]
  {{%input%}}宏表示/引用-f选项中提到的文件的内容，
  这可以在提示文件中用于引用-f选项提供的整个文件的内容。 
  如果未使用{{%input%}}宏，则在处理之前将-f选项提供的文件内容附加到提示中。 
  如果没有同时提供-f和-p，CLI将提示输入内容以继续。

-f，--file [要在其中执行提示文件中描述的操作/说明的文件。]
-c，--context [在非交互模式下保存上下文/对话历史记录的文件。]
-m，--model [在当前会话中用于AI查询的模型类型。模型类型可以设置为 BASIC/ADVANCED，不区分大小写。]
-k，--key [期望使用Bito访问密钥来执行Bito CLI。Bito访问密钥可以在此处生成：https://alpha.bito.ai/home/settings/advanced]
-v，--version [获取Bito CLI的版本]

示例：

bito
这将启动我们的默认聊天功能。

bito -p prompt.txt -f filname.txt -c context.txt
这将首先从prompt.txt中读取提示，然后是filename中提供的文件，然后处理Bito AI请求。

bito config --global -l 或 bito config --list
这将在标准输出中列出所有全局用户配置文件变量。

bito config -e
这将使用用户的默认文本编辑器打开Bito AI CLI配置文件

```

