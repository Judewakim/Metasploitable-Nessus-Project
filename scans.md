## Scans

**Nessus Scans**<br>
-Download and install Nessus on either the attacker machine or even better, the host machine or anywhere else.<br>
-Connect to Nessus Essentials (the free one), create an account/sign in, wait for plugins to finish (which takes hours)<br>
-Start a new scan (host discovery finds lives hosts and open ports like nmap and a basic network scan runs a full scan on the host).<br>
-Configure the template for the scan so you can reuse it later if needed. Configure the name, description, targets, schedule, scan types, reporting details, credentials (for authenticated scans which go deeper and provide more information).<br>
-When the scan is complete a PDF report will be available that identifies the vulnerabilities, categorizes them based on severity, and provides actionable recommendations to resolve them.<br>
-Details about each vulnerabilty is available for further analysis which contains CVE information from MITRE&ACK, how to vulnerability was discovered by the scan, the severity grading scale, ways to resolve the vulnerability, and information about the affects of explotation.<br>
-Review each vulnerability, create a more in-depth report than the one provided including which vulnerability should be handled first, the reasoning the order, the path to closure, the cost of each fix, and any other relevant information.<br>
<br>

**Advanced Nmap Scans**<br>
-**nmap [ip addr] >> [txt doc]** : to append the results of the nmap scan to a text doc. Example: "nmap 111.222.333.444 >> nmap_results.txt"<br>
-**nmap -A [ip addr]** : an aggressive scan which detects OS, versions of running software, script scanning, traceroute scan. If the target has an Intrusion Prevension System (IPS) this type of scan might get most of the detailed blocked. Example: "nmap -A 111.222.333.4444"<br>
-**nmap -F [ip addr]** : a fast scan which only scans most common 100 ports instead of a normal scan which which thousands. Example: "nmap -F 111.222.333.444"<br>
-**nmap -sS [ip addr]** : this is a tcp/syn scan. It sends packets to the target and if the port is open it will send ack packages back to the attacker but it does not complete the handshake. Because it never completes the handshake it is a quiet scan, meaning no established connections will show in log files. Example: "nmap -sS 111.222.333.444"<br>
-**nmap --scan-delay [time value in ms] [ip addr]** : to slow down the scan so it does not get detected by an IDS and blocked by an IPS because activity that is too fast is suspicious and will get flagged. Example: "nmap --scan-delay 200ms 111.222.333.444"<br>
-**nmap -f [ip addr]** : to fragment the scan into smaller packets to help bypass firewalls and filters. Large scans may have limits and may get blocked for being suspiciously large trying to get through the network. Example: "nmap -f 111.222.333.444"<br>
 <br>

 -For a walkthrough on these scans, watch this YouTube video:
 https://youtu.be/zfn-p_D_ADQ?si=M_NkdA6HKeMUejdU <br>
