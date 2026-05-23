
After I had a server go down for days without notice, I needed more alerting setup using Uptime Kuma. I created a webhook within Discord and then hooked it up to all of my uptime Kuma synthetics.  

![[Pasted image 20260523080241.png]]

Then to test they worked I used the test button, but I was not happy with that. I wanted to give it a full test and see what an alert would look like. So I sent the shutdown command to a less critical server and waited for the synthetics to trigger. 

![[Pasted image 20260523080546.png]]

When the server "crashed" it alerted as expected. Then it came up and I was notified as well. 

**TASK COMPLETE BOOYAH**