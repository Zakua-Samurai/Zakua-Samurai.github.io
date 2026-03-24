---
title: PG Stapler Walkthrough
date: 2026-03-24
categories: Offsec Proving-Grounds
tags: exploitation,metasploit,web,smb
toc: True
---

# Offsec Proving Grounds 

# Stapler Walkthrough

Stapler is an intermediate-difficulty Linux machine found in OffSec Proving Grounds.

The machine is known for having multiple attack vectors—including FTP, SMB, and SSH which tests your enumeration skills.

These Machines run on vulnerable systems.It allows us to find the structure of a machine like what type of technology it is using and how can we exploit it.

--

## How to deploy machine

**Important:**

You have to connect the openvpn before starting the machine otherwise there will be a web-interface

### How to Install the VPN

When you are on this stage so you will see the VPN option at left side

![how to find and start machine](/assets/img/pg-stapler/vpn.png)

Now have to start the Machine:

![how to find and start machine](/assets/img/pg-stapler/find.png)

![how to find and start machine](/assets/img/pg-stapler/find2.png)

![how to find and start machine](/assets/img/pg-stapler/find3.png)

After Downloading the VPM Configuration file run this command

```bash
sudo openvpn example.ovpn
```

Now we can work on it

--

## Enumeration

The first step to enumerate what ports are open and allow traffic.What services and technologies are running on ports

We will do this with the help of Nmap

Nmap is Powerful tool which is used for networking scanning

```bash
sudo nmap -Pn -A IP
```

We got some ports open

![nmap results](/assets/img/pg-stapler/stapler.png)

![showing nmap results](/assets/img/pg-stapler/scan.png)

**Lets Enumerate FTP Server**

--

## FTP Enumeration

### What is FTP

FTP stands for file transfer protocol which runs on port 21

The File Transfer Protocol (FTP) is a widely used protocol for transferring files between
computers over a network. FTP servers often contain sensitive information and can be
vulnerable to various attacks if not properly configured.

**As we can see** that port 21 allows us anonymous login

Now this one is one of the most important thing you have to check while performing FTP
enumeration as this can create potential entry on to the server.

### What is Anonymous FTP login

Anonymous FTP is a misconfiguration on the server where it allows us to access public files
on an FTP server without needing a special username and password. It's like a public library
where anyone can walk in and borrow books.

```bash
ftp IP
```

![anonymous login](/assets/img/pg-stapler/stapler2.png)

For name I will use `Anonymous` and for password I will let it blank and `press enter`

![A login confirmed](/assets/img/pg-stapler/ftp2.png)

If I click ls so it doesn't work but it shows a file name → `note`

I will download that file

```bash
get note
```

Now let's see the file 

```bash
cat note
```

We got a message from `John` but we didn't get anything juicy

**Lets Enumerate SSH**

--

## Lets Enumerate SSH

Secure Shell (SSH) is a widely used protocol for securely accessing remote systems over a
network. While SSH provides strong encryption and authentication, improper configuration
or the use of weak credentials can leave SSH servers vulnerable to enumeration and
exploitation.

Just like FTP. We can also login to the SSH server with the credentials → `Username`and`Password`

But we don't know the exact username and password so we will enumerate the usernames with metasploit-framework

Launch Metasploit → `msfconsole`

Search → `ssh_enumusers`





