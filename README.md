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

### This threat hunt was 39 flags, I will walk you through some of the questions and screenshots of the queries i used to find them.

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

### 11.I want the sign-in and the later activity tied to one session. Find the identifier that's in both and give it to me. Format: session GUID
<br>
<img width="1464" height="657" alt="Query + Highlight 11 Part 2" src="https://github.com/user-attachments/assets/9ee43e15-a41d-4113-b8d6-d10f327de2ac" />
<br>

<br>
<img width="1595" height="668" alt="Question 11 Query Pt 2" src="https://github.com/user-attachments/assets/95f79b77-8a1a-4cc6-9d30-bd0b29b00be1" />
<br>

### 14 From the mailbox they sent an internal email to redirect a payment. Find it. Subject line, exact
<br>
<img width="1457" height="749" alt="Query + Highlight 14" src="https://github.com/user-attachments/assets/5a0c1544-10c2-4adb-b6b6-a3c407976622" />
<br>

### 18 A mailbox forwarding rule was discovered that forwarded incoming responses from emails that the attacker sent posing as m.smith@lognpacific.com to be sent to the attackers actual email. The true M.smith never saw any responses from their co-workers so the actual victim never knew their account was being used to send malicious phishing payment emails. 

### 20 There's a second rule that sends mail outside the org. Give me the destination address.
<br>
<img width="1683" height="734" alt="Query + Highligh 20" src="https://github.com/user-attachments/assets/2616e231-8747-44f4-8751-c7071a490f92" />
<br>

### 27 Walk the apps that session touched. One of them is where automation gets built, not where a finance user works. Name it.

I viewed all of the apps that this session touched by filtering on the SessionID
<br>
<img width="1224" height="734" alt="Query 27" src="https://github.com/user-attachments/assets/d76d678b-760b-47be-8a1b-bb530ee0b421" />
<br>

### 34 Before you delete a rule or a flow, one action comes first or they're straight back in. What is it.
<img width="996" height="634" alt="Q + A 34" src="https://github.com/user-attachments/assets/82ae6fb6-c2e8-40ca-893b-3bb546f9fc1f" />




## Summary

Quiet but hostile take over the m.smith@lognpacific.org account was obtained via token theft and persistence was established through email forwarding rules via Power Automate. The bad actor using 7 of the Microsoft Suite apps. 



## Response Taken

Revoked all sessions from the *m.smith* account and reset the password. Without revoking the sessions the bad actor could steal the token again and successfully pose as that user.
