---
title: Bounty Hack3r
date: 2025-09-18 14:00:00 +0200
media_subpath: /assets/img/2025-09-18-bountyHack3r
---

## **Recon**
```bash
nmap -T4 -sC -sV 10.10.13.8
```
<img src="nmapBounty.png" width="600" alt="agentC" style="display: block; margin: 0 auto;"/>

## **Enumeration**

#### *FTP: authenticate with Anon*
<img src="ftpBounty.png" width="600" alt="agentC" style="display: block; margin: 0 auto;"/>

#### *Website*
<img src="webpageBounty.png" width="600" alt="agentC" style="display: block; margin: 0 auto;"/>
#### *Source code*
<img src="sourcecodeBounty.png" width="600" alt="agentC" style="display: block; margin: 0 auto;"/>
#### *Nothing Here*
<img src="ffufBounty.png" width="600" alt="agentC" style="display: block; margin: 0 auto;"/>

## **Exploit**
#### *Password crack*
<img src="hydraBounty.png" width="600" alt="agentC" style="display: block; margin: 0 auto;"/>

#### *ssh with cracked passwd*

<img src="sshBounty.png" width="600" alt="agentC" style="display: block; margin: 0 auto;"/><img src="suidBounty.png" width="600" alt="agentC" style="display: block; margin: 0 auto;"/>


#### **root.txt!**
<img src="rootflagBounty.png" width="600" alt="agentC" style="display: block; margin: 0 auto;"/>

### **NOTE: I did a lot of mistakes trying to find a tar file and extract. I thought the root.txt was inside a tar file somewhere in the sys. 

### **PS: Better approach, once you have commands to run as sudo discovered by 'sudo -l' or SUID  try using [GTFOBins](https://gtfobins.github.io/#) and find ways to Esc Priv OKAY!!
