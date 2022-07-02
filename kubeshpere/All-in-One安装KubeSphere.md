## 在 Linux 上以 All-in-One 模式安装 KubeSphere



https://kubesphere.com.cn/docs/v3.3/quick-start/all-in-one-on-linux/



### 1. 安装前提

**配置纯净的centos7系统**

**查看系统信息**

```bash
[root@kubeshpere ~]# uname -r
3.10.0-1160.el7.x86_64
[root@kubeshpere ~]# cat /etc/centos-release
```

**更新yum**

```bash
[root@kubeshpere ~]# yum -y update
```

**安装yum-utils软件包，使用yum-config-manager设置yum源，更新yum包索引**

```bash
[root@kubeshpere ~]# yum install -y yum-utils
  
// 这个地址要去浏览器验证下是否正确
[root@kubeshpere ~]# yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
// 生效
[root@kubeshpere ~]# yum makecache
```

**关闭防火墙**

```bash
[root@kubeshpere ~]# systemctl stop firewalld
[root@kubeshpere ~]# systemctl disable firewalld
```

**关闭 selinux**

````bash
# 查看selinux状态 SELinux status: enabled
[root@kubeshpere ~]# /usr/sbin/sestatus -v
# 永久关闭
[root@kubeshpere ~]# sed -i s#SELINUX=enforcing#SELINUX=disabled# /etc/selinux/config
# 临时关闭 setenforce 0 
# 重启查看是否关闭
[root@kubeshpere ~]# systemctl reboot
[root@kubeshpere ~]# /usr/sbin/sestatus -v
````

**关闭 swap**

```bash
# 临时关闭 swapoff ‐a 
# 永久关闭
[root@kubeshpere ~]# vim /etc/fstab 
# 注释掉swap这行 
# /dev/mapper/centos‐swap swap swap defaults 0 0
#重启生效 free‐m 查看下swap交换区是否都为0，如果都为0则swap关闭成功
[root@kubeshpere ~]# systemctl reboot
[root@kubeshpere ~]# free -m
```

**将桥接的IPv4流量传递到iptables**

```bash
# 这有个空等号左右没有空格不生效
[root@kubeshpere ~]# cat >> /etc/sysctl.d/k8s.conf << EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

# 生效
[root@kubeshpere ~]# sysctl --system
```

**确认可以使用 sudo curl openssl tar 命令**

```shell
[root@kubeshpere ~]# yum install socat

[root@kubeshpere ~]# yum install conntrack
```

**请确保 `/etc/resolv.conf` 中的 DNS 地址可用**



### 2. 下载 KubeKey

访问受限先执行: **export KKZONE=cn**

```bash
[root@kubeshpere ks]# export KKZONE=cn

[root@kubeshpere ks]# curl -sfL https://get-kk.kubesphere.io | VERSION=v2.2.1 sh -
```

![image-20220702134552463](images/image-20220702134552463.png)

```bash
[root@kubeshpere ks]# chmod +x kk
```

**安装版本参考官方**

```bash
[root@kubeshpere ks]# export KKZONE=cn

[root@kubeshpere ks]# ./kk create cluster --with-kubernetes v1.22.10 --with-kubesphere v3.3.0
```

![image-20220702134720637](images/image-20220702134720637.png)

**等待安装完成, 验证安装结果**

![image-20220702134822469](images/image-20220702134822469.png)

```bash
[root@kubeshpere ks]# kubectl logs -n kubesphere-system $(kubectl get pod -n kubesphere-system -l app=ks-installer -o jsonpath='{.items[0].metadata.name}') -f
```



使用以下命令检查运行状态

````bash
[root@kubeshpere ks]# kubectl get pod --all-namespaces
````



### 3. 浏览器访问

```bash
Console: http://192.168.0.2:30880
Account: admin
Password: P@88w0rd
```



### 4. 启动可插拔组件

https://kubesphere.com.cn/docs/v3.3/pluggable-components/overview/

按需启动, 如果资源可以建议启动所有

`admin` 用户登录控制台，点击左上角的**平台管理**，选择**集群管理**。

![image-20220702141713007](images/image-20220702141713007.png)



![image-20220702141732868](images/image-20220702141732868.png)



点击**定制资源定义**，在搜索栏中输入 **clusterconfiguration**，点击搜索结果查看其详细页面。



![image-20220702141857847](images/image-20220702141857847.png)



![image-20220702142020930](images/image-20220702142020930.png)

**以devops为例**

修改 devops 的enable 的值为 true **注意 两个devops 都要修改**

![image-20220702144218535](images/image-20220702144218535.png)



![image-20220702144250649](images/image-20220702144250649.png)

**在 kubectl 中执行以下命令检查安装过程：**

```bash
kubectl logs -n kubesphere-system $(kubectl get pod -n kubesphere-system -l 'app in (ks-install, ks-installer)' -o jsonpath='{.items[0].metadata.name}') -f
```

**点击确定后等待片刻, 会提示重新登录, 查看是否启动**

![image-20220702144027873](images/image-20220702144027873.png)



### 5.  卸载

```bash
[root@kubeshpere ks]# ./kk delete cluster
```













































