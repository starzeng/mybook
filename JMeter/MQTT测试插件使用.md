### 一. 安装

#### 1. 搭建MQTT服务器

先决条件：是否高可用，是否集群，并发如何

##### 1）RubbitMQ 搭建MQTT服务器

​	Rubbit安装教程：https://www.rabbitmq.com/download.html

​	开启MQTT插件：https://www.rabbitmq.com/mqtt.html#requirements

##### 2）开源的 EMQ X

​	安装教程：https://www.emqx.com/zh/downloads?product=broker

##### 3）。。。。



#### 2. 安装JMeter MQTT测试插件

​	下载地址：https://github.com/xmeter-net/mqtt-jmeter

​	下载Release的 mqtt-xmeter jar包。

![image-20210920123453632](/Users/zengweixiong/work/mybook/JMeter/images/image-20210920123453632.png)

​	重启JMeter，查看是否安装成功。

![image-20210920123553761](/Users/zengweixiong/work/mybook/JMeter/images/image-20210920123553761.png)

### 二. 使用

查看mqtt-xmeter手册：https://github.com/xmeter-net/mqtt-jmeter/wiki/Test-scenario

#### 1. 链接压测

##### 1）新建线程组

![image-20210920124059404](/Users/zengweixiong/work/mybook/JMeter/images/image-20210920124059404.png)



##### 2）新建mqtt链接取样

![image-20210920124215153](/Users/zengweixiong/work/mybook/JMeter/images/image-20210920124215153.png)



##### 3）填写测试信息

![image-20210920130244264](/Users/zengweixiong/work/mybook/JMeter/images/image-20210920130244264.png)

槽点：attampt啥意思，attempt才是吧



##### 4）添加结果树和汇总报告

![image-20210920124638234](/Users/zengweixiong/work/mybook/JMeter/images/image-20210920124638234.png)

##### 5）压测效果













#### 2. Pub 压测







#### 3. Sub 压测























