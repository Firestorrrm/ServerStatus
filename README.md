# ServerStatus (en)：   

* ServerStatus (en)

[![Python Support](https://img.shields.io/badge/python-3.6%2B%20-blue.svg)](https://github.com/cppla/ServerStatus)
[![C++ Compiler](http://img.shields.io/badge/C++-GNU-blue.svg?style=flat&logo=cplusplus)](https://github.com/cppla/ServerStatus)
[![License](https://img.shields.io/badge/license-MIT-4EB1BA.svg?style=flat-square)](https://github.com/cppla/ServerStatus)
[![Version](https://img.shields.io/badge/Version-Build%201.0.9-red)](https://github.com/cppla/ServerStatus)


`Watchdog 🐶` - Trigger alerts based on system information    

# Overview：

* Clients       	Client File
* Server       	 	Server File  
* Web           	Web Files

* server/config.json	Server Config File                                
* web/json      	Monthly Traffic Files        

# Automatic Deployment：

【Server】：
```bash

`Docker`:     

wget --no-check-certificate -qO ~/serverstatus-config.json https://raw.githubusercontent.com/cppla/ServerStatus/master/server/config.json && mkdir ~/serverstatus-monthtraffic    
docker run -d --restart=always --name=serverstatus -v ~/serverstatus-config.json:/ServerStatus/server/config.json -v ~/serverstatus-monthtraffic:/usr/share/nginx/html/json -p 80:80 -p 35601:35601 cppla/serverstatus:latest     

`Docker-compose (Recommended)`: docker-compose up -d
```

【Client】：
```bash
wget --no-check-certificate -qO client-linux.py 'https://raw.githubusercontent.com/cppla/ServerStatus/master/clients/client-linux.py' && nohup python3 client-linux.py SERVER={$SERVER} USER={$USER} PASSWORD={$PASSWORD} >/dev/null 2>&1 &

eg:
wget --no-check-certificate -qO client-linux.py 'https://raw.githubusercontent.com/cppla/ServerStatus/master/clients/client-linux.py' && nohup python3 client-linux.py SERVER=45.79.67.132 USER=s04  >/dev/null 2>&1 &
```

# Manual Deployment：     
   
**【Server】**           
          
#### 1. Compile server files              
```
`Debian/Ubuntu`: apt-get -y install gcc g++ make libcurl4-openssl-dev
`Centos/Redhat`: yum -y install gcc gcc-c++ make libcurl-devel

cd ServerStatus/server && make
./sergate
```
If there's no error press, Ok or ctrl+c to exit.
If there's an error, check if port 35601 is in use.

#### 2. Modify the config file  
```diff
! watchdog rule 可以为任何已知字段的表达式。         
! watchdog interval 最小通知间隔。
! watchdog callback 可自定义为Post方法的URL，告警内容将拼接其后并发起回调。   

! watchdog callback Telegram：https://api.telegram.org/bot你自己的密钥/sendMessage?parse_mode=HTML&disable_web_page_preview=true&chat_id=你自己的标识&text=
! watchdog callback Server酱: https://sctapi.ftqq.com/你自己的密钥.send?title=ServerStatus&desp=
! watchdog callback PushDeer: https://api2.pushdeer.com/message/push?pushkey=你自己的密钥&text=  
```

```
{
        "servers":
	[
		{
			"username": "s01",
			"name": "vps-1",
			"type": "kvm",
			"host": "chengdu",
			"location": "🇨🇳",
			"password": "USER_DEFAULT_PASSWORD",
			"monthstart": 1
		},
	],
	"watchdog":
	[
	        {
			"name": "服务器负载高监控",
			"rule": "cpu>90&load_5>3",
			"interval": 600,
			"callback": "https://yourSMSurl"
		},
		{
			"name": "你可以组合任何已知字段的表达式",
			"rule": "(hdd_used/hdd_total)*100>95",
			"interval": 1800,
			"callback": "https://yourSMSurl"
		}
	]
}       
```

#### 三、拷贝ServerStatus/status到你的网站目录        
例如：
```
sudo cp -r ServerStatus/web/* /home/wwwroot/default
```

#### 四、运行服务端：             
web-dir参数为上一步设置的网站根目录，务必修改成自己网站的路径   
```
./sergate --config=config.json --web-dir=/home/wwwroot/default   
```

**【客户端配置】**    

客户端有两个版本，client-linux为普通linux，client-psutil为跨平台版，普通版不成功，换成跨平台版即可。        

#### 一、client-linux版配置：       
1、vim client-linux.py, 修改SERVER地址，username帐号， password密码        
2、python3 client-linux.py 运行即可。      

#### 二、client-psutil版配置:                
1、安装psutil跨平台依赖库       
```
`Debian/Ubuntu`: apt -y install python3-pip && pip3 install psutil    
`Centos/Redhat`: yum -y install python3-pip gcc python3-devel && pip3 install psutil      
`Windows`: https://pypi.org/project/psutil/    
```
2、vim client-psutil.py, 修改SERVER地址，username帐号， password密码       
3、python3 client-psutil.py 运行即可。    

服务器和客户端自行加入开机启动，或进程守护，或后台方式运行。 例如： nohup python3 client-linux.py &    

`extra scene (run web/ssview.py)`
![Shell View](http://dl.cpp.la/Archive/serverstatus-shell.png)


# Make Better        

* BotoX：https://github.com/BotoX/ServerStatus
* mojeda: https://github.com/mojeda 
* mojeda's ServerStatus: https://github.com/mojeda/ServerStatus
* BlueVM's project: http://www.lowendtalk.com/discussion/comment/169690#Comment_169690

# Jetbrains    

<a href="https://www.jetbrains.com/?from=ServerStatus"><img src="https://resources.jetbrains.com/storage/products/company/brand/logos/jb_square.png" width="100px"></a>
