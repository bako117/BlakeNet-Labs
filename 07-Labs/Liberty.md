
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
