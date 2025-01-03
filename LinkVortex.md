# LinkVortex

## Table Of Contents:
<!--ts-->
  * [Port & Service Enumeration](#port-&-service-enumeration)
    * [HTTP Enumeration](#http-enumeration)
      * [Robots.txt](#checking-robots.txt)
      * [Vhost Enumeration](#vhost-enumeration)
      * [Directory Enumeration](#directory-enumeration)
  * [Initial Access](#intial-access)
    * [Git-Dumper](#git-dumper)
    * [Searching For Keywords](#searching-for-keywords)
  * [Privilege Escalation](#privilege-escalation)
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

### **Vhost Enumeration**

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

### **Directory Enumeration**

#### **Checking *robots.txt* **

When going to *http://linkvortex.htb/robots.txt* we see the target is running *Ghost CMS*

Additionally, going to *http://linkvortex.htb/ghost* gets to a login page.

![image](https://github.com/user-attachments/assets/ec4700e4-949b-45d8-b418-dc09a61fa876)

#### **Importance of Trying Different Tools and Wordlists**

This box shows the importance of trying different wordlists and tools to enumerate directories as you might miss something!

Here is an example of how the same tool, *feroxbuster* had two different results using different wordlists:

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-sgld3m8c2v]─[~]
└──╼ [★]$ feroxbuster -u http://dev.linkvortex.htb/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 
                                                                                                                                                                                              
 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.11.0
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://dev.linkvortex.htb/
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.11.0
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET        7l       23w      196c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
403      GET        7l       20w      199c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET      115l      255w     2538c http://dev.linkvortex.htb/
[####################] - 53s   220546/220546  0s      found:1       errors:158    
[####################] - 52s   220546/220546  4242/s  http://dev.linkvortex.htb/  
```

Now using the *common.txt* wordlist:

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-sgld3m8c2v]─[~]
└──╼ [★]$ feroxbuster -u http://dev.linkvortex.htb/ -w /usr/share/wordlists/dirb/common.txt 
                                                                                                                                                                                              
 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.11.0
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://dev.linkvortex.htb/
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/wordlists/dirb/common.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.11.0
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET        7l       23w      196c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
403      GET        7l       20w      199c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET      115l      255w     2538c http://dev.linkvortex.htb/
200      GET        1l        1w       41c http://dev.linkvortex.htb/.git/HEAD
200      GET      115l      255w     2538c http://dev.linkvortex.htb/index.html
[####################] - 2s      9228/9228    0s      found:3       errors:0      
[####################] - 1s      4614/4614    3272/s  http://dev.linkvortex.htb/ 
[####################] - 1s      4614/4614    5322/s  http://dev.linkvortex.htb/cgi-bin/  
```

Now lets try the tool *dirsearch* which is a go-to tool for me personally:

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-sgld3m8c2v]─[~]
└──╼ [★]$ dirsearch -u http://dev.linkvortex.htb

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/lemagickonch/reports/http_dev.linkvortex.htb/_25-01-03_06-02-59.txt

Target: http://dev.linkvortex.htb/

[06:02:59] Starting: 
[06:03:00] 200 -   73B  - /.git/description
[06:03:00] 200 -   41B  - /.git/HEAD
[06:03:00] 301 -  239B  - /.git  ->  http://dev.linkvortex.htb/.git/
[06:03:00] 200 -  557B  - /.git/
[06:03:00] 200 -  201B  - /.git/config
[06:03:00] 200 -  620B  - /.git/hooks/
[06:03:00] 200 -  402B  - /.git/info/
[06:03:00] 200 -  240B  - /.git/info/exclude
[06:03:00] 200 -  401B  - /.git/logs/
[06:03:00] 200 -  175B  - /.git/logs/HEAD
[06:03:00] 200 -  147B  - /.git/packed-refs
[06:03:00] 200 -  393B  - /.git/refs/
[06:03:00] 200 -  418B  - /.git/objects/
[06:03:00] 301 -  249B  - /.git/refs/tags  ->  http://dev.linkvortex.htb/.git/refs/tags/
[06:03:00] 200 -  691KB - /.git/index
[06:03:00] 403 -  199B  - /.ht_wsr.txt
[06:03:00] 403 -  199B  - /.htaccess.bak1
[06:03:00] 403 -  199B  - /.htaccess.sample
[06:03:00] 403 -  199B  - /.htaccess.orig
[06:03:00] 403 -  199B  - /.htaccess_sc
[06:03:00] 403 -  199B  - /.htaccessBAK
[06:03:00] 403 -  199B  - /.htaccess.save
[06:03:00] 403 -  199B  - /.htaccessOLD2
[06:03:00] 403 -  199B  - /.html
[06:03:00] 403 -  199B  - /.htm
[06:03:00] 403 -  199B  - /.htaccessOLD
[06:03:00] 403 -  199B  - /.htpasswds
[06:03:00] 403 -  199B  - /.htpasswd_test
[06:03:00] 403 -  199B  - /.httr-oauth
[06:03:00] 403 -  199B  - /.htaccess_orig
[06:03:00] 403 -  199B  - /.htaccess_extra
[06:03:09] 403 -  199B  - /cgi-bin/
[06:03:23] 403 -  199B  - /server-status/
[06:03:23] 403 -  199B  - /server-status

Task Completed
```

# **Initial Access**

## **Git-Dumper.py**

After looking through the directory at *http://dev.linkvortex.htb/.git/* in the browser I started thinking that a lot was missing.

I googled how to enumerate *.git* directories for GitHub Projects and came across a tool called *git-dumper.py*

This lets us pull all the files and store them locally to disect.

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-sgld3m8c2v]─[~/Desktop/git-dumper/website]
└──╼ [★]$ python git_dumper.py http://dev.linkvortex.htb/.git/ ./website/
```

## **Searching For KeyWords**

There are a lot of files here. I tried to find some keywords that might expose credentials

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-sgld3m8c2v]─[~/Desktop/git-dumper/website]
└──╼ [★]$ find . -type f -exec grep -H "password" {} \;
```
