# ServerStatus中文版：   

* ServerStatus中文版是一个酷炫高逼格的云探针、云监控、服务器云监控、多服务器探针~。
* 在线演示：https://tanzhen.tujidu.com/

# 目录介绍：

* autodeploy    自动部署.
* clients       客户端文件
* server        服务端文件
* web           网站文件  

# 更新说明：

* 20190129, 降低CPU占用            
* 20181221, 增加实时到三网的延迟, 鼠标移到丢包率列,tips显示        
* 20181126, add tupd(tcp, udp, process ,thread) count for view ddcc attack    
* 20180829, 网络情况：主机到三网(CU,CT,CM)每小时丢包率的检测
* 20180726, 一切皆容器额,查看自动部署或autodeploy/readme
* 20180314, 调整前端，置默认密码为，设置ip和user即可上线　　　　　　
* 20180312, 加入失联(被照顾)检测【正常：MH361, 屏蔽：MH370】，校准虚拟化(container)流量统计异常　　　　　　
* 20170807, 更新平均1，5，15负载, 去掉无用的IPV6信息，增加服务器总流量监控                           

# docker自动部署 - 不支持价格参数：

##【服务端】：
```bash
wget https://raw.githubusercontent.com/langren1353/ServerStatus/master/autodeploy/config.json
docker run -d --restart=always --name=serverstatus -v {$path}/config.json:/ServerStatus/server/config.json -p {$port}:80 -p {$port}:35601 cppla/serverstatus

eg:
docker run -d --restart=always --name=serverstatus -v ~/config.json:/ServerStatus/server/config.json -p 80:80 -p 35601:35601 cppla/serverstatus
```

##【客户端】：
```bash
wget --no-check-certificate -qO client-linux.py 'https://raw.githubusercontent.com/langren1353/ServerStatus/master/clients/client-linux.py' && nohup python client-linux.py SERVER={$SERVER} USER={$USER} PASSWORD={$PASSWORD} >/dev/null 2>&1 &

eg:
wget --no-check-certificate -qO client-linux.py 'https://raw.githubusercontent.com/langren1353/ServerStatus/master/clients/client-linux.py' && nohup python client-linux.py SERVER=45.79.67.132 USER=s04  >/dev/null 2>&1 &
```

# 手动安装教程 - 支持价格参数：   
## 一、服务端配置
   
### 1.【克隆代码】:
```
cd /www/wwwroot/
git clone https://github.com/langren1353/ServerStatus.git
```

###2.【进行服务端配置】（服务端程序在ServerStatus/web下）:  
          
一、生成服务端程序，安装依赖项目，并`编译-这里需要编译环境自己先配好`
```
cd ServerStatus/server
make
./sergate
```
如果没错误提示，OK，ctrl+c关闭；如果有错误提示，检查`35601`端口是否被占用    

二、修改配置文件         
修改config.json文件，注意username, password的值，之后的客户端配置的值需要和服务端保持一致，简单点可以全都一致就行了         
```
{"servers":
	[
		{
			"username": "s01",
			"name": "Mainserver 1",
			"type": "Dedicated Server",
			"host": "GenericServerHost123",
			"location": "Austria",
			"password": "some-hard-to-guess-copy-paste-password",
			"extra1": "17$/yr",
			"extra2": "预计2020年12月12日到期",
			"extra3":""
		},
                {
			"username": "s01",
			"name": "Mainserver 1",
			"type": "Dedicated Server",
			"host": "GenericServerHost123",
			"location": "Austria",
			"password": "some-hard-to-guess-copy-paste-password",
			"extra1": "#200/年",
			"extra2": "吃灰，丢弃",
			"extra3":""
		},
	]
}       
```
其中extra1暂时用于设置服务器价格参数

 1. 日期支持y/yr/semi year/year/mon/month/day/h/hour/qua/quater/年/月/日/半年/2年/3年/季度/天/小时  不分大小写
 2. 金币支持￥/y/RMB/元/$/o/r/hkd/美元/日元/円 不分大小写
 3. 价格中，使用前缀`#`的话，那么将不会计入年度价格，并且会被着色为灰色


### 3.拷贝ServerStatus/status到你的网站目录，或者nginx直接指向该目录      
例如：
```
sudo cp -r ServerStatus/web/* /home/wwwroot/default
```

### 4.运行服务端：             
web-dir参数为上一步设置的网站根目录，务必修改成自己网站的路径   
```
./sergate --config=config.json --web-dir=/home/wwwroot/ServerStatus/server 
常用代码如：
kill -9 `ps -ef |grep sergate| grep -v 'grep' | awk '{print $2}'`
cd /www/wwwroot/ServerStatus/server && nohup ./sergate --config=config.json > /dev/null 2>&1 &  
```

## 二、【客户端配置】（客户端程序在ServerStatus/clients下）：          
### 1.修改对应的配置文件
vim client-linux.py, 修改SERVER地址，username帐号， password密码        
### 2. 启动程序
python client-linux.py 运行即可。打开云探针页面，就可以正常的监控。接下来把服务器和客户端脚本自行加入开机启动，或者进程守护，或以后台方式运行！
例如： `nohup python client-linux.py &`

# 为什么会有ServerStatus中文版：

* 有些功能确实没用
* 原版本部署，英文说明复杂
* 不符合中文版的习惯
* 没有一次又一次的轮子，哪来如此优秀的云探针

# 相关开源项目，感谢： 

* ServerStatus：https://github.com/cppla/ServerStatus
* ServerStatus：https://github.com/BotoX/ServerStatus
* mojeda: https://github.com/mojeda 
* mojeda's ServerStatus: https://github.com/mojeda/ServerStatus
* BlueVM's project: http://www.lowendtalk.com/discussion/comment/169690#Comment_169690
