<h1>Nessus Home Lab</h1>

<h2>Description</h2>
In this lab, I installed Nessus Essentials on my host computer and used a new Windows 11 virtual machine as the scanning target. I followed the 
<a href="https://community.tenable.com/s/article/Troubleshooting-Credential-scanning-on-Windows?language=en_US">vendor recommended configuration</a>
steps on the host machine to enable credentialed scans, including editing registry files, opening ports, and starting services. After successfully running a credentialed scan, I remediated a few of the vulnerabilities that were found.

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

<h2>Contents</h2>

[1. Nessus Essentials setup](#1-nessus-essentials-setup)<br />
[2. Configuring vulnerability scan target](#2-configuring-vulnerability-scan-target)<br />
[3. Non-credentialed scan](#3-non-credentialed-scan)<br />
[4. Credentialed scan setup](#4-credentialed-scan-setup)<br />
[5. Credentialed scan](#5-credentialed-scan)<br />
[6. Vulnerability remediation](#6-vulnerability-remediation)<br />
[7. Remediated scan](#7-remediated-scan)<br />
[8. Finishing off](#8-finishing-off)<br />

<h2>Walkthrough</h2>

<h3>1. Nessus Essentials setup</h3>
This project was possible because Nessus Essentials is available as a free trial.
<br />
<br />
<img src="https://i.imgur.com/SqEy2qS.png"/>

After registering for an activation code, I connected to the Nessus dashboard through the web client.
<br />
<br />
<img src="https://i.imgur.com/1KofJKy.png"/>

Now that Nessus is set up, I set up a virtual machine to act as the vulnerability scan target.
<br />
<br />
<img src="https://i.imgur.com/bXc5kiR.png"/>

<h3>2. Configuring vulnerability scan target</h3>
The scan target is a freshly installed Windows 11 machine that I set up. I configured the virtual machine to accept pings from my host machine to ensure connectivity.
<br />
<br />
<img src="https://i.imgur.com/kClJSy3.png"/>

Afterwards, I ran a host discovery scan on the virtual machine’s IP address so that I could start vulnerability scanning.
<br />
<br />
<img src="https://i.imgur.com/8al2XQ9.png"/>

<h3>3. Non-credentialed scan</h3>
To test if everything was set up correctly, I ran a non-credentialed scan.
<br />
<br />
<img src="https://i.imgur.com/APSsPoP.png"/>

As expected, most of the vulnerabilities reported here are informational. To get a more in-depth analysis of the target machine, I moved onto the credentialed scan setup.
<br />
<br />
<img src="https://i.imgur.com/Rvw79QI.png"/>

<h3>4. Credentialed scan setup</h3>
These are troubleshooting steps to follow for credentialed scanning on Windows machines, which are provided in the 
<a href="https://community.tenable.com/s/article/Troubleshooting-Credential-scanning-on-Windows?language=en_US">vendor documentation</a>.
<br />
<br />
<img src="https://i.imgur.com/0Z3gya0.png"/>

First, I went into registry editor to enable LocalAccountTokenFilterPolicy. This allows the Nessus scanner to connect remotely to the virtual machine.
<br />
<br />
<img src="https://i.imgur.com/o89WfZT.png"/>

Next, I enabled the Windows Management Instrumentation (WMI) service on the virtual machine. 
<br />
<br />
<img src="https://i.imgur.com/7O3kc6d.png"/>

The documentation also specifies that ports 49152-65535 be open between the scanner and target, so I added a rule in the target’s firewall.
<br />
<br />
<img src="https://i.imgur.com/GRJe8oi.png"/>

Remote Registry must also be enabled on the target, so I double-checked that it was started on the target.
<br />
<br />
<img src="https://i.imgur.com/ID9jvrM.png"/>

Afterwards, I ensured that File & Printer Sharing was enabled on the target.
<br />
<br />
<img src="https://i.imgur.com/0F7Owmj.png"/>

TCP ports 139 and 445 must also be enabled between the scanner and the target, so I enabled the rules that were originally disabled on the target’s firewall.
<br />
<br />
<img src="https://i.imgur.com/w1SrH0B.png"/>
<img src="https://i.imgur.com/yE2v7hI.png"/>

Finally, I provided the virtual machine’s credentials on the Nessus dashboard so that the scanner could access it.
<br />
<br />
<img src="https://i.imgur.com/qPSY3h4.png"/>

<h3>5. Credentialed scan</h3>
<br />
<br />
<img src="https://i.imgur.com/l0ln427.png"/>

<br />
<br />
<img src="https://i.imgur.com/oFBlBP3.png"/>

<h3>6. Vulnerability remediation</h3>
<br />
<br />
<img src="https://i.imgur.com/70CyOPJ.png"/>

<br />
<br />
<img src="https://i.imgur.com/l1Jm6zs.png"/>

<br />
<br />
<img src="https://i.imgur.com/FHVDo9O.png"/>

<br />
<br />
<img src="https://i.imgur.com/5Z4YlvT.png"/>

<br />
<br />
<img src="https://i.imgur.com/XDDbiee.png"/>

<br />
<br />
<img src="https://i.imgur.com/Lzq3zsY.png"/>

<h3>7. Remediated scan</h3>
<br />
<br />
<img src="https://i.imgur.com/5KAzXie.png"/>

<br />
<br />
<img src="https://i.imgur.com/e71fgF3.png"/>

<h3>8. Finishing off</h3>
<br />
<br />
<img src="https://i.imgur.com/lIISmv8.png"/>

<br />
<br />
<img src="https://i.imgur.com/jyEl2sT.png"/>

<br />
<br />
<img src="https://i.imgur.com/2QBjuDB.png"/>

<!--

<br />
<br />
<img src=""/>

--!>
