# SOC176 - RDP Brute Force Detected

## Investigation Overview
Ticket Type: RDP Brute Force Detected
EventID: 234
Event Time: Mar, 07, 2024, 11:44 AM
Source IP: 218.92.0.56
Destination IP: 172.16.17.148
Destination Name: Matthew
Protocol: RDP
Firewall Action: Allowed
Alert Trigger Reason: "Login failure from a single source with different non existing accounts"

## Objectives
- Identify whether this was a True Positive or False Positive
- Investigate the source of the attack

## Findings
### Log Management Inspection
The first thing I did was look up Matthew's destination IP on the log management software to inspect the logs to see if there was any suspicious activities that occured.
Unsurprisingly, the alert was correct that there was a Bruteforce attempt starting at 08:44 AM.
There was a total of 15 attempts that occured from the start time with multiple cases of different but reoccuring usernames such as:
- Matthew
- sysadmin
- guest
- admin
Out of the 15 attempts, there was 1 successful login.

### Endpoint Security Inspection
After discovering this, I went to the provided endpoint security software and looked up Matthew's host IP and contained it.
In this window, we are able to see the past processes that occured on the Event Time as well as Network Actions, Terminal History, and Browser history.
In this case, I observed the Network Actions by the Event Time and discovered 5 instances where the malicious IP was recorded.

### Threat Intelligence Inspection
When searching up the Source IP on Virustotal.com, it was flagged by 11/94 security vendors that the IP address was malicious.
Using cisco's Talosintelligence.com, I was able to observe that the location that the IP came from was from Nanjing, China.


### Case Management
For my final actions regarding this ticket, I filled a general summary of what I triaged for the incident. This was my analysis:
- The Source IP was external.
- Based on information found from VirusTotal and TalosIntelligence, I verified that the attacker IP was malicious.
- After inspecting the Log Management, there were requests from the attacker IP to the target server's RDP port.
- The attacker IP tries to establish connection with only 1 server/client as the target.
- Inspecting the audit logs, there was one successful attempt of Bruteforce into the user's Windows 10 OS
- In order to reduce the impact of the cyber-attack, I isolated the user's client.
- This attack happened due a external Bruteforce attempt on an internal user client.
- What management can do differently next time is to improve proactive controls like limiting the RDP access. Due to the delayed detection, this allowed the attacker to successfully login before action was taken.
- One corrective action I can think of would be to automate containment processes like instant IP blocking after a predefined threshold of failed logins.
- Another would be to enforce stronger password policies in order to prevent brute force successes.

## Final Thoughts
I think that this ticket is a clear example of an attack that could happen to a vulnerable RDP port and weak password policies. If the company were to make a quick remediation to prevent attacks like this in the future, I would recommend stronger password policies like complex passwords or earlier password rotation.