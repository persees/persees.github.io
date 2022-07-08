---
layout: posts
title: "Committed"
tags: writeup, tryhackme
---

This is an easy machine from [TryHackMe](https://tryhackme.com/room/committed).

Unzip the given files:

![rustscan](/assets/committed/1.png)

As we can see we have a .git folder that we can probably interact with the git binary.

So first of all we can see that we have two branches:

![rustscan](/assets/committed/2.png)

<p style='text-align: justify;'>
Lets see the commits made to master:
</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/committed/3.png)
{: refdef}

<p style='text-align: justify;'>
So, there are at least 4 commits before the finish one that can contain sensitive information.

Checking for commit information shows nothing.

Lets see the commits made to dbint:
</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/committed/4.png)
{: refdef}

{:refdef: style="text-align: center;"}
![rustscan](/assets/committed/5.png)
{: refdef}

<p style='text-align: justify;'>
We can see that there is some interesting commits that may containt sensitive information. Lets check them out:

By checking the "Oops" commit we can see the flag to the challenge:
</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/committed/6.png)
{: refdef}
