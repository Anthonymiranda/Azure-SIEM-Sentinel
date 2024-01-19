<h2>Failed RDP to IP Geolocation Information</h2>

Made a Honeypot on Azure cloud running a Windows 10 Virtual Machine with all ports open to draw on brute force attacks and parse the logs via the SIEM

<h2>Description</h2>

The Powershell script in this repository is responsible for parsing out Windows Event Log information for failed RDP attacks and using a third-party API to collect geographic information about the attackers' location.
   - [Powershell Script](https://github.com/Anthonymiranda/Azure-SIEM-Sentinel/blob/main/Powershell%20Scrip)
     
The script is used in this demo where I set up Azure Sentinel (SIEM) and connect it to a live virtual machine acting as a honey pot. We will observe live attacks (RDP Brute Force) from all around the world. I will use a custom PowerShell script to Look up the attackers' Geolocation information and plot it on an Azure Sentinel Map!
<br>
<br>
This is the Query used to parse the log data and extract the location, usernames, passwords, and other fields from the log:
 - [Log Query](https://github.com/Anthonymiranda/Azure-SIEM-Sentinel/blob/main/Query%20for%20Log%20Parsing)

<h2>Languages Used</h2>

PowerShell: Extract RDP failed logon logs from Windows Event Viewer
Utilities 
<h2>Utilities Used</h2>
ipgeolocation.io: IP Address to Geolocation API
<br>
<br>
<h2>World map of incoming attacks so far (built custom logs including geodata)</h2>
<img width="1440" alt="Screenshot 2024-01-19 at 11 31 47" src="https://github.com/Anthonymiranda/Azure-SIEM-Sentinel/assets/25447589/ac1e4d2f-e60c-4865-994e-0fed6dfe1eb6">


<h2> Steps Breakdown</h2>
Some steps are simple, like creating a virtual machine so I have skipped with the assumption that it's pretty easy for anyone to logging into Azure and click "create virtual machine"
<br>
<br>
After creating a virtual machine, it is time to make it open to the word. For this, I created a new network security group with inbound rules allowing all traffic.
<br>
<br>
I further proceeded to turn off the firewall in the machine to make it easy discoverable via pings. <br>
<br>
Soon after that, I started getting failed authentication logs in event viewer: <br>
<br>
<img width="1440" alt="Screenshot 2024-01-18 at 10 53 54" src="https://github.com/Anthonymiranda/Azure-SIEM-Sentinel/assets/25447589/dd83fa22-98f9-4da7-9881-0044812be164">
<br>
<br>
I wanted to know where the attacks where coming from so I created a custom log in Log Analytics Workspace for this (This is now under the "Tables" section:
<br>
<br>
Log analytics worspace created and connected to the virtual machine
<img width="1440" alt="Screenshot 2024-01-18 at 11 18 10" src="https://github.com/Anthonymiranda/Azure-SIEM-Sentinel/assets/25447589/0c671d3c-b6f2-4dfb-be3d-ba618e272296">
<br>
Creating "Table" which is the new way to create "custom logs" in Azure.
<img width="1440" alt="Screenshot 2024-01-18 at 11 40 43" src="https://github.com/Anthonymiranda/Azure-SIEM-Sentinel/assets/25447589/1428157a-f251-4c7f-bf1f-d025ad5c24a8">
<br>
<br>
<br>
To create this custom log, I had to let Azure know what the logs would look like, so I copy the logs from the virtual machine and uploaded the sample to Azure to teach it what a failed authenticaton event would look like:
<br>
<br>
<img width="1440" alt="Screenshot 2024-01-18 at 11 26 21" src="https://github.com/Anthonymiranda/Azure-SIEM-Sentinel/assets/25447589/46afb0f5-acd7-4062-9b17-371beb6474b0">
<br>
<img width="1440" alt="Screenshot 2024-01-18 at 11 34 06" src="https://github.com/Anthonymiranda/Azure-SIEM-Sentinel/assets/25447589/5460ba1b-42be-4774-a9da-33dbeab116ee">
<br>
<br>
Azure is now logging failed authentication and providing me with the logs necessary for further investigation:
<br>
<br>
honeypot log analytics without parsing 
<img width="1440" alt="Screenshot 2024-01-19 at 10 40 57" src="https://github.com/Anthonymiranda/Azure-SIEM-Sentinel/assets/25447589/3fa5f085-7faf-40a7-ae3c-31ad22827d1e">
<br>
<br>
Now, I want to programatically get the IP addresses from the logs and use the geolocation capabilities of "ipgeolocation.io" to get the lattitude and longitude of the attackers:
<br>
<br>
audit failure with the geoplocation
<img width="1440" alt="Screenshot 2024-01-18 at 10 56 16" src="https://github.com/Anthonymiranda/Azure-SIEM-Sentinel/assets/25447589/8419c15f-d7b1-4d34-8ecb-2dc881f2785e">
<br>
<br>
usign powershell to programatically get geolocations of attackers
<img width="1440" alt="Screenshot 2024-01-18 at 11 15 51" src="https://github.com/Anthonymiranda/Azure-SIEM-Sentinel/assets/25447589/6844ebd8-d82f-4ad3-8f29-49a65ce703e4">
<br>
<br>
Here we can see visually where the attackers are coming from:
<br>
<img width="1440" alt="Screenshot 2024-01-19 at 11 31 47" src="https://github.com/Anthonymiranda/Azure-SIEM-Sentinel/assets/25447589/ac1e4d2f-e60c-4865-994e-0fed6dfe1eb6">




