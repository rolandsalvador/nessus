<h1>Nessus Home Lab</h1>

<h2>Description</h2>
In this lab, I installed Nessus Essentials on my host computer and used a new Windows 11 virtual machine as the scanning target. I followed the 
<a href="https://community.tenable.com/s/article/Troubleshooting-Credential-scanning-on-Windows?language=en_US">vendor recommended configuration</a>
steps on the host machine to enable credentialed scans, such as editing registry files, opening ports, and starting services. After successfully running a credentialed scan, I remediated a few of the vulnerabilities that were found.

<br /> In the walkthrough below, I detail many of the steps I took to configure Nessus and the results of my scans. Thank you for taking a look!

<h2>Skills Used</h2>

- <b>Vulnerability Management</b> 
- <b>Firewalls</b>
- <b>Registry Editor</b>
- <b>Windows Configuration</b>
- <b>Virtualization</b>

<h2>Environments Used </h2>

- <b>Windows 11</b>
- <b>VMware Workstation Pro</b>

<h2>Walkthrough</h2>

<!--
<p align="center">
Launch the utility: <br/>
<img src="https://i.imgur.com/62TgaWL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select the disk:  <br/>
<img src="https://i.imgur.com/tcTyMUE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Enter the number of passes: <br/>
<img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Confirm your selection:  <br/>
<img src="https://i.imgur.com/cdFHBiU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
--!>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
