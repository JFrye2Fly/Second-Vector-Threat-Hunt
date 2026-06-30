# Second-Vector Threat Hunt hreat-hunt-scenario-suspicious-rdp-login

# Official [Cyber Range](http://joshmadakor.tech/cyber-range) Project

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

### 2. Same incident. What address did the flagged sign-in come from
<br>
<img width="1232" height="756" alt="Query + Highlight 2" src="https://github.com/user-attachments/assets/aa45f03e-3a00-4494-9b0f-4adcbb4cb472" />
<br>


### 4. The incident title is the friendly name. Pull the risk telemetry and give me the detection type as it's stored. Format: single token, as stored in the risk telemetry
<br>
<img width="1330" height="762" alt="Query + Highlight 4" src="https://github.com/user-attachments/assets/e43afa67-ae7b-4b38-99b3-bdacac07dc97" />
<br>

### 5. This user has more than one risk detection. Aggregate them by state and tell me where most of them ended up.
<br>
<img width="1240" height="795" alt="Query 5" src="https://github.com/user-attachments/assets/b1bb7ba0-e132-4cae-95d7-30baa48e86fe" />
<br>

### 7. Tenant enforces MFA. Their session ran anyway. Pull the successful sign-ins from that address and give me the field that explains it."
<br>
<img width="1087" height="910" alt="Query + Highlight 7" src="https://github.com/user-attachments/assets/c9170dac-4d1b-4749-8e66-ac1e37823de8" />
<br>

### 8. Some attempts were stopped, one app let them in. Order the sign-ins from that address and name the app on the first success. Format:the application display name exactly as stored in the sign-in telemetry
<br>
<img width="1087" height="910" alt="Query + Highlight 7" src="https://github.com/user-attachments/assets/c9170dac-4d1b-4749-8e66-ac1e37823de8" />
<br>

### 10. That session reached multiple apps with no re-prompt. Count the distinct apps it got into.
<br>
<img width="1158" height="707" alt="Query + Highlight 10" src="https://github.com/user-attachments/assets/f687523f-b0f5-49dc-b515-ab6feeff363a" />
<br>



## Summary

Quiet but hostile take over the m.smith@lognpacific.org account was obtained via token theft and persistence was established through email forwarding rules via Power Automate. The bad actor using 7 of the Microsoft Suite apps. 



## Response Taken

Revoked all sessions from the *m.smith* account and reset the password. Without revoking the sessions the bad actor could steal the token again and successfully pose as that user.
