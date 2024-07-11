<h1>Nessus Home Lab</h1>

<h2>Description</h2>
In this lab, I installed Nessus Essentials on my host computer and used a new Windows 11 virtual machine as the scanning target. I followed the 
<a href="https://community.tenable.com/s/article/Troubleshooting-Credential-scanning-on-Windows?language=en_US">vendor recommended configuration</a>
steps on the host machine to enable credentialed scans, including editing registry files, opening ports, and starting services. After successfully running a credentialed scan, I remediated a few of the vulnerabilities that were found.
<br />
<br />
<img src="https://i.imgur.com/l0ln427.png"/>
In the walkthrough below, I detail many of the steps I took to configure Nessus and the results of my scans. Thank you for taking a look!

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
[7. Post-remediation scan](#7-post-remediation-scan)<br />
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

[Back to top](#nessus-home-lab)

<h3>2. Configuring vulnerability scan target</h3>
The scan target is a freshly installed Windows 11 machine that I set up. I configured the virtual machine to accept pings from my host machine to ensure connectivity.
<br />
<br />
<img src="https://i.imgur.com/kClJSy3.png"/>

Afterwards, I ran a host discovery scan on the virtual machine’s IP address so that I could start vulnerability scanning.
<br />
<br />
<img src="https://i.imgur.com/8al2XQ9.png"/>

[Back to top](#nessus-home-lab)

<h3>3. Non-credentialed scan</h3>
To test if everything was set up correctly, I ran a non-credentialed scan.
<br />
<br />
<img src="https://i.imgur.com/APSsPoP.png"/>

As expected, most of the vulnerabilities reported here are informational. To get a more in-depth analysis of the target machine, I moved onto the credentialed scan setup.
<br />
<br />
<img src="https://i.imgur.com/Rvw79QI.png"/>

[Back to top](#nessus-home-lab)

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

[Back to top](#nessus-home-lab)

<h3>5. Credentialed scan</h3>
After completing the configuration steps above, I ran a test credentialed scan to see if everything was set up correctly.
<br />
<br />
<img src="https://i.imgur.com/l0ln427.png"/>

Thankfully, it worked. We can now see some high and medium severity vulnerabilities in the scan now.
<br />
<br />
<img src="https://i.imgur.com/oFBlBP3.png"/>

[Back to top](#nessus-home-lab)

<h3>6. Vulnerability remediation</h3>
The remediations tab is available now. Since this is a freshly installed Windows 11 machine, the only remediation Nessus suggests is a Windows update to the latest version. However, there was a high and medium severity vulnerability that I had to fix manually.
<br />
<br />
<img src="https://i.imgur.com/70CyOPJ.png"/>

The high severity vulnerability described that the target machine is missing the EnableCertPaddingCheck registry keys in two places. One is required for the machine, and a second one is required because the machine is running a 64-bit OS.
<br />
<br />
<img src="https://i.imgur.com/l1Jm6zs.png"/>

To remediate this, I created a text file to add these keys to the registry and enable them. Saving and then executing the file remediated the vulnerability.
<br />
<br />
<img src="https://i.imgur.com/FHVDo9O.png"/>

The medium severity vulnerability describes that SMB signing is not required on the target machine.
<br />
<br />
<img src="https://i.imgur.com/5Z4YlvT.png"/>

The related registry file is named “EnableSecuritySignature,” so I set its value to 1 to enable SMB signing.
<br />
<br />
<img src="https://i.imgur.com/XDDbiee.png"/>

The low severity vulnerability describes that the target machine answers to ICMP timestamp requests. The solution is to filter out the incoming ICMP timestamp requests and outgoing ICMP timestamp replies.
<br />
<br />
<img src="https://i.imgur.com/Lzq3zsY.png"/>

I created an inbound rule on the target's firewall to block ICMP Timestamp requests and ICMP Timestamp replies (
<a href="https://www.oreilly.com/library/view/internet-core-protocols/1565925726/re57.html">type 14, code 0</a>
).
<br />
<br />
<img src="https://i.imgur.com/sKkcqpC.png"/>

In addition, I added a registry file per this 
<a href="https://learn.microsoft.com/en-us/answers/questions/1691269/disable-icmp-timestamp-responses">Microsoft forum question</a>
to disable ICMP Timestamp replies.
<br />
<br />
<img src="https://i.imgur.com/heVIgb1.png"/>

Though I created an inbound rule and created a registry file to block these specific ICMP messages, the vulnerability persisted in the post-remediation scan. 
<br />
<br />
I believe this may be a false positive, and research on my part shows that many others have taken similar procedures just to end up with the alert still being there. I decided to leave the alert and move on.

[Back to top](#nessus-home-lab)

<h3>7. Post-remediation scan</h3>
After remediating the vulnerabilities, I ran another credentialed scan to confirm that they were fixed. 
<br />
<br />
<img src="https://i.imgur.com/e71fgF3.png"/>

As we can see in the screenshots, the remediation steps I took worked, and we no longer see them in the Nessus dashboard.
<br />
<br />
<img src="https://i.imgur.com/5KAzXie.png"/>

[Back to top](#nessus-home-lab)

<h3>8. Finishing off</h3>
To recap - our initial scan on the target was uncredentialed, so we did not see many vulnerabilities.
<br />
<br />
<img src="https://i.imgur.com/lIISmv8.png"/>

After configuring settings for credentialed scans, we saw more vulnerabilities on the target that we could then remediate.
<br />
<br />
<img src="https://i.imgur.com/jyEl2sT.png"/>

After remediating the vulnerabilities, they no longer appear on the dashboard. The remainder of the alerts are informational.
<br />
<br />
<img src="https://i.imgur.com/2QBjuDB.png"/>

[Back to top](#nessus-home-lab)
