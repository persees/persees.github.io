---
layout: posts
title: "Unauthenticated LFI on TP-Link WR740N"
tags: research tp-link-WR740N
categories: Research
---

TP-Link WR740N suffers from an LFI vulnerability in the /help/ directory.

<p style='text-align: justify;'>
This is a PoC to demonstrate how to exploit the vulnerability and get the shadow file present on the linux system.
</p>

Make a request as the following:

{:refdef: style="text-align: center;"}
![rustscan](/assets/research/tp-link-WR740N/1.png)
{: refdef}

<p style='text-align: justify;'>
From the research made, it does not look like there are previously LFI vulnerabilities discovered.
</p>

<p style='text-align: justify;'>
TP-Link was contacted regarding this vulnerability and they said that the product reach EOF and so no mitigation will be made to the router.
</p>

