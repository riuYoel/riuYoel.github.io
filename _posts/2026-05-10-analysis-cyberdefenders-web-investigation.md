---
title: "Write-up: Web Investigation"
date: 2026-05-10 15:00:00 +0200
categories: [Writeups, BlueTeam]
tags: [forensics, network, web]
image:
  path: /assets/img/web-investigation.png
---

[Web Investigation Lab](https://cyberdefenders.org/blueteam-ctf-challenges/web-investigation/)

## Scenario
Now we are analysts in a SOC of an bookstore online, we have an pcap file (packet capture) so we can start with our favorite tool; Wireshark

## Tools used
- [ ] Wireshark
- [ ] AbuseIPDB
- [ ] CyberChef

## Solution

### Q1: By knowing the attacker's IP, we can analyze all logs and actions related to that IP and determine the extent of the attack, the duration of the attack, and the techniques used. Can you provide the attacker's IP?

Lets proceed and open our pcap file on Wireshark
![Wireshark](/assets/img/p22.png)

As soon as we open it, if we type "http" on wireshark and scroll down, there is the first answer after the server sends text to the attacker's IP

![Source](/assets/img/p23.png)

`111.224.250.131`

### Q2: If the geographical origin of an IP address is known to be from a region that has no business or expected traffic with our network, this can be an indicator of a targeted attack. Can you determine the origin city of the attacker?

Now just place the IP on [AbuseIPDB](https://www.abuseipdb.com/check/111.224.250.131)

![Image](/assets/img/p24.png)

`Shijiazhuang`

### Q3: Identifying the exploited script allows security teams to understand exactly which vulnerability was used in the attack. This knowledge is critical for finding the appropriate patch or workaround to close the security gap and prevent future exploitation. Can you provide the vulnerable PHP script name?

For this one, you can see it on Wireshark, if you look closely theres a consistence on the searches

![Image](/assets/img/p25.png)

`search.php`

### Q4: Establishing the timeline of an attack, starting from the initial exploitation attempt, what is the complete request URI of the first SQLi attempt by the attacker?

If we scroll down a bit we are going to find its first attempt of SQLi

![Image](/assets/img/p26.png)


Now just do Right Click > Copy > Summary as Text and paste it into [CyberChef](https://gchq.github.io/CyberChef/#recipe=URL_Decode(true)&input=MzU3CTE0NzAuNzAyNzEwCTExMS4yMjQuMjUwLjEzMQk3My4xMjQuMjIuOTgJSFRUUAk0NTIJR0VUIC9zZWFyY2gucGhwP3NlYXJjaD1ib29rJTIwYW5kJTIwMT0xOyUyMC0tJTIwLSBIVFRQLzEuMSAK) and select URL Decode

![Image](/assets/img/p27.png)

Just copy the result and ignore the rest

`/search.php?search=book and 1=1; -- -`

### Q5: Can you provide the complete request URI that was used to read the web server's available databases?

Here just filter by the word "SCHEMATA" wich contains the databases

![Image](/assets/img/p28.png)

Now same as with the Q4, Right Click > Copy > Summary as Text and paste it into [CyberChef](https://gchq.github.io/CyberChef/#recipe=URL_Decode(true)&input=MTUyMAkxNzU3LjkzMTQ3NwkxMTEuMjI0LjI1MC4xMzEJNzMuMTI0LjIyLjk4CUhUVFAJNDU3CUdFVCAvc2VhcmNoLnBocD9zZWFyY2g9Ym9vayUyNyUyMFVOSU9OJTIwQUxMJTIwU0VMRUNUJTIwTlVMTCUyQ0NPTkNBVCUyODB4NzE3ODc2NjI3MSUyQ0pTT05fQVJSQVlBR0clMjhDT05DQVRfV1MlMjgweDdhNzY2NzZhNjM2YiUyQ3NjaGVtYV9uYW1lJTI5JTI5JTJDMHg3MTc2NzA2YTcxJTI5JTIwRlJPTSUyMElORk9STUFUSU9OX1NDSEVNQS5TQ0hFTUFUQS0tJTIwLSBIVFRQLzEuMSAK)


![Image](/assets/img/p29.png)

Just copy the result and ignore the rest

`/search.php?search=book' UNION ALL SELECT NULL,CONCAT(0x7178766271,JSON_ARRAYAGG(CONCAT_WS(0x7a76676a636b,schema_name)),0x7176706a71) FROM INFORMATION_SCHEMA.SCHEMATA-- -`

### Q6: Assessing the impact of the breach and data access is crucial, including the potential harm to the organization's reputation. What's the table name containing the website users data?

Now for this one, just apply this command `http contains "INFORMATION_SCHEMA.COLUMNS"` wich is the next step to see whats inside the DB

![Image](/assets/img/p30.png)

And with these 3 searchs, just copy one by one and paste it into CyberChef with 'From HEX' and 'URL Decode'

![Image](/assets/img/p31.png)

Doing this with the 3 searchs we will find the following [=admin, =`customers`, =books]

### Q7: The website directories hidden from the public could serve as an unauthorized access point or contain sensitive functionalities not intended for public access. Can you provide the name of the directory discovered by the attacker?

If you saw on the examples from Q6 he used 2 and discovered one wich was, [=admin,=customers,=books]

![Image](/assets/img/p32.png)

Using logic we just add the slashes `/admin/`

### Q8: Knowing which credentials were used allows us to determine the extent of account compromise. What are the credentials used by the attacker for logging in?

For this one, we just use the filter `ip.src==111.224.250.131 and http.request.method==POST`

And we find 4 requests, just go one by one and check the 'HTML from URL encoded' side donwn below

`admin:admin123!`

### Q9: We need to determine if the attacker gained further access or control of our web server. What's the name of the malicious script uploaded by the attacker?

If you use `http.request.uri contains ".php"` and scroll down until you find admin/uploads/`NVri2vhp.php` there will be the answer

# And thats it, Web Investigation Completed
