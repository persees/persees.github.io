---
layout: posts
title: "Cyber Heros"
tags: writeup, tryhackme
---

This is an easy machine from [TryHackMe](https://tryhackme.com/room/cyberheroes).

Running some scans reveals the ports 22 (SSH) and 80 (HTTP).

```rustscan -a 10.10.142.243```

![rustscan](/assets/cyberheros/1.png)


<p style='text-align: justify;'>
    After observing the source code, we see that the authentication mechanism is on the client side (which is a very insecure piece of code, since we can leverage the Javascript to get a cleartext username and password):
</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/cyberheros/2.png)
{: refdef}

<p style='text-align: justify;'>
    So we first receive the arguments for the user and password via "getElementById" functions. The comparison against the user is fairly simple, we just have to check if the user submitted is equals to "h3ck3rBoi".
    For the password, we also need to insert something that equals to the result of "ReverseString(54321@terceSrepuS')".
    It is pretty straightforward to get the result just by consulting the <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse">Javascript documentation</a>. If you are lazy like me, you can use the Javascript console to get the correct password by declaring the function in the console and use that same function to evaluate the result of the string "54321@terceSrepuS":
</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/cyberheros/3.png)
{: refdef}

<p style='text-align: justify;'>
    Now that we know the the username and the password, we can login onto the application and retrieve the flag:
</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/cyberheros/4.png)
{: refdef}

