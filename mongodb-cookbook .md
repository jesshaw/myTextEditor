# MongoDB Cookbook

## windows install
下载新版，自定指定安装路径，完成安装退出可

## 以服务的形式配置MongoDB
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

## 删除集合（表）
```bash
db.user.drop()
```
