# SOC251 - Quishing Detected

## Investigation Overview
Ticket Type: Quishing Detected
EventID: 214
Event Time: Jan, 01, 2024, 12:37 PM
SMPTP IP Address: 158.69.201.47
Source Address: security@microsecmfa.com
Destination Address: Claire@letsdefend.io
E-mail Subject:"New Year's Mandatory Security Update: Implementing Multi-Factor Authentication (MFA)"
Device Action: Allowed

## Objectives
- Identify and describe the nature of the phishing attack.
- Assess why the email might be convincing to the recipient.
- Investigate the SMTP source address (158.69.201.47) for it's origin and reputation.
- Analyze the sender email (security@microsecmfa.com) for spoofing or association with malicious activity.
- Explain how Quishing works and the potential risks if the QR code was scanned.

## Findings
### Log Management Inspection
First, I searched the logs for SMTP IP Address (158.69.201.47) to find any instances in the log management software.
There was only one log for the SMTP Address and it's destination was to Claire@letsdefend.io with a destination address of 172.16.20.3.

### Endpoint Security Inspection
I then checked the endpoint security log for the address 172.16.20.3 to find out if the IP was to a person's computer.
I found out that the destination IP was for an exchange server.
An Exchange Server is a a Microsoft developed email, calendaring, and contact managment system. It's a server application that runs on Windows Server operating systems.

### Email Security Inspection
This is where I preformed an analysis on the suspicious email itself.
The email's subject header says "New Year's Mandatory Security Update: Implementing Multi-Factor Authentication (MFA)" which makes the victim gain a sense of urgency as it looks like their account needs security attention.
What we can see on the email is the contents immitates an official Microsoft email and tells the victim that there was a mandated update to enable 2 factor authentication security on the associated account by 02/01/2024 further proving my statement that the threatactor is creating a sense of urgency to the victim.
The contents of the email also provides an image of a QR code that tries to entice the victim to change the user's login details there.
At the end of the email, the content even says that if the user fails to comply, then their account may be blocked.

### Threat Intelligence Inspection
Using Virustotal.com, I first inspected the source IP address (158.69.201.47) and found that it was flagged by 8/94 security vendors stating that it was either malicious or phishing.
The source IP came from Dorval, Canada according to AbuseIPDB.
Using Cyberchef, I inspected the QR code and it actually provided me the malicious URL (https://ipfs.io/ipfs/Qmbr8wmr41C35c3K2GfiP2F8YGzLhYpKpb4K66KU6mLmL4ipfs.io).
I wasn't able to see the contents of the website in a virtual environment because it was already blocked but, from previous experience, I would asssume that the URL from the QR code would also be disguised as an official Microsoft website in order to gain more credentials from the victim user much like a normal phishing attempt.

### Case Management
For my final actions regarding this ticket, I filled a general summary of what I triaged for the incident. This was my analysis:
- I verified that the alert for the suspcicious email was malicious by checking the validity of the source IP.
- Through the log management tool, I confirmed that the activity was done through the use of the Exchange server.
- Upon inspecting the endpoint security tool, there was no suspicious HTTP requests or unusual DNS requests in the destination IP.
- Through analysis made by the endpoint security tool, we are confident to say that the type of Reconnaissance technique this attack used was Phishing for Information.
- From the IP Logs, we can see that the attacker IP is external.
- Upon checking AbuseIPDB and VirusTotal, we confirm that the attacker IP is malicious.
- Only one affected device was found in the Email Security tool.
- The user was attacked, therefore I contain the host.
- This attack happened due to a Quishing attempt on an internal user client.
- Staff and management would need to fix the email rules and filters to better find out that the email was a falsified Microsoft email.
- To prevent further incidents, users who find a suspicious email should immediately report it to IT security.

## Final Thoughts
The Quishing attempt was a true positive, with the malicious email being successfully flagged by the security tools. Although no system compromise occurred, this incident highlights the importance of proactive defenses and user education in mitigating social engineering attacks.