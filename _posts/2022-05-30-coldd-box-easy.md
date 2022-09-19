---
layout: posts
title: "ColddBox: Easy"
tags: writeup tryhackme
categories: TryHackMe
---

This is an easy machine from [TryHackMe](https://tryhackme.com/room/colddboxeasy).

Running some scans reveals the ports 80 (HTTP) and 4512 (SSH).

```rustscan -a 10.10.199.75```

![rustscan](/assets/colddbox_easy/1.png)

```nmap -p 80,4512 -sC -sV 10.10.199.75```

![rustscan](/assets/colddbox_easy/2.png)

<p style='text-align: justify;'>Since our website is running Wordpress 4.1.31, we can launch wpscan too check plugins, themes and users.
From those users, we will also specify a wordlist to wpscan () to bruteforce passwords against the discovered users.
These were the results:</p>

```wpscan --url http://10.10.118.34 -e -P /usr/share/seclists/Passwords/Common-Credentials/10-million-password-list-top-10000.txt```

{:refdef: style="text-align: center;"}
![rustscan](/assets/colddbox_easy/3.png)
{: refdef}

<p style='text-align: justify;'>We find valid credentials for the user c0ldd.
We can use these credentials to login onto the wordpress admin dashboard:</p>


After loggin onto the admin dashboard, we create the following reverse shell:

{:refdef: style="text-align: center;"}
![rustscan](/assets/colddbox_easy/4.png)
{: refdef}

And zip it to upload to wordpress as a plugin:

{:refdef: style="text-align: center;"}
![rustscan](/assets/colddbox_easy/5.png)
{: refdef}

We set up a listener on port 80 and upload the shell.zip to the wordpress plugins tab:

{:refdef: style="text-align: center;"}
![rustscan](/assets/colddbox_easy/6.png)
{: refdef}

{:refdef: style="text-align: center;"}
![rustscan](/assets/colddbox_easy/7.png)
{: refdef}

<p style='text-align: justify;'>After uploading the plugin, it automatically activates and return to us a callback, we are now wordpress user:</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/colddbox_easy/8.png)
{: refdef}

Update the shell to a pty shell using python3:

```python3 -c 'import pty;pty.spawn("/bin/bash")'```


We can then check for SUID binaries available on the machine by using the following command:

```find / -perm -4000 2>/dev/null```

{:refdef: style="text-align: center;"}
![rustscan](/assets/colddbox_easy/10.png)
{: refdef}

Since find binary has an SUID bit we can use it to escalate privieleges to root:

{:refdef: style="text-align: center;"}
![rustscan](/assets/colddbox_easy/11.png)
{: refdef}
