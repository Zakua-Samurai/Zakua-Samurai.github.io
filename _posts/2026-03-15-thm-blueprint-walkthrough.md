---
title: "THM Blueprint Walkthrough"
date: 2026-03-15
categories: [TryHackMe]
tags: [ctf,exploitation,Privilege Escalation ]
toc: True
---

# blueprint

Blueprint is a THM room which runs on a vulnerable windows 7 operating system.

![main](/assets/img/blueprint/main.jpg)

In this room, We will learn about NTLM(New Technology Lan Manager) which is the main thing in this walkthrough

We will learn about Privilege escalation

---

## Privilege Escalation

![privilege](/assets/img/blueprint/privilege.png)

Privilege Escalation is a cyberattack where a user or application gains unauthorized, higher-level permissions (e.g., admin or root) than intended.

![](/assets/img/blueprint/escalation.gif)

---

---

## What are TH(TryHackMe) rooms ?

TryHackMe rooms runs on a vulnerable system.We Implement the techniques to hack the vulnerable systems.

---

---

## VPN Configuration

Once You Start click “Start Machine” then you have to wait for 60 seconds and then you will have your Machine’s IP

![IP-address](/assets/img/blueprint/ip.jpg)

Now you have to configure you VPN which you can get from Manage Account < VM and VPN Setting and then download the Configuration file.

![click](/assets/all/click.jpg/)



Now we have to configure the VPN Properly:

After Downloading the VPN Configuration file run this command

```bash
sudo openvpn example.ovpn
```

Now we can work on it


















