---
title: "Write-up: RedLine"
date: 2026-05-14 13:00:00 +0200
categories: [Writeups, BlueTeam]
tags: [endpoint, c2, forensics]
image:
  path: /assets/img/redline.png
---

[RedLine Lab](https://cyberdefenders.org/blueteam-ctf-challenges/redline/)

## Scenario
As part of the SOC team, our assignment today is to analyze a memory dump using redline and volatily tools, lets begin.

## Tools used

- [ ] Volatily
- [ ] VirusTotal 

## Solution

### Q1: What is the name of the suspicious process?

For this question we can use 'pstree' on our volatily and if we scroll down we will notice a suspicious process, yall saw it too, right?

![Image](/assets/img/p68.png)

`oneetx.exe`

### Q2: What is the child process name of the suspicious process?

We can see it right below, there its child live

![Image](/assets/img/p69.png)

`rundll32.exe`

### Q3: What is the memory protection applied to the suspicious process memory region?

On this one you can execute the following command ` python vol.py -f ~/[YourDirectory]/MemoryDump.mem windows.vadinfo --pid 5896` since we got the pid earlier now we can check its specific VAD (Virtual Address Descriptor) info

![Image](/assets/img/p70.png)

`PAGE_EXECUTE_READWRITE`

### Q4: What is the name of the process responsible for the VPN connection?

For this one we have to go back to `pstree` if we look closely we can notice a child that manages the logic of the VPN and `tun2socks` that manages traffic using a VPN (SOCKS)

![Image](/assets/img/p71.png)

`Outline.exe`

### Q5: What is the attacker's IP address?

Now just use `netscan` and u will see a suspicious IP on the oneetx.exe process

![Image](/assets/img/p72.png)

`77.91.124.20`

### Q6: What is the full URL of the PHP file that the attacker visited?

Just paste the IP on [VirusTotal](https://www.virustotal.com/gui/ip-address/77.91.124.20/details) go to 'Details' and scroll until you see 'Google results'

![Image](/assets/img/p73.png)

`http://77.91.124.20/store/games/index.php`

### Q7: What is the full path of the malicious executable?

Lets do a filescan with volatily and grep oneetx.exe

`python vol.py -f ~/[YD]/MemoryDump.mem windows.filescan | grep "oneetx.exe"`

![Image](/assets/img/p74.png)


There it is `C:\Users\Tammam\AppData\Local\Temp\c3912af058\oneetx.exe`

# And thats it, RedLine Completed!
