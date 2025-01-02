# UnderPass

## Table Of Contents:
<!--ts-->
  * [Port & Service Enumeration](#port-&-service-enumeration)
    * [HTTP Enumeration](#http-enumeration)
      * [Directory Enumeration](#directory-enumeration)
      * [Vhost Enumeration](#vhost-enumeration)
    * [Misc Enumeration](#misc-enumeration)
  * [Initial Access](#intial-access)
    * [DaloRadius Server Enumeration](#daloradius-server-enumeration)
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

**NOTE:** At this point we are getting nowhere! We can start to look to see if the Apache Server Version is vulnerable. But first lets try using *snmpbulkwalk* just to see what info we can get as it has saved me before.

## **Misc Enumeration**

### *snmpbulkwalk*

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-xmn3euvuef]─[~]
└──╼ [★]$ snmpbulkwalk -c public -v2c underpass.htb
iso.3.6.1.2.1.1.1.0 = STRING: "Linux underpass 5.15.0-126-generic #136-Ubuntu SMP Wed Nov 6 10:38:22 UTC 2024 x86_64"
iso.3.6.1.2.1.1.2.0 = OID: iso.3.6.1.4.1.8072.3.2.10
iso.3.6.1.2.1.1.3.0 = Timeticks: (1110150) 3:05:01.50
iso.3.6.1.2.1.1.4.0 = STRING: "steve@underpass.htb"
iso.3.6.1.2.1.1.5.0 = STRING: "UnDerPass.htb is the only daloradius server in the basin!"
iso.3.6.1.2.1.1.6.0 = STRING: "Nevada, U.S.A. but not Vegas"
iso.3.6.1.2.1.1.7.0 = INTEGER: 72
iso.3.6.1.2.1.1.8.0 = Timeticks: (3) 0:00:00.03
iso.3.6.1.2.1.1.9.1.2.1 = OID: iso.3.6.1.6.3.10.3.1.1
iso.3.6.1.2.1.1.9.1.2.2 = OID: iso.3.6.1.6.3.11.3.1.1
iso.3.6.1.2.1.1.9.1.2.3 = OID: iso.3.6.1.6.3.15.2.1.1
iso.3.6.1.2.1.1.9.1.2.4 = OID: iso.3.6.1.6.3.1
iso.3.6.1.2.1.1.9.1.2.5 = OID: iso.3.6.1.6.3.16.2.2.1
iso.3.6.1.2.1.1.9.1.2.6 = OID: iso.3.6.1.2.1.49
iso.3.6.1.2.1.1.9.1.2.7 = OID: iso.3.6.1.2.1.50
iso.3.6.1.2.1.1.9.1.2.8 = OID: iso.3.6.1.2.1.4
iso.3.6.1.2.1.1.9.1.2.9 = OID: iso.3.6.1.6.3.13.3.1.3
iso.3.6.1.2.1.1.9.1.2.10 = OID: iso.3.6.1.2.1.92
iso.3.6.1.2.1.1.9.1.3.1 = STRING: "The SNMP Management Architecture MIB."
iso.3.6.1.2.1.1.9.1.3.2 = STRING: "The MIB for Message Processing and Dispatching."
iso.3.6.1.2.1.1.9.1.3.3 = STRING: "The management information definitions for the SNMP User-based Security Model."
iso.3.6.1.2.1.1.9.1.3.4 = STRING: "The MIB module for SNMPv2 entities"
iso.3.6.1.2.1.1.9.1.3.5 = STRING: "View-based Access Control Model for SNMP."
iso.3.6.1.2.1.1.9.1.3.6 = STRING: "The MIB module for managing TCP implementations"
iso.3.6.1.2.1.1.9.1.3.7 = STRING: "The MIB module for managing UDP implementations"
iso.3.6.1.2.1.1.9.1.3.8 = STRING: "The MIB module for managing IP and ICMP implementations"
iso.3.6.1.2.1.1.9.1.3.9 = STRING: "The MIB modules for managing SNMP Notification, plus filtering."
iso.3.6.1.2.1.1.9.1.3.10 = STRING: "The MIB module for logging SNMP Notifications."
iso.3.6.1.2.1.1.9.1.4.1 = Timeticks: (3) 0:00:00.03
iso.3.6.1.2.1.1.9.1.4.2 = Timeticks: (3) 0:00:00.03
iso.3.6.1.2.1.1.9.1.4.3 = Timeticks: (3) 0:00:00.03
iso.3.6.1.2.1.1.9.1.4.4 = Timeticks: (3) 0:00:00.03
iso.3.6.1.2.1.1.9.1.4.5 = Timeticks: (3) 0:00:00.03
iso.3.6.1.2.1.1.9.1.4.6 = Timeticks: (3) 0:00:00.03
iso.3.6.1.2.1.1.9.1.4.7 = Timeticks: (3) 0:00:00.03
iso.3.6.1.2.1.1.9.1.4.8 = Timeticks: (3) 0:00:00.03
iso.3.6.1.2.1.1.9.1.4.9 = Timeticks: (3) 0:00:00.03
iso.3.6.1.2.1.1.9.1.4.10 = Timeticks: (3) 0:00:00.03
iso.3.6.1.2.1.25.1.1.0 = Timeticks: (1111696) 3:05:16.96
iso.3.6.1.2.1.25.1.2.0 = Hex-STRING: 07 E9 01 02 14 0D 13 00 2B 00 00 
iso.3.6.1.2.1.25.1.3.0 = INTEGER: 393216
iso.3.6.1.2.1.25.1.4.0 = STRING: "BOOT_IMAGE=/vmlinuz-5.15.0-126-generic root=/dev/mapper/ubuntu--vg-ubuntu--lv ro net.ifnames=0 biosdevname=0
"
iso.3.6.1.2.1.25.1.5.0 = Gauge32: 0
iso.3.6.1.2.1.25.1.6.0 = Gauge32: 218
iso.3.6.1.2.1.25.1.7.0 = INTEGER: 0
iso.3.6.1.2.1.25.1.7.0 = No more variables left in this MIB View (It is past the end of the MIB tree)
```

From the output we can see the name of a potential user *steve@underpass.htb* and we see a line that makes me think the server is a *daloradius server*...

Time to Google!

# **Initial Access**

## **DaloRadius Server Enumeration**

After some googling i noticed this file directory that gets created : */var/www/daloradius* 

![image](https://github.com/user-attachments/assets/43ebab35-b824-473c-b1db-c5c023c11fcf)

This made me want to rerun the directory enumeration but this time using the given url of *http://underpass.htb/daloradius/*

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-xmn3euvuef]─[~]
└──╼ [★]$ dirb http://underpass.htb/daloradius/

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Thu Jan  2 14:23:58 2025
URL_BASE: http://underpass.htb/daloradius/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://underpass.htb/daloradius/ ----
==> DIRECTORY: http://underpass.htb/daloradius/app/                                                                                                                                         
+ http://underpass.htb/daloradius/ChangeLog (CODE:200|SIZE:24703)                                                                                                                           
==> DIRECTORY: http://underpass.htb/daloradius/contrib/                                                                                                                                     
==> DIRECTORY: http://underpass.htb/daloradius/doc/                                                                                                                                         
==> DIRECTORY: http://underpass.htb/daloradius/library/                                                                                                                                     
+ http://underpass.htb/daloradius/LICENSE (CODE:200|SIZE:18011)                                                                                                                             
==> DIRECTORY: http://underpass.htb/daloradius/setup/

<SNIP>
```

### **User Login Page Found**

![image](https://github.com/user-attachments/assets/c34af58f-1c8b-4742-adda-c83e93d9f6d8)

Unfortunately, none of the default passwords worked here. Time to check the login page for the operators.

### **Operator Login Page Found**

![image](https://github.com/user-attachments/assets/7407f242-dbee-493e-ac9b-54d40cc280d2)

Found on the documentation for *daloradius* that the default credentials are *Administrator : radius* which worked on this login portal...

![image](https://github.com/user-attachments/assets/7dcfa15f-c77d-4401-a0a3-82c60be36858)

### **Credentials Discovered**

In the User Management pane we see a *svcMosh* with a password that looks encrypted.

![image](https://github.com/user-attachments/assets/d8b9c6aa-b3f1-4b2e-aa66-4d31296572f4)

As this looks like weak encryption I threw it into *crackstation* and got the password *underwaterfriends*

![image](https://github.com/user-attachments/assets/88610120-e13f-4409-a9cf-06917d800e26)



