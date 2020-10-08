                                                         Hogwarts Writeup(KoTH)
    
                                                                               ~by Cipher007
    
                                                        Discord: Cipher007#1099 
                                                        Credits: Rogue-CTF
                                  
Scanning

 `nmap -sS -A -vvv -oN nmap-initial &lt;IP&gt;`

When I performed a rustscan on the machine, many different ports turned up. Also all the ports, passwords etc. keep changing on this machine so you probably wouldn’t be getting the same ports and passwords as me.

![](https://github.com/Cipher7/Writeups/blob/main/Hogwarts/images/nmap.png?raw=true)



`rustscan <IP>`

![](https://github.com/Cipher7/Writeups/blob/main/Hogwarts/images/rustscan.png?raw=true)

Enumerating FTP

As we see in the above scan output that FTP allows anonymous access. Let us see what files are present in FTP.

`ftp <IP> <PORT>`

![](https://github.com/Cipher7/Writeups/blob/main/Hogwarts/images/ftp.png?raw=true)

Now when we do “ls -la” we get a “…” path, chanding our directory to that we get a file .I\_saved\_it\_harry.zip , Let us copy this to our machine.

Now let us use zip2john to crack this zip file through bruteforce using rockyou.txt as the wordlist.

`zip2john .I_saved_it_harry.zip > tbcracked`

![](https://github.com/Cipher7/Writeups/blob/main/Hogwarts/images/zip2john.png?raw=true)

`john tbcracked –wordlist=/path_to_rockyou.txt/`

![](https://github.com/Cipher7/Writeups/blob/main/Hogwarts/images/john.png?raw=true)

![](https://github.com/Cipher7/Writeups/blob/main/Hogwarts/images/unzip.png?raw=true)

![](https://github.com/Cipher7/Writeups/blob/main/Hogwarts/images/ssh-creds.png?raw=true)

We get a username and password! Now let us ssh into the machine using this.

Note that **ssh is also running on a different port**. Let us login to the

machine.

SSH into the machine

`ssh neville@<IP> -p <PORT>

![](https://github.com/Cipher7/Writeups/blob/main/Hogwarts/images/user.png?raw=true)`

Enter the password and now you’re user on the box! Get the user flag.

Linpeas

I used linpeas to find ways to privesc.

![](https://github.com/Cipher7/Writeups/blob/main/Hogwarts/images/linpeastransfer.png?raw=true)

![](https://github.com/Cipher7/Writeups/blob/main/Hogwarts/images/linpeas.png?raw=true)

Ahah! There you go, a SUID vulnerability! Lets go to GTFO bins and get the exploit.

Root!!

Let’s use the last set of commands of “ip” in GTFO bins. After executing the

second , we get root!!

![](https://github.com/Cipher7/Writeups/blob/main/Hogwarts/images/root.png?raw=true)



Now we are root! Let’s get the root flag and put our username in the king.txt file. But wait, there isn’t a king.txt! Just create one in the root folder and put your username in it, you will then become the king and start searching for flags in the user folders.
