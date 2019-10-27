# shadowsocks
> CentOS下搭建shadowsocks

#### 安装环境
1. 安装python-setuptools

	```shell
	yum install python-setuptools
	```
2. 安装epel

	```shell
	yum install -y epel-release
	```
3. 启用epel

	```shell
	yum install -y yum-utils && yum-config-manager --enable epel
	```
4. 安装pip

	```shell
	yum install -y python-pip
	```
5. 更新pip

	```shell
	pip install --upgrade pip
	```
6. 安装shadowsocks

	```shell
	pip install shadowsocks
	```

#### 配置shadowsocks
1. 创建配置文件夹，文件夹名称随意，这里取 `shadowsocks`

	```shell
	mkdir /etc/shadowsocks
	```
2. 创建配置文件，名字随意，这里取 `ss`

	```shell
	vi /etc/shadowsocks/ss.json
	```
3. 写入配置信息(两种模式二选一)
	> 单用户模式
	
	```json
	{
		"server":"0.0.0.0",
		"server_port":8080,
		"local_address":"127.0.0.1",
		"local_port":1080,
		"password":"123456",
		"timeout":300,
		"method":"aes-256-cfb",
		"fast_open":false,
		"workers": 1
	}
	```
	> 多用户模式
	
	```json
	{
		"server":"0.0.0.0",
		"port_password":{
			"8023":"123456",
			"8024":"123456"
		},
		"timeout":300,
		"method":"aes-256-cfb",
		"fast_open": false
	}
	```

4. 保存并退出编辑
	```shell
	:wq
	```

#### 防火墙设置
1. 如果服务商是国内的，如阿里云、腾讯云、华为云等，直接在控制台放行对应的端口即可
2. 如果是其它服务商，可以通过系统命令放行，具体如以下操作
	1. 查看防火墙状态
	
	```shell
	systemctl status firewalld
	```
	2. 开启防火墙（若防火墙不可用）
	
	```shell
	systemctl start firewalld
	```
	
	3. 添加防火墙端口，添加`--permanent`是让端口永久有效
	
	```shell
	firewall-cmd --zone=public --add-port=8023/tcp --permanent
	```
	4. 重载防火墙策略，使新增的端口生效
	
	```shell
	firewall-cmd --reload
	```
	5. 注 ： 配置多少个账户，就必须放行对应的端口，否则无法连接
	
#### 服务端
1. 开启shadowsocks

	```shell
	ssserver -c /etc/shadowsocks/ss.json -d start
	```
2. 停止shadowsocks

	```shell
	ssserver -c /etc/shadowsocks/ss.json -d stop
	```
3. 重启shadowsocks

	```shell
	ssserver -c /etc/shadowsocks/ss.json -d restart
	```

#### 客户端使用
> 下载对应版本的shadowsocks客户端，根据配置信息添加服务器，连接即可

1. Android
[安卓版客户端下载](https://github.com/shadowsocks/shadowsocks-android/releases "安卓版客户端下载")

2. IOS
[IOS版客户端下载](https://github.com/shadowsocks/shadowsocks-iOS/releases "IOS版客户端下载") （苹果手机建议直接搜索ss客户端）

3. Windows
[Windows版客户端下载](https://github.com/shadowsocks/shadowsocks-windows/releases "Windows版客户端下载")

4. 其它版本
[其它版本客户端下载](https://github.com/shadowsocks "其它版本客户端下载")
