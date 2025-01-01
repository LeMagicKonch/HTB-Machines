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
└─$ smbclient --no-pass -L //10.129.200.84
Anonymous login successful

        Sharename       Type      Comment
        ---------       ----      -------
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.200.84 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available

```

The anonymous login appears to work; however, not seeing any shares...

## **LDAP**
