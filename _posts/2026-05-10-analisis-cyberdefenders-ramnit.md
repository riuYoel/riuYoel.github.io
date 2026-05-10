---
title: "Write-up: Ramnit in CyberDefenders"
date: 2026-05-10 11:00:00 +0200
categories: [Writeups, BlueTeam]
tags: [forensics, worm, exfiltrator]
image:
  path: /assets/img/ramnit.png # Opcional: una imagen de portada
---

## Scenario
So, our IDS has alerted suspicious behavior on a workstation, pointing to a likely malware intrusion. We got a memory dumb for our analysis so lets being :)
## Tools used
- [ ] Volatily3
- [ ] VirusTotal
- [ ] AbuseIPDB

## Solution (Step by step)

### Q1: What is the name of the process responsible for the suspicious activity?

We check the file type after extracting it and we continue, lets start with volatily, checking the family tree

```bash
python3 vol.py -f ~/[yourDirectory]/memory.dmp windows.pstree
```
![Result](/assets/img/p14.png)

Here we found the first answer if you look closely, the user executed it manually

`ChromeSetup.exe`

### Q2: What is the exact path of the executable for the malicious process?

This one is literally on the same path for the first question, just copy the full direction there

`C:\Users\alex\Downloads\ChromeSetup.exe`

### Q3: Identifying network connections is crucial for understanding the malware's communication strategy. What IP address did the malware attempt to connect to?

So, for this step yall can try the following commands
```bash
python3 vol.py -f ~/[yourDirectory]/memory.dmp windows.netstat or windows.netscan
```
But since mine doesnt work, i did the following;

```bash
mkdir ~/cyberdefenders/extracted_files

python3 vol.py -f ~/cyberdefenders/memory.dmp -o ~/cyberdefenders/extracted_files windows.dumpfiles --pid 4628
```
And found this once checked the results in the folder

![Image](/assets/img/p15.png) 

We got it, now we just have to extract its SHA256 and paste it on [VirusTotal](https://www.virustotal.com/gui/file/56133c0d017af35f49253926e3583cf72c36146ab7faa65b6058971685166652)

```bash
➜ sha256sum ~/cyberdefenders/extracted_files/file.0xca82b85325a0.0xca82b83c7770.DataSectionObject.ChromeSetup.exe.dat
```

![Image](/assets/img/p16.png)

`56133c0d017af35f49253926e3583cf72c36146ab7faa65b6058971685166652`

Now on VirusTotal, scroll down on the Community section and u will find the IP

![Image](/assets/img/p17.png)

`58.64.204.181`

### Q4: To determine the specific geographical origin of the attack, Which city is associated with the IP address the malware communicated with?

For this one just paste the IP on [AbuseIPDB](https://www.abuseipdb.com/check/58.64.204.181)

![Image](/assets/img/p18.png)

`Hong Kong`

### Q5: Hashes serve as unique identifiers for files, assisting in the detection of similar threats across different machines. What is the SHA1 hash of the malware executable?

This one is simple too, just go back to our terminal and execute sha1sum on the img 
```bash
➜ sha1sum ~/cyberdefenders/extracted_files/file.0xca82b85325a0.0xca82b7e06c80.ImageSectionObject.ChromeSetup.exe.img
```
![Image](/assets/img/p19.png)

`280c9d36039f9432433893dee6126d72b9112ad2`

### Q6: Examining the malware's development timeline can provide insights into its deployment. What is the compilation timestamp for the malware?

For this one just go back to [VirusTotal](https://www.virustotal.com/gui/file/56133c0d017af35f49253926e3583cf72c36146ab7faa65b6058971685166652/details) and scroll down on the 'Details' section until you find the 'History'

![Image](/assets/img/p20.png)

`2019-12-01 08:36`

### Q7: Identifying the domains associated with this malware is crucial for blocking future malicious communications and detecting any ongoing interactions with those domains within our network. Can you provide the domain connected to the malware?

This one is on [VirusTotal](https://www.virustotal.com/gui/file/56133c0d017af35f49253926e3583cf72c36146ab7faa65b6058971685166652/relations) Check relations, Contacted Domains

![Image](/assets/img/p21.png)

`dnsnb8.net`

# I hope this was helpful, please check my other write-ups :)
