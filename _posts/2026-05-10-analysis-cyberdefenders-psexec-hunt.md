---
title: "Write-up: PsExec Hunt"
date: 2026-05-10 18:50:00 +0200
categories: [Writeups, BlueTeam]
tags: [lateral, endpoint, smb]
image:
  path: /assets/img/psexec.png
---

[PsExec Hunt Lab](https://cyberdefenders.org/blueteam-ctf-challenges/psexec-hunt/)

## Scenario
An alert from our IDS flagged suspicious lateral movement activity wich involves PsExec, lets start

## Tools used
- [ ] Wireshark

## Solution

### Q1: To effectively trace the attacker's activities within our network, can you identify the IP address of the machine from which the attacker initially gained access?

Once you open Wireshark and scroll down u will notice something suspicious, a 3-way handshake, this tells us the malicious IP

![Result](/assets/img/p35.png)


`10.0.0.130`

### Q2: To fully understand the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted?

You can Right Click > Follow > TCP Stream on almost any and it will tell you the answer on the Packet

![Image](/assets/img/p36.png)

`SALES-PC`

### Q3: Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?

Just filter SMB2 on Wireshark and u will see it quick

![Image](/assets/img/p37.png)

`ssales`


### Q4: After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What's the name of the service executable the attacker set up on the target?

This one is below of the Q3 answer

![Image](/assets/img/p38.png)

`PSEXESVC.exe`

### Q5: We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machin

Same as with the other 2, the answer is right there, he used `ADMIN$`

![Image](/assets/img/p39.png)

### Q6: We must identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?

Again there we have the answer below, he used `IPC$`

![Image](/assets/img/p40.png)

### Q7: Now that we have a clearer picture of the attacker's activities on the compromised machine, it's important to identify any further lateral movement. What is the hostname of the second machine the attacker targeted to pivot within our network?

If you filter 'SMB' u have the answer, not once, twice, thrice but four times, thats the hostname of the second machine involved

![Image](/assets/img/p41.png)

`MARKETING-PC`

# And thats it, PsExec Hunt Completed
