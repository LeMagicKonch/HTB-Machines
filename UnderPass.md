# UnderPass

## Table Of Contents:
<!--ts-->
  * [Port & Service Enumeration](#port-&-service-enumeration)
    * [HTTP Enumeration](#http-enumeration)
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


