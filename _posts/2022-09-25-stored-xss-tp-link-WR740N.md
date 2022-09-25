---
layout: posts
title: "Stored XSS on TP-Link WR740N"
tags: research tp-link-WR740N
categories: Research
---

TP-Link WR740N suffers from a few stored XSS vulnerabilities.

<p style='text-align: justify;'>
This is a demonstration of a few XSS vulnerabilities present in TP-Link WR740N. It is possible to inject Javascript code by adding crafted descriptions onto the MAC Filtering tab and the target descriptions from Access Control tab.
</br>
Other PoCs exist but they don’t actually allow for the injection of Javascript code: https://www.exploitdb.com/exploits/43148
</br>
This is a PoC to demonstrate that is actually possible to inject script tags within the MAC description that leads to store XSS.
</p>

Go to “Wireless MAC Filtering”:

{:refdef: style="text-align: center;"}
![rustscan](/assets/research/tp-link-WR740N/2.png)
{: refdef}

Add new MAC Address filtering with the following fields:

{:refdef: style="text-align: center;"}
![rustscan](/assets/research/tp-link-WR740N/3.png)
{: refdef}

Notice the strange array on top of the page:

{:refdef: style="text-align: center;"}
![rustscan](/assets/research/tp-link-WR740N/4.png)
{: refdef}

Add a new MAC Address filtering but this time with the following:

{:refdef: style="text-align: center;"}
![rustscan](/assets/research/tp-link-WR740N/5.png)
{: refdef}

{:refdef: style="text-align: center;"}
![rustscan](/assets/research/tp-link-WR740N/6.png)
{: refdef}

<p style='text-align: justify;'>
This is a PoC to demonstrate that is actually possible to inject script tags within the Access Control target description that leads to store XSS.
</p>

<p style='text-align: justify;'>
Send the following first request to the website:
</p>

{:refdef: style="text-align: center;"}
![rustscan](/assets/research/tp-link-WR740N/7.png)
{: refdef}

Send the following second request to the website:

{:refdef: style="text-align: center;"}
![rustscan](/assets/research/tp-link-WR740N/8.png)
{: refdef}

Check Access Control – Target tab:

{:refdef: style="text-align: center;"}
![rustscan](/assets/research/tp-link-WR740N/9.png)
{: refdef}

<p style='text-align: justify;'>
TP-Link was contacted regarding this vulnerabilities and they said that the product reach EOF and so no mitigation will be made to the router.
</p>

