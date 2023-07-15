---
title: "Tryhackme - Game Server"
date: 2023-07-15T15:59:25+07:00
draft: false
---

# Tryhackme - Game server
---
> Any actions and or activities related to the material contained within this Website is solely your responsibility. This site contains materials that can be potentially damaging or dangerous. If you do not fully understand something on this site, then GO OUT OF HERE! Refer to the laws in your province/country before accessing, using, or in any other way utilizing these materials.These materials are for educational and research purposes only.

- NMAP 
- GOBUSTER
- SSH2JOHN 
- lxd-alpine-builder

The gaming server is an easy-level boot2root room, where the attacker is asked to find two flags namely user.txt and root.txt, without further ado let's get started. The first step I did was scan IP to see which ports were open using nmap, here I see two open ports, port 22 for ssh and port 80 for http, for more details, see the image below.

---
# PORT SCANING
---

![image](/images/gameserver/nmap.jpg)

 After finding an open port then I brute-forced the directory using Gobuster, this tool is built in from Kalilinux, if you don't have it, you can download it on GitHub. From the results I found, there were two interesting directories, namely the secret directory and the uploads directory. I didn't have to wait long. I immediately checked the directory. 

![image](/images/gameserver/dirb.jpg)

![image](/images/gameserver/secret.jpg)

after I checked, it turned out that there was an RSA private key file, then I downloaded the code

![image](/images/gameserver/upload.jpg)


before that I looked at the secret directory and found the rsa code, and now I find a file that contains a password wordlist, maybe? I also download it

Until here I am confused, because previously from the results I found from the scan results, I only found two ports, one of which was SSH, to do SSH of course we must have a user and I am here starting to look for who the user is. I finally found the username, namely John, by looking at the source code

![image](/images/gameserver/source-code.jpg)

after I find the user, private key rsa, wordlist then what I do next is crack the private key rsa using ssh2john as below

![image](/images/gameserver/id_rsa.jpg)

after you crack then you can brute force it with john like the picture below

![image](/images/gameserver/lxd.jpg)

once you find the password for the user you can ssh and get the user.txt

![image](/images/gameserver/john.jpg)

before we go to root make sure we know the uid first, here they use lxd, so I immediately searched for an lxd privilege tutorial on the internet, and I suggest you install lxd-alpine-builder first on github

![image](/images/gameserver/dir-john.jpg)

after you clone lxd then your next step is to create a tar lxd file by typing the command in terminal cd lxd-alpine-builder then ./build-alpine

![image](/images/gameserver/build-lxd.jpg)

after you create alpine, the next step is to send the file to John's machine, here I use python in the following way.

![image](/images/gameserver/server.jpg)

then I took the alpine file on John's machine by wget

![image](/images/gameserver/shell.jpg)

after that we check whether we have got it or not

![image](/images/gameserver/user.jpg)

then I import

![image](/images/gameserver/tar.png)

safter we run the command above to be able to access root you can follow the steps below

![image](/images/gameserver/root.jpg)

and that's how i got root access. honestly to get lxd/lxc privileges is hard for me and i search many references on internet for this. and this time I learned something new about lxd/lxc exploits. I really enjoy this room. Thanks for reading, happy hacking!

