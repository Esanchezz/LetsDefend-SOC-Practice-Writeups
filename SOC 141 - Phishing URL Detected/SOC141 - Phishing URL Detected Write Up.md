# SOC141 - Phishing URL Detected

## Investigation Overview
Ticket Type: Phishing URL Detected <br>
EventID: 86 <br>
Event Time: Mar, 22, 2021, 09:23 PM <br>
Source IP: 172.16.17.49 <br>
Source Hostname: EmilyComp <br>
Destination IP: 91.189.114.8 <br>
Destination Name: mogagrocol <br>
Username: ellie <br>
Request URL: http://mogagrocol.ru/wp-content/plugins/akismet/fv/index.php?email=ellie@letsdefend.io<br>
User Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.88 Safari/537.36 <br>
Device Action: Allowed <br>

## Objectives
- Identify whether this was a True Positive or False Positive
- Investigate the URL
- Analyze what techniques the threat actor used in the URL

## Findings
### Log Management Inspection
The first thing I did was look up the Source IP  log management software to look for suspicious content from the logs. <br>
There was 2 logs that caught my eye at the Event Time (Mar 22, 2021 09:23). <br>
The both the logs shared the same fields except the the types. The first log was a Proxy while the second one was a firewall. <br>
The log with the proxy gave me the suspicious URL link that I saved for later investigation.

### Endpoint Security Inspection
I then went to contain the host system: Emilycomp, for good practice of preventing potential further infection. <br>
Although there weren't any suspicious logs in the Network Action, Terminal History, and Browser History at the time of the event, there was a Process log that had a interesting .exe file that was ran. <br>
The executable file was named "KBDYAK.exe" and I took note of this file for later inspection.

### Threat Intelligence Inspection
I then looked into a few things on Virustotal, Talosintelligence, and Any.run. <br>
On Virustotal I looked into the Source IP, the suspicious URL, and the "KBDYAK.exe" MD5 and these were the results I found. <br>
The Source IP was found normal, even after I reanalyzed the address. <br>
On the other hand, the URL was labeled malicious or phishing by 11/96 security vendors at the time of making this writeup. <br>
The MD5 of the "KBDYAK.exe" file was labeled as a trojan.emotet by 68/76 security vendors. <br>

**What is a Trojan.emotet?**
Emotet is a trojan that acts as a downloader/dropper of other malware. It is spread through phishing email attachments and links where once it is clicked, it launches a payload. This is where the malware tries to spread itself in the network by attempting brute force into shared devices. It's like a worm, where it tries to infect others through the whole network. To find out more about the Emotet trojan, please learn more from the official CISA.gov website. https://www.cisa.gov/news-events/cybersecurity-advisories/aa20-280a <br>

Using Any.run, I was able to simulate another instance of KBDYAK.exe being executed and in this case, it downloaded another malicious virus disguising itself by  using the name "ws2help.exe". <br> 
There is a real file under the name "ws2help.dll".<br>
Using Talosintelligence I identified that the destination IP was located in Moscow, Russia and again, the URL is considered untrusted.



### Case Management
This is my general summary of what I triaged for the incident. This was my analysis:
- The Log Management shows that the type of incident was done through a Proxy but another log shows that it was also met with a Firewall as well.
- Using VirusTotal, Talosintelligence, and Any.run, I was able to identify that the URL was malicious, the file associated with the URL is a Emotet trojan, and the destination for the URL leads to Russia.
- The URL was accessed.
- In order to reduce the impact of the potential infection to other systems, I isolated the client user's system.
- This attack happened due the user clicking on the malicious link.

## Final Thoughts
This ticket is a simple example of a phishing attempt URL. I think a simple way this could be prevented is simply by training users to not click on suspicious links. I was surprised to learn about the emotet trojan that could be downloaded once the link was accessed. I think the other log that shows the Firewall is defining that the Firewall successfully stopped any further damage once the link was clicked. This was a True Positive event.
