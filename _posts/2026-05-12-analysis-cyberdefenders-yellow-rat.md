---
title: "Write-up: Yellow RAT"
date: 2026-05-12 10:00:00 +0200
categories: [Writeups, BlueTeam]
tags: [rat, forensics, intel]
image:
  path: /assets/img/yellow-rat.png
---

[Yellow RAT Lab](https://cyberdefenders.org/blueteam-ctf-challenges/xlmrat/)

## Scenario
During a regular IT security check we found abnormal traffic from multiple workstations, our task is to find out whats going on, lets begin.

## Tools used

- [ ] VirusTotal
- [ ] Red Canary

## Solution

### Q1: Understanding the adversary helps defend against attacks. What is the name of the malware family that causes abnormal network traffic?

So, first of all lets see whats inside `hash.txt`
![Image](/assets/img/p50.png)

Now lets paste the hash on [VirusTotal](https://www.virustotal.com/gui/file/30e527e45f50d2ba82865c5679a6fa998ee0a1755361ab01673950810d071c85)

![Image](/assets/img/p51.png)

Now for the first answer just go to 'Community' and u will find the family

![Image](/assets/img/p52.png)

`Yellow Cockatoo RAT`

### Q2: As part of our incident response, knowing common filenames the malware uses can help scan other workstations for potential infection. What is the common filename associated with the malware discovered on our workstations?

For this one just go to 'Details' and scroll down, u will find the name

![Image](/assets/img/p53.png)

`111bc461-1ca8-43c6-97ed-911e0e69fdf8.dll`

### Q3: Determining the compilation timestamp of malware can reveal insights into its development and deployment timeline. What is the compilation timestamp of the malware that infected our network?

For this one scroll up a bit and u will see the timestamps

![Image](/assets/img/p54.png)

`2020-09-24 18:26`


### Q4: Understanding when the broader cybersecurity community first identified the malware could help determine how long the malware might have been in the environment before detection. When was the malware first submitted to VirusTotal?

This one is on the same place, just look on the 'First Submission' side

![Image](/assets/img/p55.png)


`2020-10-15 02:47`

### Q5: To completely eradicate the threat from Industries' systems, we need to identify all components dropped by the malware. What is the name of the .dat file that the malware dropped in the AppData folder?

For this one we need to go to [Red Canary](https://redcanary.com/blog/threat-intelligence/yellow-cockatoo/) scroll down until we notice the only `.dat` file there a dev saw

![Image](/assets/img/p56.png)

`solarmarker.dat`

### Q6: It is crucial to identify the C2 servers with which the malware communicates to block its communication and prevent further data exfiltration. What is the C2 server that the malware is communicating with?

Now just go back to VirusTotal, in the 'Behavior' tab scroll down to 'Activity Summary' > 'Network Communications' > 'Memory Pattern URL'

![Image](/assets/img/p57.png)

`https://gogohid.com`

# And thats it, Yellow RAT Completed!
