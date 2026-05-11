---
title: "Write-up: The Crime"
date: 2026-05-11 14:00:00 +0200
categories: [Writeups, BlueTeam]
tags: [forensics, aleapp, endpoint]
image:
  path: /assets/img/the-crime.png
---

[The Crime Lab](https://cyberdefenders.org/blueteam-ctf-challenges/the-crime/)

## Scenario
In this scenario we are investigating a murder case, we obtained the victims phone and now we have to get evidence, lets start

## Tools used
- [ ] ALEAPP

## Solution (Step by step)

### Q1: Based on the accounts of the witnesses and individuals close to the victim, it has become clear that the victim was interested in trading. This has led him to invest all of his money and acquire debt. Can you identify the SHA256 of the trading application the victim primarily used on his phone?

So first of all we open `ALEAPP` on our terminal after extracting the folder with the mobile information

![Result](/assets/img/p42.png)

Here i did a folder with the command
```bash
mkdir aleapp-temp
```

Then select the new folder we did in Output and for the select folder do `home/[yourUser]/[folderWhereYouUnzipedTheFile]/data`

![Result](/assets/img/p43.png)

Then click 'Process' and wait until the results are done then click 'View' an browser page will be open, now scroll down to the section 'Installed Apps (GMS)' 

![Image](/assets/img/p44.png)

Now just copy the trade app hash

`4f168a772350f283a1c49e78c1548d7c2c6c05106d8b9feb825fdc3466e9df3c`

### Q2: According to the testimony of the victim's best friend, he said, "While we were together, my friend got several calls he avoided. He said he owed the caller a lot of money but couldn't repay now". How much does the victim owe this person?

For this one just scroll down to 'SMS Messages' and u will see a message with the amount of the debt

![Image](/assets/img/p45.png)

`250000`

### Q3: What is the name of the person to whom the victim owes money?

For this one just go to 'Contacts' and scroll down until you see the matching phone number from the message of the debt

![Image](/assets/img/p46.png) 

`Shady Wahab`

### Q4: Based on the statement from the victim's family, they said that on September 20, 2023, he departed from his residence without informing anyone of his destination. Where was the victim located at that moment? 


For this one lets check its activity; go to 'Recent Activity' and scroll down in the snapshots, there will be the location

![Image](/assets/img/p47.png)

`The Nile Ritz-Carlton`

### Q5: The detective continued his investigation by questioning the hotel lobby. She informed him that the victim had reserved the room for 10 days and had a flight scheduled thereafter. The investigator believes that the victim may have stored his ticket information on his phone. Look for where the victim intended to travel.

For this one just go to 'Google Photos' and click on any of the ticket images, there is his destination

![Image](/assets/img/p48.png)

`Las Vegas`

### Q6: After examining the victim's Discord conversations, we discovered he had arranged to meet a friend at a specific location. Can you determine where this meeting was supposed to occur?

For this one go to 'Discord chats' and scroll down, on 'rob1ns0n' chat u will see the answer

![Image](/assets/img/p49.png)

`The Mob Museum`

# And thats it, The Crime Completed
