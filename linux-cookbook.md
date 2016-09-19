# Linux Cookbook
## 删除当前目录及子目录所有以js结尾的文件或目录
```bash
$ find . -name "*.js" -delete
```

## 列出含用node的所有进程
```bash
$  ps aux | grep node
```

## 更改密码
```bash
$  passwd
```

## centos-7 安装 nodejs

1. 选择 centos-7-x86_64 操作系统

    ``` bash
      [root@localhost ~]# yum -y install gcc gcc-c++ openssl-devel
      [root@localhost ~]# mkdir node && cd node
      [root@localhost node]# wget http://nodejs.org/dist/v5.12.0/node-v5.12.0.tar.gz
      [root@localhost node]# tar zxvf node-v5.12.0.tar.gz
      [root@localhost node]# mkdir -p /usr/local/node/5.12.0
      [root@localhost node]# cd node-v5.12.0
      [root@localhost node-v5.12.0]# ./configure --prefix=/usr/local/node/5.12.0
      [root@localhost node-v5.12.0]# make    #编译时间长
      [root@localhost node-v5.12.0]# make install
    ```


2. 设置 nodejs 系统环境
    ``` bash
      [root@localhost node-v5.12.0]# vim /etc/profile
    ```
    在此行之前 export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL 增加

    ```
      export NODE_HOME=/usr/local/node/5.12.0
      export NODE_PATH=$NODE_HOME/lib/node_modules
      export PATH=$NODE_HOME/bin:$PATH
    ```

3. 生效
    ``` bash
      [root@localhost node-v5.12.0]# source /etc/profile
      [root@localhost node-v5.12.0]# node -v
    ```

## 安装forever
	``` bash
	  [root@localhost node]# npm install forever -g
	```
	ref:[https://github.com/foreverjs/forever]

	因为node index.js & 执行后台进程容易因错误而终止,forever会自动重启

## nodejs hellworld示例
	``` bash
	  [root@localhost node]# mkdir www && cd www
	  [root@localhost www]# mkdir helloWorld && cd helloWorld
	  [root@localhost helloWorld]# vim app.js
	```

	``` js
	const http = require('http');

	const hostname = '104.128.88.249';
	const port = 3000;

	const server = http.createServer((req, res) => {
	  res.statusCode = 200;
	  res.setHeader('Content-Type', 'text/plain');
	  res.end('Hello World\n');
	});

	server.listen(port, hostname, () => {
	  console.log(`Server running at http://${hostname}:${port}/`);
	});
	```

	``` bash
	  [root@localhost helloWorld]# forever start app.js
	```

[参考1](http://www.cnblogs.com/vicowong/p/4156931.html)
[参考2](http://www.cnblogs.com/cjky/p/4971284.html)

## 安装git

	``` bash
	$ sudo yum install git-all
	```

	[ref](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)


## centos-7 install ftp

1. 安装
	``` bash
	  [root@localhost ~]# yum install vsftpd -y
	```

2. 启动vsftpd

	``` bash
	  [root@localhost ~]# systemctl start vsftpd.service
	```

3. 设置vsftpd开机自启动

	``` bash
	  [root@localhost ~]# systemctl enable vsftpd.service
	```

4. 配置VSFTPD
	``` bash
	  [root@localhost ~]# vim /etc/vsftpd/vsftpd.conf
	```

	> 是否允许匿名用户登陆FTP

	> anonymous_enable=NO

	> 切换目录时，显示目录下.message文件中的内容,默认是开启的

	> dirmessage_enable=YES


	>FTP上本地的文件权限，默认是077，不过vsftpd安装后的配置文件里默认是022.没有什么特殊情况不用修改。

	>ocal_umask=022
	 
	>启用上传和下载的日志功能，默认开启。建议开启此功能，它可以对用户的操作进行日志记录，当出现问题的时候可以通过日志排查问题。

	>xferlog_enable=YES

	 
	>FTP的欢迎信息。在FTP登陆成功之后，服务器会往客户端发送一个欢迎消息以表示登陆成功。这是一个个性化的功能，您可以自由的设置其值，也可以在配置最前加上#注释本行。

	>ftpd_banner=XXXX

	>数据连接超时时间。如果在使用vsftpd上传下载碎小文件的时候容易发生超时中断的问题，可以将本行前的#注释符去掉，然后将120改成5或者更小，然后重启vsftpd即可。

	>data_connection_timeout=120

6. 启动vsftpd
	``` bash
	[root@localhost ~]# systemctl restart vsftpd.service
	```

7. 创建FTP用户
	``` bash
	  [root@localhost ~]# useradd -d /var/www/html -s /sbin/nologin ftpuser
	  [root@localhost ~]# passwd ftpuser
	``` 

8. 调整文件夹权限
	``` bash
	  [root@localhost ~]# cd /var/www
	  [root@localhost www]# ls –l
	  [root@localhost www]# chmod 777 html
	  [root@localhost www]# ls –l
	```
[参考](https://www.yanning.wang/archives/184.html)


## centos-7 install shadowsocks-libev

一键安装 libev 版的 shadowsocks 最新版本。该版本的特点是内存占用小（600k左右），低 CPU 消耗，甚至可以安装在基于 OpenWRT 的路由器上。

1. 安装
	``` bash
	$ wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev.sh
	$ chmod +x shadowsocks-libev.sh
	$ ./shadowsocks-libev.sh 2>&1 | tee shadowsocks-libev.log
	```
2. 卸载方法
	``` bash
	$ ./shadowsocks-libev.sh uninstall
	```
3. 查看是否已启动（安装完成后会自动设置为开机启动）

	``` bash
	$ ps aux | grep ss-server | grep -v grep
	```
4. 其他命令

	``` bash
	$ /etc/init.d/shadowsocks start #启动
	$ /etc/init.d/shadowsocks stop #停止
	$ /etc/init.d/shadowsocks restart #重启
	$ /etc/init.d/shadowsocks status #查看状态
	$ vim /etc/shadowsocks-libev/config.json #查看配置信息
	```


