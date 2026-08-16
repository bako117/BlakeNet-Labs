

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

The answer is located within the registry. I used KAPE along with EZParser modules, paired with the given triage image, to generate easily digestible artifacts. This let me identify registry entries for network shares located at: `ROOT\ControlSet001\Services\LanmanServer\Shares`. Here we see an entry for a share named `Proposal`, with permissions equal to 9, implying anyone can reach it.

I made a note here as well, because the share's description says any file uploaded will be reviewed by user Texus. This could be very concerning if abused.

![[Pasted image 20260816084956.png]]

To answer the next four questions, I used `MFT Explorer` by Eric Zimmerman to open the `$MFT` file on the given image. This lets us view information about files on the device in a database format. If a file is small enough, it's actually stored within the `$MFT` file itself.

Q4: The threat actor uploaded several files to the previously identified shared folder. One of these files can be used to capture the hash of a user who opens it. What is the name of that file? 

Answer: `Proposal.url`

After navigating to the shared folder in `MFT Explorer` , I immediately noticed a file with a suspicious extension. `Proposal.url` . `.url` files are shortcuts that can be weaponized to initiate NTLM authentication just by being viewed in Explorer, leaking the user's hash to an attacker-controlled listener.

Q5: What is the full URL used by threat actor to mimic the fake proposal of the project?

Answer: `http://argonaut.ark/proposal.html`

The file itself is actually small enough that it is within the MFT. While viewing it's contents you can see the URL set up for the shortcut file. 

![[Pasted image 20260816000237.png]]

Q6: What is the full UNC path of the network share that the threat actor used to capture hash of the victim? 

Answer: `\\192.168.189.129\%USERNAME%.icon`

Within the same place you can view the URL, you can also see the share the attacker is convincing the victim to attempt to authenticate to. Thus giving over their password hash that can be used against them. 

Q7: What is the format of the hash that the threat actor captured via this method? 

Answer: `Net-NTLMv2`

When the victim's system authenticates to the attacker's SMB share, it doesn't send the raw NTLM hash it completes a challenge-response handshake instead. This response is what gets captured, that default exchange format is `Net-NTLMv2`.

Q8: What is the full name of the second compromised user? 

Answer: `Kuneo Texus`

Going back to the security logs used for questions 1 and 2, I was curious whether there were any other logons from the suspicious IP. I noticed RDP logons from that IP under the user `k.texus`. Using the registry parsing output from KAPE, I viewed the Windows user account data in the registry, which revealed the final answer.

![[Pasted image 20260816085215.png]]

Q9: When was the time that the threat actor connected to the server via RDP in UTC? 

Answer: `2025-06-11 14:44:48`

I searched for a a `Logon Type` equal to 10 within the security logs. This specific type of success is indicative of an RDP logon. Within the event XML, there is a field called TimeCreated SystemTime that was the answer. 

![[Pasted image 20260816084833.png]]

Q10: The threat actor discovered a folder that stores files about the project, What is the full path of this folder? 

Answer: `C:\ProjectArk`

I discovered this folder previously while searching through the `$MFT` and submitted it as the answer. Another, more technical way this could've been done is by examining **Shellbags**, which track folders a user has browsed via Explorer, even after the folder itself has been deleted. `k.texus`'s `UsrClass.dat` reveals that he last interacted with the folder around the same time as the RDP logon, tying the access directly to the attacker's session

![[Pasted image 20260816085729.png]]

Q11: The threat actor created an archive file containing all files of the previously identified folder, What is the name of this archive file? 

Answer: `arkproj.zip`

The answer to this was found within the seven zip registry data for `k.texus`. 

![[Pasted image 20260816090026.png]]

Q12: What is the total bytes of all files on that folder which were compressed into previously identified archive file? (not including Zone Identifier) 

Answer: `783907`

For this question I went back the the `$MFT`. First, I used the `MFTECmd` to parse the entire table. Here is the command I ran: 

```
```

Q13: The threat actor uploaded the previously identified file to C2 website, What is the domain of this website? 

Answer: `yourc2filemanager.cn`

Q14: While reviewing users on this server, you found a suspicious user on this server, What is the name of this user? 

Answer: `t.minami`

Q15: The threat actor installed a web-based gateway as a backdoor to the server. What is the full command used to install this feature? 

Answer: `Install-WindowsFeature -Name WindowsPowerShellWebAccess -IncludeManagementTools`

Q16: Which protocol has to be enabled to use this feature? Answer: `WinRM`

Q17: Provide the UTC timestamp when the threat actor confirmed successful backdoor access through the previously identified user account. Answer: `2025-06-11 14:54:55`

Q18: What is the Session ID of this connection? Answer: `LIBERYSV08\t.minami.250611.075455`

Q19: Provide the UTC timestamp When was this session terminated by the threat actor Answer: `2025-06-11 14:55:40`

Q20: What is the name of shared folder that was created by the threat actor during the invasion? Answer: `ProjectArk`

