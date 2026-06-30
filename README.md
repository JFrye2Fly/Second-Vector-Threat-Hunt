# Second-Vector Threat Hunt hreat-hunt-scenario-suspicious-rdp-login

# Official [Cyber Range](http://joshmadakor.tech/cyber-range) Project
<img width="420" alt="Screen Shot 2025-06-23 at 6 48 22 AM" src="https://github.com/user-attachments/assets/82097391-d0fa-4572-8eb9-b10ec41fc2b2" />

## Platforms and Languages Leveraged
- Microsoft Sentinel
- EDR Platform: Microsoft Defender for Endpoint
- Kusto Query Language (KQL)


##  Scenario

LOG(N) Pacific runs on Microsoft 365. Overnight, Entra ID Protection raised a low-severity incident against a finance user. A single sign-in from an anonymous IP. The night shift triaged it, found
nothing they could act on, and left it in the queue.
The detection caught one sign-in. It did not ask what happened next. No malware, no endpoint to image, no outage. Everything the attacker did, they did through identity, mail, and cloud services. The case
file is open.

### This threat hunt was 39 flags, I will walk you through some of the questions and how I found the answers

## Question and Answers below

### 1. Open the incident. Who's the account this fired on. Full UPN.

<br>
<img width="1229" height="706" alt="image" src="https://github.com/user-attachments/assets/df1aa8e3-f63d-4886-8e11-5076ccafbdf8" />
<br>




## Summary

Quiet but hostile take over the m.smith@lognpacific.org account was obtained via token theft and persistence was established through email forwarding rules via Power Automate. The bad actor using 7 of the Microsoft Suite apps. 



## Response Taken

Revoked all sessions from the *m.smith* account and reset the password. Without revoking the sessions the bad actor could steal the token again and successfully pose as that user.
