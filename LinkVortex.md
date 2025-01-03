# LinkVortex

## Table Of Contents:
<!--ts-->
  * [Port & Service Enumeration](#port-&-service-enumeration)
    * [HTTP Enumeration](#http-enumeration)
      * [Directory Enumeration](#directory-enumeration)
      * [Vhost Enumeration](#vhost-enumeration)
    * [Misc Enumeration](#misc-enumeration)
  * [Initial Access](#intial-access)
    * [DaloRadius Server Enumeration](#daloradius-server-enumeration)
      * [Default Credentials](#default-credentials)
      * [Discovered Credentials](#discovered-credentials)
      * [SSH to Target](#ssh-to-target)
  * [Privilege Escalation](#privilege-escalation)
    * [Exploiting Mosh](#Spawn-Root-Shell-Using-Mosh)
  * [Alternate Methods](#alternate-methods)

# **Port & Service Enumeration**

## **Nmap Scanning**

### Generic Port Scan

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-sgld3m8c2v]─[~]
└──╼ [★]$ nmap -sC -sV linkvortex.htb
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-03 05:51 CST
Nmap scan report for linkvortex.htb (10.129.204.252)
Host is up (0.0093s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:f8:b9:68:c8:eb:57:0f:cb:0b:47:b9:86:50:83:eb (ECDSA)
|_  256 a2:ea:6e:e1:b6:d7:e7:c5:86:69:ce:ba:05:9e:38:13 (ED25519)
80/tcp open  http    Apache httpd
|_http-server-header: Apache
|_http-title: BitByBit Hardware
| http-robots.txt: 4 disallowed entries 
|_/ghost/ /p/ /email/ /r/
|_http-generator: Ghost 5.58
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.51 seconds
```

## **HTTP Enumeration**

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-sgld3m8c2v]─[~]
└──╼ [★]$ ffuf -u http://linkvortex.htb -H "Host: FUZZ.linkvortex.htb" -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -fs 230

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://linkvortex.htb
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.linkvortex.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response size: 230
________________________________________________

dev                     [Status: 200, Size: 2538, Words: 670, Lines: 116, Duration: 11ms]
:: Progress: [19966/19966] :: Job [1/1] :: 4761 req/sec :: Duration: [0:00:04] :: Errors: 0 ::
```

### **Vhost Enumeration**


### **Directory Enumeration**
