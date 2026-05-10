---
title: "Write-up: XMLRat in CyberDefenders"
date: 2026-05-09 18:00:00 +0200
categories: [Writeups, BlueTeam]
tags: [forensics, malware, dfir]
image:
  path: /assets/img/rat.png # Opcional: una imagen de portada
---

[Lab](https://cyberdefenders.org/blueteam-ctf-challenges/xlmrat/)

## Scenario
So, we have an machine that has been compromised and flagged due to suspicious network traffic. In order to solve this lab we need to analyze the PCAP file and identify various malicious files, lets begin :)

## Tools used
- [ ] Wireshark
- [ ] AbuseIPDB
- [ ] CyberChef
- [ ] VirusTotal

## Solution (Step by step)

### Q1: The attacker successfully executed a command to download the first stage of the malware. What is the URL from which the first malware stage was installed?

For practices i always check the type of file before anything
![Result](/assets/img/p1.png)

Once checked, now we proceed and open it on Wireshark
![Wireshark](/assets/img/p2.png)

As soon as we open it, we notice the first answer, we can get it also by typing "http" on wireshark

![Source](/assets/img/p3.png)

`http://45.126.209.4:222/mdm.jpg`

### Q2: Which hosting provider owns the associated IP address?

For this one, just place the suspicious IP on [AbuseIPDB](https://www.abuseipdb.com/check/45.126.209.4)

![Image](/assets/img/p4.png)

`reliableSite.net`

### Q3: By analyzing the malicious scripts, two payloads were identified: a loader and a secondary executable. What is the SHA256 of the malware executable?

Im going to make this one as clear as possible because here a lot of people get stuck, so we first go to wireshark, back to our /mdm.jpg there, now we right click it.

![Image](/assets/img/p5.png) 

Follow these steps: Right click > Follow > TCP Stream

![Image](/assets/img/p6.png)

If we see something like this, we are in good steps, now scroll down

![Image](/assets/img/p7.png)

When you find this other line means the first one ended, now copy the first one

![Image](/assets/img/p8.png)

Once we have selected, now just copy and paste it on [CyberChef](https://gchq.github.io/CyberChef/) 

Now select From Hex to MD5, Remember to delete the "$hexString_bbb ="

![Image](/assets/img/p9.png)

`88e8cee71f454bc1fa6b3a7741a3bd7d` Copy it and paste it into [VirusTotal](https://www.virustotal.com/gui/file/1eb7b02e18f67420f42b1d94e74f3b6289d92672a0fb1786c30c03d68e81d798)

Go to 'details' and there is SHA256

`1eb7b02e18f67420f42b1d94e74f3b6289d92672a0fb1786c30c03d68e81d798`

### Q4: What is the malware family label based on Alibaba?

![Image](/assets/img/p10.png)

On detection we have the answer `asyncrat`

### Q5: What is the timestamp of the malware's creation?

![Image](/assets/img/p11.png)

Just go to 'Details' and scroll down `2023-10-30 15:08`

### Q6: Which LOLBin is leveraged for stealthy process execution in this script? Provide the full path.

To answer this question, we need to go back to Wireshark and same as beofre: Right Click > Follow > TCP Stream

Now scroll down

![Image](/assets/img/p12.png)

Look at these lines "$NA = 'C:\W#######indow############s\Mi####cr'-replace  '#', ''
$AC = $NA + 'osof#####t.NET\Fra###mework\v4.0.303###19\R##egSvc#####s.exe'-replace  '#', ''"

Heres the answer, just delete the '#'

`C:\Windows\Microsoft.NET\Framework\v4.0.30319\RegSvcs.exe`

### Q7: The script is designed to drop several files. List the names of the files dropped by the script.

If you saw on the early image there was 2 of them, we scroll down a bit more for the third one

![Image](/assets/img/p13.png)

There is the last one, the answer is: `Conted.vbs,Conted.ps1,Conted.bat`

# I hope this was helpful :)
