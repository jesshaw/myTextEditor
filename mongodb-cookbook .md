# MongoDB Cookbook

## windows 安装 MongoDB
下载新版，自定指定安装路径，完成安装退出可

### 以服务的形式配置
1. 管理员身份运行cmd.exe

2. 创建数据库目录及日志目录
	```bash
	mkdir c:\data\db
	mkdir c:\data\log
	```

3. MongoDB目录下创建配置文件C:\MongoDB\Server\3.2\mongod.cfg，文件内容为
	```bash
	systemLog:
	    destination: file
	    path: c:\data\log\mongod.log
	storage:
	    dbPath: c:\data\db
	```

4. 安装MongoDB服务
	```bash
	"C:\MongoDB\Server\3.2\bin\mongod.exe" --config "C:\MongoDB\Server\3.2\mongod.cfg" --install
	```

5. 启动MongoDB服务
	```bash
	net start MongoDB
	```

6. 停止或移除MongoDB服务
	```bash
	net stop MongoDB
	"C:\MongoDB\Server\3.2\bin\mongod.exe" --remove
	```
## 连接本地库
```bash
mongo
```

## 删除集合（表）
```bash
db.user.drop()
```

## centos 7 安装 MongoDB
1. Configure the package management system
	```bash
	vim /etc/yum.repos.d/mongodb-org-3.2.repo
	```
	> [mongodb-org-3.2]
	> name=MongoDB Repository
	> baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
	> gpgcheck=1
	> enabled=1
	> gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc

2. Install the MongoDB packages and associated tools.
	```bash
	sudo yum install -y mongodb-org
	```

3. Run MongoDB Community Edition
	```bash
	sudo service mongod start
	```
	> <port> is the port configured in /etc/mongod.conf, 27017 by default.

