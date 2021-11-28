# 介绍
## kcp
Kcptun 是一个非常简单和快速的，基于 KCP 协议的 UDP 隧道，它可以将 TCP 流转换为 KCP+UDP 流
## ss
Shadowsocks 常用的上网工具(不稳)
## kcp +ss =以10%-20%带宽浪费的代价换取了比 TCP快30%-40%的传输速度 + 稳
![kcp加速图](file:///D:/Download/64031552309695.png)

## server:
### 搭建ss:
yum install -y python-setuptools

easy_install pip

pip install shadowsocks

ssserver -p port(ss-server-port)(server 监听的端口) -m rc4-md5 -K ss_password -d start (开启ss服务)

firewall-cmd --zone=public --add-port=ssport/tcp --permanent 将该端口加入防火墙白名单中

firewall-cmd --reload 

### 搭建kcp

mkdir /root/kcptun

cd /root/kcptun

yum -y install wget

weget https://github.com/xtaci/kcptun/releases/download/v20181114/kcptun-linux-amd64-20181114.tar.gz (可以去下最新版本)

tar -zxvf kcptun-linux-amd64-20181114.tar.gz

vi /root/kcptun/server-config.json(配置kcp服务端文件)
```csharp
{
"listen": ":kcp_server_port",
"target": "ss_server_ip:ss_server_port",
"key"   : "kcp_password",
"crypt" : "aes",
"mode"  : "fast2",
}
```


chmod +x server_linux_amd64 (服务kcp服务器可执行权限)

nohup /root/kcptun/server_linux_amd64 -c /root/kcptun/server-config.json 2>&1 & (后台执行kcptun)

firewall-cmd --zone=public --add-port=kcp-server-port/udp --permanent (注意kcptun用的是udp协议)

firewall-cmd --reload

## client:
### kcp
下载 windows 版本的客户端
```csharp
https://github.com/xtaci/kcptun/releases
```


kcptun_client.json
```csharp
{
"localaddr" : ":kcp_client_port",
"remoteaddr": "kcp_server_port",
"key"       : "kcp_password",
"crypt"     : "aes",
"mode"      : "fast2"
}
```

start.bat:(运行脚本)
```csharp
client_windows_amd64.exe -c kcptun_client.json
```
### ss
下载windows 客户端
```csharp
https://github.com/shadowsocks/shadowsocks-windows/releases
```
ip 为127.0.0.1

port:kcp_client_port

password:ss_password


