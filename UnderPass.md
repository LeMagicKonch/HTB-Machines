# UnderPass

## Table Of Contents:
<!--ts-->
  * [Port & Service Enumeration](#port-&-service-enumeration)
    * [HTTP Enumeration](#http-enumeration)
      * [Directory Enumeration](#directory-enumeration)
      * [Vhost Enumeration](#vhost-enumeration)
  * [Initial Access](#intial-access)
  * [Privilege Escalation](#privilege-escalation)
  * [Alternate Methods](#alternate-methods)

# **Port & Service Enumeration**

## **Nmap Scanning**

### **Generic Port Scan**

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-xmn3euvuef]─[~]
└──╼ [★]$ nmap -sC -sV -vv 10.129.163.87
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-02 11:15 CST
NSE: Loaded 156 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 11:15
Completed NSE at 11:15, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 11:15
Completed NSE at 11:15, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 11:15
Completed NSE at 11:15, 0.00s elapsed
Initiating Ping Scan at 11:15
Scanning 10.129.163.87 [4 ports]
Completed Ping Scan at 11:15, 0.04s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 11:15
Completed Parallel DNS resolution of 1 host. at 11:15, 0.00s elapsed
Initiating SYN Stealth Scan at 11:15
Scanning 10.129.163.87 [1000 ports]
Discovered open port 22/tcp on 10.129.163.87
Discovered open port 80/tcp on 10.129.163.87
Completed SYN Stealth Scan at 11:15, 0.17s elapsed (1000 total ports)
Initiating Service scan at 11:15
Scanning 2 services on 10.129.163.87
Completed Service scan at 11:16, 6.04s elapsed (2 services on 1 host)
NSE: Script scanning 10.129.163.87.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 11:16
Completed NSE at 11:16, 0.53s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 11:16
Completed NSE at 11:16, 0.04s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 11:16
Completed NSE at 11:16, 0.00s elapsed
Nmap scan report for 10.129.163.87
Host is up, received echo-reply ttl 63 (0.0090s latency).
Scanned at 2025-01-02 11:15:54 CST for 7s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 48:b0:d2:c7:29:26:ae:3d:fb:b7:6b:0f:f5:4d:2a:ea (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBK+kvbyNUglQLkP2Bp7QVhfp7EnRWMHVtM7xtxk34WU5s+lYksJ07/lmMpJN/bwey1SVpG0FAgL0C/+2r71XUEo=
|   256 cb:61:64:b8:1b:1b:b5:ba:b8:45:86:c5:16:bb:e2:a2 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJ8XNCLFSIxMNibmm+q7mFtNDYzoGAJ/vDNa6MUjfU91
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.52 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-server-header: Apache/2.4.52 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 11:16
Completed NSE at 11:16, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 11:16
Completed NSE at 11:16, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 11:16
Completed NSE at 11:16, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.12 seconds
           Raw packets sent: 1004 (44.152KB) | Rcvd: 1001 (40.036KB)
```

## **HTTP Enumeration**

Add *underpass.htb* to */etc/hosts*

Landing page

![image](https://github.com/user-attachments/assets/36ddff4b-2f73-41b1-819d-a0e3c256a8fa)

### **Directory Enumeration**

#### Using *gobuster*

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-xmn3euvuef]─[~]
└──╼ [★]$ gobuster dir -u http://underpass.htb/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt 
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://underpass.htb/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
Progress: 87664 / 87665 (100.00%)
===============================================================
Finished
===============================================================
```

#### Using *dirb*

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-xmn3euvuef]─[~]
└──╼ [★]$ dirb http://underpass.htb

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Thu Jan  2 14:04:56 2025
URL_BASE: http://underpass.htb/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://underpass.htb/ ----
+ http://underpass.htb/index.html (CODE:200|SIZE:10671)                                                                                                                                     
+ http://underpass.htb/server-status (CODE:403|SIZE:278)                                                                                                                                    
                                                                                                                                                                                            
-----------------
END_TIME: Thu Jan  2 14:05:36 2025
DOWNLOADED: 4612 - FOUND: 2
```

### **Vhost Enumeration**

#### Using *ffuf*  

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-xmn3euvuef]─[~]
└──╼ [★]$ ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -u http://underpass.htb -H "HOST: FUZZ.underpass.htb" -fs 10671

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://underpass.htb
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
 :: Header           : Host: FUZZ.underpass.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response size: 10671
________________________________________________

:: Progress: [114441/114441] :: Job [1/1] :: 1428 req/sec :: Duration: [0:01:12] :: Errors: 0 ::
```

#### Using *gobuster*

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-xmn3euvuef]─[~]
└──╼ [★]$ gobuster vhost -u http://underpass.htb/ -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt  -p pattern --exclude-length 301 -t 10
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:              http://underpass.htb/
[+] Method:           GET
[+] Threads:          10
[+] Wordlist:         /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
[+] Patterns:         pattern (1 entries)
[+] User Agent:       gobuster/3.6
[+] Timeout:          10s
[+] Append Domain:    false
[+] Exclude Length:   301
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
Progress: 228882 / 114442 (200.00%)
===============================================================
Finished
===============================================================

```
