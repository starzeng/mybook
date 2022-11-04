## **一、docker常用命令**

```shell
docker --help #查看docker命令
docker info #docker 详细信息，镜像和容器
docker version #查看docker版本

docker build -t nginx:latest

docker build -t registry.cn-shanghai.aliyuncs.com/wxay/platform-manager-api:1.0.0 ./platform-manager-api
sudo docker login --username=1369866181@qq.com registry.cn-shanghai.aliyuncs.com
docker push registry.cn-shanghai.aliyuncs.com/wxay/waxy-oneid-server:1.0.0
docker pull registry.cn-shanghai.aliyuncs.com/wxay/waxy-oneid-server:1.0.0

# 清理none镜像
docker image prune
```

> 帮助文档地址：https://docs.docker.com/reference/

## **二、镜像命令**

```
docker images # 查看docker镜像；
# 具体列解释含义：
REPOSITORY#镜像仓库源                
TAG#镜像的标签                 
IMAGE ID#镜像id            
CREATED#创建时间             
SIZE#大小
```

同一个仓库源可以有多个TAG，表示这个仓库源的不同版本，我们使用`REPOSITORY:TAG`来定义不同的镜像。如果不指定一个镜像的版本标签，例如只使用tomcat，docker将默认使用`tomcat:latest`镜像

```
docker images -a#列出本地所有的镜像
docker images -q#只显示镜像ID
docker images --digests#显示镜像的摘要信息
docker images --no-trunc#显示完整的镜像信息
```

示例：

```bash
[root@izbp1hcw0fjg64l58525bqz ~]# docker images -q
d1165f221234
[root@izbp1hcw0fjg64l58525bqz ~]# docker images --digests
REPOSITORY    TAG       DIGEST                                                                    IMAGE ID       CREATED        SIZE
hello-world   latest    sha256:0fe98d7debd9049c50b597ef1f85b7c1e8cc81f59c8d623fcb2250e8bec85b38   d1165f221234   5 months ago   13.3kB
[root@izbp1hcw0fjg64l58525bqz ~]# docker images --no-trunc
REPOSITORY    TAG       IMAGE ID                                                                  CREATED        SIZE
hello-world   latest    sha256:d1165f2212346b2bab48cb01c1e39ee8ad1be46b87873d9ca7a4e434980a7726   5 months ago   13.3kB
```

- dockerhub

```
docker search tomcat #从Docker Hub上查找tomcat镜像

STARS：关注度
docker search --filter=stars=300 tomcat#从Docker Hub上查找关注度大于300的tomcat镜像
docker pull tomcat#从Docker Hub上下载tomcat镜像。等价于：docker pull tomcat:latest
```

从Docker Hub上查找关注度大于300的tomcat镜像

```
NAME #名称
DESCRIPTION #描述
STARS #点赞
OFFICIAL #是否官方
AUTOMATED #是否自动构建
```

- 镜像下载

```
# 下载Redis官方最新镜像，相当于：docker pull redis:latest
[root@izbp1hcw0fjg64l58525bqz ~]# docker pull redis
Using default tag: latest
latest: Pulling from library/redis
33847f680f63: Pull complete
26a746039521: Pull complete
18d87da94363: Pull complete
5e118a708802: Pull complete
ecf0dbe7c357: Pull complete
46f280ba52da: Pull complete
Digest: sha256:cd0c68c5479f2db4b9e2c5fbfdb7a8acb77625322dd5b474578515422d3ddb59
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest
```

- 删除镜像命令

```
##单个镜像删除，相当于：docker rmi redis:latest
docker rmi redis
##强制删除(针对基于镜像有运行的容器进程)
docker rmi -f redis
##多个镜像删除，不同镜像间以空格间隔
docker rmi -f redis tomcat nginx
##删除本地全部镜像
docker rmi -f $(docker images -q)
```

## **三、容器命令**

只有下载镜像才能运行容器命令

- 容器启动与停止

```
##新建并启动容器，参数：-i  以交互模式运行容器；-t  为容器重新分配一个伪输入终端；--name  为容器指定一个名称
docker run -i -t --name mycentos
##后台启动容器，参数：-d  已守护方式启动容器
docker run -d mycentos

#启动或者停止容器
docker start 容器id
docker restart 容器id
docker kill 容器id
docker stop 容器id
```

示例：

```
#运行centos镜像
[root@izbp1hcw0fjg64l58525bqz ~]# docker run -it centos
# 查看镜像文件目录
[root@9ec4a30b3209 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@9ec4a30b3209 /]#exit
#并没有运行中的镜像
[root@izbp1hcw0fjg64l58525bqz ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

- 容器进入与退出

```
##使用run方式在创建时进入
docker run -it centos /bin/bash
##关闭容器并退出
exit
##仅退出容器，不关闭
快捷键：Ctrl + P + Q
```

示例：

```
#启动镜像
[root@izbp1hcw0fjg64l58525bqz ~]# docker run -it centos /bin/bash
#ctrl +p +q退出，查看运行的容器
[root@f6db6f0661af /]# [root@izbp1hcw0fjg64l58525bqz ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED              STATUS              PORTS     NAMES
f6db6f0661af   centos    "/bin/bash"   About a minute ago   Up About a minute             elegant_shtern
# 停止容器
[root@izbp1hcw0fjg64l58525bqz ~]# docker stop f6db6f0661af
f6db6f0661af
[root@izbp1hcw0fjg64l58525bqz ~]# docker ps -q
```

- 容器日志

```
##查看redis容器日志，默认参数
docker logs rabbitmq
##查看redis容器日志，参数：-f  跟踪日志输出；-t   显示时间戳；--tail  仅列出最新N条容器日志；
docker logs -f -t --tail=20 redis
##查看容器redis从2021年08月10日后的最新10条日志。
docker logs --since="2021-08-10" --tail=10 redis
```

- 进入当前正在运行的容器

通常容器使用后台的方式运行，需要进入容器，修改一些配置

方式一

```bash
docker exec -it 容器id bash
```

方式二

```bash
docker attach 容器id bash(/bin/bash)
```

exec：进入容器后，开启一个新的终端，可以再里面操作；

attach：进入容器正在执行的终端，不会启动新的终端进程；

- 容器内拷贝文件到主机

```
docker cp 容器id:容器内路径  目的主机路径

```