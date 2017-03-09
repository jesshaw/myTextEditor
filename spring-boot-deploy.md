# Spring boot centos7 后台服务安装部署

## Spring boot 应用服务安装部署（maven工程）

### 1.首先在maven工程的pom文件中引入以下标签并保存

```xml
<build>
     <plugins>
         <plugin>
             <groupId>org.springframework.boot</groupId>
             <artifactId>spring-boot-maven-plugin</artifactId>
         </plugin>
     </plugins>
 </build>
 ```
这样就可以将Spring boot工程打包成可执行jar包

打开windows cmd 或linux 命令行  执行打好的可执行jar包 用以下命令就可以执行

java -jar  abcd.jar 就可以执行spring boot 应用程序

### 2.编辑安装linux服务安装文件

本人是在windows环境下 用记事本先编辑好再上传到centos7 系统上面的

#### （1）首先创建记事本文件

#### （2）编写以下语句为了方便粘贴直接上文本

```shell
[Unit]
Description=abcd service
After=syslog.target

[Service]
Type=simple
ExecStart= /usr/bin/java -jar /home/app/abcd.jar

[Install]
WantedBy=multi-user.target
```
#### 说明

Description 服务描述

/usr/bin/java java路径（我这里是绝对路径，可以使用其他可执行java的路径）

/home/app/abcd.jar 可执行jar包的路径

然后将文本文件保存成后缀名为.service，上面的文件保存之后 可以是abcd.service

### 3.上传可执行jar包和.service安装文件

jar包程序文件上传到自定义的位置（我们会在每个系统用户下定义一个叫app的文件夹将jar包保存在此文件夹）

.service文件上传到系统/etc/systemd/system 目录下（本人用的是centos7系统，其他系统大同小异酌情处理）（如果你对linux文本编辑熟练的话 可以直接创建文件进行编辑）

注意编码要一致（验证是否一致只需在linux服务器上打开.service文件看是都和windows一致是否有乱码）

### 4.在部署服务器上执行以下命令（centos7）

首先 sudo systemctl daemon-reload 刷新服务配置文件

然后 sudo systemctl enable abcd.service 设置开机重启（视情况而定）

再　 sudo systemctl start  abcd.service 启动服务

### 5.查看日志

sudo journalctl -u abcd.service

以上linux各种操作都是在centos7下，其他版本系统基本差不多只是命令不同

这样部署就可以免去打成war包部署而存在的多一块项目路径的问题同时也支持spring cloud 注册中心（其实也主要是为了使用spring cloud注册中心）