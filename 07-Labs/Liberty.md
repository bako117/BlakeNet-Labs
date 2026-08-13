

Liberty launched a massive project with a budget exceeding $100 million, with the goal of granting true freedom to humanity.

One day, a system administrator discovered a shared folder configured with “Full Control” permissions granted to “Everyone”, raising concerns about a potential security incident.

You have been tasked with investigating evidence collected from the affected endpoint to determine what occurred. Threat Intelligence has previously identified that an employee’s credentials were harvested by a RedLine Stealer, which is suspected to have been used for initial access to this system.

Open security logs, filter for event codes 4625 to spot the password spray, then go by 4624s to find the successful account. 

1- multiple fails in a row from same source and different accounts
2 - v.hunter shown by 4624's
3 - registry items
4 - explain the existence of this is sus
5 - view the file using mft viewer
6 - view the file using mft viewer
7 - Net-NTLMv2
8 - Found using registry for users
9 - Event logs RDP type 10
10 - Registry for users
11 - SevenZip registry read - arkproj.zip
12 - mafs it up after using mftcmd
13 - "C:\Users\bako\Desktop\Liberty\Users\k.texus\AppData\Local\Microsoft\Edge\User Data\Default\History"
14 -regixsstry for users - User with no comment added, initially thought it was the sus one
15 - "C:\Users\bako\Desktop\Liberty\Users\k.texus\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt"
16 - 
17 - "C:\Users\bako\Desktop\Liberty\Windows\System32\winevt\logs\Microsoft-Windows-PowerShellWebAccess%4Operational.evtx"
18 - "C:\Users\bako\Desktop\Liberty\Windows\System32\winevt\logs\Microsoft-Windows-PowerShellWebAccess%4Operational.evtx"
19 -  "C:\Users\bako\Desktop\Liberty\Windows\System32\winevt\logs\Microsoft-Windows-PowerShellWebAccess%4Operational.evtx"
20 - 

File Size
13857+
132675+
74110+
16395+
255143+5380+
286347



The fictional enterprise Liberty had a $100M+ project promising humanity “true freedom.” But a single misconfiguration in its file shares opened the door to an attack chain that progressed from RedLine Stealer credential theft, to a phishing page capturing a Net-NTLM hash, to RDP access and, ultimately, a persistent backdoor. Across 20 questions in this retired Hack The Box blue-team capture-the-flag challenge, I reconstructed the attack using the provided Windows triage image and my blue team toolset.

To complete this challenge, I worked entirely in my **FLARE-VM**, Mandiant’s Windows-based security distribution. While FLARE-VM is primarily designed for malware analysis and reverse engineering, I’ve found it to be an excellent all-purpose blue-team and DFIR environment thanks to the extensive collection of security tools that come preinstalled. If you haven’t used it before, I highly recommend checking out the project [here](https://github.com/mandiant/flare-vm).

Additional tools I used were:

- [**KAPE**](https://www.kroll.com/en/services/cyber/reactive-services/kroll-artifact-parser-and-extractor-kape) -  for collecting and working with forensic artifacts from the Windows triage image.
- [**Eric Zimmerman’s Tools**](https://ericzimmerman.github.io/) - for parsing and analyzing Windows forensic artifacts such as the MFT, registry data, event logs, and other evidence collected during the investigation.
- [**Event Log Expert**](https://github.com/microsoft/Eventlogexpert) - I have yet to meet someone who is a fan of Window's event viewer....This tool is easier and faster to use than the native installed windows event viewer. 

Now that the stage is set, it's time to dive into the challenge. 

Q1: You suspect that a threat actor might conduct password spraying attack on this server, How many failed logon attempts identified before successfully identifying the correct pair of the credential?

Answer: `5`

To investigate, I opened the triage image’s Security event logs in Event Log Expert and filtered for **Event ID 4625**, which records failed Windows logon attempts. I identified five failed logons originating from the same source in rapid succession and targeting different accounts before a successful authentication occurred. This pattern is consistent with password-spraying activity.

![[Pasted image 20260812203640.png]]

Q2: What is the user that was identified by the threat actor?

Answer: `v.hunter`

Expanding the previous filter to include successful Windows logons (**Event ID 4624**), I identified a successful authentication for the `v.hunter` account originating from the same suspicious source associated with password spray. 

![[Pasted image 20260812204441.png]]

Q3: There is a shared folder that can be accessed by all users, what is the name of this shared folder?

Answer: `Proposal`

